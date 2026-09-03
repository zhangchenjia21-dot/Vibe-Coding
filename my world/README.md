# my world｜项目治理入口

`my world` 是基于 SillyTavern / The World / DSH 多轮开发与长期真实试玩经验启动的独立单人 AI RPG。

## Start Here｜五份核心文档

默认先读：

1. [`MY_WORLD_项目启动总纲_CURRENT.md`](./MY_WORLD_项目启动总纲_CURRENT.md) — 产品是什么、为什么做、核心体验；
2. [`MY_WORLD_核心设计原则_CURRENT.md`](./MY_WORLD_核心设计原则_CURRENT.md) — 跨阶段原则；
3. [`MY_WORLD_架构_CURRENT.md`](./MY_WORLD_架构_CURRENT.md) — Source / Game / Runtime ownership；
4. [`MY_WORLD_总体规划路线图_CURRENT.md`](./MY_WORLD_总体规划路线图_CURRENT.md) — G1–G9 顺序与 Stage Gate；
5. [`MY_WORLD_CURRENT_STATUS.md`](./MY_WORLD_CURRENT_STATUS.md) — 当前 Task / PASS / blocker。

> **Root is map; subfolders are depth.**

实时执行状态始终以 `MY_WORLD_CURRENT_STATUS.md` 为准。

---

## 当前状态

```text
G1 Foundation                         PASS / CLOSED
G2 AI Conversation Spine              PASS / CLOSED
G3 Persistent Game / Save / Timeline PASS / CLOSED
G4-01 ... G4-09                       PASS / CLOSED
G4-10 Runtime Asset Resolution        DEFERRED / MOVED TO G6
G4-10M1                               SUPERSEDED / DO NOT EXECUTE
G4-11 Two Primary Asset Families      ACTIVE
G4-11P1 Engineering Reality Prep      PASS / CLOSED
G4-11UAT Owner Reality Test           ACTIVE — OWNER
G4-GATE                               NOT YET
```

当前路线：

```text
G4-11UAT Owner Two-Family Reality Test
→ G4-GATE
→ G5 World Semantics & GM Runtime
```

Owner 已明确把 portrait / scene / authored-map runtime 接入延期到 G6；视觉资源不再是 G4-GATE 或 G5 的前置。

---

## 第一代产品路径

```text
Managed Source Library
→ World Pack + Character Card + optional Expansion Pack
→ explicit Composition
→ atomic Final Create
→ independent Game-local Reality
→ real AI GM play
→ durable Save / reopen / Continue
```

第一代不支持无 World / 无 Character 的空白 AI 世界直接建局。

Character Card 是 reusable Character Source，不是“主角专用卡”。Guaranteed NPC 从 Final Create 起属于 canonical cast，但不自动等于 opening appearance / player-known / relationship / current Context membership。

---

## 当前 G4-11 Reality Test

最终 G4 reality pressure 使用两套真实 full-fidelity Source：

```text
历史 / 低魔
汉末三国：天下未定
208｜赤壁前夕
刘备

高魔 / 幻想
埃瑟维亚：诸界余辉
t0-1287-ovista
莉维娅·塞兰
```

两边 `Expansion = none`。G4-11P1 已用同一个实际 selected runtime model profile 跑通两套 real-provider 工程纵向、Save/reopen 和 A→B→A 隔离。

当前只剩 Owner Product UAT。最终 Gate 不是“数据库里两个 asset_id 不同”，而是 Owner 实际游玩后明显感觉：

> **这是两个不同的 RPG 世界。**

视觉 polish 不属于这次验收。

---

## 项目事实

```text
实现仓库       zhangchenjia21-dot/my-world
治理仓库       zhangchenjia21-dot/Vibe-Coding
Host           Godot 4.7.2 Standard / non-.NET Windows x64
Language       GDScript
Runtime        Godot same-process Runtime
Provider       Runtime Model Settings v0.1 selected profile
Persistence    SQLite via godot-sqlite v4.9
Game topology  One Game = One SQLite
```

---

## 核心原则速览

> **迁移经验，不迁移宿主债务。**
>
> **Commodity Foundation, Owned Game Semantics.**
>
> **Engine-native, not engine-semantic-coupled.**
>
> **Model freedom first. Reversibility over prevention.**
>
> **Narrative richness over artificial brevity.**
>
> **Source provides inertia; actors create history.**
>
> **Context stays bounded, not starved.**

当前 runtime ordering：

```text
Model Freedom First
+
Visible Narrative First
+
Canonical Commit Behind a Turn Finalize Barrier
```

G4/G5 新增长期边界：

```text
Application Lifetime != Game Session Lifetime
Source Library != Game Library
Source stable identity != exact immutable generation
Source package total != selected T0 projection != Game-local Reality
portrait / scene / map image != gameplay semantic authority
map image != topology / travel / current location / GIS
```

---

## Visual work deferral

Canonical decision：

`architecture/source/G4_VISUAL_ASSET_DEFERRAL_TO_G6_DECISION.md`

现在不建设：

- portrait / scene / authored-map resolver；
- image generation/editing pipeline；
- map topology / travel / GIS；
- visual authoring UI。

G6 在真实 RPG presentation consumer 存在后重新审计并实现所需的 visual runtime seam。

---

## 架构专题

统一从 [`MY_WORLD_架构_CURRENT.md`](./MY_WORLD_架构_CURRENT.md) 导航。专题文档保存 contract / trade-off / migration 深度，不与顶层 current 文件争夺 authority。

Source 相关 current decisions 位于：

`architecture/source/`

Persistence / UI / foundation 等专题分别位于对应 `architecture/` 子目录。

---

## AI 读取规则

默认初始工作集：

```text
Vibe-Coding/AGENTS.md
+ 本目录五份核心文档中的任务相关最小集合
+ 实现仓库 AGENTS.md
+ 当前 Task Packet
+ 当前代码 / 测试事实
```

不要默认阅读整个治理目录。只有 current Architecture / Roadmap / Task 真正指向某专题时，再进入对应深度文件。
