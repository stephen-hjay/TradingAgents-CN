# TradingAgents-CN Product PRD — v0.2.0

日期：2026-03-29
状态：Draft
参考项目：`/Users/jay/Documents/AI-Investment-Pro`

## 1. Product Vision

把当前偏中国本地化、偏黑盒研报风格的产品，重构为一个面向美股研究的投资工作台。

本阶段的核心方向不是做复杂的 agent transparency，而是先把产品骨架做对：

- 视觉语言升级到 `Bloomberg inspired`
- 产品语境切换到 `US equities / US macro / Treasury / volatility`
- 报告入口弱化，先把 `macro dashboard` 和 `stock detail` 做扎实
- 去掉 A 股、港股、中国本地数据源默认路径和文案污染

## 2. Product Goals

### 2.1 Goals

- 建立一个可信、专业、偏机构终端感的首页宏观面板
- 建立一个以财务、估值、技术面、AI 分析为核心的个股页面
- 引入 watchlist，而不是 portfolio 暴露真实仓位
- 让 Claude / Codex agent team 可以并行开发，不互相踩文件

### 2.2 Non-Goals

- 暂不优先做透明化工作流
- 暂不优先做 agent 可视化
- 暂不优先做复杂 debate / challenge orchestration
- 暂不优先做个人 portfolio / 持仓管理
- 暂不优先做中国市场支持

## 3. Target User

- 主要用户是研究美股的个人投资者
- 用户希望先看到宏观位置和个股关键信息，而不是一上来读长篇 AI 报告
- 用户愿意使用 Claude 和 Codex 协同开发产品，因此产品规划必须适合 agent 分工推进

## 4. Design Direction

### 4.1 Visual Direction

- 风格参考：`AI-Investment-Pro` 的 macro 模块
- 视觉气质：`Bloomberg inspired`
- 关键词：terminal-like, dense but readable, serious, data-first, dark professional

### 4.2 What To Avoid

- 中国本地散户工具感
- 花哨但无信息密度的卡片墙
- 首页一上来就是“生成研报”按钮
- 暴露用户真实仓位的 portfolio 叙事
- 能直接参考或复用 `AI-Investment-Pro` 的设计、数据获取方式、页面结构和任务组织时，优先照抄成熟方案，尽量不要重复造轮子

## 5. Scope

本阶段只做三个主域：

1. Macro Dashboard
2. Stock Detail
3. Watchlist + navigation skeleton

## 6. Product Requirements

### 6.1 Macro Dashboard

页面目标：

- 用户一眼看到当前美股市场是否处于超买、超卖、恐慌、宽松或高压环境
- 把多个信号整理成一个统一的 market state
- 页面风格参考 `AI-Investment-Pro`，但不包含个人 portfolio 区域

必备模块：

1. `US 10Y Treasury Yield`
2. `VIX 30D`
3. `CNN Fear & Greed`
4. `S&P 500 Breadth`
5. `SPY Weekly RSI`
6. `SPY Put/Call Ratio`
7. `Next Fed Meeting`

统一规则：

- 每个信号等权
- 系统输出一个“当前超卖概率”的聚合判断
- 同时展示原始值、状态标签、最近趋势、更新时间

#### 6.1.1 US 10Y Treasury Yield

需求：

- 展示最近一段时间的 10Y yield 曲线
- 至少展示 30D 视图
- 卡片中显示当前值、近一周变化

#### 6.1.2 VIX 30D

需求：

- 展示最近 30 天 VIX
- 支持 AI 辅助解释当前处于历史什么百分位
- 展示状态标签

阈值：

- `< 12`：超买提示
- `> 25`：超卖预警
- `> 30`：强超卖
- `> 35`：极端超卖
- `> 40`：危机级超卖

#### 6.1.3 CNN Fear & Greed

需求：

- 展示当前数值和标签
- 支持趋势感知

阈值：

- `< 5`：超跌
- `> 75`：超买

#### 6.1.4 S&P 500 Breadth

需求：

- 展示标普五百成分股中，位于 `200D MA` 之上的公司占比
- 额外展示一组对照指标，例如位于中长期均线之上的公司数量

阈值：

- `< 20%` 公司位于 `200D MA` 之上，视为超卖信号

