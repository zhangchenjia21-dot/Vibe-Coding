# Personal Workbench｜Discussion / Pre-Approval Drafts

本目录用于保存 **Owner 明确要求持久化的 pre-approval 提案、产品讨论收敛和 decision evidence**。

默认规则仍然是：**先在聊天中讨论，草案不自动写 GitHub。**

## Authority

本目录中的文件：

- **不是 current Owner**；
- **不是 Architecture Authority**；
- **不是正式 Roadmap / Task Axis**；
- **不是 implementation authorization**；
- 只有经 Owner 明确批准并 Promotion 到 `current/ / architecture/ / decisions/` 的内容才获得相应 Authority。

## 当前文档

### `产品方向评审.md`

状态：**DISCUSSION EVIDENCE — 已由 Owner 于 2026-09-06 批准核心方向并完成 Promotion**。

其批准内容已经传播到：

- `../current/产品定义.md`
- `../current/项目总纲.md`
- `../current/项目状态.md`
- `../decisions/D-002_正式Pivot到PersonalStateCore.md`

因此后续不得把 `产品方向评审.md` 当作与 current Product Definition 并行的事实源；需要当前产品事实时只读 `current/产品定义.md`。

### `未来备选功能计划.md`

继续是 **DISCUSSION / FUTURE / NON-CURRENT**。其中 Launch Target 等能力尚未进入 V0。

## Historical AI Collaboration drafts

此前 03 生成的：

- `开发路线_未审核草案.md`
- `架构问题登记表_未审核草案.md`

已随 AI Collaboration V0 整体归档到：

`Vibe-Coding/99_归档/workbench/AI协作路线_V0/`

## Promotion

```text
Working Draft
→ Owner Discussion
→ Owner 明确：批准 / 修改 / 延后 / 否决
→ 若批准：写入或提升到 current/ / architecture/ / decisions/
→ 如涉及 Stage Gate：00 独立审核
```

没有 Owner 明确批准，不发生 Promotion。
