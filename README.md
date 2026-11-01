# Claude Code skills for local SEO and safe deploys

Four [Claude Code skills](https://docs.claude.com/en/docs/claude-code/skills) I use on client sites
every week. Each one is a single `SKILL.md`, with no scripts and no dependencies.

| Skill | Use it when |
|---|---|
| [non-commodity-content](non-commodity-content/SKILL.md) | Writing anything meant to rank or be cited: posts, guides, landing pages. A pre-publish check for whether a page is unique, specific and first-hand, or a rewrite of what already ranks. |
| [local-eeat](local-eeat/SKILL.md) | Writing for a local service business: "[service] in [city]" pages, venue pages, service-area pages. Adds the local layer and a nine-part city page anatomy, plus the data gate that keeps generated city pages from becoming doorway pages. |
| [ai-search-readiness](ai-search-readiness/SKILL.md) | A site sells a priced service or takes bookings. Publishing a real price range, booking schema an agent can act on, and answers to the questions customers actually ask. |
| [pre-deploy-security](pre-deploy-security/SKILL.md) | Before pushing to production. Runs Gitleaks, Semgrep and OSV-Scanner, then reads the auth, database, cron and CORS code that scanners miss, and reports by severity. |

## Install

Copy the folders you want into your skills directory:

```bash
git clone https://github.com/popnetmedia/claude-seo-skills
cp -r claude-seo-skills/local-eeat ~/.claude/skills/          # every project
cp -r claude-seo-skills/local-eeat .claude/skills/            # one project
```

Claude Code picks them up from the description in each file's front matter. You can also call one
by name, such as `/pre-deploy-security`.

`local-eeat` and `ai-search-readiness` refer to `non-commodity-content`, so install it with them.
`pre-deploy-security` installs the scanners it needs only after asking.

## Contributing

Issues and pull requests are welcome, especially corrections where a search or schema rule has
changed. Please link the source for any claim about how Google or an AI search product behaves.

## License

MIT
