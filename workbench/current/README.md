# Personal Workbench｜Current 路由

本目录只保存 **已经进入当前项目路线的核心事实 Owner**。未经 Owner 审核的新 Product / Architecture / Roadmap / Task Axis 提案不得因为标记为 DRAFT 就进入本目录。

## 当前 Owner

| 文件 | 作用 | 状态 |
|---|---|---|
| [`项目总纲.md`](项目总纲.md) | 项目基础定位、Authority、协作模型、仓库边界与 Stage 0 约束 | current v0.2 |
| [`产品定义.md`](产品定义.md) | Primary Purpose / JTBD / Core Value / Core Journey / V0 Scope / Success Criteria | **DRAFT v0.1 — G0.1 / G0.2 PASS，NOT ROUTE-FROZEN** |
| [`项目状态.md`](项目状态.md) | Current Stage / Goal / Blocker / Gate / Active Work / Next Action | current；Gate 结论仅由 00 维护 |

## 待 Owner 讨论的 03 草案

03 已形成一版未经 Owner 审核的 G0.3 工作草案。由于未经过 Owner Discussion & Promotion Gate，它们已降权保存到：

- [`../discussion/开发路线_未审核草案.md`](../discussion/开发路线_未审核草案.md)
- [`../discussion/架构问题登记表_未审核草案.md`](../discussion/架构问题登记表_未审核草案.md)

这些文件：

- 不是 current route；
- 不是 Architecture Authority；
- 不是 G0.3 submission；
- 其中任何 `DECIDED` 字样都不能作为已生效决定；
- 不得触发 02 / 04 / implementation。

## 当前流转

```text
Product Definition v0.1
→ G0.1 Product Baseline PASS
→ G0.2 Scope PASS
→ 03 已生成未审核 Working Draft
→ Owner 与 03 逐项讨论 / 修改 / 批准
→ 获 Owner 明确批准后才允许 GitHub Promotion
→ 00: G0.3 Gate Review
→ 若 G0.3 PASS：02 Reference Audit Pass 1 + Pass 2 / TV-01～TV-05
→ 03: Revised Route / Architecture
→ Owner Discussion + Approval
→ 00 + Owner: Route Freeze
```

## 规则

- `current/` 不是草稿暂存区。
- `DRAFT` 标签不等于 GitHub Promotion permission。
- 未经 Owner 审核的新路线 / 新架构默认只在聊天讨论；Owner 明确要求保存草案时才进 `../discussion/`。
- Reference Audit 证据写入 `../research/`；经 Owner 提升的长期架构写入 `../architecture/`；正式 Owner 裁定写入 `../decisions/`。
- 实现代码、测试、runtime 与 executable Task 回到 `zhangchenjia21-dot/Workbench`。
- 当前 Owner 原位更新，不新增 `final2/latest/new` 平行版本。
