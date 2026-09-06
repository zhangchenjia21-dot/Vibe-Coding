# Personal Workbench｜Current 路由

本目录只保存 **当前仍生效、已经经过 Owner 认可的项目级事实 Owner**。

## 当前 Owner

| 文件 | 作用 | 状态 |
|---|---|---|
| [`项目总纲.md`](项目总纲.md) | 当前产品方向、Authority、仓库边界与 Stage 0 规则 | current v0.4 |
| [`产品定义.md`](产品定义.md) | Primary Purpose / JTBD / CORE / V0 Scope / Success / Failure / Hard Constraints | **current v1.0 — G0.1 / G0.2 PASS** |
| [`开发路线.md`](开发路线.md) | Revised Task Axis / First Usable / Complete V0 / UAT sequencing | **v1.1 — OWNER-APPROVED G0.5 SUBMISSION，等待 00 Review** |
| [`项目状态.md`](项目状态.md) | Current Stage / Goal / Blocker / Gate / Active Work / Next Action | **current — Gate 结论仅由 00 维护** |

## 当前 Architecture

- [`../architecture/架构方案.md`](../architecture/架构方案.md) — **v1.0 / OWNER-APPROVED G0.5 CANDIDATE / NOT ROUTE-FROZEN**
- [`../architecture/架构问题登记表.md`](../architecture/架构问题登记表.md) — **v1.0 / G0.3 question register / supporting input**

当二者对 G0.4 已关闭问题的状态表述不同，以较新的 `架构方案.md` 为 G0.5 Candidate；`架构问题登记表.md` 继续保留问题来源与 G0.3 审计上下文。

## G0.4 Research Evidence

- [`../research/G0.4_Reference_Audit_Report.md`](../research/G0.4_Reference_Audit_Report.md)
- [`../research/G0.4_Reference_Sources.md`](../research/G0.4_Reference_Sources.md)
- [`../research/G0.4_Technical_Spike_Results.md`](../research/G0.4_Technical_Spike_Results.md)
- [`../research/G0.4_Gate_Review_Handoff_to_00.md`](../research/G0.4_Gate_Review_Handoff_to_00.md)

## 当前正式产品方向

```text
Personal State / Information & Action Desktop

CORE
├─ Today
├─ Plan
└─ Tracks

V0
= Thin Tracks + Usable Plan + Derived Today
```

正式 Pivot 裁定：

[`../decisions/D-002_正式Pivot到PersonalStateCore.md`](../decisions/D-002_正式Pivot到PersonalStateCore.md)

## G0.5 已收敛的 Architecture Candidate

Owner 已批准作为 G0.5 submission 的主要候选：

- Electron = V0 Desktop Host；
- SQLite = V0 local canonical persistent store；
- Today = derived projection + 极窄当日 acknowledgement；
- immutable internal identity；
- occurrence identity = `SeriesStableID + OriginalScheduledOccurrenceKey`；
- `date-only != timestamp`，recurrence 使用 local wall-time semantics；
- Current Vector `[startDate, endDate]` 两端包含且禁止 overlap；
- recurrence = series + dynamic occurrence + exception；
- whole-series edit 在确认后清除旧 exceptions，不自动 remap；
- restore 前 safety snapshot，schema migration 前 safety backup；
- First Usable 提前：`TA-1A → Owner UAT → TA-1B Complete V0`。

这些内容已经 Owner-approved for G0.5 submission，但 **尚未 Route Freeze**。

## 当前 Stage 0 Gate

```text
Product Direction        = APPROVED
G0.1 Product Baseline    = PASS
G0.2 Scope               = PASS
G0.3 Draft Task Axis     = PASS
G0.4 Reference Audit     = PASS
G0.5 Revised Route       = OWNER-APPROVED SUBMISSION / WAITING FOR 00 REVIEW
G0.6 Route Freeze        = NOT PASS
Implementation           = NOT AUTHORIZED
```

只有 `00｜项目总控` 可以把 G0.5 / G0.6 更新为 PASS。

## 当前流转

```text
G0.5 Owner-approved Architecture Candidate + Revised Route
→ 00: G0.5 Gate Review
→ 00 + Owner: G0.6 Route Freeze
→ Implementation（仅在 Freeze PASS 后）
→ TA-1A First Usable
→ Owner Real-data UAT
→ TA-1B Complete V0
→ Complete V0 UAT
→ Correction / Hardening
→ V0 Acceptance
```

## 强制规则

- 当前 `开发路线.md` / `架构方案.md` 不构成 implementation authorization；
- 未经 00 记录 G0.6 Route Freeze PASS，04 不得把它们解释成正式 Coding 授权；
- implementation code / tests / runtime / executable Task 回到 `zhangchenjia21-dot/Workbench`；
- Future Milestones / AI / Sync / Custom / Plugin / SDK 等继续 Deferred，不因 Route Freeze 自动进入下一阶段。