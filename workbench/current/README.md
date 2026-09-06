# Personal Workbench｜Current 路由

本目录只保存 **当前仍生效、已经经过 Owner 认可的项目级事实 Owner**。

## 当前 Owner

| 文件 | 作用 | 状态 |
|---|---|---|
| [`项目总纲.md`](项目总纲.md) | 当前产品方向、Authority、仓库边界与 Stage 0 规则 | current v0.4 |
| [`产品定义.md`](产品定义.md) | Primary Purpose / JTBD / CORE / V0 Scope / Success / Failure / Hard Constraints | **current v1.0 — G0.1 / G0.2 PASS** |
| [`开发路线.md`](开发路线.md) | Owner-reviewed Draft Task Axis / First Usable / Reference Audit Handoff | **v1.0 — G0.3 PASS，NOT ROUTE-FROZEN** |
| [`项目状态.md`](项目状态.md) | Current Stage / Goal / Blocker / Gate / Active Work / Next Action | **current — G0.4 NEXT** |

架构问题与 Owner 已认可约束：

- [`../architecture/架构问题登记表.md`](../architecture/架构问题登记表.md) — **v1.0 / OWNER-REVIEWED / G0.3 PASS INPUT / NOT ROUTE-FROZEN**

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

## 当前 Stage 0 Gate

```text
Product Direction        = APPROVED
G0.1 Product Baseline    = PASS
G0.2 Scope               = PASS
G0.3 Draft Task Axis     = PASS
G0.4 Reference Audit     = NEXT / AUTHORIZED FOR RESEARCH
G0.5 Revised Route       = WAITING FOR G0.4
G0.6 Route Freeze        = NOT PASS
Implementation           = NOT AUTHORIZED
```

G0.3 PASS 只表示当前 Draft Route 已足够完整，可以进入独立 Reference / Path Audit；**不表示路线已经被证明正确，也不表示 Route Freeze。**

## 当前 G0.3 关键边界

Owner 与 03 已讨论并认可的主要约束包括：

- Today 主要从 Plan / Tracks 派生，不建立第二份业务 truth；
- Today “已完成”只是当天视觉确认，不推进 Plan / Track，不做统计 / streak / 评分；
- Current Vector 日期区间禁止重叠；
- 循环事项采用 series definition + dynamic occurrence + exception；
- 循环编辑 / 删除 V0 只提供“仅这一次 / 整个循环”；
- Core Personal State 本地可靠保存，并需要 restart recovery、手动 backup / restore 与 migration；
- 系统托盘属于 V0 Required；开机启动 Deferred；
- Today / Plan / Tracks 采用克制的 V0 IA，不提前建设 Personal OS 平台；
- Route Freeze 后当前 Draft 倾向集中实现完整小型 V0，再进行第一次完整 Owner Product UAT；但该顺序仍必须由 G0.4 主动攻击。

## 当前流转

```text
Product Direction APPROVED
→ G0.1 PASS
→ G0.2 PASS
→ G0.3 PASS
→ 02: Reference Audit Pass 1 + Pass 2 / 必要 Spike
→ 03 + Owner: Revised Route / Architecture Candidate
→ 00: G0.5 Gate Review
→ 00 + Owner: G0.6 Route Freeze
→ Implementation（仅在 Freeze PASS 后）
```

02 的直接任务输入是 `开发路线.md` 中的 **Reference Audit Handoff to 02**。其职责是主动寻找反证、遗漏和顺序问题，不是替当前路线找支持材料。

## 历史路线

原 **AI Collaboration V0**：

```text
HOLD / HISTORICAL / FUTURE RE-DISCOVERY
```

归档：

[`../../99_归档/workbench/AI协作路线_V0/`](../../99_归档/workbench/AI协作路线_V0/)

旧 Product Definition、Scope、Gate PASS、Draft Task Axis 与架构假设均不得覆盖当前产品方向。

## Product Discovery Evidence

当前 Pivot Discovery 的讨论证据：

[`../discussion/产品方向评审.md`](../discussion/产品方向评审.md)

它不与 `current/产品定义.md` 竞争 Authority。

## 强制规则

- `current/` 不是草稿暂存区；
- G0.4 Research / Spike evidence 进入 `../research/`，不得自动升级为 Architecture Decision；
- 02 发现路线问题后，必须先向 Owner 提交 finding / impact / proposal，再由 03 + Owner 修订路线；
- 经 Owner 提升的长期架构进入 `../architecture/`；正式项目级裁定进入 `../decisions/`；
- 实现代码、测试、runtime 与 executable Task 回到 `zhangchenjia21-dot/Workbench`；
- 当前 Route Freeze 未通过，正式 implementation 未授权。