---
name: pre-deploy-security
description: Use before pushing to main, deploying to production, going live, or promoting to prod. Runs a layered security audit covering exposed secrets, publicly-readable databases, fake or missing login protection, SQL injection, dependency CVEs, and accidentally-committed credentials in git history. Trigger on phrases like "ready to deploy", "push to prod", "go live", "merge to main", or before any `git push origin main` / `vercel deploy --prod`.
---

# Pre-Deploy Security Check

## Overview

A layered pre-deploy security audit. Combines well-maintained OSS scanners with stack-aware code review, because tools catch known patterns, but real-world breaches usually come from custom auth code and misconfigured infra that scanners don't understand.

The four classes of bug this catches:

1. **Exposed secrets**: in source, in `.env*` files that shouldn't be tracked, or buried in git history
2. **Unprotected data layer**: public DB-mutation endpoints, missing auth on admin/cron routes, world-open CORS on cost-incurring routes
3. **Fake login protection**: plaintext or reversible "hashing", forgeable tokens, no rate limiting, predictable reset flows
4. **Injection + dependency CVEs**: string-concatenated SQL, vulnerable transitive deps, deprecated crypto

## When to use

**Use when:**
- User says "ready to deploy", "push to main", "promote to prod", "go live", "merge this", "ship it"
- About to run `git push origin main`, `vercel --prod`, `vercel deploy --prod`, `gh pr merge`, `flyctl deploy`, `wrangler deploy`
- Reviewing a PR that touches auth, payment, admin, API routes, or env-var handling
- Onboarding an unfamiliar repo and need a baseline read

**Do NOT use when:**
- Pushing to a feature branch with no deploy hooks
- Just running the local dev server
- The user explicitly says "skip the security check": record their reason but don't argue

## The audit pipeline

Run these in order. Stop at the first phase only if it surfaces something that absolutely must block deploy; otherwise continue and produce one consolidated report at the end.

```
Phase 1: Tool scans (Gitleaks + Semgrep + OSV-Scanner, optionally TruffleHog + njsscan)
Phase 2: Bespoke code review for stack-aware bugs the tools miss
Phase 3: Env-var presence + git/file hygiene
Phase 4: Consolidated report with severity, file:line, fix recommendation
```

## Phase 1: Tool scans

Skip a tool only if it isn't installed AND the user declined to install it. Never silently skip.

### 1a. Gitleaks (required): secrets in git history

```bash
# Install: brew install gitleaks
gitleaks detect --no-banner --redact --report-format json --report-path /tmp/gitleaks.json || true
gitleaks protect --no-banner --staged --redact || true  # also check unstaged
```

Parse `/tmp/gitleaks.json`. Each finding = one entry in the report.

If the tool flags `.env*` files: cross-check with `.gitignore` to confirm they're not tracked. Run `git ls-files | grep -E '\.env'`: only `.env.example` should ever appear.

### 1b. Semgrep (required): SAST with OWASP rules

```bash
# Install: brew install semgrep   (or pipx install semgrep)
semgrep --config p/owasp-top-ten \
        --config p/javascript \
        --config p/typescript \
        --config p/jwt \
        --config p/nextjs \
        --config p/react \
        --error --json --output /tmp/semgrep.json --quiet \
        --exclude node_modules --exclude .next --exclude dist --exclude build || true
```

For non-JS stacks swap rulesets:
- Python → `p/python`, `p/django`, `p/flask`, `p/fastapi`
- Go → `p/golang`, `p/gosec`
- Ruby → `p/ruby`, `p/rails`

### 1c. OSV-Scanner (required): dependency CVEs

```bash
# Install: brew install osv-scanner
osv-scanner --recursive --format json --output /tmp/osv.json . || true
```

Prefer OSV-Scanner over `npm audit`: it covers GHSA + npm + PyPI + Go modules + crates.io in one pass and has fewer false positives.

### 1d. TruffleHog (optional): verified live keys

```bash
if command -v trufflehog >/dev/null; then
  trufflehog git file://. --only-verified --json > /tmp/trufflehog.json 2>/dev/null || true
fi
```

`--only-verified` calls the real provider API for each candidate. A verified hit means the key is **currently live**, not just leaked; treat as P0.

### 1e. njsscan (optional): Node-specific patterns

```bash
if command -v njsscan >/dev/null; then
  njsscan --exit-warning --json -o /tmp/njsscan.json . || true
fi
```

