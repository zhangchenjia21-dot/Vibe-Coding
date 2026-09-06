# Personal Workbench｜Current 路由

本目录只保存 **当前仍生效、已经经过 Owner 认可的项目级事实 Owner**。

## 当前 Owner

| 文件 | 作用 | 状态 |
|---|---|---|
| [`项目总纲.md`](项目总纲.md) | 当前产品方向、Authority、仓库边界与 Stage 0 规则 | current v0.4 |
| [`产品定义.md`](产品定义.md) | Primary Purpose / JTBD / CORE / V0 Scope / Success / Failure / Hard Constraints | **current v1.0 — G0.1 / G0.2 PASS** |
| [`项目状态.md`](项目状态.md) | Current Stage / Goal / Blocker / Gate / Active Work / Next Action | current；Gate 结论仅由 00 维护 |

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
→ 03: Architecture Questions + Draft Task Axis Working Draft
→ Owner Discussion / Approval
→ 00: G0.3 Gate Review
→ 02: Reference Audit Pass 1 + Pass 2
→ 03: Revised Route / Architecture
→ Owner Approval
→ 00: Route Freeze
→ Implementation
```

## 强制规则

- `current/` 不是草稿暂存区。
- 03 的新路线 / 架构默认先在聊天中形成 Working Draft；未经 Owner 明确批准不得 Promotion。
- `DRAFT` 标签不等于 GitHub Promotion permission。
- Reference Audit 证据进入 `../research/`；经 Owner 提升的长期架构进入 `../architecture/`；正式 Owner 裁定进入 `../decisions/`。
- 实现代码、测试、runtime 与 executable Task 回到 `zhangchenjia21-dot/Workbench`。
- 当前 Route Freeze 未通过，正式 implementation 未授权。