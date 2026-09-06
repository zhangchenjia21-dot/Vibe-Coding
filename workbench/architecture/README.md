# Personal Workbench｜Architecture

本目录只保存 **已经经过 Owner 讨论并明确提升的跨阶段长期架构约束、contract、ownership/state model 与关键设计裁定**。

当前项目处于 **Stage 0｜Product Direction Re-evaluation / Pivot Discovery**，因此本目录目前**没有已批准的具体技术架构**。

原 AI Collaboration V0 的 Architecture Questions Register 已随旧路线归档至：

`Vibe-Coding/99_归档/workbench/AI协作路线_V0/架构问题登记表_未审核草案_v0.1.md`

该文件只作历史证据，不是当前 Architecture Authority。

## 写入条件

新的架构文档进入本目录前必须同时满足：

1. 已有 Owner-approved Product Baseline；
2. 已有足够 Product / Reference / Spike evidence；
3. 已在聊天中向 Owner 展示关键问题、选项、推荐与风险；
4. Owner 已明确批准该内容进入项目架构事实源；
5. 文档状态与实际批准范围一致，不把未裁定部分混入 `DECIDED`。

如果只是 Working Draft / Proposal：

- 默认留在聊天；
- 只有 Owner 明确要求保存草案时，才进入 `../discussion/`。

## Canonical rule

- Product / Roadmap 当前状态由 `../current/` 拥有；
- 未审核提案由聊天或 `../discussion/` 承载；
- Reference evidence 由 `../research/` 拥有；
- Owner 正式裁定由 `../decisions/` 拥有；
- 代码与 runtime implementation fact 由 `zhangchenjia21-dot/Workbench` 拥有；
- `99_归档/workbench/` 只提供历史证据，不参与 current authority。

新产品方向未冻结前，不提前继承旧路线的 state model、process ownership、ChatGPT host、DSH 或 Agent contract。
