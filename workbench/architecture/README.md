# Personal Workbench｜Architecture

本目录只保存 **已经经过 Owner 讨论并明确提升的跨阶段长期架构约束、contract、ownership/state model 与关键设计裁定**。

当前项目仍处于 Stage 0，因此本目录目前**没有已批准的具体技术架构**。

03 曾生成的 `架构问题登记表` 因未经 Owner 审核，已降权移至：

- [`../discussion/架构问题登记表_未审核草案.md`](../discussion/架构问题登记表_未审核草案.md)

该草案不是 Architecture Authority，其中任何 `DECIDED` 状态在 Owner 复核前都不生效。

## 写入条件

新的架构文档进入本目录前必须同时满足：

1. 已有足够 Product / Reference / Spike evidence；
2. 已在聊天中向 Owner 展示关键问题、选项、推荐与风险；
3. Owner 已明确批准该内容进入项目架构事实源；
4. 文档状态与实际批准范围一致，不把未裁定部分混入 `DECIDED`。

如果只是 Working Draft / Proposal：

- 默认留在聊天；
- 只有 Owner 明确要求保存草案时，才进入 `../discussion/`。

## Canonical rule

- Product / Roadmap 当前状态由 `../current/` 拥有；
- 未审核提案由聊天或 `../discussion/` 承载；
- Reference evidence 由 `../research/` 拥有；
- Owner 正式裁定由 `../decisions/` 拥有；
- 代码与 runtime implementation fact 由 `zhangchenjia21-dot/Workbench` 拥有。

不要为了“以后可能复用”在真实 consumer 出现前提前冻结大型 platform / protocol / plugin architecture。
