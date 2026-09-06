# Personal Workbench｜Architecture

本目录只保存 **已经经过 Owner 讨论并明确提升的跨阶段架构约束、contract、ownership/state model 与关键设计裁定**。

当前项目处于：

> **Stage 0｜Pre-Implementation Alignment / G0.5 Submission**

Route Freeze 尚未由 00 + Owner 完成，因此当前 Architecture Candidate 虽已 Owner-reviewed / approved for submission，仍不是 production implementation authorization。

## 当前 Architecture Owner

### [`架构方案.md`](架构方案.md)

**v1.0 / OWNER-APPROVED G0.5 CANDIDATE / NOT ROUTE-FROZEN**

当前主要架构候选：

- Electron = V0 Desktop Host；
- SQLite = local canonical persistent store；
- Tracks / Plan authoritative，Today 主要 derived；
- Today acknowledgement 为极窄当日辅助状态；
- immutable internal identity；
- occurrence identity = `SeriesStableID + OriginalScheduledOccurrenceKey`；
- date-only / local wall-time / instant 明确分离；
- Current Vector `[startDate, endDate]` inclusive + no-overlap；
- recurrence = series + dynamic occurrence + exception；
- whole-series edit 清除已有 exceptions 前必须显式警告 / 确认；
- backup / restore / migration safety contracts；
- system tray V0 lifecycle；
- First Usable → Owner real-data UAT → Complete V0 的路线顺序。

### [`架构问题登记表.md`](架构问题登记表.md)

**v1.0 / G0.3 QUESTION REGISTER / SUPPORTING INPUT**

保留 G0.3 时暴露的问题、风险来源和原始状态。G0.4 已解决或 G0.5 已收敛的问题，以 `架构方案.md` 的较新候选结论为准。

## Evidence Chain

当前 Architecture Candidate 依据：

1. `../current/产品定义.md`；
2. `架构问题登记表.md`；
3. `../research/G0.4_Reference_Audit_Report.md`；
4. `../research/G0.4_Technical_Spike_Results.md`；
5. Owner 与 03 的 G0.5 逐项讨论与明确批准。

G0.4 已由 00 判定 PASS；Technical Spikes 已证明：

- Electron / Tauri packaged tray lifecycle 均可行；
- SQLite durability / backup / restore / migration 可行；
- recurrence stable identity / exception / Today acknowledgement / date boundary 可行。

这些 evidence 支撑 G0.5 Candidate，但最终 Route Freeze 仍需 00 + Owner 审核。

## Current High-sunk-cost Contracts

G0.5 Candidate 已收敛：

- Desktop Host：Electron；
- Persistence：SQLite；
- identity：stable internal IDs；
- recurrence occurrence：original scheduled occurrence identity；
- date semantics：date-only ≠ timestamp，local recurrence ≠ fixed UTC instant；
- backup / restore：consistent snapshot + validation + restore safety snapshot；
- migration：schema version + ordered transactional migration + upgrade safety backup；
- UAT ordering：TA-1A First Usable 后提前 Owner UAT。

仍不在当前架构中冻结：

- 具体 SQLite binding / ORM；
- production schema 字段级 DDL；
- UI component framework；
- installer / updater 具体实现；
- Future AI / sync / plugin / custom-page platform。

## Route Freeze Residual

G0.6 前保留一个极小 Owner Windows focused check：

```text
launch
→ notification-area icon 实际可见
→ 鼠标点击恢复 / focus
→ close → tray
→ tray real exit
```

这不是新大型 Spike，也不授权业务实现。

## Canonical Rule

- Product / Roadmap 当前状态由 `../current/` 拥有；
- G0.5 Architecture Candidate 由本目录 `架构方案.md` 拥有；
- Reference / Spike evidence 由 `../research/` 拥有；
- Gate / Stage 状态由 `../current/项目状态.md` 与 00 拥有；
- 代码与 runtime implementation fact 由 `zhangchenjia21-dot/Workbench` 拥有；
- `99_归档/workbench/` 只作历史证据。

> 当前状态：**G0.5 OWNER-APPROVED SUBMISSION / NOT ROUTE-FROZEN / IMPLEMENTATION NOT AUTHORIZED**。