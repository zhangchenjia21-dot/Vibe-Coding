# Personal Workbench｜Current 路由

本目录只保存 **当前仍生效、已经经过 Owner 认可的项目级事实 Owner**。

## 当前 Owner

| 文件 | 作用 | 状态 |
|---|---|---|
| [`项目总纲.md`](项目总纲.md) | 当前产品方向、Authority、仓库边界与 Stage 0 规则 | current v0.4 |
| [`产品定义.md`](产品定义.md) | Primary Purpose / JTBD / CORE / V0 Scope / Success / Failure / Hard Constraints | **current v1.0 — G0.1 / G0.2 PASS** |
| [`开发路线.md`](开发路线.md) | Owner-reviewed Draft Task Axis / First Usable / Reference Audit Handoff | **v1.0 — G0.3 PASS，等待 G0.5 Revision** |
| [`项目状态.md`](项目状态.md) | Current Stage / Goal / Blocker / Gate / Active Work / Next Action | **current — G0.5 NEXT** |

架构问题与 Owner 已认可约束：

- [`../architecture/架构问题登记表.md`](../architecture/架构问题登记表.md) — **v1.0 / G0.3 PASS INPUT / NOT ROUTE-FROZEN**

G0.4 Research Evidence：

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

## 当前 Stage 0 Gate

```text
Product Direction        = APPROVED
G0.1 Product Baseline    = PASS
G0.2 Scope               = PASS
G0.3 Draft Task Axis     = PASS
G0.4 Reference Audit     = PASS
G0.5 Revised Route       = NEXT / AUTHORIZED FOR WORKING DRAFT
G0.6 Route Freeze        = NOT PASS
Implementation           = NOT AUTHORIZED
```

G0.4 已确认没有 route-killing technical feasibility issue，但发现当前 Draft Route 需要在 G0.5 处理重要修改：提前 First Usable Owner UAT、收敛 SQLite persistence contract、在 Electron/Tauri 之间做 host 选型、关闭 recurrence whole-series exception policy，并吸收 identity / date / backup / restore 证据。

## 当前流转

```text
Product Direction APPROVED
→ G0.1 PASS
→ G0.2 PASS
→ G0.3 PASS
→ G0.4 PASS
→ 03 + Owner: Revised Route / Architecture Candidate
→ 00: G0.5 Gate Review
→ 00 + Owner: G0.6 Route Freeze
→ Implementation（仅在 Freeze PASS 后）
```

## 强制规则

- `current/` 不是草稿暂存区；
- G0.4 research evidence 不自动覆盖 current Route / Architecture；
- 03 必须逐项处理 G0.4 findings，并先与 Owner 讨论；
- 未经 Owner 明确批准，不得把 G0.5 Working Draft Promotion 为 frozen architecture；
- 经 Owner 提升的长期架构进入 `../architecture/`；正式项目级裁定进入 `../decisions/`；
- 实现代码、测试、runtime 与 executable Task 回到 `zhangchenjia21-dot/Workbench`；
- 当前 Route Freeze 未通过，正式 implementation 未授权。