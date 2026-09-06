# Personal Workbench｜Current 路由

本目录只保存 **当前仍生效、已经经过 Owner 认可的项目级事实 Owner**。

## 当前 Owner

| 文件 | 作用 | 状态 |
|---|---|---|
| [`项目总纲.md`](项目总纲.md) | 当前产品方向、Authority、仓库边界与治理规则 | current v0.6 |
| [`产品定义.md`](产品定义.md) | Primary Purpose / JTBD / CORE / V0 Scope / Success / Failure / Hard Constraints | **current v1.0 — frozen product baseline** |
| [`开发路线.md`](开发路线.md) | Frozen Task Axis / First Usable / Complete V0 / UAT sequencing | **v1.2 — ROUTE-FROZEN** |
| [`项目状态.md`](项目状态.md) | Current Stage / Goal / Gate / Active Work / Next Action | **current — TA-1B AUTHORIZED FOR DISPATCH** |

## 当前 Architecture

- [`../architecture/架构方案.md`](../architecture/架构方案.md) — **v1.1 / ROUTE-FROZEN V0 ARCHITECTURE**
- [`../architecture/架构问题登记表.md`](../architecture/架构问题登记表.md) — G0.3 supporting input

正式 Route Freeze：[`../decisions/D-003_V0RouteFreeze与TA-1AImplementationAuthorization.md`](../decisions/D-003_V0RouteFreeze与TA-1AImplementationAuthorization.md)

TA-1A Stage Exit：[`../decisions/D-005_TA-1AStageExit与TA-1BAuthorization.md`](../decisions/D-005_TA-1AStageExit与TA-1BAuthorization.md)

## 当前产品与路线

```text
Personal State / Information & Action Desktop
CORE = Today / Plan / Tracks
V0 = Thin Tracks + Usable Plan + Derived Today
Desktop Host = Electron
Canonical Persistence = SQLite
```

路线：

```text
TA-1A First Usable        = PASS / INTEGRATED
→ TA-1B Complete V0       = CURRENT / AUTHORIZED FOR DISPATCH
→ Complete V0 UAT
→ TA-2 Correction / Hardening
→ TA-3 V0 Acceptance
```

Implementation canonical base：

`zhangchenjia21-dot/Workbench main@50bebd2b07619e4e06852ad8a20c110be0553315`

## TA-1B 当前授权边界

进入 frozen V0 中被 First Usable 有意延后的能力：

- recurrence；
- Reminder；
- Unscheduled；
- Memo；
- Week View；
- Today 对当天 recurrence / Reminder / Memo 的派生；
- 完整 date / DST / exception / backup-restore regression。

不得回退 TA-1A 已通过的 Month View / day detail / right-click / multi-delete / Current Vector correction。

继续 Deferred：Milestones / AI / Sync / Custom / Plugin / SDK / Knowledge / Habit / Finance / Health / AI Collaboration / cloud sync 等。

## 当前流转

```text
TA-1A Review + Owner UAT PASS
→ 00 Stage Exit + main integration
→ 04 TA-1B Task Packet / Dispatch
→ Codex implementation
→ 05 Independent Review
→ Owner Complete V0 UAT
→ 00 next Stage decision
```

## 强制规则

- 04 只把 TA-1B frozen scope 转成 executable work，不扩大产品范围；
- Codex 完成后直接去 05，不经过 04 中转；
- 普通 implementation rework 由 05 ↔ Codex 闭环；
- Product / Architecture / Route blocker 才回 00 决定是否流转 01 / 03；
- implementation code / tests / runtime / executable Task 由 `zhangchenjia21-dot/Workbench` 拥有。
