# Agent Delivery Kit — v0.2.0

本目录是 `TradingAgents-CN v0.2.0` 的 agent 协作入口。

核心文件：

- `TASK_POOL.md`：任务池，唯一任务状态来源
- `DEPENDENCIES.md`：任务依赖和关键路径
- `WORKFLOW.md`：分支、PR、验证、done 流程
- `changelog/claude-code.md`：Claude 的开发记录
- `changelog/codex.md`：Codex 的开发记录

默认分工：

- `claude-code`：核心后端、数据层、Prompt、系统主链路
- `codex`：独立 UI / 全栈功能闭环、文档、验证、清理

规则：

- 开发前先看 `TASK_POOL.md`
- 不允许直接推 `main`
- 任务完成后必须开 PR，并把 task 标记为 `done`
- 能直接参考或复用 `AI-Investment-Pro` 的成熟实现时，优先照抄，不要为了“重新设计”而造轮子
