# 验证说明

## 已运行验证

主 Skill 结构验证：

```bash
python3 /Users/guozhi/.codex/skills/standard-skill-maker/scripts/validate_skill.py .
python3 /Users/guozhi/.codex/skills/.system/skill-creator/scripts/quick_validate.py .
```

配合 Skill 结构验证：

```bash
python3 /Users/guozhi/.codex/skills/standard-skill-maker/scripts/validate_skill.py skills/ai-tool-link-researcher
python3 /Users/guozhi/.codex/skills/.system/skill-creator/scripts/quick_validate.py skills/ai-tool-link-researcher
```

安装目录同步验证：

```bash
diff -qr /Users/guozhi/Documents/Codex/口播视频工作室/ai-tool-script-producer-skill /Users/guozhi/.codex/skills/ai-tool-script-producer -x .git
diff -qr /Users/guozhi/.codex/skills/ai-tool-link-researcher /Users/guozhi/.codex/skills/ai-tool-script-producer/skills/ai-tool-link-researcher
```

上传文档验证：

```bash
python3 scripts/validate_skill_upload.py .
```

模板残留扫描：

```bash
rg -n -e 'TO''DO' -e '\\[TO''DO' .
```

敏感信息扫描由 `scripts/validate_skill_upload.py` 执行，覆盖常见 private key、GitHub token、API key、secret、password 模式。

## 预期结果

```text
OK: /path/to/ai-tool-script-producer
Skill is valid!
OK: /path/to/ai-tool-script-producer/skills/ai-tool-link-researcher
Skill is valid!
PASSED: 0 error(s), 0 warning(s)
```

模板残留扫描应无输出。

如果在本机发布工作目录 `ai-tool-script-producer-skill` 运行 `validate_skill.py .`，可能出现目录名与 frontmatter name 不一致的警告；安装目录和 GitHub 克隆目录应使用 `ai-tool-script-producer`，与 Skill 名一致。

## 本轮新增检查点

- 主 Skill 必须先经过 `references/00-intent-and-routing.md` 的意图门。
- Agent 协议必须包含 `Intent Gate -> Source Gate -> Handoff Gate -> Material Gate -> Draft Gate -> Output Gate -> Validation Gate`。
- `TOOL_RESEARCH_HANDOFF_V1` 必须支持 `input.research_intent`，且旧字段不删除。
- 只做研究的请求不得生成口播稿，维护请求不得进入内容生成路径。

## 验证边界

- 结构验证确认 Skill frontmatter、命名、基础结构可用。
- 上传文档验证确认 README 必要主题、NOTICE 建议和敏感信息扫描。
- 这些验证不保证外部网页可访问，也不保证每个工具链接都能检索到完整公开事实。
- `diff -qr` 无输出表示发布包、安装目录和内嵌检索副本当前一致。
