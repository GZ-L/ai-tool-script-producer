# 03 Output And Quality

Use this file before delivering the final script.

## Default Output Sections

Choose sections from `output_mode` set in `references/00-intent-and-routing.md`.

For `compact_script`, return:

1. `口播稿`: one complete 30-45 second version.
2. `事实依据/待核验`: only key verified facts and uncertainties.

For `full_package`, return:

1. `标题候选`: 3-5 options.
2. `口播稿`: one complete 30-45 second version.
3. `分段节奏`: time-coded structure.
4. `画面/字幕建议`: what to show on screen.
5. `下载和使用步骤`: factual access path.
6. `事实依据/待核验`: what was verified and what is uncertain.

For `research_handoff`, return only `TOOL_RESEARCH_HANDOFF_V1` and one short limitation note if needed. Do not add a口播稿.

For `maintenance_report`, return concrete change results, files changed, validation commands, sync/install status, and any remaining risk. Do not add content-generation analysis.

For `blocked`, state the missing target, invalid handoff, or blocking gap. Ask only for the minimum missing input required to continue.

Keep the final answer practical. Do not bury the actual口播稿 under long analysis.

For script-only requests, do not show `TOOL_RESEARCH_HANDOFF_V1` by default. The script still must be derived from a completed handoff. Show the handoff only when the user asks for research details, debugging, or Agent integration output.

## Quality Gate

Check before final delivery:

- The tool is named once clearly.
- The first sentence follows `这是一个能帮你【一个核心结果】的【类型】。`
- The first sentence does not include secondary capabilities, mechanisms, or advanced positioning.
- The sentence immediately after the tool name is a verified achievement or an explicit positioning fallback.
- The script answers: what it solves, who it is for, what result it creates, how to access/download, and how to start using it.
- Dynamic facts are current or marked as needing verification.
- Claims are not stronger than the evidence.
- A completed `TOOL_RESEARCH_HANDOFF_V1` exists, or fallback research limits are explicitly marked.
- The script has one clear CTA.

## Style Controls

Default style:

- High-density but credible.
- Conversational Chinese.
- Short sentences.
- Specific nouns over hype words.
- Practical tool value over generic praise.

Use aggressive style only when requested:

- `王炸`, `天花板`, `集体失业`, `破防`, `狠活`.

Use professional style when the tool is technical, B2B, open-source, or developer-facing:

- `工作流`, `开源项目`, `静态数据`, `自动更新`, `本地部署`, `可 fork`, `无需服务器`.

## Risk Notes

Mark these clearly:

- Star count may change.
- A "free" claim needs current official evidence.
- GitHub open-source does not always mean hosted service is free.
- Some tools require API keys, local setup, account login, or technical deployment.
- Do not write install commands unless the official README provides them.