## Phase 2: Bespoke code review (stack-aware)

Tools won't catch a custom auth implementation that *looks* fine but isn't. Read the actual code for these patterns.

### A. Password hashing

Grep for any of these and read the result:

```bash
grep -rnE "hashPassword|verifyPassword|password_hash|bcrypt|argon2|scrypt|pbkdf2|crypto\.createHash" \
     --include="*.ts" --include="*.tsx" --include="*.js" --include="*.py" --include="*.go" \
     . 2>/dev/null
```

Red flags:
- Custom `hashPassword` that uses `crypto.createHash('md5'|'sha1'|'sha256')` directly; these are NOT password hashes
- `Buffer.from(password).toString('base64')` masquerading as a hash
- `crypto.createHash('sha256').update(password).digest()` without a per-user salt
- A comment that says "TODO: use bcrypt in production"
- bcrypt cost < 10 / argon2id memory < 32 MB

The fix is always: bcrypt (cost 12), scrypt, or argon2id, with a backward-compat path that re-hashes legacy rows on next successful login.

### B. Login / reset-password protection

For each file under `app/api/auth/`, `app/api/login/`, `app/api/*/login`, `app/api/*/reset-password`:

- Is there a rate limit on the credential check? (per-IP AND per-email, both matter)
- Is the password policy enforced server-side? Floors below 10 chars + no complexity = weak
- Does the reset flow leak user-existence? (`"that email is not registered"` is a confirmation oracle)
- Are reset tokens generated with `crypto.randomBytes(32)` and stored hashed?
- Are session tokens HMAC-signed (or JWT with verification), NOT raw base64 of a JSON payload?

### C. Database access surface

```bash
# Find every DB-touching route
grep -rln "sql\`\|sql\.query\|prisma\.\|drizzle\|knex\|@vercel/postgres\|@supabase" app/ --include="*.ts" 2>/dev/null
```

For each:
- Is there a session check at the top? `getServerSession`, `getToken`, `auth()`, or custom; one must exist
- Are there any `INSERT`/`UPDATE`/`DELETE`/`DROP`/`TRUNCATE` paths without auth?
- Find any `sql.query(\`...${var}...\`)`: that's SQL injection. Tagged-template `sql\`...${var}...\`` is parameterized; `sql.query(string)` is not unless params array is also passed.
- Find any `ORDER BY ${...}` or `LIMIT ${...}`: these can't be parameterized by most drivers; need an allow-list.
- For Supabase / PostgREST: confirm Row Level Security is enabled on every table (`SELECT relname, relrowsecurity FROM pg_class WHERE relnamespace = 'public'::regnamespace;`).

### D. Migration / setup / debug endpoints

These get forgotten and shipped:

```bash
grep -rln "CREATE TABLE\|DROP TABLE\|migrate\|setup-db\|run-migrations" app/api/ --include="*.ts" 2>/dev/null
```

Any endpoint that runs DDL or seeds the DB MUST require an admin session OR a `Bearer` token (never `?key=` in the URL; that lands in logs, browser history, Referer headers).

### E. Cron / scheduled job auth

For platforms with built-in cron (Vercel, Cloudflare, Render):

```bash
grep -rln "CRON_SECRET\|x-vercel-cron\|authorization" app/api/cron/ --include="*.ts" 2>/dev/null
```

The check must fail-closed: `if (!process.env.CRON_SECRET) return 401`. A pattern like `if (process.env.CRON_SECRET && header !== ...) return 401` is fail-open: an unset env var disables auth entirely.

### F. CORS

For Next.js: read `vercel.json` / `next.config.js`. For Express / Hono / FastAPI: grep `Access-Control-Allow-Origin`. Any wildcard `*` on a route that calls a paid API (OpenAI, Anthropic, Twilio, SendGrid) is a billing-DDoS vector, any malicious site can drain the budget cross-origin.

### G. Public AI / paid-API endpoints

```bash
grep -rln "anthropic\.messages\|openai\.\|chat\.completions\|generateContent" app/api/ --include="*.ts" 2>/dev/null
```

Each must have: per-IP rate limit, request size cap, and either auth OR origin allow-list.

## Phase 3: Env-var presence + file hygiene

```bash
# Files that should never be tracked
git ls-files | grep -E '\.env($|\.)' | grep -v '\.env\.example'   # must be empty
git ls-files | grep -iE '(credentials|secret|private[_-]?key|service[_-]?account).*\.(json|pem|key)$'  # must be empty

# Files in the working tree that shouldn't even be on disk in deploy artifacts
ls -la | grep -E '\.env\.|credentials\.json|.*\.pem'  # informational
```

