# Task Pool — v0.2.0

Status values: `todo`, `in_progress`, `blocked`, `done`, `deferred`.

Owner conventions:

- `claude-code` — 核心代码、后端、数据层、prompt、复杂系统改造
- `codex` — 可独立推进的 UI / 全栈任务、文档、测试、清理
- `unassigned` — 尚未认领

## Product Direction

本阶段目标：

- 做成 `Bloomberg inspired` 的美股研究工作台
- 首页以 `macro dashboard` 为主
- 个股页先展示 `company intro + financials + technicals + AI cards`
- 用 `watchlist` 替代 portfolio
- 去除 A 股 / 港股 / 中国本地数据源主路径

---

## M1: Foundation / Repositioning

| Task ID | Title | Owner | Status | Depends On | Deliverable |
|---|---|---|---|---|---|
| TASK-400 | US-only product scope cleanup plan | claude-code | todo | - | 梳理当前代码中的 A 股 / 港股 / 中国数据源引用，输出具体改造点清单 |
| TASK-401 | Shared API contract for v0.2.0 pages | codex | todo | TASK-400 | 新版 macro dashboard、watchlist、stock detail 所需 API contract 文档 |
| TASK-402 | Navigation and IA reset | codex | todo | TASK-401 | 前端信息架构调整：首页、Watchlist、Stock Detail、Reports、System |
| TASK-403 | Replace portfolio-first homepage with macro-first shell | codex | todo | TASK-402 | 首页主框架切换，移除 portfolio-first 叙事和入口 |

## M2: US Data Layer

| Task ID | Title | Owner | Status | Depends On | Deliverable |
|---|---|---|---|---|---|
| TASK-410 | US market data routing strategy | claude-code | todo | TASK-400 | `Finnhub -> yfinance` 的美股 quote / financials 主路径；不再暴露中国市场默认 routing |
| TASK-411 | Macro signal aggregation service | claude-code | todo | TASK-410 | 聚合 `^TNX`、`^VIX`、Fear & Greed、SPY RSI、breadth、put/call、Fed 的统一 service |
| TASK-412 | Breadth calculation pipeline | claude-code | todo | TASK-410 | 计算 `S&P 500 above 200D MA` 占比，并提供 API |
| TASK-413 | Put/Call ratio ingestion | claude-code | todo | TASK-410 | `SPY put/call ratio` 数据拉取与缓存 |
| TASK-414 | Fed calendar and policy snapshot | claude-code | todo | TASK-410 | 下一次 FOMC 日期 + 可扩展的 cut/hold/hike 数据结构 |
| TASK-415 | Company profile summary service | claude-code | todo | TASK-410 | 个股页公司简介摘要服务，输出业务、客户、上游依赖 |
| TASK-416 | Stock fundamentals snapshot API | claude-code | todo | TASK-410 | 最近两年核心财务字段 API：price, revenue, EPS, margins, PE, FCF, ROE, ROIC, growth |
| TASK-417 | Technical indicators snapshot API | claude-code | todo | TASK-410 | `50D MA`, `150D MA`, weekly RSI, `52W high/low` API |

## M3: Macro Dashboard

| Task ID | Title | Owner | Status | Depends On | Deliverable |
|---|---|---|---|---|---|
| TASK-420 | Macro dashboard layout and design system pass | codex | todo | TASK-403 | Bloomberg-inspired 宏观首页框架、栅格和视觉基准 |
| TASK-421 | Treasury and VIX chart cards | codex | todo | TASK-411, TASK-420 | 10Y Treasury、VIX 30D 图表卡片 |
| TASK-422 | Fear & Greed, Fed, and put/call cards | codex | todo | TASK-411, TASK-413, TASK-414, TASK-420 | 三个信号卡片组件 |
| TASK-423 | SPY RSI and breadth cards | codex | todo | TASK-411, TASK-412, TASK-420 | RSI / breadth 指标卡片 |
| TASK-424 | Oversold probability engine UI | codex | todo | TASK-411, TASK-421, TASK-422, TASK-423 | 把等权信号汇总成“超卖概率”展示模块 |
| TASK-425 | Macro page copy and localization cleanup | codex | todo | TASK-420 | 去掉中国市场文案，统一美股语境和 Bloomberg 风格 copy |

