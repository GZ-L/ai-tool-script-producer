# Design Spec

Maintenance-only notes for `ai-tool-link-researcher`. Do not read this file during ordinary research.

## Purpose

This Skill researches AI tool links and returns a stable handoff for downstream writing:

```text
ai-tool-link-researcher -> TOOL_RESEARCH_HANDOFF_V1 -> ai-tool-script-producer
```

It is modeled after the useful parts of `ai-news-radar`'s ability tree:

- source classification before action;
- context pass before conclusions;
- depth control for simple vs complex sources;
- primary-source preference;
- safety rules for secrets and private feeds;
- clear validation and handoff boundary.

It does not inherit `ai-news-radar`'s project-specific source-ingestion workflow.

For package-level modification rules, read `../../docs/SKILL_MAINTENANCE_PROTOCOL.md` from the repository root when available.

## Runtime Files

- `SKILL.md`: trigger, routing, and boundary.
- `CALLING_SPEC.md`: top runtime contract.
- `references/00-source-routing.md`: source type and research depth.
- `references/01-evidence-extraction.md`: fact fields and achievement choice.
- `references/02-claim-risk-gate.md`: claim confidence and forbidden claims.
- `references/03-handoff-contract.md`: stable output contract.
- `agents/openai.yaml`: UI metadata only.

## Edit Map

| Change | Edit |
|---|---|
| Trigger wording or Skill boundary | `SKILL.md` |
| Required execution order | `CALLING_SPEC.md` |
| Source categories or depth rules | `references/00-source-routing.md` |
| Evidence fields | `references/01-evidence-extraction.md` |
| Claim safety rules | `references/02-claim-risk-gate.md` |
| Downstream interface | `references/03-handoff-contract.md` |
| Maintenance guidance | `DESIGN_SPEC.md` |

## Compatibility Contract

Downstream Skills should depend only on `TOOL_RESEARCH_HANDOFF_V1`, not on this Skill's internal files.

If the handoff shape changes, update:

- `references/03-handoff-contract.md`
- `../ai-tool-script-producer/references/01-intake-and-research.md`
- `../ai-tool-script-producer/CALLING_SPEC.md` if call order changes

## Audit Checklist

- The Skill returns research handoff only, not final口播稿.
- `ai-news-radar` is used only for AI News Radar / 伯乐Skill / Scout Skill targets.
- Every dynamic claim is verified, downgraded, or omitted.
- Secret, private feed, and account-bound source rules remain strict.
- `TOOL_RESEARCH_HANDOFF_V1` remains stable enough for `ai-tool-script-producer`.
- Handoff blocks include `handoff_status`, `confidence`, and `blocking_gaps` so downstream Agents do not guess readiness.

## Validation

```bash
python3 /Users/guozhi/.codex/skills/standard-skill-maker/scripts/validate_skill.py skills/ai-tool-link-researcher
python3 /Users/guozhi/.codex/skills/.system/skill-creator/scripts/quick_validate.py skills/ai-tool-link-researcher
rg -n -e 'TO''DO' -e '\\[TO''DO' -e '占''位' skills/ai-tool-link-researcher
```
