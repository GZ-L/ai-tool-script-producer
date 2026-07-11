# 00 Intent And Routing

Use this file before research or drafting.

Its job is to identify what the user is asking for and choose the correct runtime path. Do not infer intent from preference. Use only observable cues in the user message, supplied files, links, or handoff blocks.

## Intent Variables

Set these working variables before calling any downstream file:

```text
user_intent: script_generation|research_only|handoff_to_script|full_package|maintenance_or_audit|out_of_scope
input_type: url|github_repo|official_site|docs_page|store_listing|tool_name|pasted_description|screenshot|handoff|skill_folder|unknown
output_mode: compact_script|research_handoff|full_package|maintenance_report|blocked
research_required: yes|no|fallback_only
handoff_required: yes|no
```

Do not show these variables to the user unless they ask for Agent debugging or call-flow explanation.

## Intent Routing

| User Cue | user_intent | Required Action |
|---|---|---|
| Asks for `口播稿`, `脚本`, `文案`, `工具种草`, `小红书/抖音/B站工具推荐` | `script_generation` | Research first if no handoff, then generate compact script |
| Provides `TOOL_RESEARCH_HANDOFF_V1` and asks to write or revise a script | `handoff_to_script` | Skip research and draft from the handoff |
| Asks `查一下`, `核验`, `总结事实`, `生成交接块`, `给 Agent 用` without asking for final口播 | `research_only` | Use or route to `ai-tool-link-researcher`; do not draft |
| Asks for titles, rhythm, screen notes, subtitles, download steps, or complete deliverable | `full_package` | Research first, then output the full package |
| Asks to modify, audit, install, publish, validate, or document this Skill | `maintenance_or_audit` | Read maintenance docs; do not run content-generation flow |
| Requests generic copywriting, raw video/audio transcription, broad calendars, or non-tool content | `out_of_scope` | Do not force this Skill; route to a better Skill or explain the mismatch |

## Path Matrix

Set the runtime variables from this matrix after identifying `user_intent`.

| user_intent | output_mode | research_required | handoff_required |
|---|---|---|---|
| `script_generation` | `compact_script` | `yes` unless valid handoff exists | `yes` |
| `research_only` | `research_handoff` | `yes` | `no` for producer output; researcher still returns handoff |
| `handoff_to_script` | `compact_script` | `no` unless handoff is invalid or stale | `yes` |
| `full_package` | `full_package` | `yes` unless valid handoff exists | `yes` |
| `maintenance_or_audit` | `maintenance_report` | `no` unless the maintenance task explicitly needs external source research | `no` |
| `out_of_scope` | `blocked` | `no` | `no` |

## Input Type Routing

| Input Cue | input_type |
|---|---|
| `github.com/{owner}/{repo}` | `github_repo` |
| Product homepage URL | `official_site` |
| Docs, API, quickstart, install, README page | `docs_page` |
| Chrome Web Store, App Store, plugin marketplace | `store_listing` |
| Bare product/project name | `tool_name` |
| User pasted descriptive paragraph | `pasted_description` |
| User supplied image or screenshot | `screenshot` |
| Message contains `TOOL_RESEARCH_HANDOFF_V1` | `handoff` |
| Local Skill path, `SKILL.md`, `CALLING_SPEC.md`, or Skill folder request | `skill_folder` |
| No reliable target | `unknown` |

If multiple cues exist, prefer the most specific target: handoff, GitHub repo, official source, docs/store, pasted description, tool name.

## Runtime Rhythm

Use this order:

```text
Intent Gate -> Source Gate -> Handoff Gate -> Material Gate -> Draft Gate -> Output Gate
```

- Intent Gate: set `user_intent`, `input_type`, and `output_mode`.
- Source Gate: decide whether research is required and which researcher to use.
- Handoff Gate: obtain or validate `TOOL_RESEARCH_HANDOFF_V1` when `handoff_required: yes`.
- Material Gate: map handoff facts into script material.
- Draft Gate: write only after the material is sufficient.
- Output Gate: choose compact script, full package, research handoff, maintenance report, or blocked response.

## Default Decisions

- URL/tool name plus a script request means `script_generation`; do not ask the user to confirm.
- URL/tool name plus no script request, but asks for facts, means `research_only`.
- Handoff plus script request means `handoff_to_script`; do not search again unless the handoff is stale, contradictory, or missing required fields.
- Tool request plus titles/rhythm/screen notes means `full_package`.
- Skill editing, Skill audit, GitHub upload, installation, validation, or protocol work means `maintenance_or_audit`.
- If the user says only `写这个工具` but provides no tool name, link, pasted facts, screenshot, or handoff, set `output_mode: blocked` and ask for the missing target.

## Anti-Guessing Rules

- Do not decide that the user wants a full package just because it would be useful.
- Do not decide that the user wants research-only when they ask for `口播稿` or `脚本`.
- Do not use subjective impressions like "probably", "seems popular", or "should be free" as routing evidence.
- Do not let the researcher write the final script.
- Do not let the producer invent facts to avoid calling the researcher.
