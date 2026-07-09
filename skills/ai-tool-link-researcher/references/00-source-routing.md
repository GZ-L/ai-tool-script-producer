# 00 Source Routing

Use this file before opening sources.

## Research Depth

| Depth | Use When | Work Required |
|---|---|---|
| Quick | User pasted enough facts and only needs light verification | Check the primary source and mark weak facts |
| Standard | User provides a normal tool link, repo, docs page, or tool name | Verify identity, purpose, achievement, capabilities, access path, and risks |
| Deep | Claims involve self-hosting, GitHub Actions, generated feeds, API keys, pricing, public data output, or multiple user paths | Inspect docs, README, workflows, releases/changelog, demo/data files, and setup instructions |

Default to standard. Use deep when a wrong claim would make the final口播 misleading.

## Source Type Routing

| Input Type | First Sources | Extra Sources |
|---|---|---|
| GitHub repo | Repo page, README, release/changelog, docs, license | `.github/workflows`, demo links, package files, Docker/config examples, public output files |
| Official website | Homepage, pricing, docs, download/get-started, changelog | Terms, support docs, app links, integrations page |
| Docs page | Overview, quickstart, install, auth/API key section | Examples, deployment, limits |
| Store listing | Store metadata, rating, version, privacy/support link | Official site and docs |
| Generated-feed project | README, workflow files, output JSON/RSS/feed files, config examples | Raw GitHub URLs, Pages demo, data archive |
| Tool name only | Search for official source first | GitHub/docs/store only after official identity is clear |
| Pasted description | Use pasted text as user-provided input | Search official source if factual claims will be used |
| Screenshot | Extract visible claims | Search official source for dynamic facts |

## Project-Specific Skill Routing

Use a project-specific Skill before this generic researcher only when the target exactly matches that project.

- AI News Radar / 伯乐Skill / Scout Skill / `LearnPrompt/ai-news-radar`: use `ai-news-radar` first, then return this Skill's handoff block.

Do not use `ai-news-radar` for unrelated AI tools. Its source-ingestion rules can distort ordinary tool research.

## Source Priority

1. Official product, repo, docs, changelog, release, store listing.
2. Official demo, public data files, public workflow files, package metadata.
3. Primary public announcements from the maker.
4. Third-party articles only for context, not as sole proof of dynamic claims.

## Stop Conditions

Stop researching when these fields are strong enough for口播:

- Correct tool name and type.
- One core result.
- One verified achievement or safe fallback.
- Two or three useful capabilities.
- One ordinary-user access path when available.
- One technical/self-host path when relevant.
- Clear risk notes.
