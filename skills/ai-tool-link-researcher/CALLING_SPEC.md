# Calling Spec

Mandatory runtime contract for `ai-tool-link-researcher`.

## Authority

This Skill owns upstream research and evidence quality. Downstream Skills own writing style and final packaging.

For research tasks:

1. `CALLING_SPEC.md`
2. `references/00-source-routing.md`
3. `references/01-evidence-extraction.md`
4. `references/02-claim-risk-gate.md`
5. `references/03-handoff-contract.md`

## Required Sequence

1. **Identify research intent**
   - Set `research_intent` before opening sources.
   - Use `script_support` when called by `ai-tool-script-producer` for a口播稿 or full package.
   - Use `research_only` when the user asks only for facts, verification, or a handoff block.
   - Use `source_evaluation` when the user asks whether sources are reliable, current, or safe to claim.
   - Use `maintenance_context` only when researching a tool as part of Skill maintenance or documentation.
   - Do not let `research_intent` change this Skill into a script writer.

2. **Confirm fit**
   - Use this Skill for AI tools, GitHub repos, websites, SaaS products, apps, extensions, docs, store listings, screenshots, pasted descriptions, and tool names.
   - Do not use it to draft final口播稿.

3. **Detect project-specific Skill**
   - If the target is AI News Radar, 伯乐Skill, Scout Skill, or `LearnPrompt/ai-news-radar`, use installed `ai-news-radar` first for project-specific source, workflow, and product context.
   - If `ai-news-radar` is unavailable, use deep GitHub repository research and mark `ai-news-radar unavailable` in `research_limits`.
   - Convert the findings into `TOOL_RESEARCH_HANDOFF_V1`.
   - Do not copy `ai-news-radar`'s source-ingestion workflow into this Skill.

4. **Classify source**
   - Read `references/00-source-routing.md`.
   - Classify the input and choose quick, standard, or deep research.
   - Use deep research when the project has GitHub Actions, public data output, multiple install paths, pricing ambiguity, generated feeds, or self-hosting claims.

5. **Collect evidence**
   - Read `references/01-evidence-extraction.md`.
   - Prefer official, primary, and product-owned sources.
   - Capture source links next to the facts they support.

6. **Gate claims**
   - Read `references/02-claim-risk-gate.md`.
   - Decide which claims are verified, weak, unknown, or forbidden.
   - Downgrade unclear claims instead of polishing them.

7. **Output handoff**
   - Read `references/03-handoff-contract.md`.
   - Return exactly one `TOOL_RESEARCH_HANDOFF_V1` block plus a short note only when needed.
   - Put `research_intent` under `input.research_intent`.
   - Set `handoff_status: ready` only when identity, achievement or fallback positioning, core capabilities, access path, and claim safety are sufficient for script generation.
   - Set `handoff_status: partial` when a script can be written but important unknowns must be disclosed.
   - Set `handoff_status: blocked` when the tool identity or core use cannot be verified.

## Tool Use

- Use available web, browser, GitHub, or local file-reading tools as needed.
- For time-sensitive facts, verify in the current turn.
- If a page cannot be accessed, state the access failure in `research_limits`.
- Do not use user cookies, private browser exports, account-bound feeds, or hidden credentials.

## Final Check

Before returning:

- Source type and research depth are clear.
- `handoff_status` matches the evidence.
- Every achievement sentence has evidence or is marked as fallback.
- Access/download/use path is verified or marked unknown.
- Risk notes include API key, login, paid limits, local deployment, and platform limits when relevant.
- The output is a handoff, not a口播稿.
