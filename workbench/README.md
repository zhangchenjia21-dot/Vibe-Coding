# Personal Workbench｜项目治理与 GPT 协作工作区

本目录是 **Personal Workbench 项目的规划、产品、架构、Reference Audit、正式裁定与 Current Status 事实源**。

代码、测试、构建、运行行为、实现级 Git 事实与 repository-native Task Packet 由 `zhangchenjia21-dot/Workbench` implementation repository 拥有。

当前项目处于：**Stage 0｜Pre-Implementation Alignment**。

## AI Start here

1. 先读仓库根 [`AGENTS.md`](../AGENTS.md) 获取全局 Authority / Freshness / Decision Propagation 规则。
2. 再读本目录 [`AGENTS.md`](AGENTS.md) 获取 Workbench 项目读写协议。
3. 当前项目理解从 [`current/`](current/) 开始。
4. 项目专属参考审计、技术研究与 Spike 证据进入 [`research/`](research/)。
5. 已形成的长期架构约束进入 [`architecture/`](architecture/)；正式 Owner 裁定进入 [`decisions/`](decisions/)。
6. 代码、测试、CI、runtime、正式 executable Task 与实现 Review 回到 `zhangchenjia21-dot/Workbench`。

不要把历史聊天、旧 Workbench 设想、DeepSeek Harness 方案或某个 Agent 偏好自动提升为 current decision。

## Repository map

| 路径 | 角色 | Authority |
|---|---|---|
| `current/` | 项目总纲、当前 Stage / Gate / Goal / Blocker，以及后续 Product Definition / Roadmap 路由 | **当前项目入口** |
| `research/` | Reference Audit、技术研究、Build/Fork/Extend/Rewrite 证据、Spike 结果 | 证据，不自动构成决策 |
| `architecture/` | 已批准或明确标记状态的长期架构约束与设计 | 架构 Authority（仅对已批准内容） |
| `decisions/` | Owner 已批准的项目级正式裁定 | 决策 Authority |

历史 / superseded 项目资料应进入仓库根 `99_归档/workbench/`，不要在 active 目录长期并列 `final2/latest/new`。

## Project boundary

```text
Vibe-Coding/workbench
= product / planning / architecture / decisions / status / research evidence

zhangchenjia21-dot/Workbench
= code / tests / build / runtime / repository-native executable tasks / implementation review evidence

D:\AI\Projects\Workbench
= 当前 Owner 工作站的本地 implementation checkout；不是跨机器 contract
```

同一事实只能有一个 canonical Owner。治理资料需要实现事实时读取 implementation repository，而不是复制一份会过期的 HEAD / 测试 / runtime 状态。

> Root is map; subfolders are depth.
