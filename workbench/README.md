# Personal Workbench｜项目治理工作区

本目录是 **Personal Workbench 项目的规划、产品、架构、Reference Audit、正式裁定与 Current Status 事实源**。

代码、测试、构建、运行行为、实现级 Git 事实与 repository-native Task Packet 由 `zhangchenjia21-dot/Workbench` implementation repository 拥有。

当前项目处于：**Stage 0｜Pre-Implementation Alignment**。

当前正式产品方向已经由 Owner 批准：

> **Personal State / Information & Action Desktop**

当前 CORE：

```text
Today
Plan
Tracks
```

当前 V0：

> **Thin Tracks + Usable Plan + Derived Today**

原 AI Collaboration V0 保持 `HOLD / HISTORICAL / FUTURE RE-DISCOVERY`。

## AI Start here

1. 先读仓库根 [`AGENTS.md`](../AGENTS.md) 获取全局 Authority / Freshness / Decision Propagation 规则。
2. 再读本目录 [`AGENTS.md`](AGENTS.md) 获取 Workbench 项目读写协议，特别是 **Owner Discussion & Promotion Gate**。
3. 当前项目理解只从 [`current/`](current/) 开始。
4. Product Definition 读取 [`current/产品定义.md`](current/产品定义.md)。
5. 未经 Owner 审核的 Product / Architecture / Roadmap 草案默认留在聊天；只有 Owner 明确要求保存草案时才进入 [`discussion/`](discussion/)。
6. 项目专属 Reference Audit、技术研究与 Spike 证据进入 [`research/`](research/)。
7. 经 Owner 审核并明确提升的长期架构约束进入 [`architecture/`](architecture/)；正式 Owner 裁定进入 [`decisions/`](decisions/)。
8. 代码、测试、CI、runtime、正式 executable Task 与实现 Review 回到 `zhangchenjia21-dot/Workbench`。
9. 旧 AI Collaboration V0 仅在 [`../99_归档/workbench/AI协作路线_V0/`](../99_归档/workbench/AI协作路线_V0/) 中作为历史证据读取。

## Repository map

| 路径 | 角色 | Authority |
|---|---|---|
| `current/` | 当前项目总纲、Product Definition、Stage / Gate / Goal / Blocker | **当前项目入口** |
| `discussion/` | Owner 明确要求保存的 pre-approval / decision evidence | 非 Authority |
| `research/` | Reference Audit、技术研究、可行性证据、Spike 结果 | 证据，不自动构成决策 |
| `architecture/` | 经 Owner 审核并明确提升的长期架构约束与设计 | 架构 Authority |
| `decisions/` | Owner 已批准的项目级正式裁定 | 决策 Authority |
| `../99_归档/workbench/` | superseded / hold / historical route | 历史证据，不参与 current |

## Current product baseline

当前 Product Definition 的核心关系：

```text
Tracks
长期事项与当前真实状态
        ↓ 可选关联
Plan
未来安排 / Calendar / Reminder / Memo / Vector / Unscheduled
        ↓ 按今天过滤
Today
当前日期的信息切片 / 默认首页
```

产品原则：

- Workbench 自己拥有 Core Personal State；
- Today 是 derived view，不复制第二份 live truth；
- AI 不是 V0 成立前提；
- 系统帮助 Owner 看清与维护状态，不以监督 / 评分为核心；
- V0 不扩张成全功能 Personal OS；
- 桌面长期驻留 / 高频唤起是 Product Experience Requirement，但技术方案未冻结。

## Current stage

```text
Product Direction = APPROVED
G0.1 Product Baseline = PASS
G0.2 Scope = PASS
G0.3 Draft Task Axis = NEXT
Route Freeze = NOT PASS
Implementation = NOT AUTHORIZED
```

下一主线：`03｜架构与开发路线`。

03 必须先在聊天中形成 Architecture Questions Register + Draft Task Axis Working Draft，并与 Owner 讨论；未经 Owner 明确批准，不得把新路线 / 架构 Promote 到 authoritative GitHub source。

## Discussion-first rule

对 Product / Architecture / Roadmap / Task Axis / Stage Gate 等重大内容：

```text
专业聊天形成 Working Draft
→ 先与 Owner 讨论
→ Owner 明确批准 / 修改 / 延后 / 否决
→ 才写入或提升到 GitHub authoritative source
→ 涉及 Gate 时再由 00 做 Gate Review
```

`DRAFT` 标签不能替代 Owner Approval；专业聊天框不能自行宣布 Gate PASS。

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
