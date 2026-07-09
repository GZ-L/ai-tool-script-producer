# 02 Claim Risk Gate

Use this file before writing the handoff.

## Claim Levels

| Level | Meaning | Downstream Use |
|---|---|---|
| verified | Directly supported by official or primary source | Can be used in口播 |
| inferred | Reasonable from verified features, but not explicitly stated | Use plain language, do not overstate |
| unknown | Not found or not accessible | Mark as unknown or omit |
| forbidden | Unsupported dynamic/promotional/sensitive claim | Do not use |

## Claims That Need Current Verification

- GitHub stars, forks, releases, recent activity.
- Store rating, user count, download count.
- Pricing, free tier, trial, open-source/commercial status.
- API key, login, server, local deployment, platform requirements.
- "No code", "no server", "no API key", "free", "unlimited", "official", "secure".

## Common Downgrades

| Risky Claim | Safer Version |
|---|---|
| 免费 | 开源 / 可自部署 / 有在线页面, depending on evidence |
| 不需要 API Key | 默认核心流程不需要 API Key, only if verified |
| 所有人都在用 | 在 GitHub 上有可见 stars / 有公开案例 |
| 自动帮你完成全部工作 | 可以帮你减少其中一部分重复流程 |
| 官方支持所有平台 | 支持的入口以官方页面为准 |

## Forbidden Behavior

- Do not infer "free hosted service" from "open source".
- Do not infer "no API key" from "GitHub Actions" or "static site".
- Do not cite private sources, cookies, account-bound pages, inbox bodies, or hidden files.
- Do not use third-party blog claims as the only proof for achievements.
- Do not write install commands unless the official README/docs provide them.

## Conflict Handling

When sources conflict:

1. Prefer current official docs over old README screenshots.
2. Prefer release/changelog over third-party summaries.
3. Mark uncertainty in `research_limits`.
4. Choose a non-dynamic achievement fallback if the number is unclear.
