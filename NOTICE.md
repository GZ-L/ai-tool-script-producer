# Notice

This repository documents and packages a Codex / Agent Skill workflow.

## Source

- Maintainer: `GZ-L`
- Source type: `original private workflow`
- Upstream account: not applicable
- Upstream repository: not applicable
- Upstream URL: not applicable

## Reference Boundary

This Skill package was organized according to the documentation expectations in `GZ-L/skill-upload-standard`.

The research-to-script coordination pattern also references the public behavior style of the local `ai-news-radar` Skill: classify source type, prefer primary sources, gate risky claims, and pass structured evidence downstream. This repository does not redistribute `ai-news-radar` source code, data output, feeds, private OPML, GitHub Actions artifacts, credentials, or project-specific implementation.

## Redistribution Boundary

This repository does not include third-party source code, binaries, datasets, models, private account data, audio, video, cookies, API keys, tokens, `.env` files, or build outputs.

It includes:

- Skill instruction files.
- Reference Markdown files.
- A reusable口播 template.
- A documentation validation script copied from the owner's `skill-upload-standard` repository for local upload checks.

## Validation

Validation commands are documented in [docs/VALIDATION.md](docs/VALIDATION.md).
