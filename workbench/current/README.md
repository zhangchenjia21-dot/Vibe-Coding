# Personal Workbench｜Current 路由

本目录只保存 **当前仍生效或正在被 Owner 审核的核心项目事实 Owner**。

## 当前 Owner

| 文件 | 作用 | 状态 |
|---|---|---|
| [`项目总纲.md`](项目总纲.md) | 项目基础定位、Authority、协作模型、仓库边界与 Stage 0 约束 | current v0.2 |
| [`产品定义.md`](产品定义.md) | Primary Purpose / JTBD / Core Value / Core Journey / V0 Scope / Success Criteria | **DRAFT v0.1 — G0.1 / G0.2 PASS，NOT ROUTE-FROZEN** |
| [`开发路线.md`](开发路线.md) | Draft Task Axis / First Reality Gate / First Usable / Reference Audit Handoff | **DRAFT v0.1 — G0.3 SUBMISSION，NOT ROUTE-FROZEN，NOT IMPLEMENTATION AUTHORIZATION** |
| [`项目状态.md`](项目状态.md) | Current Stage / Goal / Blocker / Gate / Active Work / Next Action | current；Gate 结论仍由 00 维护 |

## 待建立的 Current Owner

以下文件只有在对应专业聊天完成工作并通过必要审核后才建立，不提前创建空 Authority：

- 必要的其它长期 current owner — 仅在已有文件无法清晰承载时新增；
- Revised Route / Architecture 应优先原位演化现有稳定语义 Owner，除非事实所有权明确需要拆分。

## 当前流转

```text
Product Definition v0.1
→ G0.1 Product Baseline PASS
→ G0.2 Scope PASS
→ 03: Architecture Questions Register + Draft Task Axis v0（已形成，提交 00 审核）
→ 00: G0.3 Gate Review
→ 若 G0.3 PASS：02 Reference Audit Pass 1 + Pass 2 / TV-01～TV-05
→ 03: Revised Route / Architecture
→ 00 + Owner: Route Freeze
```

Architecture Questions Register 位于：

- [`../architecture/架构问题登记表.md`](../architecture/架构问题登记表.md) — **DRAFT / EXPLORATION / NOT ROUTE-FROZEN**。

通过 Product Baseline / Scope 只授权后续 **Stage 0 planning / research**，不授权正式 implementation。当前 `开发路线.md` 的存在也**不等于 G0.3 PASS**；只有 `00｜项目总控` 可以更新 Gate 状态。

## 规则

- `current/` 不是草稿垃圾场；只有确实需要作为当前项目路由的内容才进入这里。
- DRAFT 可以存在，但必须在文档内明确标记 `DRAFT / NOT ROUTE-FROZEN / NOT IMPLEMENTATION AUTHORIZATION`。
- Reference Audit 证据写入 `../research/`；长期架构写入 `../architecture/`；正式 Owner 裁定写入 `../decisions/`。
- 实现代码、测试、runtime 与 executable Task 回到 `zhangchenjia21-dot/Workbench`。
- 当前 Owner 原位更新，不新增 `final2/latest/new` 平行版本。
