# AI Tool Script Producer Skill

## 这是什么

这是一个面向 AI 工具推荐内容的 Codex / Agent Skill 包。它把用户提供的 AI 工具链接、GitHub 仓库、产品官网、插件页或工具名称，先拆成可核验事实，再生成中文短视频口播稿。

这个仓库包含两个互相配合但边界独立的 Skill：

- `ai-tool-script-producer`：主 Skill，负责把已核验事实写成 30-45 秒中文口播稿。
- `ai-tool-link-researcher`：配合 Skill，负责链接检索、事实核验、风险判断，并输出 `TOOL_RESEARCH_HANDOFF_V1`。

面向 Agent 的稳定调用协议见 [docs/AGENT_CALLING_PROTOCOL.md](docs/AGENT_CALLING_PROTOCOL.md)。后续修改本 Skill 时，先读 [docs/SKILL_MAINTENANCE_PROTOCOL.md](docs/SKILL_MAINTENANCE_PROTOCOL.md)。

## 来源与归属

- 维护账号：`GZ-L`
- 来源类型：原创工作流 / 私有内容生产流程
- 上游来源：无第三方源码上游
- 对标参考：能力边界参考了 `GZ-L/skill-upload-standard` 的上传文档规范，以及本机已有 `ai-news-radar` Skill 的“来源分类、证据优先、风险门、交接格式”思路
- 授权边界：本仓库只包含 Skill 指令、参考文档、模板和验证脚本；不包含第三方私有数据、音频、视频、账号数据、API Key、token、cookie 或 `.env`

详细来源和再分发边界见 [NOTICE.md](NOTICE.md)。

## 适用场景

适合在这些场景使用：

- 用户给一个 AI 工具链接，要求生成小红书、抖音、B 站口播稿。
- 用户给 GitHub 仓库，要求提炼用途、战绩、使用路径、适合人群。
- 用户给官网、文档页、插件商店页，要求写工具种草脚本。
- 用户要求固定结构：项目一句话定位、痛点、工具名、战绩、三点能力、使用路径、升维定位、收藏人群。

不适合：

- 非工具类泛文案、情绪文案、品牌广告语。
- 原始视频转写或音频拆解。
- 未经核验的“免费、无限、无需 API Key、全网爆火”等宣传性断言。
- 需要复制第三方源码、私有内容、账号数据或受限素材的任务。

## 能力与效果

这个 Skill 包可以：

- 先通过 `ai-tool-link-researcher` 对链接做来源分类、事实核验和 claim safety 判断。
- 用 `TOOL_RESEARCH_HANDOFF_V1` 把事实交给 `ai-tool-script-producer`，避免边查边写导致事实和文案混乱。
- 通过 `handoff_status`、`confidence`、`blocking_gaps` 判断是否能写稿，减少 Agent 主观判断。
- 生成符合固定节奏的中文口播稿：一句话定位、痛点、工具名、战绩、三点能力、使用路径、升维定位、收藏人群。
- 对动态事实保持克制：stars、forks、价格、API Key、登录、部署方式等必须核验或标记待核验。

预期输出：

- 默认输出一版 30-45 秒口播稿。
- 附带简短 `事实依据/待核验`，说明关键事实来源或不确定点。
- 默认不展示完整 `TOOL_RESEARCH_HANDOFF_V1`，但必须先生成或接收可用 handoff。
- 如果用户要求完整包，可输出标题、分段节奏、画面/字幕建议、下载和使用步骤。

质量标准：

- 第一句必须是：`这是一个能帮你【一个核心结果】的【开源项目/工具/网站】。`
- 工具名之后必须紧跟一条已核验战绩或明确定位句。
- 不编造 star、免费、下载方式、API Key、平台支持等动态事实。

## 安装

推荐把两个 Skill 分别安装到 Codex Skill 目录。运行时以顶层 `~/.codex/skills/ai-tool-link-researcher` 为检索 Skill 权威来源，仓库内 `skills/ai-tool-link-researcher` 是发布和离线安装副本。

```bash
git clone https://github.com/GZ-L/ai-tool-script-producer.git
mkdir -p ~/.codex/skills
cp -R ai-tool-script-producer ~/.codex/skills/ai-tool-script-producer
cp -R ai-tool-script-producer/skills/ai-tool-link-researcher ~/.codex/skills/ai-tool-link-researcher
```