#### 6.1.5 SPY Weekly RSI

需求：

- 展示 SPY 周线 RSI
- 给出状态标签

阈值：

- `RSI < 30`：超卖标识

#### 6.1.6 SPY Put/Call Ratio

需求：

- 展示最新 put/call ratio
- 展示最近趋势
- 作为市场情绪信号参与总分计算

#### 6.1.7 Next Fed Meeting

需求：

- 显示下一次 FOMC 日期
- 为后续扩展保留 cut / hold / hike 概率位

### 6.2 Stock Detail Page

页面目标：

- 先看懂公司，再看财务与技术面，最后再看 AI 分析
- 不让用户一进入个股页就看到一整篇黑盒长报告

页面结构建议：

1. Header summary
2. Company intro
3. Financials + valuation block
4. Price / technical block
5. AI analysis block

#### 6.2.1 Company Intro

需求：

- 使用 AI 总结公司是做什么的
- 说明主要客户是谁
- 说明上游或关键依赖是什么
- 输出要简洁、像 sell-side initiation 的 opening summary，而不是 marketing copy

#### 6.2.2 Financials

至少展示最近两年核心数据：

- Stock Price
- Revenue
- EPS
- Operating Margin
- Net Margin
- PE
- Free Cash Flow
- ROE
- ROIC
- Revenue Growth
- PE Growth

要求：

- 以结构化表格或 grid 展示
- 每个字段需要清楚单位和口径
- 支持后续继续加更多财务字段

#### 6.2.3 Price / Technicals

需求：

- 价格图
- 最好支持 TradingView 集成
- `50D MA`
- `150D MA`
- 周线 RSI
- `52W High`
- `52W Low`
- `AI 推荐的压力与支撑` 作为 optional 模块

#### 6.2.4 AI Analysis

本阶段先做两个主要分析模块：

1. 护城河分析报告
2. 企业基本面分析报告

要求：

- 每个分析模块是独立卡片，不是把所有输出混成一篇大报告
- 默认先显示结果摘要，再允许展开看全文
- 护城河分析必须有分项打分条形图和理由
- 基本面分析要围绕长期基本面，而不是情绪化结论

## 7. Watchlist

方向：

- 用 watchlist 替代 portfolio 作为主跟踪对象
- 不暴露真实仓位
- 未来可从 watchlist 进入 stock detail 和 AI analysis

MVP 要求：

- 添加 / 删除 watchlist symbol
- watchlist 列表展示基本 quote 和状态
- 可从 watchlist 进入个股页

## 8. Data Strategy

### 8.1 Product-Level Principles

- 默认只围绕美股和美国宏观
- 优先复用 `AI-Investment-Pro` 的数据思路
- 不再把 `Tushare / AKShare / BaoStock` 作为默认产品主路径

### 8.2 Preferred Sources

- US quotes / financials:
  - `Finnhub` primary
  - `yfinance` fallback
- Macro:
  - `yfinance` for `^TNX`, `^VIX`, SPY data
  - `CNN Fear & Greed` endpoint
  - `FRED` for Fed / rates / macro series
- Fundamentals enrichment:
  - `FMP` optional
- Charting:
  - `TradingView` embed if integration cost acceptable

### 8.3 Explicit De-scope

- A 股行情默认入口
- 港股默认入口
- 中国本地财经数据源作为主 UI 文案
- 中国市场优先级的 scheduler / sync 命名

## 9. Success Criteria

当 v0.2.0 MVP 完成时，应该满足：

1. 首页已经是 `macro-first`，不再是旧式中国风研报首页
2. 首页能给出超卖概率判断，并展示 6-7 个核心宏观信号
3. 个股页能先展示公司简介、财务、估值和技术面
4. AI 分析是模块化卡片，不是一整篇黑盒长文
5. 产品默认语境已经切换到美股
6. agent team 可以按 task + dependency + PR 流程稳定推进

## 10. Delivery Notes For Agent Team

- `Claude` 负责核心代码、数据层、prompt、后端主链路
- `Codex` 负责可独立推进的 UI、全栈小闭环、文档、测试与收口
- 每个 feature 必须先认领 task，再开 branch，再提交 PR
- 一个 PR 只解决一个主任务，避免超大杂糅 PR
