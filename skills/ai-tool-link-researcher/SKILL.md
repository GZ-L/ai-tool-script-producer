---
name: ai-tool-link-researcher
description: Research AI tool links, GitHub repos, product sites, docs, store listings, screenshots, pasted descriptions, or tool names before script writing. Use when the user asks for AI工具链接检索, 工具事实核验, 工具链接研究, verified facts, achievements, access paths, pricing/open-source/API-key risks, source confidence, or a TOOL_RESEARCH_HANDOFF_V1 block for ai-tool-script-producer. Do not use for writing final口播稿.
---

# AI Tool Link Researcher

Research a tool or project and return verified facts that another Skill can safely use.

## Role Boundary

This Skill owns **research, source judgment, claim safety, and handoff structure**.

It does not write the final口播稿, title, CTA, subtitle rhythm, or video packaging. Use `ai-tool-script-producer` for that.

When the target is AI News Radar, 伯乐Skill, Scout Skill, or `LearnPrompt/ai-news-radar`, use `ai-news-radar` first if installed. If it is unavailable, use deep GitHub repository research and record that fallback in `research_limits`.

## Runtime Order

Read files in this order:

1. `CALLING_SPEC.md` — mandatory invocation contract.
2. `references/00-source-routing.md` — classify the input and choose research depth.
3. `references/01-evidence-extraction.md` — collect only script-relevant facts.
4. `references/02-claim-risk-gate.md` — decide what can be claimed.
5. `references/03-handoff-contract.md` — output `TOOL_RESEARCH_HANDOFF_V1`.

Read `DESIGN_SPEC.md` only when modifying, auditing, or extending this Skill.

## Workflow

1. Identify the research intent: script support, research-only handoff, source evaluation, or maintenance context.
2. Classify the user input: GitHub repo, official website, docs page, store listing, generated feed project, pasted description, screenshot, or tool name only.
3. Choose quick, standard, or deep research depth.
4. Verify facts from official or primary sources first.
5. Extract purpose, audience, pain point, achievement, core capabilities, access path, first-use steps, and risk notes.
6. Pass every dynamic or promotional claim through the claim-risk gate.
7. Return one stable handoff block for downstream script generation or research-only use.

## Non-Negotiables

- Do not invent stars, forks, users, ratings, pricing, "free", API-key status, supported platforms, download paths, or release milestones.
- Do not expose secrets, cookies, private OPML, private inboxes, tokens, `.env` values, or private newsletter contents.
- Do not turn a GitHub open-source repo into a "free hosted service" claim unless the hosted path is verified.
- Do not write the final口播稿. Output facts and handoff only.
- Always set `input.research_intent` to explain why the research was performed.
- Always set `handoff_status` to `ready`, `partial`, or `blocked`.

## Validation

After editing this Skill, run:

```bash
python3 /Users/guozhi/.codex/skills/standard-skill-maker/scripts/validate_skill.py skills/ai-tool-link-researcher
python3 /Users/guozhi/.codex/skills/.system/skill-creator/scripts/quick_validate.py skills/ai-tool-link-researcher
```
