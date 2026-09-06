# Personal Workbench｜Current 路由

本目录只保存 **当前仍生效、已经经过 Owner 认可的项目级事实 Owner**。

## 当前 Owner

| 文件 | 作用 | 状态 |
|---|---|---|
| [`项目总纲.md`](项目总纲.md) | 当前产品方向、Authority、仓库边界与 Stage 0 规则 | current v0.4 |
| [`产品定义.md`](产品定义.md) | Primary Purpose / JTBD / CORE / V0 Scope / Success / Failure / Hard Constraints | **current v1.0 — G0.1 / G0.2 PASS** |
| [`开发路线.md`](开发路线.md) | Owner-reviewed Draft Task Axis / First Usable / Reference Audit Handoff | **v1.0 — G0.3 SUBMISSION，NOT ROUTE-FROZEN** |
| [`项目状态.md`](项目状态.md) | Current Stage / Goal / Blocker / Gate / Active Work / Next Action | current；Gate 结论仅由 00 维护 |

架构开放问题与 Owner 已认可约束：

- [`../architecture/架构问题登记表.md`](../architecture/架构问题登记表.md) — **v1.0 / OWNER-REVIEWED / G0.3 SUBMISSION / NOT ROUTE-FROZEN**

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

Owner 已批准上述方向；正式裁定见：

[`../decisions/D-002_正式Pivot到PersonalStateCore.md`](../decisions/D-002_正式Pivot到PersonalStateCore.md)

## 当前 G0.3 Submission 关键边界

Owner 与 03 已讨论并明确认可：

- Today 主要从 Plan / Tracks 派生，不建立第二份业务 truth；
- Today “已完成”只是当天视觉确认，不推进 Plan / Track，不做统计 / streak / 评分；
- Current Vector 日期区间禁止重叠；
- 循环事项采用 series definition + dynamic occurrence + exception；
- 循环编辑 / 删除 V0 只提供“仅这一次 / 整个循环”；
- Core Personal State 本地可靠保存，并需要手动 backup / restore 与 migration 能力；
- 系统托盘属于 V0 Required；开机启动 Deferred；
- Today / Plan / Tracks 采用克制的 V0 IA，不提前建设 Personal OS 平台；
- Route Freeze 后倾向集中实现完整小型 V0，再进行第一次完整 Owner Product UAT；实现内部仍必须有工程 Reality Gates。

这些内容已经进入 `开发路线.md` 与 `architecture/架构问题登记表.md`，但**仍不等于 G0.3 PASS 或 Route Freeze**。

## 历史路线

原 **AI Collaboration V0**：

```text
HOLD / HISTORICAL / FUTURE RE-DISCOVERY
```

归档：

[`../../99_归档/workbench/AI协作路线_V0/`](../../99_归档/workbench/AI协作路线_V0/)

旧 Product Definition、Scope、Gate PASS、Draft Task Axis 与架构假设均不得覆盖当前产品方向。

## Product Discovery Evidence

本轮 Pivot Discovery 的原始讨论收敛保存在：

[`../discussion/产品方向评审.md`](../discussion/产品方向评审.md)

它是产品决策的讨论证据，不与 `current/产品定义.md` 竞争 Authority。

## 当前流转

```text
Product Direction APPROVED
→ G0.1 Product Baseline PASS
→ G0.2 Scope PASS
→ 03 + Owner: Architecture Questions + Draft Task Axis（已形成 G0.3 submission）
→ 00: G0.3 Gate Review
→ 若 PASS：02 Reference Audit Pass 1 + Pass 2 / 必要 Spike
→ 03 + Owner: Revised Route / Architecture
→ 00 + Owner: Route Freeze
→ Implementation
```

## 强制规则

- `current/` 不是草稿暂存区。
- 当前 `开发路线.md` 已经获得 Owner Promotion 授权，但仍只是 G0.3 submission；只有 00 可以判定 Gate PASS。
- Reference Audit 证据进入 `../research/`；经 Owner 提升的长期架构进入 `../architecture/`；正式 Owner 裁定进入 `../decisions/`。
- 实现代码、测试、runtime 与 executable Task 回到 `zhangchenjia21-dot/Workbench`。
- 当前 Route Freeze 未通过，正式 implementation 未授权。
