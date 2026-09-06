# Personal Workbench｜Current 路由

本目录只保存 **当前仍生效或正在被 Owner 审核的核心项目事实 Owner**。

## 当前 Owner

| 文件 | 作用 | 状态 |
|---|---|---|
| [`项目总纲.md`](项目总纲.md) | 项目基础定位、Authority、协作模型、仓库边界与 Stage 0 约束 | current v0.2 |
| [`产品定义.md`](产品定义.md) | Primary Purpose / JTBD / Core Value / Core Journey / V0 Scope / Success Criteria | **DRAFT v0.1 — G0.1 / G0.2 PASS，NOT ROUTE-FROZEN** |
| [`项目状态.md`](项目状态.md) | Current Stage / Goal / Blocker / Gate / Active Work / Next Action | current |

## 待建立的 Current Owner

以下文件只有在对应专业聊天完成工作并通过必要审核后才建立，不提前创建空 Authority：

- Current Roadmap / Task Axis Owner — 下一步由 `03｜架构与开发路线` 形成 Draft v0，再经 Reference Audit / Owner review 演化；
- 必要的其它长期 current owner — 仅在已有文件无法清晰承载时新增。

## 当前流转

```text
Product Definition v0.1
→ G0.1 Product Baseline PASS
→ G0.2 Scope PASS
→ 03: Architecture Questions Register + Draft Task Axis v0
→ 02: Reference Audit Pass 1 + Pass 2 / TV-01～TV-05
→ 03: Revised Route / Architecture
→ 00 + Owner: Route Freeze
```

通过 Product Baseline / Scope 只授权后续 **Stage 0 planning / research**，不授权正式 implementation。

## 规则

- `current/` 不是草稿垃圾场；只有确实需要作为当前项目路由的内容才进入这里。
- DRAFT 可以存在，但必须在文档内明确标记 `DRAFT / NOT ROUTE-FROZEN / NOT IMPLEMENTATION AUTHORIZATION`。
- Reference Audit 证据写入 `../research/`；长期架构写入 `../architecture/`；正式 Owner 裁定写入 `../decisions/`。
- 实现代码、测试、runtime 与 executable Task 回到 `zhangchenjia21-dot/Workbench`。
- 当前 Owner 原位更新，不新增 `final2/latest/new` 平行版本。