## M4: Watchlist

| Task ID | Title | Owner | Status | Depends On | Deliverable |
|---|---|---|---|---|---|
| TASK-430 | Watchlist data model and API cleanup | claude-code | todo | TASK-410 | 只保留 watchlist 主路径，不暴露 portfolio 语义 |
| TASK-431 | Watchlist page UI | codex | todo | TASK-430, TASK-402 | watchlist 列表页，可查看 quote 和跳转到个股页 |
| TASK-432 | Add/remove watchlist actions | codex | todo | TASK-430, TASK-431 | watchlist CRUD 交互 |

## M5: Stock Detail

| Task ID | Title | Owner | Status | Depends On | Deliverable |
|---|---|---|---|---|---|
| TASK-440 | Stock detail page IA and layout | codex | todo | TASK-402 | 个股页整体结构：header / intro / financials / technicals / AI |
| TASK-441 | Company intro card | codex | todo | TASK-415, TASK-440 | AI summary 公司简介卡片 |
| TASK-442 | Financial snapshot module | codex | todo | TASK-416, TASK-440 | 最近两年财务和估值模块 |
| TASK-443 | Technical chart module | codex | todo | TASK-417, TASK-440 | 价格图、均线、RSI、52W 区间展示 |
| TASK-444 | TradingView integration spike | codex | todo | TASK-443 | 评估并接入 TradingView，或给出降级实现 |

## M6: AI Analysis Cards

| Task ID | Title | Owner | Status | Depends On | Deliverable |
|---|---|---|---|---|---|
| TASK-450 | Moat analysis prompt implementation | claude-code | todo | TASK-415 | 实现严苛版护城河分析 prompt 和结构化结果 |
| TASK-451 | Moat score visualization card | codex | todo | TASK-450, TASK-440 | 护城河条形图 + 理由展示卡片 |
| TASK-452 | Fundamental analysis prompt implementation | claude-code | todo | TASK-415, TASK-416 | 企业基本面分析 prompt 和结果结构 |
| TASK-453 | Fundamental analysis card UI | codex | todo | TASK-452, TASK-440 | 基本面分析卡片，默认摘要，可展开全文 |
| TASK-454 | AI analysis orchestration endpoint | claude-code | todo | TASK-450, TASK-452 | 个股页 AI 分析统一调用入口 |

## M7: De-China Cleanup

| Task ID | Title | Owner | Status | Depends On | Deliverable |
|---|---|---|---|---|---|
| TASK-460 | Remove A-share and HK references from primary UI | codex | todo | TASK-402 | 清理主页面、筛选器、报告页中的 A 股 / 港股默认选项 |
| TASK-461 | Remove China-first scheduler and system wording | codex | todo | TASK-400 | 清理系统页、调度页、数据源管理页的中国数据源优先语义 |
| TASK-462 | Default settings migration to US market | claude-code | todo | TASK-410 | 把默认市场、默认数据源、默认文案迁移到 US-first |

## M8: Validation / Polish

| Task ID | Title | Owner | Status | Depends On | Deliverable |
|---|---|---|---|---|---|
| TASK-470 | End-to-end smoke checklist | codex | todo | TASK-424, TASK-432, TASK-453 | 首页、watchlist、个股页、AI 卡片的手动回归清单 |
| TASK-471 | Frontend visual polish pass | codex | todo | TASK-425, TASK-431, TASK-443, TASK-453 | 统一 Bloomberg-inspired 视觉细节 |
| TASK-472 | Release notes for v0.2.0 MVP | codex | todo | TASK-470, TASK-471 | 版本说明和已知限制 |
