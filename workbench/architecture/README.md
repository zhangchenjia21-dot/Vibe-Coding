# Personal Workbench｜Architecture

本目录保存 **已经经过 Owner 讨论、证据验证并进入 current 的 V0 架构约束、contract、ownership/state model 与关键设计裁定**。

当前状态：

> **ROUTE-FROZEN / V0 IMPLEMENTATION AUTHORIZED FOR TA-1A**

## 当前 Architecture Owner

### [`架构方案.md`](架构方案.md)

**v1.1 / ROUTE-FROZEN / CURRENT V0 ARCHITECTURE**

冻结内容包括：

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
- consistent backup / validated staged restore / safety snapshot；
- schema version + ordered transactional migrations + upgrade safety backup；
- system tray V0 lifecycle；
- First Usable = TA-1A，随后立即 Owner real-data UAT。

### [`架构问题登记表.md`](架构问题登记表.md)

G0.3 QUESTION REGISTER / SUPPORTING INPUT。

保留问题来源、风险背景和原始状态；对于已由 G0.4 / G0.5 / G0.6 关闭的问题，以 frozen `架构方案.md` 与 Decision Record 为准。

## Route Freeze Decision

正式裁定：

[`../decisions/D-003_V0RouteFreeze与TA-1AImplementationAuthorization.md`](../decisions/D-003_V0RouteFreeze与TA-1AImplementationAuthorization.md)

当前：

```text
G0.5 Revised Route / Architecture = PASS
G0.6 Route Freeze = PASS
Implementation = AUTHORIZED FOR TA-1A DISPATCH
```

## Evidence Chain

Frozen Architecture 依据：

1. `../current/产品定义.md`；
2. `架构问题登记表.md`；
3. `../research/G0.4_Reference_Audit_Report.md`；
4. `../research/G0.4_Technical_Spike_Results.md`；
5. Owner 与 03 的 G0.5 讨论与批准；
6. 00 G0.5 Review；
7. Owner G0.6 Route Freeze Approval。

## Implementation Detail Boundary

以下仍未作为架构冻结，可在 04 Task / implementation 中选择，只要不改变 frozen contracts：

- 具体 SQLite binding / ORM；
- 字段级 production schema / DDL；
- UI component framework；
- installer / updater 具体实现；
- internal folder/module organization。

如果实现现实证明 frozen contract 本身错误，不得由 Agent 静默改写；必须回 03 + Owner + 00 做显式 change / Gate。

## TA-1A Exit Residual

以下 Owner-machine focused check 已获 Owner 接受，不再阻塞 Route Freeze，但必须在 TA-1A Exit 前完成：

```text
launch
→ notification-area icon 实际可见
→ 鼠标点击恢复 / focus
→ close → tray
→ tray real exit
```

失败则 TA-1A 不得 Exit，并进入 05 Root Cause / 必要架构回流。

## Canonical rule

- Product / Roadmap / Stage 状态由 `../current/` 拥有；
- Frozen V0 Architecture 由 `架构方案.md` 拥有；
- Reference / Spike evidence 由 `../research/` 拥有；
- Owner 正式项目级裁定由 `../decisions/` 拥有；
- 代码、runtime implementation fact 与 executable Task 由 `zhangchenjia21-dot/Workbench` 拥有；
- `99_归档/workbench/` 只提供历史证据。
