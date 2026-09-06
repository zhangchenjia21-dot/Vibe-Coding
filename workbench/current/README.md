# Personal Workbench｜Current 路由

本目录只保存 **当前仍生效、已经经过 Owner 认可的项目级事实 Owner**。

当前项目正在重新评估 Primary Purpose，因此这里**暂时没有 Product Definition / Roadmap / Architecture current owner**。

## 当前 Owner

| 文件 | 作用 | 状态 |
|---|---|---|
| [`项目总纲.md`](项目总纲.md) | 方向重评阶段的项目定位、Authority、仓库边界与治理规则 | current v0.3 |
| [`项目状态.md`](项目状态.md) | Current Stage / Goal / Blocker / Gate / Active Work / Next Action | current；Gate 结论仅由 00 维护 |

## 已退出 current 的旧路线

原 **AI Collaboration V0** 已于 2026-09-06 由 Owner 明确要求归档，不再作为当前产品基线。

历史资料统一位于：

[`../../99_归档/workbench/AI协作路线_V0/`](../../99_归档/workbench/AI协作路线_V0/)

其中包括旧 Product Definition、旧总纲/状态快照、未审核 Task Axis / Architecture Questions 草案以及旧 Idea Pool。

这些资料：

- 只作历史 / 经验 / 未来 revisit evidence；
- 不参与 current Authority；
- 旧 `G0.1 / G0.2 PASS` 不可迁移到新方向；
- 旧草案中的 `DECIDED` 字样不构成当前决定。

## 当前产品方向状态

```text
AI Collaboration V0
→ HOLD / HISTORICAL

Personal OS / Life Operating System
→ PROPOSED CANDIDATE / PRODUCT DISCOVERY

Current Product Definition
→ NONE
```

当前不应根据参考图自动创建 `Today / Plan / Tracks / Dashboard` 等 Requirement；也不应因为 AI Collaboration 曾经是 current 就继续沿用其 Scope。

## 当前流转

```text
Product Direction Re-evaluation
→ 01 与 Owner 做 Pivot Discovery
→ Owner 明确选择方向
→ 形成新的 Product Definition
→ 00 重新审核 G0.1 Product Baseline
→ G0.2 Scope
→ G0.3 Draft Task Axis
→ G0.4 Reference Audit
→ G0.5 Revised Route / Architecture
→ G0.6 Route Freeze
→ Implementation
```

## 规则

- `current/` 不是草稿暂存区。
- `DRAFT` 标签不等于 GitHub Promotion permission。
- 未经 Owner 审核的新产品 / 路线 / 架构默认只在聊天讨论；Owner 明确要求保存草案时才进 `../discussion/`。
- Reference Audit 证据写入 `../research/`；经 Owner 提升的长期架构写入 `../architecture/`；正式 Owner 裁定写入 `../decisions/`。
- 实现代码、测试、runtime 与 executable Task 回到 `zhangchenjia21-dot/Workbench`。
