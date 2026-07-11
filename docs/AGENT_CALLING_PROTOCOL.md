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

1. Trigger `ai-tool-script-producer` only when the user wants a tool口播稿, tool script, tool recommendation copy, full package, or a Skill maintenance/audit task for this package.
2. Read `references/00-intent-and-routing.md` and set the intent variables before calling another Skill.
3. If the input contains `TOOL_RESEARCH_HANDOFF_V1`, use it directly for script-generation intents.
4. If the input contains a URL/tool name and no handoff, call `ai-tool-link-researcher` first when the intent requires research.
5. If the target is AI News Radar / 伯乐Skill / Scout Skill / `LearnPrompt/ai-news-radar`, the researcher should use `ai-news-radar` first when that Skill is installed.
6. Generate the final script only after research has produced a usable handoff.

## Intent Gate

Every Agent call must classify the request before execution:

| user_intent | Trigger | Next Step |
|---|---|---|
| `script_generation` | User asks for口播稿, 脚本, 文案, 工具种草, or platform tool recommendation copy | Research if needed, then write compact script |
| `research_only` | User asks to核验, 查询, 总结事实, or output `TOOL_RESEARCH_HANDOFF_V1` | Call `ai-tool-link-researcher`; do not write final口播稿 |
| `handoff_to_script` | User provides `TOOL_RESEARCH_HANDOFF_V1` and asks to write/revise | Skip research unless the handoff is invalid |
| `full_package` | User asks for title, rhythm, screen notes, subtitle beats, download/use steps, or complete deliverable | Research first, then produce full package |
| `maintenance_or_audit` | User asks to modify, audit, install, upload, validate, or document this Skill | Read `docs/SKILL_MAINTENANCE_PROTOCOL.md`; do not generate content |
| `out_of_scope` | User asks for non-tool generic copy, raw transcription, broad calendar, or unrelated task | Route elsewhere or explain mismatch |

Do not expose this table in normal user-facing output. It exists to prevent ambiguous routing.

## Runtime Rhythm

Use this exact sequence:

```text
Intent Gate -> Source Gate -> Handoff Gate -> Material Gate -> Draft Gate -> Output Gate -> Validation Gate
```

- Source Gate chooses installed researcher, packaged researcher, or fallback checklist.
- Handoff Gate accepts only `TOOL_RESEARCH_HANDOFF_V1` or a clearly marked fallback.
- Material Gate maps handoff fields into口播素材.
- Draft Gate follows `references/02-script-structure.md`.
- Output Gate follows `references/03-output-and-quality.md`.
- Validation Gate checks first sentence, achievement placement, claim safety, and requested output mode.

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

If `handoff_status: blocked`, return the blocking gaps instead of drafting. If `handoff_status: partial`, draft only when required identity, one core result, capability, and access-path facts are sufficient, and disclose missing facts.

## Output Rules

For normal script requests:

1. Do not show the full handoff by default.
2. Output `【口播稿】` first.
3. Output only minimal `【事实依据/待核验】`.
4. Do not add titles, rhythm notes, screenshots, or usage walkthroughs unless requested.

For Agent integration/debug requests:

1. Show `TOOL_RESEARCH_HANDOFF_V1`.
2. Then show the generated script or explain why generation is blocked.

For research-only requests:

1. Show only `TOOL_RESEARCH_HANDOFF_V1`.
2. Do not add a口播稿, title, CTA, or video packaging.

For maintenance/audit requests:

1. Report concrete files changed, validation commands, and current sync status.
2. Do not discuss script style unless a runtime script rule changed.

## Compatibility Rules

- Downstream script generation depends only on `TOOL_RESEARCH_HANDOFF_V1`, not on the researcher's internal files.
- If a future researcher Skill replaces `ai-tool-link-researcher`, it must output the same handoff contract or update `references/01-intake-and-research.md`.
- Claims in `claim_safety.do_not_claim` must not appear in the script.
- Claims with unknown status may only appear in `事实依据/待核验`, not in confident口播句子.
