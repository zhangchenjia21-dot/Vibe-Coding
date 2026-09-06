# Personal Workbench｜项目治理工作区

本目录是 **Personal Workbench 项目的规划、产品、架构、Reference Audit、正式裁定与 Current Status 事实源**。

代码、测试、构建、运行行为、实现级 Git 事实与 repository-native Task Packet 由 `zhangchenjia21-dot/Workbench` implementation repository 拥有。

当前项目处于：**Stage 0｜Product Direction Re-evaluation / Pivot Discovery**。

当前没有已批准的新 Product Definition。原 **AI Collaboration V0** 已因核心可行性风险进入 `HOLD` 并归档；`Personal OS / Life Operating System` 仅是当前 Product Discovery 的候选方向，尚未 Promotion 为 current。

## AI Start here

1. 先读仓库根 [`AGENTS.md`](../AGENTS.md) 获取全局 Authority / Freshness / Decision Propagation 规则。
2. 再读本目录 [`AGENTS.md`](AGENTS.md) 获取 Workbench 项目读写协议，特别是 **Owner Discussion & Promotion Gate**。
3. 当前项目理解只从 [`current/`](current/) 开始。
4. 未经 Owner 审核的 Product / Architecture / Roadmap 草案默认留在聊天；只有 Owner 明确要求保存草案时才进入 [`discussion/`](discussion/)。
5. 项目专属参考审计、技术研究与 Spike 证据进入 [`research/`](research/)。
6. 经 Owner 审核并明确提升的长期架构约束进入 [`architecture/`](architecture/)；正式 Owner 裁定进入 [`decisions/`](decisions/)。
7. 代码、测试、CI、runtime、正式 executable Task 与实现 Review 回到 `zhangchenjia21-dot/Workbench`。
8. 旧 AI Collaboration V0 仅在 [`../99_归档/workbench/AI协作路线_V0/`](../99_归档/workbench/AI协作路线_V0/) 中作为历史证据读取，不构成 current authority。

不要把旧 Product Definition、旧 Draft Task Axis、DeepSeek Harness 设想、历史聊天、参考图中的模块或当前候选 Personal OS 自动提升为 current decision。

## Repository map

| 路径 | 角色 | Authority |
|---|---|---|
| `current/` | 当前项目总纲、Stage / Gate / Goal / Blocker；未来经 Owner 批准的新 Product / Roadmap Owner | **当前项目入口** |
| `discussion/` | Owner 明确要求保存的未审核 / 待讨论提案 | **非 Authority / 不触发下游** |
| `research/` | Reference Audit、技术研究、可行性证据、Spike 结果 | 证据，不自动构成决策 |
| `architecture/` | 经 Owner 审核并明确提升的长期架构约束与设计 | 架构 Authority |
| `decisions/` | Owner 已批准的项目级正式裁定 | 决策 Authority |
| `../99_归档/workbench/` | superseded / hold / historical route | **历史证据，不参与 current** |

## Direction reset

2026-09-06，Owner 决定将原 AI Collaboration V0 从 active tree 撤出并归档，避免其 Primary Purpose、Scope、Task Axis 与架构假设误导后续开发。

这项决定只表示：

```text
AI Collaboration V0
→ HOLD / HISTORICAL
→ 不再是 current Product / Roadmap
```

它**不表示**：

```text
Personal OS
→ 已经批准
```

新方向仍需：

```text
Product Discovery
→ Owner explicit decision
→ New Product Definition
→ G0.1 / G0.2
→ Draft Task Axis
→ Reference Audit
→ Route Freeze
```

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
= product / planning / architecture / decisions / status / discussion / research evidence

zhangchenjia21-dot/Workbench
= code / tests / build / runtime / repository-native executable tasks / implementation review evidence

D:\AI\Projects\Workbench
= 当前 Owner 工作站的本地 implementation checkout；不是跨机器 contract
```

同一事实只能有一个 canonical Owner。

> Root is map; subfolders are depth.
