# Personal Workbench｜项目治理与 GPT 协作工作区

本目录是 **Personal Workbench 项目的规划、产品、架构、Reference Audit、正式裁定与 Current Status 事实源**。

代码、测试、构建、运行行为、实现级 Git 事实与 repository-native Task Packet 由 `zhangchenjia21-dot/Workbench` implementation repository 拥有。

当前项目处于：**Stage 0｜Pre-Implementation Alignment**。

## AI Start here

1. 先读仓库根 [`AGENTS.md`](../AGENTS.md) 获取全局 Authority / Freshness / Decision Propagation 规则。
2. 再读本目录 [`AGENTS.md`](AGENTS.md) 获取 Workbench 项目读写协议，特别是 **Owner Discussion & Promotion Gate**。
3. 当前项目理解从 [`current/`](current/) 开始。
4. 未经 Owner 审核的 Product / Architecture / Roadmap 草案默认留在聊天；只有 Owner 明确要求保存草案时才进入 [`discussion/`](discussion/)。
5. 尚未进入 Current / Roadmap 的未来产品机会统一进入 [`备选方案池.md`](备选方案池.md)，不得把 Idea Pool 条目自动提升为已批准 Scope。
6. 项目专属参考审计、技术研究与 Spike 证据进入 [`research/`](research/)。
7. 经 Owner 审核并明确提升的长期架构约束进入 [`architecture/`](architecture/)；正式 Owner 裁定进入 [`decisions/`](decisions/)。
8. 代码、测试、CI、runtime、正式 executable Task 与实现 Review 回到 `zhangchenjia21-dot/Workbench`。

不要把历史聊天、旧 Workbench 设想、DeepSeek Harness 方案、Idea Pool 条目、未经 Owner 审核的草案或某个 Agent 偏好自动提升为 current decision。

## Repository map

| 路径 | 角色 | Authority |
|---|---|---|
| `current/` | 已经进入当前项目路线的项目总纲、Stage / Gate / Product / Roadmap Owner | **当前项目入口；不得放未经 Owner 审核的新路线** |
| `discussion/` | Owner 明确要求保存的未审核 / 待讨论提案 | **非 Authority / 不触发下游** |
| `备选方案池.md` | 尚未进入正式 Scope / Roadmap 的未来产品机会与候选能力 | Idea Pool；**非 current / 非开发授权** |
| `research/` | Reference Audit、技术研究、Build/Fork/Extend/Rewrite 证据、Spike 结果 | 证据，不自动构成决策 |
| `architecture/` | 经 Owner 审核并明确提升的长期架构约束与设计 | 架构 Authority |
| `decisions/` | Owner 已批准的项目级正式裁定 | 决策 Authority |

历史 / superseded 项目资料应进入仓库根 `99_归档/workbench/`，不要在 active 目录长期并列 `final2/latest/new`。

## Discussion-first rule

对 Product / Architecture / Roadmap / Task Axis / Stage Gate 等重大内容，默认工作方式是：

```text
专业聊天形成草案
→ 先与 Owner 讨论
→ Owner 明确批准 / 修改 / 延后 / 否决
→ 才写入或提升到 GitHub authoritative source
→ 涉及 Gate 时再由 00 做 Gate Review
```

`DRAFT` 标签不能替代 Owner Approval；专业聊天框也不能自行宣布 Gate PASS。

## Project boundary

```text
Vibe-Coding/workbench
= product / planning / architecture / decisions / status / discussion / research evidence / future idea pool

zhangchenjia21-dot/Workbench
= code / tests / build / runtime / repository-native executable tasks / implementation review evidence

D:\AI\Projects\Workbench
= 当前 Owner 工作站的本地 implementation checkout；不是跨机器 contract
```

同一事实只能有一个 canonical Owner。治理资料需要实现事实时读取 implementation repository，而不是复制一份会过期的 HEAD / 测试 / runtime 状态。

> Root is map; subfolders are depth.