确认安装：

```bash
ls ~/.codex/skills/ai-tool-script-producer/SKILL.md
ls ~/.codex/skills/ai-tool-link-researcher/SKILL.md
```

更新方式：

```bash
cd ai-tool-script-producer
git pull
rm -rf ~/.codex/skills/ai-tool-script-producer ~/.codex/skills/ai-tool-link-researcher
cp -R . ~/.codex/skills/ai-tool-script-producer
cp -R skills/ai-tool-link-researcher ~/.codex/skills/ai-tool-link-researcher
```

更详细安装说明见 [docs/INSTALL.md](docs/INSTALL.md)。

## 使用方法

最小触发示例：

```text
帮我把这个工具写成一条口播稿：https://github.com/LearnPrompt/ai-news-radar
```

明确调用示例：

```text
Use $ai-tool-script-producer to turn this AI tool link into a Chinese short-form口播稿:
https://github.com/LearnPrompt/ai-news-radar
```

内部流程：

1. `ai-tool-script-producer` 判断用户要的是口播稿。
2. 如果没有 `TOOL_RESEARCH_HANDOFF_V1`，先调用 `ai-tool-link-researcher`。
3. `ai-tool-link-researcher` 搜索并核验事实，输出 `TOOL_RESEARCH_HANDOFF_V1`。
4. `ai-tool-script-producer` 读取交接块，生成最终口播稿。

更详细使用说明见 [docs/USAGE.md](docs/USAGE.md)。

## 输入与输出

输入可以是：

- GitHub 仓库链接。
- AI 工具官网。
- 插件商店或应用商店链接。
- 文档页、下载页、快速开始页。
- 工具名称。
- 用户粘贴的工具介绍。
- 已经生成好的 `TOOL_RESEARCH_HANDOFF_V1`。

输出可以是：

- `TOOL_RESEARCH_HANDOFF_V1` 事实交接块。
- 30-45 秒中文口播稿。
- `事实依据/待核验`。
- 可选完整包：标题、分段节奏、画面/字幕建议、使用步骤。

## 文件结构

```text
.
├── SKILL.md
├── CALLING_SPEC.md
├── DESIGN_SPEC.md
├── README.md
├── NOTICE.md
├── agents/
│   └── openai.yaml
├── assets/
│   └── tool-script-template.md
├── docs/
│   ├── INSTALL.md
│   ├── USAGE.md
│   ├── VALIDATION.md
│   ├── AGENT_CALLING_PROTOCOL.md
│   └── SKILL_MAINTENANCE_PROTOCOL.md
├── references/
│   ├── 01-intake-and-research.md
│   ├── 02-script-structure.md
│   └── 03-output-and-quality.md
├── scripts/
│   └── validate_skill_upload.py
└── skills/
    └── ai-tool-link-researcher/
        ├── SKILL.md
        ├── CALLING_SPEC.md
        ├── DESIGN_SPEC.md
        ├── agents/
        └── references/
```

## 验证

已运行的验证类型：

- 主 Skill 结构验证。
- 配合 Skill 结构验证。
- 上传文档验证。
- 模板残留扫描。
- 敏感信息扫描。

命令和结果见 [docs/VALIDATION.md](docs/VALIDATION.md)。

## 边界与注意事项

- 本 Skill 不保证外部网页永远可访问。
- 动态事实需要当前回合核验，尤其是 stars、forks、价格、API Key、登录、部署方式。
- 没有 `TOOL_RESEARCH_HANDOFF_V1` 时，不应直接生成最终口播稿；除非检索 Skill 不可用并明确标记 fallback 限制。
- GitHub 开源不等于免费托管服务，脚本中不能自动写成“免费”。
- 如果目标正好是 AI News Radar / 伯乐Skill / Scout Skill，可先调用对应项目 Skill 做项目级理解。
- 最终口播稿仍建议人工 review，尤其是商业化、价格、授权、下载路径和平台合规表达。
- 不要把 API Key、token、cookie、私有 OPML、邮箱正文、私有账号数据放进仓库或交接块。

## 许可证

未显式授权前，本仓库内容默认保留所有权利。你可以在自己的 Codex 环境中安装和使用本 Skill；如需公开再分发、改编或商用，请由维护账号 `GZ-L` 另行添加许可证或授权说明。
