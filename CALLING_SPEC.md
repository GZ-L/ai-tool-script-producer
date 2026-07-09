# Calling Spec

Mandatory runtime contract for `ai-tool-script-producer`. Follow this before writing any script.

## Authority

For ordinary script generation:

1. `CALLING_SPEC.md`
2. `ai-tool-link-researcher` for upstream link/tool research when no handoff is supplied
3. `references/01-intake-and-research.md`
4. `references/02-script-structure.md`
5. `references/03-output-and-quality.md`

`DESIGN_SPEC.md` is maintenance-only. Do not read it during normal generation.

Research authority and writing authority are separate:

- `ai-tool-link-researcher` owns source classification, evidence, claim safety, and `TOOL_RESEARCH_HANDOFF_V1`.
- `ai-tool-script-producer` owns口播结构, spoken rhythm, first-sentence discipline, and output packaging.

## Required Sequence

1. **Confirm fit**
   - Use this Skill only for scripts about tools, URLs, GitHub repos, apps, extensions, SaaS products, websites, or open-source projects.

2. **Resolve research input**
   - If the user already supplied `TOOL_RESEARCH_HANDOFF_V1`, use it as the evidence source.
   - If the user supplied a URL/tool name and no handoff, use `ai-tool-link-researcher` first.
   - If `ai-tool-link-researcher` is unavailable, use `references/01-intake-and-research.md` as a fallback research checklist and mark limits.
   - Do not read `ai-tool-link-researcher` internals while drafting if a valid handoff is already present.

3. **Prepare script material**
   - Read `references/01-intake-and-research.md`.
   - Extract from the handoff or fallback notes: tool name, purpose, audience, pain point, achievement, access path, first-use steps, free/open-source status if verified, and risk notes.
   - Do not draft the final口播稿 before this pass.

4. **Select achievement sentence**
   - Pick exactly one verified achievement or positioning sentence to place immediately after the tool name.
   - Use verified stars, forks, official backing, release milestone, store rating, public adoption, funding, case study, or integration.
   - If nothing is verified, use a positioning fallback and mark it in `事实依据/待核验`.

5. **Draft script**
   - Read `references/02-script-structure.md`.
   - Use this order:

```text
one-core-result project sentence -> pain scene -> tool name -> achievement sentence -> core capabilities -> usage paths -> elevated positioning -> save CTA
```

   - First sentence must follow: `这是一个能帮你【一个核心结果】的【开源项目/工具/网站】。`
   - First sentence must not include a second capability, mechanism, comma clause, advanced positioning, `并且`, `以及`, or `同时`.

6. **Package output**
   - Read `references/03-output-and-quality.md`.
   - If the user asks for a script, use compact output:

```text
【口播稿】
...

【事实依据/待核验】
...
```

   - Use the full package only when the user asks for titles, rhythm, screen notes, download steps, or a complete deliverable.

## Final Check

Before final delivery:

- First sentence has one core result only.
- Tool name is clear.
- Achievement sentence directly follows the tool name.
- Dynamic facts are verified or marked uncertain.
- Claims marked `do_not_claim` in the handoff are not used.
- The script explains what the tool solves, who it helps, how to access it, and how to start using it.
