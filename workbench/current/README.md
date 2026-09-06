# Personal Workbench｜Current 路由

本目录只保存 **当前仍生效、已经经过 Owner 认可的项目级事实 Owner**。

## 当前 Owner

| 文件 | 作用 | 状态 |
|---|---|---|
| [`项目总纲.md`](项目总纲.md) | 当前产品方向、Authority、仓库边界与治理规则 | current v0.4 |
| [`产品定义.md`](产品定义.md) | Primary Purpose / JTBD / CORE / V0 Scope / Success / Failure / Hard Constraints | **current v1.0 — G0.1 / G0.2 PASS** |
| [`开发路线.md`](开发路线.md) | Frozen Task Axis / First Usable / Complete V0 / UAT sequencing | **v1.2 — ROUTE-FROZEN** |
| [`项目状态.md`](项目状态.md) | Current Stage / Goal / Gate / Active Work / Next Action | **current — TA-1A AUTHORIZED FOR DISPATCH** |

## 当前 Architecture

- [`../architecture/架构方案.md`](../architecture/架构方案.md) — **v1.1 / ROUTE-FROZEN V0 ARCHITECTURE**
- [`../architecture/架构问题登记表.md`](../architecture/架构问题登记表.md) — G0.3 question register / historical supporting input

正式 Route Freeze 裁定：

[`../decisions/D-003_V0RouteFreeze与TA-1AImplementationAuthorization.md`](../decisions/D-003_V0RouteFreeze与TA-1AImplementationAuthorization.md)

## 当前正式产品与路线

```text
Personal State / Information & Action Desktop

CORE
├─ Today
├─ Plan
└─ Tracks

V0
= Thin Tracks + Usable Plan + Derived Today

Desktop Host
= Electron

Canonical Persistence
= SQLite
```

实现顺序：

```text
TA-1A First Usable Core Vertical
→ Owner real-data UAT
→ TA-1B Complete V0
→ Complete V0 UAT
→ TA-2 Correction / Hardening
→ TA-3 V0 Acceptance
```

## 当前 Stage / Gate

```text
Product Direction        = APPROVED
G0.1 Product Baseline    = PASS
G0.2 Scope               = PASS
G0.3 Draft Task Axis     = PASS
G0.4 Reference Audit     = PASS
G0.5 Revised Route       = PASS
G0.6 Route Freeze        = PASS
Implementation           = AUTHORIZED FOR TA-1A DISPATCH
```

Stage 0 Pre-Implementation Alignment 已关闭。

当前下一主线：**04｜Agent 任务调度**。

## TA-1A 授权边界

TA-1A 负责第一条真实可用纵向：

```text
Thin Tracks
→ Current Vector / Single Scheduled Item / Month View
→ Derived Today
→ local SQLite durability / backup / restore / migration
→ packaged Electron + tray lifecycle
→ Owner real-data UAT
```

TA-1A 暂不实现 recurrence / Reminder / Memo / Unscheduled / Week View；这些属于 TA-1B，并且 TA-1B 仍依赖 TA-1A Review / Owner UAT。

Owner Windows notification-area / 鼠标 tray focused check 已被接受为 **TA-1A Exit 前必须完成的 residual**，不再阻塞 Route Freeze。

## Frozen Architecture 关键边界

- Tracks / Plan authoritative；Today 主要 derived；
- Today acknowledgement 只保存当日视觉确认；
- immutable internal IDs；
- occurrence identity = `SeriesStableID + OriginalScheduledOccurrenceKey`；
- `date-only != timestamp`，local recurrence 使用 local wall-time；
- Current Vector `[startDate, endDate]` inclusive + no-overlap；
- recurrence = series + dynamic occurrence + exception；
- whole-series edit 清除旧 exceptions 前警告 / 确认；
- backup / restore / migration 使用 frozen safety contracts；
- system tray V0 Required；close → tray；tray restore / real exit；开机启动 Deferred。

## 强制规则

- Route Freeze 不授权 implementation Agent 改写 frozen Product / Architecture / Route；
- 04 负责把 TA-1A 变成 executable Task Packet，不扩大 Scope；
- 05 必须独立读取 implementation facts 做 Review / Reality Gate；
- TA-1B 不得绕过 TA-1A Owner UAT 自动启动；
- frozen route 如需重大修改，必须回到 Owner Discussion / 03 / 00 Gate 流程；
- implementation code / tests / runtime / executable Task 由 `zhangchenjia21-dot/Workbench` 拥有；
- Future Milestones / AI / Sync / Custom / Plugin / SDK 等继续 Deferred。
