# 01 Intake And Research Handoff

Use this file before drafting.

This file does not replace `ai-tool-link-researcher`. Its job is to convert verified research into口播素材 and provide a small fallback when the researcher Skill is unavailable.

## Preferred Input

Prefer a `TOOL_RESEARCH_HANDOFF_V1` block from `ai-tool-link-researcher`.

Map it like this:

| Script Need | Handoff Field |
|---|---|
| Readiness | `handoff_status` |
| Research intent | `input.research_intent` |
| Tool name | `identity.tool_name` |
| Type | `identity.type` |
| First sentence result | `script_material.first_sentence_core_result` or `verified_facts.one_core_result` |
| Pain scene | `script_material.pain_scene` or `verified_facts.pain_point` |
| Achievement sentence | `verified_facts.achievement_sentence` |
| Three capabilities | `script_material.capability_triplet` or `verified_facts.core_capabilities` |
| Usage path | `verified_facts.access_paths` and `verified_facts.first_use_steps` |
| Elevated positioning | `script_material.elevated_positioning` |
| Save audience | `script_material.save_audience` or `verified_facts.target_users` |
| Risk notes | `claim_safety`, `verified_facts.status`, `research_limits` |
| Blocking gaps | `blocking_gaps` |
| Confidence | `confidence` |

Use `claim_safety.do_not_claim` as a hard blocklist.

`input.research_intent` is advisory. It explains why the researcher was called, but it does not override the producer's own `user_intent` from `references/00-intent-and-routing.md`.

## Handoff Readiness

Use `handoff_status` before drafting:

- `ready`: continue to script drafting.
- `partial`: continue only if tool identity, one core result, achievement or fallback positioning, at least two capabilities, and one access path are usable. Mark missing facts in `事实依据/待核验`.
- `blocked`: do not write the口播稿. Explain the blocking gaps and ask for a better source or more information.

Do not override `handoff_status` based on writing intuition. If the handoff is internally inconsistent, treat it as `partial` or `blocked` and explain why.

## Fallback Research

Use this only when `ai-tool-link-researcher` is unavailable.

| Input | First Action | Evidence Priority |
|---|---|---|
| GitHub repo | Open repo page, README, releases, docs, demo, install section | GitHub page, README, releases, docs |
| Official website | Open homepage, pricing/download/docs/get-started pages | Official docs, changelog, pricing, app links |
| App or extension store | Open listing and support/privacy pages | Store listing, official support page |
| Pasted description | Use the description and mark unverified facts | User-provided text, then official search if needed |
| Tool name only | Search for the official source | Official source first, then repo/docs/store |

Fallback extraction must still collect:

- Tool name and correct spelling.
- One core result.
- Target audience.
- Pain point.
- Two or three concrete capabilities.
- Achievement sentence or positioning fallback.
- Access path and first-use steps.
- Pricing/open-source/free/API-key/login/server status only if verified.
- Risk notes and unknowns.

## Achievement Sentence Rule

Place one achievement sentence immediately after the tool name in the口播稿.

Pattern:

```text
这个工具叫【工具名】。
它在 GitHub 上已经拿到【X】star / 由【机构】开源 / 最近刚发布【版本】 / 已经接入【平台】。
```

Use current facts. If the number may change, verify it in the current turn before writing.

If no achievement is available:

```text
这个工具叫【工具名】。
它的定位很明确，就是帮【人群】把【任务】从【旧方法】变成【新方法】。
```

Do not write vague fake proof such as "很多人都在用", "全网爆火", or "开发者都在推荐" unless there is visible evidence.

## Research Safety

- Use official sources before blogs or reposts.
- Do not expose credentials, tokens, cookies, private OPML, private inbox details, or `.env` values.
- Do not imply "免费" if the official source only says open source, trial, or self-hosted.
- Do not say "无需 API Key" unless the exact workflow being described really does not require one.
- Do not use any claim listed under `do_not_claim`.
- Do not hide `blocking_gaps` when they affect the generated script.
