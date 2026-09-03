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
G4-11P1 Engineering Reality Prep      PASS / CLOSED
G4-11UAT Owner Reality Test           PASS / CLOSED
G4-11C01 Narrative Voice Soft Prompt  ACTIVE — CODEX
G4-GATE                               HOLD — C01 engineering review only
```

当前路线：

```text
G4-11C01 soft prompt closeout
→ G4-GATE
→ G5-01 Minimum Playable T0 + World Turn / Semantic Materialization
```

Owner 已明确把 portrait / scene / authored-map runtime 接入延期到 G6；视觉资源不再是 G4-GATE 或 G5 的前置。

---

## G4-11 product result

Owner 已确认：

> **两套 Source 实际玩起来是两个不同的 RPG 世界。**

因此 Two-Family Reality Test 的核心产品价值已经 PASS。

剩余 finding：两个世界的叙述正文仍偏向同一种通用现代中文 RPG 旁白。

该 finding 被分类为 **non-blocking narrative-quality issue**。Owner 授权一次极小的 Host-level soft-prompt tuning，不改 Source，不增加 style gate，不单独再做一轮 Owner UAT；实际效果留到下一次合适的产品 UAT 顺带观察。

Canonical decision：

`architecture/foundation/G4_NARRATIVE_VOICE_SOFT_PROMPT_TUNING_DECISION.md`

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

G4-11C01 新增长期边界：

> **Narrative style is guidance, not an acceptance gate.**

G4/G5 长期区分：

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

## 下一阶段

C01 Independent Review PASS 后，正式关闭 G4 并进入 G5。

当前 roadmap 的第一个 G5 task 是：

```text
G5-01 Minimum Playable T0 + World Turn / Semantic Materialization
```

它不是直接建完整世界模拟器。第一步要解决的是：一次 Narrative Turn 中真正发生、且未来回合必须记住的变化，怎样成为 durable Game Reality。GPT 必须先冻结 semantic / ownership / acceptance 边界，再交给实现 Agent。

---

## 架构专题

统一从 [`MY_WORLD_架构_CURRENT.md`](./MY_WORLD_架构_CURRENT.md) 导航。专题文档保存 contract / trade-off / migration 深度，不与顶层 current 文件争夺 authority。

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
