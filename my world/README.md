# my world｜项目治理入口

`my world` 是独立、local-first、single-player-first 的长期 AI RPG 项目。

实时 Task / PASS / UAT 以 [`MY_WORLD_CURRENT_STATUS.md`](./MY_WORLD_CURRENT_STATUS.md) 为准。

## Start Here

1. [`MY_WORLD_项目启动总纲_CURRENT.md`](./MY_WORLD_项目启动总纲_CURRENT.md) — 为什么做、核心体验、产品边界
2. [`MY_WORLD_核心设计原则_CURRENT.md`](./MY_WORLD_核心设计原则_CURRENT.md) — 跨阶段长期原则
3. [`MY_WORLD_架构_CURRENT.md`](./MY_WORLD_架构_CURRENT.md) — 当前系统地图与 owner / boundary
4. [`MY_WORLD_总体规划路线图_CURRENT.md`](./MY_WORLD_总体规划路线图_CURRENT.md) — G1–G9 阶段路线
5. [`MY_WORLD_CURRENT_STATUS.md`](./MY_WORLD_CURRENT_STATUS.md) — 当前真实状态 / active work
6. [`AGENT_EXECUTION_ROUTING_CURRENT.md`](./AGENT_EXECUTION_ROUTING_CURRENT.md) — Codex / KimiCode / GPT 当前分工

> **Root is map; subfolders are depth.**

## 功能参考审计（非正式路线 / 不授权实现）

[原版 SillyTavern 功能参考审计、可选方向与取舍](./experience/SILLYTAVERN_UPSTREAM_FUNCTIONAL_REFERENCE_AUDIT_2026-09-06.md) — 固定原版 release 源码版本，提炼功能体验、适配边界与未来候选；不复制代码、架构或插件宿主。参考候选须经 Owner 讨论后才能进入正式路线；不改变 MW-019，也不替代 MW-018 / MW-019 的组合 UAT。

## 当前阶段

```text
G1 Foundation                               PASS / CLOSED
G2 AI Conversation Spine                    PASS / CLOSED
G3 Persistence / Save / Timeline            PASS / CLOSED
G4 Primary Source Assets & Local Game       PASS / CLOSED
G5 World Semantics & GM Runtime             PRODUCT PASS / CLOSED
G5-GATE                                     PRODUCT PASS

G6 RPG Experience & Internal Declarative UI Host ACTIVE
```

G6 当前已经完成：

```text
MW-011 RPG Host / Player Profile             PRODUCT PASS / CLOSED
MW-012 Zhang Chen Character Card             ENGINEERING PASS / INTEGRATED
Visual Runtime re-entry audit                DONE — implementation DEFERRED
```

当前正在做的不是新的 coding implementation，而是：

```text
G6 Surface / Information Architecture Audit
→ Owner + GPT discussion
```

讨论 Draft：

`architecture/ui/G6_SURFACE_INFORMATION_ARCHITECTURE_DRAFT_V0_1.md`

## 当前 G6 路线

Canonical 顺序：

```text
Runtime projection / ViewModel / first real consumer   DONE
→ Visual Runtime re-entry audit                        DONE / DEFER IMPLEMENTATION
→ real RPG Surfaces / information architecture         CURRENT
→ Expansion mechanic-state consumer
→ Internal Declarative UI Host v0.1
→ bounded Action Intent
→ responsive / Theme / navigation
→ Owner UAT / visual polish
```

`MW-013 Internal Declarative UI Host v0.1` 曾被过早调度，现正式：

```text
MW-013 = HOLD / NOT AUTHORIZED YET
```

先让真实 `角色 / 人物 / 其它 grounded Surface` 拉出重复 UI 模式，再抽象 Declarative Host。

## 三栏长期骨架

```text
Player Host | Narrative Host | World Surface Host
```

当前产品方向：

```text
Player Host
→ 我是谁？我现在怎么样？
→ 长期趋向高频、紧凑 HUD

Narrative Host
→ 现在发生了什么？我接下来想做什么？
→ 永远是局内视觉与交互重心

World Surface Host
→ 这个世界有哪些值得主动查看的信息？
→ 概览 / 角色 / 人物 / 行囊 / 事务 / 系统 / 地图 / 存档 等候选
→ 只在真实 Domain + player-safe projection 成立后出现
```

历史 `The World` 的已验证经验继续作为 evidence：

> **Workspace is organized for truth maintenance; UI is organized for player decisions.**

## 长期核心原则速览

> **Model Freedom First.**
>
> **Visible Narrative First.**
>
> **Narrative richness over artificial brevity.**
>
> **Model authors the world; Runtime makes it durable; Player owns the timeline.**
>
> **Source provides inertia; actors create history.**
>
> **World Truth != actor Knowledge != human-player disclosure.**
>
> **UI is a projection, not a second truth source.**
>
> **Vertical before platform. Consumer before infrastructure.**
>
> **Context stays bounded, not starved.**

## 当前 Agent 路由

```text
GPT
→ product semantics / architecture / Task Shaping / assignment / Independent Review

Codex
→ high-complexity / architecture-critical / high-blast-radius implementation

KimiCode
→ bounded frontend/UI/interaction / ordinary surfaces / content tooling / tests

Owner
→ Product UAT / explicit product verdict
```

不再默认把新任务交给 Zcode。具体以 `AGENT_EXECUTION_ROUTING_CURRENT.md` 为准。
