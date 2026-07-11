# Skill Maintenance Protocol

Use this document when modifying, auditing, publishing, or installing this Skill package.

## Stable Architecture

```text
Intent Gate -> ai-tool-link-researcher -> TOOL_RESEARCH_HANDOFF_V1 -> ai-tool-script-producer
```

Do not merge the two Skills. The producer owns intent recognition,口播结构, and final output. The researcher owns facts, source judgment, claim safety, and handoff structure.

## Edit Map

| Change | Files to update |
|---|---|
| User trigger phrases or display aliases | `SKILL.md`, `agents/openai.yaml`, `README.md`, `docs/USAGE.md` |
| User intent routing or output mode | `references/00-intent-and-routing.md`, `CALLING_SPEC.md`, `docs/AGENT_CALLING_PROTOCOL.md`, `references/03-output-and-quality.md`, `README.md`, `docs/USAGE.md` |
| Runtime call order | `CALLING_SPEC.md`, `docs/AGENT_CALLING_PROTOCOL.md`, `README.md`, `DESIGN_SPEC.md` |
| Handoff fields | `skills/ai-tool-link-researcher/references/03-handoff-contract.md`, `references/01-intake-and-research.md`, `docs/AGENT_CALLING_PROTOCOL.md`, `skills/ai-tool-link-researcher/CALLING_SPEC.md` |
| Research source routing | `skills/ai-tool-link-researcher/CALLING_SPEC.md`, `skills/ai-tool-link-researcher/references/00-source-routing.md` |
| Script structure | `references/02-script-structure.md`, `references/03-output-and-quality.md` |
| Installation or GitHub publishing | `README.md`, `docs/INSTALL.md`, `docs/VALIDATION.md` |

## Compatibility Contract

- `TOOL_RESEARCH_HANDOFF_V1` is the only supported interface between research and writing.
- `references/00-intent-and-routing.md` is the only supported interface for user intent recognition inside the producer.
- Adding handoff fields is allowed when old fields remain valid.
- Removing or renaming handoff fields requires updating every mapper and usage document in the edit map.
- The installed top-level `~/.codex/skills/ai-tool-link-researcher` is the runtime authority.
- The packaged `skills/ai-tool-link-researcher` copy is for distribution and offline install.

## Modification Sequence

1. Identify whether the change affects runtime calling, research, script writing, installation, or documentation.
2. Edit the smallest matching file set from the edit map.
3. If the change affects user recognition, update the intent file first, then update the caller docs.
4. Keep `SKILL.md` lean; move detailed rules into `docs/` or `references/`.
5. Update examples only when the actual call flow changes.
6. Sync installed Skills after editing the source package.
7. Run validation before reporting completion.

## Required Validation

Run from the repository root:

```bash
python3 /Users/guozhi/.codex/skills/standard-skill-maker/scripts/validate_skill.py .
python3 /Users/guozhi/.codex/skills/.system/skill-creator/scripts/quick_validate.py .
python3 /Users/guozhi/.codex/skills/standard-skill-maker/scripts/validate_skill.py skills/ai-tool-link-researcher
python3 /Users/guozhi/.codex/skills/.system/skill-creator/scripts/quick_validate.py skills/ai-tool-link-researcher
python3 scripts/validate_skill_upload.py .
rg -n -e 'TO''DO' -e '\\[TO''DO' .
```

After syncing to `~/.codex/skills`, also verify:

```bash
python3 /Users/guozhi/.codex/skills/.system/skill-creator/scripts/quick_validate.py /Users/guozhi/.codex/skills/ai-tool-script-producer
python3 /Users/guozhi/.codex/skills/.system/skill-creator/scripts/quick_validate.py /Users/guozhi/.codex/skills/ai-tool-link-researcher
diff -qr /Users/guozhi/Documents/Codex/口播视频工作室/ai-tool-script-producer-skill /Users/guozhi/.codex/skills/ai-tool-script-producer -x .git
diff -qr /Users/guozhi/.codex/skills/ai-tool-link-researcher /Users/guozhi/.codex/skills/ai-tool-script-producer/skills/ai-tool-link-researcher
```

## Failure Rules

- If validation fails, do not push.
- If the handoff shape changes and mapper files are not updated, do not ship.
- If installed and packaged copies differ unexpectedly, sync before testing runtime behavior.
- If a claim cannot be verified, mark it unknown instead of making the script more confident.
