# Personal Workbench｜Current 路由

本目录只保存 **当前仍生效、已经经过 Owner 认可的项目级事实 Owner**。

## 当前 Owner

| 文件 | 作用 | 状态 |
|---|---|---|
| [`项目总纲.md`](项目总纲.md) | 当前产品方向、Authority、仓库边界与治理规则 | current v0.4 |
| [`产品定义.md`](产品定义.md) | Primary Purpose / JTBD / CORE / V0 Scope / Success / Failure / Hard Constraints | **current v1.0 — frozen product baseline** |
| [`开发路线.md`](开发路线.md) | Frozen Task Axis / First Usable / Complete V0 / UAT sequencing | **v1.2 — ROUTE-FROZEN** |
| [`项目状态.md`](项目状态.md) | Current Stage / Goal / Gate / Active Work / Next Action | **current — TA-1A UAT CORRECTION ACTIVE** |

## 当前 Architecture

- [`../architecture/架构方案.md`](../architecture/架构方案.md) — **v1.1 / ROUTE-FROZEN V0 ARCHITECTURE**
- [`../architecture/架构问题登记表.md`](../architecture/架构问题登记表.md) — G0.3 supporting input

正式 Route Freeze：

[`../decisions/D-003_V0RouteFreeze与TA-1AImplementationAuthorization.md`](../decisions/D-003_V0RouteFreeze与TA-1AImplementationAuthorization.md)

当前 UAT correction：

[`../decisions/D-004_TA-1AOwnerUATCorrectionScope.md`](../decisions/D-004_TA-1AOwnerUATCorrectionScope.md)

## 当前产品与路线

```text
Personal State / Information & Action Desktop

CORE
├─ Today
├─ Plan
└─ Tracks

V0
= Thin Tracks + Usable Plan + Derived Today

Desktop Host = Electron
Canonical Persistence = SQLite
```

路线仍为：

```text
TA-1A First Usable
→ Owner real-data UAT
→ TA-1B Complete V0
→ Complete V0 UAT
→ TA-2 Correction / Hardening
→ TA-3 V0 Acceptance
```

当前 TA-1A 首轮 Owner UAT 结果：**REWORK REQUIRED**。因此 TA-1A 尚未 Exit，TA-1B 未授权。

## 当前 TA-1A Correction

必须修正：

- Plan Month View 可读性与低摩擦日程维护；
- 日期格 left-click 当天详情；
- right-click `新建 / 编辑 / 清空日程`；
- 当天详情多选删除；
- Today Current Vector 移除“今日确认 / 已完成”和 Today 直接编辑入口；
- Current Vector 使用更醒目的独立视觉层级。

不进入本轮：

- recurrence；
- Reminder；
- Memo；
- Unscheduled；
- Week View；
- AI / GPT / Codex assisted Track maintenance；
- 其它 TA-1B / Future 能力。

Tracks 手工维护的“生硬感”目前只作为观察项，不足以重开产品或提前接 AI。

## 当前流转

```text
Owner UAT REWORK
→ 00 scope convergence / D-004
→ 04 short PWB-001 UAT Correction Addendum
→ Codex correction on same task branch
→ 05 independent re-review
→ Owner focused re-UAT
→ 00 TA-1A Stage Exit decision
```

普通 correction 完成后 Codex **直接去 05**，不再经过 04 中转。

## 强制规则

- Product / Architecture / Route 本轮不重开；
- PWB-001 仍是同一 TA-1A Task；
- 04 本轮只做 Acceptance Amendment，不得扩大到 TA-1B；
- 05 复审必须验证 correction 和已通过项 regression；
- TA-1B 不得在 TA-1A focused re-UAT PASS 前启动；
- implementation code / tests / runtime / executable Task 由 `zhangchenjia21-dot/Workbench` 拥有。
