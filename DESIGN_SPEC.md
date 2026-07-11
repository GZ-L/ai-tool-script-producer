# Design Spec

Maintenance-only notes for `ai-tool-script-producer`. Do not read this file during ordinary script generation.

For future modifications, read `docs/SKILL_MAINTENANCE_PROTOCOL.md` first. For Agent runtime cooperation, read `docs/AGENT_CALLING_PROTOCOL.md`.

## Purpose

This Skill turns a researched tool name or link into a Chinese short-form口播稿. Its fixed sample rhythm is:

```text
one-core-result project sentence -> pain scene -> tool name -> achievement sentence -> core capabilities -> usage paths -> elevated positioning -> save CTA
```

It is for AI tools, GitHub repos, websites, SaaS products, browser extensions, open-source projects, app listings, docs pages, and pasted tool descriptions.

It is not for generic copywriting, raw transcription, broad calendars, or unverifiable claims.

Runtime is coordinated through an intent gate and a separate sibling research Skill:

```text
Intent Gate ->
ai-tool-link-researcher -> TOOL_RESEARCH_HANDOFF_V1 -> ai-tool-script-producer
```

This Skill owns intent recognition, spoken structure, and final output. The researcher owns facts and claim safety.

## Runtime Files

- `SKILL.md`: trigger, routing, and minimal workflow.
- `CALLING_SPEC.md`: top runtime contract.
- `docs/AGENT_CALLING_PROTOCOL.md`: Agent-facing runtime routing and Skill cooperation.
- `docs/SKILL_MAINTENANCE_PROTOCOL.md`: future modification protocol and compatibility contract.
- `skills/ai-tool-link-researcher/`: packaged upstream research Skill. Install it as a separate Skill for normal use.
- `references/00-intent-and-routing.md`: user intent, input type, output mode, and runtime path.
- `references/01-intake-and-research.md`: handoff mapping and fallback research only.
- `references/02-script-structure.md`: drafting structure.
- `references/03-output-and-quality.md`: final output gate.
- `assets/tool-script-template.md`: expanded-package template only.
- `agents/openai.yaml`: UI metadata only.

## Edit Map

| Change | Edit |
|---|---|
| Trigger wording or resource routing | `SKILL.md` |
| Required execution order or claim discipline | `CALLING_SPEC.md` |
| User intent, input recognition, or output mode | `references/00-intent-and-routing.md`, `docs/AGENT_CALLING_PROTOCOL.md` |
| Generic link research protocol | `skills/ai-tool-link-researcher/` |
| Handoff-to-script mapping | `references/01-intake-and-research.md` |
| Script structure and first-sentence examples | `references/02-script-structure.md` |
| Output format and quality checks | `references/03-output-and-quality.md` |
| Reusable full-package format | `assets/tool-script-template.md` |
| Maintenance guidance | `DESIGN_SPEC.md` |

## Audit Checklist

- `CALLING_SPEC.md` remains the top runtime authority.
- `00` classifies user intent before research or drafting.
- Research handoff or fallback `01` runs before drafting.
- No completed handoff means no final script, unless fallback research is explicitly marked.
- `02` starts with one core result, not a bloated first sentence.
- The achievement sentence still appears immediately after the tool name.
- Compact output remains default for script-only requests.
- The asset template is used only for expanded/full-package requests.
- No file asks the agent to invent stars, pricing, free status, install commands, API keys, or supported platforms.
- `ai-tool-link-researcher` can evolve independently as long as it still outputs `TOOL_RESEARCH_HANDOFF_V1`.

## Validation

```bash
python3 /Users/guozhi/.codex/skills/standard-skill-maker/scripts/validate_skill.py .
python3 /Users/guozhi/.codex/skills/.system/skill-creator/scripts/quick_validate.py .
python3 /Users/guozhi/.codex/skills/standard-skill-maker/scripts/validate_skill.py skills/ai-tool-link-researcher
python3 /Users/guozhi/.codex/skills/.system/skill-creator/scripts/quick_validate.py skills/ai-tool-link-researcher
python3 scripts/validate_skill_upload.py .
rg -n -e 'TO''DO' -e '\\[TO''DO' -e '占''位' .
```
