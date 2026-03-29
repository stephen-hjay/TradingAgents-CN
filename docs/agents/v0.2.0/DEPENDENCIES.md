# Dependency Map — v0.2.0

## Critical Path

### Phase 1: Scope and Contracts

1. `TASK-400` US-only product scope cleanup plan
2. `TASK-401` Shared API contract for v0.2.0 pages
3. `TASK-402` Navigation and IA reset
4. `TASK-403` Replace portfolio-first homepage with macro-first shell

### Phase 2: Core Data

1. `TASK-410` US market data routing strategy
2. `TASK-411` Macro signal aggregation service
3. `TASK-412` Breadth calculation pipeline
4. `TASK-413` Put/Call ratio ingestion
5. `TASK-414` Fed calendar and policy snapshot
6. `TASK-415` Company profile summary service
7. `TASK-416` Stock fundamentals snapshot API
8. `TASK-417` Technical indicators snapshot API

### Phase 3: Macro Dashboard

1. `TASK-420` Macro dashboard layout and design system pass
2. `TASK-421` Treasury and VIX chart cards
3. `TASK-422` Fear & Greed, Fed, and put/call cards
4. `TASK-423` SPY RSI and breadth cards
5. `TASK-424` Oversold probability engine UI
6. `TASK-425` Macro page copy and localization cleanup

### Phase 4: Watchlist + Stock Detail

1. `TASK-430` Watchlist data model and API cleanup
2. `TASK-431` Watchlist page UI
3. `TASK-432` Add/remove watchlist actions
4. `TASK-440` Stock detail page IA and layout
5. `TASK-441` Company intro card
6. `TASK-442` Financial snapshot module
7. `TASK-443` Technical chart module
8. `TASK-444` TradingView integration spike

### Phase 5: AI Cards

1. `TASK-450` Moat analysis prompt implementation
2. `TASK-451` Moat score visualization card
3. `TASK-452` Fundamental analysis prompt implementation
4. `TASK-453` Fundamental analysis card UI
5. `TASK-454` AI analysis orchestration endpoint

### Phase 6: Cleanup and Release

1. `TASK-460` Remove A-share and HK references from primary UI
2. `TASK-461` Remove China-first scheduler and system wording
3. `TASK-462` Default settings migration to US market
4. `TASK-470` End-to-end smoke checklist
5. `TASK-471` Frontend visual polish pass
6. `TASK-472` Release notes for v0.2.0 MVP

## Parallel Work Guidance

适合 Claude 优先启动的任务：

- `TASK-400`
- `TASK-410`
- `TASK-411`
- `TASK-415`
- `TASK-416`
- `TASK-417`

适合 Codex 与 Claude 并行的任务：

- `TASK-401`
- `TASK-402`
- `TASK-420`
- `TASK-440`

必须等待后端接口稳定后再开工的 UI 任务：

- `TASK-421`
- `TASK-422`
- `TASK-423`
- `TASK-431`
- `TASK-441`
- `TASK-442`
- `TASK-443`
- `TASK-451`
- `TASK-453`

## Dependency Rules

- 没有被依赖链解锁的 task 不要抢跑
- 一次只认领一个主任务
- PR 的 scope 不要跨多个未关联模块
- 如果一个任务变大，先在 `TASK_POOL.md` 拆任务，再开发
