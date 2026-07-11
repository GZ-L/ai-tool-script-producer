# 安装说明

## 前提

- 本机已安装 Codex 或支持 Skill 的 Agent 环境。
- 默认 Skill 目录为 `~/.codex/skills`。
- 如你使用自定义 `CODEX_HOME`，把命令中的 `~/.codex` 替换为对应目录。

## 安装主 Skill 和配合 Skill

```bash
git clone https://github.com/GZ-L/ai-tool-script-producer.git
mkdir -p ~/.codex/skills
cp -R ai-tool-script-producer ~/.codex/skills/ai-tool-script-producer
cp -R ai-tool-script-producer/skills/ai-tool-link-researcher ~/.codex/skills/ai-tool-link-researcher
```

## 确认安装

```bash
ls ~/.codex/skills/ai-tool-script-producer/SKILL.md
ls ~/.codex/skills/ai-tool-link-researcher/SKILL.md
```

两个命令都能看到 `SKILL.md` 时，说明文件已经复制到 Skill 目录。

## 推荐安装形态

保持两个 Skill 独立安装。运行时以顶层 `ai-tool-link-researcher` 为权威检索 Skill，主 Skill 内的 `skills/ai-tool-link-researcher` 只作为发布包副本和离线安装来源。

```text
~/.codex/skills/
├── ai-tool-script-producer/
└── ai-tool-link-researcher/
```

不要把 `ai-tool-link-researcher` 的内部文件合并到 `ai-tool-script-producer`，否则后续改检索逻辑和改文案逻辑会互相影响。

如果更新了仓库内的 `skills/ai-tool-link-researcher`，必须同步安装到：

```text
~/.codex/skills/ai-tool-link-researcher
```

## 更新

```bash
cd ai-tool-script-producer
git pull
rm -rf ~/.codex/skills/ai-tool-script-producer ~/.codex/skills/ai-tool-link-researcher
cp -R . ~/.codex/skills/ai-tool-script-producer
cp -R skills/ai-tool-link-researcher ~/.codex/skills/ai-tool-link-researcher
```

## 卸载

```bash
rm -rf ~/.codex/skills/ai-tool-script-producer ~/.codex/skills/ai-tool-link-researcher
```
