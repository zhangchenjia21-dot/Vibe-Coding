# Personal Workbench｜Current 路由

本目录只保存 **当前仍生效、已经经过 Owner 认可的项目级事实 Owner**。

## 当前 Owner

| 文件 | 作用 | 状态 |
|---|---|---|
| [`项目总纲.md`](项目总纲.md) | 当前产品方向、Authority、仓库边界与 Stage 0 规则 | current v0.4 |
| [`产品定义.md`](产品定义.md) | Primary Purpose / JTBD / CORE / V0 Scope / Success / Failure / Hard Constraints | **current v1.0 — G0.1 / G0.2 PASS** |
| [`开发路线.md`](开发路线.md) | Owner-reviewed Draft Task Axis / First Usable / Reference Audit Handoff | **v1.0 — G0.3 PASS，NOT ROUTE-FROZEN** |
| [`项目状态.md`](项目状态.md) | Current Stage / Goal / Blocker / Gate / Active Work / Next Action | **current — G0.4 SPIKES OPEN** |

架构问题与 Owner 已认可约束：

- [`../architecture/架构问题登记表.md`](../architecture/架构问题登记表.md) — **v1.0 / OWNER-REVIEWED / G0.3 PASS INPUT / NOT ROUTE-FROZEN**

G0.4 研究证据：

- [`../research/G0.4_Reference_Audit_Report.md`](../research/G0.4_Reference_Audit_Report.md) — Pass 1 + Pass 2 / Evidence Matrix / Route-changing findings；
- [`../research/G0.4_Reference_Sources.md`](../research/G0.4_Reference_Sources.md) — standards / official docs / historical evidence index。

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
G0.4 Reference Audit     = RESEARCH COMPLETE / REQUIRED SPIKES OPEN
G0.5 Revised Route       = WAITING FOR G0.4 SPIKE EVIDENCE
G0.6 Route Freeze        = NOT PASS
Implementation           = NOT AUTHORIZED
```

G0.4 文档审计已完成，但报告明确判定三组问题需要 Route Freeze 前现实验证，因此当前仍停留在 02：

```text
S1 Packaged Desktop Lifecycle
S2 SQLite Durability / Backup / Migration
S3 Recurrence Identity / Exception
```

所有 Spike 必须是：

`EXPLORATION / NOT CANONICAL ARCHITECTURE / NOT PRODUCTION COMMITMENT`。

## 当前流转

```text
Product Direction APPROVED
→ G0.1 PASS
→ G0.2 PASS
→ G0.3 PASS
→ 02 Pass 1 + Pass 2 COMPLETE
→ 02 S1 / S2 / S3 Feasibility Spikes
→ 00 G0.4 Gate Review
→ 若 PASS：03 + Owner Revised Route / Architecture Candidate
→ 00 G0.5 Gate Review
→ 00 + Owner G0.6 Route Freeze
→ Implementation（仅在 Freeze PASS 后）
```

02 的 RC-01～RC-04、SQLite preferred candidate、TA-1A/TA-1B 等建议目前仍只是 **research proposals**，不得直接覆盖 `开发路线.md` 或 `architecture/`。G0.4 完成后由 03 + Owner 在 G0.5 讨论、采纳、修改或拒绝。

## 历史路线

原 **AI Collaboration V0**：

```text
HOLD / HISTORICAL / FUTURE RE-DISCOVERY
```

归档：

[`../../99_归档/workbench/AI协作路线_V0/`](../../99_归档/workbench/AI协作路线_V0/)

旧 Product Definition、Scope、Gate PASS、Draft Task Axis 与架构假设均不得覆盖当前产品方向。

## 强制规则

- `current/` 不是草稿暂存区；
- G0.4 Research / Spike evidence 进入 `../research/`，不得自动升级为 Architecture Decision；
- 02 发现路线问题后不得自行改 current route / architecture；
- 经 Owner 提升的长期架构进入 `../architecture/`；正式项目级裁定进入 `../decisions/`；
- 实现代码、测试、runtime 与 executable Task 回到 `zhangchenjia21-dot/Workbench`；
- 当前 Route Freeze 未通过，正式 implementation 未授权。