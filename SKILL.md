---
name: ai-tool-script-producer
description: Generate Chinese short-form口播稿 for AI tools, GitHub repositories, websites, SaaS products, browser extensions, open-source projects, or tool links. Use when the user says 果汁的AI+工具脚本生成, AI+工具脚本生成, AI工具口播, 工具种草口播, 工具链接转口播, or provides a tool name/URL and asks for 小红书/抖音/B站工具推荐文案, 下载/安装/使用步骤文案, or the structure "项目一句话定位、痛点、工具名、战绩、三点能力、使用路径、升维定位、收藏人群". Do not use for generic copywriting unrelated to tools, raw video transcription, broad content calendars, or unverifiable product claims.
---

# AI Tool Script Producer

Turn a researched tool, GitHub repo, product site, or link into a concise Chinese short-video口播稿.

## Runtime Order

Read files in this order during normal generation:

1. `CALLING_SPEC.md` — mandatory invocation contract.
2. `docs/AGENT_CALLING_PROTOCOL.md` — Agent-facing routing and cooperation contract.
3. Use `ai-tool-link-researcher` when the input has a URL/tool name and no `TOOL_RESEARCH_HANDOFF_V1` block is provided.
4. `references/01-intake-and-research.md` — convert research handoff or fallback facts into口播素材.
5. `references/02-script-structure.md` — script structure and first-sentence rule.
6. `references/03-output-and-quality.md` — final packaging and quality gate.

Use `assets/tool-script-template.md` only when the user asks for a reusable template or full package.

Read `docs/SKILL_MAINTENANCE_PROTOCOL.md` and `DESIGN_SPEC.md` only when modifying, auditing, or extending this Skill. Do not read them for ordinary script generation.

## Workflow

1. Read `CALLING_SPEC.md` and follow it as the top runtime authority.
2. Read `docs/AGENT_CALLING_PROTOCOL.md` for runtime cooperation rules.
3. If the user supplied `TOOL_RESEARCH_HANDOFF_V1`, consume it directly.
4. If the user supplied a URL/tool name without a handoff, use `ai-tool-link-researcher` first and obtain `TOOL_RESEARCH_HANDOFF_V1`.
5. Read `references/01-intake-and-research.md` to map facts into口播素材.
6. Read `references/02-script-structure.md` and draft the口播稿 in the required sample rhythm.
7. Read `references/03-output-and-quality.md` before final delivery.

## Non-Negotiables

- First sentence uses one core result only: `这是一个能帮你【一个核心结果】的【开源项目/工具/网站】。`
- The sentence immediately after the tool name is a verified achievement sentence or an explicit positioning fallback.
- No handoff, no final script: research facts must come from a completed `TOOL_RESEARCH_HANDOFF_V1` unless the researcher Skill is unavailable and fallback limits are explicitly marked.
- Never invent stars, pricing, free status, install paths, API keys, supported platforms, or download methods.
- For "脚本/口播稿" requests, return the script first and keep notes minimal.

## Validation

After editing this Skill, run:

```bash
python3 /Users/guozhi/.codex/skills/standard-skill-maker/scripts/validate_skill.py .
python3 /Users/guozhi/.codex/skills/.system/skill-creator/scripts/quick_validate.py .
python3 /Users/guozhi/.codex/skills/standard-skill-maker/scripts/validate_skill.py skills/ai-tool-link-researcher
python3 /Users/guozhi/.codex/skills/.system/skill-creator/scripts/quick_validate.py skills/ai-tool-link-researcher
```
