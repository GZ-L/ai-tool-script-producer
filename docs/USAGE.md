# 使用说明

## 最小用法

```text
帮我把这个工具写成一条口播稿：https://github.com/LearnPrompt/ai-news-radar
```

## 明确调用主 Skill

```text
Use $ai-tool-script-producer to turn this AI tool link into a Chinese short-form口播稿:
https://github.com/LearnPrompt/ai-news-radar
```

## 单独调用检索 Skill

当你只想要事实核验和交接块，不想生成口播稿：

```text
Use $ai-tool-link-researcher to research this tool and return TOOL_RESEARCH_HANDOFF_V1:
https://github.com/LearnPrompt/ai-news-radar
```

## 正常流程

```text
用户给链接并要求口播稿
-> ai-tool-script-producer
-> ai-tool-link-researcher
-> TOOL_RESEARCH_HANDOFF_V1
-> ai-tool-script-producer
-> 口播稿
```

运行约束：

- 没有 `TOOL_RESEARCH_HANDOFF_V1`，不得直接写最终口播稿。
- `handoff_status: ready` 可以直接生成。
- `handoff_status: partial` 可以生成，但必须在 `事实依据/待核验` 标记缺口。
- `handoff_status: blocked` 不生成口播稿，先说明缺少哪些事实。
- 默认不把完整 handoff 展示给用户，除非用户要求看检索结果或做 Agent 调试。

## 已有交接块时

如果用户已经提供 `TOOL_RESEARCH_HANDOFF_V1`，主 Skill 会直接读取交接块，不再重复搜索。

```text
Use $ai-tool-script-producer to write a口播稿 from this handoff:

TOOL_RESEARCH_HANDOFF_V1
...
END_TOOL_RESEARCH_HANDOFF_V1
```

## 典型输出

默认脚本输出：

```text
【口播稿】
这是一个能帮你自动盯 AI 圈动态的开源项目。
...

【事实依据/待核验】
- GitHub stars 已核验。
- 价格或登录状态未核验时会标记。
```

## 常见失败情况

- 链接无法访问：检索 Skill 会在 `research_limits` 标记。
- 动态数据无法确认：脚本中不使用该 claim，或写入 `待核验`。
- `handoff_status: blocked`：说明无法生成口播稿，并列出 `blocking_gaps`。
- 用户只给工具名：需要先搜索官方来源，不能直接凭记忆写。
- 用户要求夸张表达：默认不写“全网爆火、一键搞定、永久免费”等无证据词。

## 使用者需要准备什么

- 工具链接最优。
- 只有工具名也可以，但准确性依赖检索结果。
- 如果是私有工具、内测产品或登录后页面，用户需要提供公开可引用的信息，不能提供 token、cookie、私钥或 `.env`。
