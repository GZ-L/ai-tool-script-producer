# 01 Evidence Extraction

Use this file after source routing.

## Evidence Map

Collect facts in this shape before writing the handoff:

| Field | What To Extract | Evidence Standard |
|---|---|---|
| identity | Tool name, owner/company, repo owner, official URL | Official page or repo |
| type | Open-source project, hosted web app, SaaS, extension, app, docs/API, model, library | Official positioning |
| one_core_result | The single result the tool helps users get | Homepage, README, docs overview |
| target_users | Who benefits most | Official use cases plus inference from features |
| pain_point | The old workflow pain this replaces | Inference allowed, but keep it grounded |
| achievement_candidates | Stars, forks, rating, release, backing, launch, integrations, public adoption | Visible primary source |
| core_capabilities | 2-3 things the tool actually does | README/docs/product pages |
| access_paths | Website, demo, GitHub, install command, extension/app store, Docker, npm, pip, hosted page | Official path only |
| first_use_steps | What the user does first after opening/installing | Quickstart/docs/demo flow |
| status | Open-source, pricing, login, API key, server, local setup, platform limits | Verify or mark unknown |
| risks | Anything that would make a script claim misleading | Evidence or visible absence |

## Achievement Selection

Pick exactly one best achievement for downstream writing.

Priority:

1. Verified public number: GitHub stars/forks, store rating, users, downloads.
2. Official backing: maker/company/foundation/open-source owner.
3. Release or milestone: recent version, launch, product update.
4. Public integration or adoption.
5. Positioning fallback when no achievement is verified.

Return the achievement as a ready-to-use sentence:

```text
在 GitHub 上已经拿到 1.5K+ star。
```

If no achievement is verified:

```text
它的定位很明确，就是帮【人群】把【任务】从【旧方法】变成【新方法】。
```

## Capability Extraction

Prefer capabilities that are visible and easy to understand in口播:

- Input: what the tool monitors, accepts, imports, or connects to.
- Process: filtering, merging, tagging, generating, translating, analyzing, automating.
- Output: page, report, file, workflow, dashboard, code, API result.

Avoid abstract feature names unless the audience would understand them.

## Evidence Notes

For each important fact, keep the source URL or source label beside it. The downstream script may only cite a short source list, but the handoff must preserve where facts came from.
