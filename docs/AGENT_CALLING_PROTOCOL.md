# Agent Calling Protocol

This is the runtime protocol for Agents using `ai-tool-script-producer`.

## Goal

Generate a Chinese short-form口播稿 from a tool name, URL, GitHub repo, docs page, store listing, pasted description, screenshot, or an existing research handoff.

## Skill Names

| Role | Skill name | Display name |
|---|---|---|
| Script generation | `ai-tool-script-producer` | 果汁的AI+工具脚本生成 |
| Link research | `ai-tool-link-researcher` | AI工具链接检索 |
| Project-specific context | `ai-news-radar` | AI News Radar project Skill, only for AI News Radar targets |

Use the Skill name for machine routing. Treat display names as user-facing labels and trigger aliases only.

## Runtime Decision

1. If the user asks for a口播稿, trigger `ai-tool-script-producer`.
2. If the input contains `TOOL_RESEARCH_HANDOFF_V1`, use it directly.
3. If the input contains a URL/tool name and no handoff, call `ai-tool-link-researcher` first.
4. If the target is AI News Radar / 伯乐Skill / Scout Skill / `LearnPrompt/ai-news-radar`, the researcher should use `ai-news-radar` first when that Skill is installed.
5. Generate the final script only after research has produced a usable handoff.

## Researcher Resolution Order

When `ai-tool-script-producer` needs research, resolve `ai-tool-link-researcher` in this order:

1. Installed Skill: `~/.codex/skills/ai-tool-link-researcher/SKILL.md`.
2. Packaged copy: `skills/ai-tool-link-researcher/SKILL.md`.
3. Fallback checklist: `references/01-intake-and-research.md`.

Use the installed Skill as the runtime authority. The packaged copy exists for distribution and offline install.

## Handoff Gate

No handoff, no final script.

Valid ways to pass the gate:

- A `TOOL_RESEARCH_HANDOFF_V1` block is provided by the user.
- `ai-tool-link-researcher` returns `TOOL_RESEARCH_HANDOFF_V1`.
- The researcher Skill is unavailable; fallback research is used and the final output marks the limitation.

Do not silently skip the handoff gate.

## Output Rules

For normal script requests:

1. Do not show the full handoff by default.
2. Output `【口播稿】` first.
3. Output only minimal `【事实依据/待核验】`.
4. Do not add titles, rhythm notes, screenshots, or usage walkthroughs unless requested.

For Agent integration/debug requests:

1. Show `TOOL_RESEARCH_HANDOFF_V1`.
2. Then show the generated script or explain why generation is blocked.

## Compatibility Rules

- Downstream script generation depends only on `TOOL_RESEARCH_HANDOFF_V1`, not on the researcher's internal files.
- If a future researcher Skill replaces `ai-tool-link-researcher`, it must output the same handoff contract or update `references/01-intake-and-research.md`.
- Claims in `claim_safety.do_not_claim` must not appear in the script.
- Claims with unknown status may only appear in `事实依据/待核验`, not in confident口播句子.
