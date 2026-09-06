# Personal Workbench｜项目治理工作区

本目录是 **Personal Workbench 项目的规划、产品、架构、Reference Audit、正式裁定与 Current Status 事实源**。

代码、测试、构建、运行行为、实现级 Git 事实与 repository-native Task Packet 由 `zhangchenjia21-dot/Workbench` implementation repository 拥有。

当前项目处于：

> **Stage 1｜TA-1A First Usable Implementation — Route Frozen / Authorized for Dispatch**

当前正式产品方向：

> **Personal State / Information & Action Desktop**

当前 CORE：

```text
Today
Plan
Tracks
```

当前 V0：

> **Thin Tracks + Usable Plan + Derived Today**

当前 frozen implementation foundation：

```text
Electron
+ SQLite
+ stable identity/date/recurrence contracts
+ local backup/restore/migration
+ system tray lifecycle
```

原 AI Collaboration V0 保持 `HOLD / HISTORICAL / FUTURE RE-DISCOVERY`。

## AI Start here

1. 先读仓库根 [`AGENTS.md`](../AGENTS.md) 获取全局 Authority / Freshness / Decision Propagation 规则。
2. 再读本目录 [`AGENTS.md`](AGENTS.md) 获取 Workbench 项目读写协议。
3. 当前项目理解从 [`current/`](current/) 开始。
4. Product Definition：[`current/产品定义.md`](current/产品定义.md)。
5. Frozen Route：[`current/开发路线.md`](current/开发路线.md)。
6. Frozen Architecture：[`architecture/架构方案.md`](architecture/架构方案.md)。
7. Current Stage / Implementation authorization：[`current/项目状态.md`](current/项目状态.md)。
8. Route Freeze Decision：[`decisions/D-003_V0RouteFreeze与TA-1AImplementationAuthorization.md`](decisions/D-003_V0RouteFreeze与TA-1AImplementationAuthorization.md)。
9. implementation code / tests / executable Task / Review evidence 回到 `zhangchenjia21-dot/Workbench`。

## Repository map

| 路径 | 角色 | Authority |
|---|---|---|
| `current/` | 当前 Product / frozen Route / Stage / Gate / Goal / Blocker | **当前项目入口** |
| `architecture/` | Frozen V0 Architecture 与 supporting questions | **架构 Authority** |
| `decisions/` | Owner 已批准的项目级正式裁定 | **决策 Authority** |
| `research/` | Reference Audit / Spike evidence | Evidence，不自动构成新 decision |
| `discussion/` | pre-approval / decision evidence | 非 Authority |
| `../99_归档/workbench/` | superseded / HOLD / historical route | 历史证据 |

## Current stage

```text
G0.1 = PASS
G0.2 = PASS
G0.3 = PASS
G0.4 = PASS
G0.5 = PASS
G0.6 Route Freeze = PASS
Implementation = AUTHORIZED FOR TA-1A DISPATCH
```

下一主线：**04｜Agent 任务调度**。

04 只应围绕 `TA-1A First Usable Core Vertical` 生成正式 executable Task Packet；TA-1B 仍由 TA-1A Independent Review + Owner real-data UAT gate 控制。

## Current implementation path

```text
04: TA-1A Task Packet / Dispatch
→ implementation
→ 05: Independent Review / Reality Gate
→ Owner real-data UAT
→ 00: TA-1A Stage decision
→ 若通过，再进入 TA-1B
```

Owner Windows notification-area / tray 鼠标 focused check 已明确后移为 **TA-1A Exit 前必须完成**，不再阻塞 Route Freeze。

## Discussion-first rule remains

Route Freeze 并不取消 Owner Discussion & Promotion Gate。

对 frozen Product / Architecture / Route 的重大修改，仍必须：

```text
Finding / Proposal
→ Owner Discussion
→ Owner explicit approval
→ GitHub Promotion / Gate
```

Implementation Agent 不得因为编码便利自行改变 frozen contracts 或扩大 Scope。

## Project boundary

```text
Vibe-Coding/workbench
= product / planning / architecture / decisions / status / research evidence

zhangchenjia21-dot/Workbench
= code / tests / build / runtime / repository-native executable tasks / implementation review evidence

D:\AI\Projects\Workbench
= Owner 当前工作站 checkout；不是跨机器 contract
```

> Root is map; subfolders are depth.
