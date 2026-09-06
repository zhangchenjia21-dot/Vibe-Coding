# Personal Workbench｜Discussion / Pre-Approval Drafts

本目录用于保存 **Owner 明确要求持久化、但尚未通过 Owner 审核与 Promotion Gate 的提案 / 草案**。

默认规则仍然是：**先在聊天中讨论，草案不自动写 GitHub。**

只有以下情况才进入本目录：

1. Owner 明确要求“保存这个草案 / 写入讨论区”；
2. 为纠正已经发生的越权写入，需要把未审核文件从 authoritative 位置降权迁入此处。

## Authority

本目录中的文件：

- **不是 current Owner**；
- **不是 Architecture Authority**；
- **不是正式 Roadmap / Task Axis**；
- **不是 Owner Decision**；
- **不得触发下游 implementation 或 Gate 自动推进**。

文件中即使历史上写有 `DECIDED`，若该决定没有 Owner 明确批准，也只能按 `PROPOSED / HYPOTHESIS` 理解。

## 当前待讨论草案

- [`开发路线_未审核草案.md`](开发路线_未审核草案.md) — 03 生成的 G0.3 Draft Task Axis；未经 Owner 讨论，已从 `current/` 降权迁入。
- [`架构问题登记表_未审核草案.md`](架构问题登记表_未审核草案.md) — 03 生成的 Architecture Questions Register；未经 Owner 讨论，已从 `architecture/` 降权迁入。

## Promotion

```text
Working Draft
→ Owner Discussion
→ Owner 明确：批准 / 修改 / 延后 / 否决
→ 若批准：写入或提升到 current/ / architecture/ / decisions/
→ 如涉及 Stage Gate：00 独立审核
```

没有 Owner 明确批准，不发生 Promotion。