For Vercel deployments, confirm critical env vars are set:

```bash
if command -v vercel >/dev/null && [ -f .vercel/project.json ]; then
  vercel env ls production 2>/dev/null | grep -E "NEXTAUTH_SECRET|CRON_SECRET|DATABASE_URL|POSTGRES_URL" || \
    echo "WARN: could not enumerate production env vars (auth or link issue)"
fi
```

Stack-specific env-var checklists:
- Next.js + NextAuth: `NEXTAUTH_SECRET`, `NEXTAUTH_URL`
- Vercel cron: `CRON_SECRET` (and any handler should fail-closed when unset, see 2E)
- Stripe: `STRIPE_SECRET_KEY` not `STRIPE_PUBLISHABLE_KEY` accidentally on the server
- Any admin script endpoint: a separate `ADMIN_API_KEY` (rotated, not the DB password)

## Phase 4: Report

Produce one consolidated report. Structure each finding as:

```
[SEVERITY]  <one-line title>
File:       <path:line>  (or "git history" / "vercel.json" / "dependency")
Source:     <Gitleaks | Semgrep rule id | OSV CVE | Manual | TruffleHog>
Impact:     <one sentence: what an attacker gets>
Fix:        <concrete patch suggestion, 1–3 lines>
```

Severity ladder:
- **CRITICAL**: remote unauthenticated path to data exfiltration, account takeover, RCE, or unbounded billing. Block deploy.
- **HIGH**: authenticated escalation, sensitive PII leak, persistent XSS, SQLi requiring auth. Block deploy unless time-boxed exception agreed.
- **MEDIUM**: defense-in-depth gap, weak rate-limit, info disclosure. Flag, don't block.
- **LOW**: hygiene, deprecation, noisy log. Note in summary.

Group findings by severity in the report. Present **CRITICAL first**, and after the list, ask: *"apply all critical+high fixes now, only the criticals, or surgical-subset by number?"*

## Tool installation cheat sheet

```bash
# macOS
brew install gitleaks semgrep osv-scanner trufflehog

# Linux (one-liners)
curl -sSfL https://raw.githubusercontent.com/gitleaks/gitleaks/master/install.sh | sh
pipx install semgrep
curl -sSfL https://github.com/google/osv-scanner/releases/latest/download/osv-scanner_linux_amd64 -o osv-scanner && chmod +x osv-scanner
curl -sSfL https://raw.githubusercontent.com/trufflesecurity/trufflehog/main/scripts/install.sh | sh

# Node-specific (optional)
pipx install njsscan
```

If a required tool isn't installed: ask the user once whether to install it. If they decline, mark that phase as **skipped** in the final report; never silently drop a layer.

## What NOT to do

- Do NOT delete `.env*` files or `.git/` to "clean things up"; destructive and the user did not ask for that
- Do NOT auto-apply fixes without showing the diff and asking which to apply
- Do NOT push commits during the audit. Read-only by default.
- Do NOT report a clean bill of health if a required tool was skipped; say "audit incomplete: gitleaks not installed"
- Do NOT echo raw secret values back to the user, even when found. Reference by file:line and the first 4 chars only.

## Common rationalizations and the answer

| Excuse | Reality |
|---|---|
| "This is a small change, skip the audit" | Small changes are how secrets get committed. 90 seconds. |
| "We already ran this last week" | Dependencies, code, and env vars all changed since. Re-run. |
| "Gitleaks gave a false positive last time, skip it" | Add it to `.gitleaksignore` with a comment. Don't disable the layer. |
| "I don't have time to install semgrep" | `brew install semgrep` is 30 seconds. The argument is the time-waster. |
| "The CORS issue is fine, our users only come from one domain" | CORS is enforced by the browser, not the server. CSRF + cross-origin abuse don't care about your "only" users. |

## Reference: minimum bar by stack

- **Any:** Gitleaks + Semgrep + OSV-Scanner
- **Next.js / NextAuth:** + middleware coverage check on `/api/admin/*` and `/api/cron/*`, + CORS scoping
- **Supabase / PostgREST:** + RLS-on-every-table check
- **Stripe / payments:** + webhook signature verification check
- **Public AI chat endpoint:** + per-IP rate limit + Origin allow-list + request size cap
