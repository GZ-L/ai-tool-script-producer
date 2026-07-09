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

## 验证边界

- 结构验证确认 Skill frontmatter、命名、基础结构可用。
- 上传文档验证确认 README 必要主题、NOTICE 建议和敏感信息扫描。
- 这些验证不保证外部网页可访问，也不保证每个工具链接都能检索到完整公开事实。
