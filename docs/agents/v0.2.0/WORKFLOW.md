# Multi-Agent Delivery Workflow — v0.2.0

本项目当前主要由 `Claude` 和 `Codex` 协作开发。

默认原则：

- 核心代码优先由 `claude-code` 负责
- `codex` 负责可以独立交付的小闭环和 UI / 全栈任务
- 每个新 feature 必须在自己的 branch 上完成
- 每个完成的 feature 必须通过 PR 合入
- PR 合入后必须把对应 task 标记为 `done`

## Source of Truth

唯一任务来源：

- `docs/agents/v0.2.0/TASK_POOL.md`

唯一协作规范：

- `docs/agents/v0.2.0/WORKFLOW.md`

唯一依赖参考：

- `docs/agents/v0.2.0/DEPENDENCIES.md`

## Required Lifecycle

每个 task 必须严格按照下面顺序执行：

1. Claim
2. Plan
3. Branch
4. Execute
5. Validate
6. Submit
7. Close

## 1) Claim

开始写代码前，agent 必须：

- 从 `TASK_POOL.md` 里选择一个 `unblocked` task
- 把该 task 的 `Owner` 改成自己
- 把该 task 的 `Status` 改成 `in_progress`
- 在自己的 changelog 文件中记录计划

Changelog 文件：

- `docs/agents/v0.2.0/changelog/claude-code.md`
- `docs/agents/v0.2.0/changelog/codex.md`

规则：

- 一个 agent 同时只持有一个主任务
- 如果 task 被别的 agent 标成 `in_progress`，默认不可抢

## 2) Plan

开始改代码前，必须写清楚：

- task 目标
- 改哪些文件
- 不改哪些文件
- 验证方法
- 风险点

如果发现 task 太大：

- 先回到 `TASK_POOL.md`
- 拆成两个或更多子任务
- 更新 dependency 后再开工

## 3) Branch

每个新 feature 必须新开自己的 branch。

分支命名格式：

- `claude/task-XXX-short-name`
- `codex/task-XXX-short-name`

示例：

- `claude/task-410-us-data-routing`
- `codex/task-420-macro-dashboard-shell`

规则：

- 不允许直接在 `main` 上开发
- 不允许多个 task 共用一个 feature branch

## 4) Execute

执行阶段要求：

- 只做当前 task scope 内的事情
- 不顺手夹带 unrelated refactor
- 改共享接口前先更新相关文档

推荐分工：

- `claude-code`：
  - 数据源
  - 后端 API
  - prompt
  - 业务逻辑
  - 默认配置迁移
- `codex`：
  - 页面骨架
  - 组件实现
  - 前后端小闭环联调
  - 文档
  - smoke test

## 5) Validate

每个 task 至少要提供：

- 自动化验证
  - lint
  - build
  - test
- 手动验证
  - 页面截图或交互说明
  - API 调用结果
- 已知限制

如果无法完整验证，必须明确写出来，不能默认“应该可以”。

## 6) Submit

提交前要求：

- commit message 包含 task id
- 开 PR
- PR 描述中包含：
  - task id
  - 改动范围
  - 验证证据
  - known gaps

推荐 commit 格式：

- `feat(module): summary [TASK-XXX]`
- `fix(module): summary [TASK-XXX]`
- `refactor(module): summary [TASK-XXX]`
- `test(module): summary [TASK-XXX]`
- `docs(module): summary [TASK-XXX]`

## 7) Close

PR 合入后，agent 必须完成收尾动作：

1. 把 `TASK_POOL.md` 中对应 task 改成 `done`
2. 更新自己的 changelog
3. 如果影响依赖链，更新 `DEPENDENCIES.md`

没有完成这三步，task 不算真正完成。

## Coordination Rules

### Branch and PR Rules

- 每个 agent 每次只做一个新 feature
- 每个新 feature 必须在自己的 branch
- 每个 feature 结束必须 PR
- merge 后才可以标记 `done`

### File Ownership Rules

- Claude 优先负责核心后端和数据链路
- Codex 优先负责 UI 和局部闭环
- 对共享文件的大改动要先在 changelog 说明

### Conflict Rules

- 不随意覆盖别人的进行中修改
- 如果同一个文件可能冲突，先拆 task
- 遇到 scope 漂移，先回文档，不要直接扩写

### Documentation Rules

- 新增或修改接口时，先更新 API 说明
- 新增 task 时，先更新 `TASK_POOL.md`
- 交付节奏变化时，更新 `DEPENDENCIES.md`

## Reuse Rule

- 能直接参考或复用 `AI-Investment-Pro` 的成熟实现时，优先照抄，不要重复造轮子
