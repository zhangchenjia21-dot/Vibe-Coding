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
G4 Primary Source Assets & Local Game Creation
                                      PASS / CLOSED
G4-10 Runtime Asset Resolution        DEFERRED / MOVED TO G6
G4-11 Two Primary Asset Families      PASS / CLOSED
G4-11C01 Narrative Voice Soft Prompt  PASS / CLOSED
G4-GATE                               PASS

G5 World Semantics & GM Runtime       ACTIVE
G5-01 Minimum Playable T0 + World Turn / Semantic Materialization
                                      ACTIVE
G5-01M1 Semantic Materialization Spine ACTIVE — CODEX
```

当前路线：

```text
G5-01M1 backend/runtime implementation
→ GPT Independent Review
→ short G5-01 Owner product checkpoint
→ G5-02 Knowledge Provenance
```

---

## G4 final product result

G4 已经证明：

```text
Managed Source Library
→ exact World / Character / optional Expansion selection
→ Atomic Final Create
→ independent Game-local Reality
→ real AI GM play
→ durable Save / reopen / Continue
→ multiple Games without cross-world leakage
```

Owner 已确认 Han/刘备 与 Afterglow/莉维娅实际玩起来是两个不同的 RPG 世界。

叙述文风仍有 convergence，但这只是 non-blocking quality finding。G4-11C01 已用一条通用 Host-level soft creative prompt 做最小修正，没有 style gate、关键词 validator 或 style-triggered retry；实际效果留到下一次合适 UAT 顺带观察。

Formal G4 closeout：

`my-world/docs/g4_11/G4_GATE_CLOSEOUT.md`

---

## G5-01｜现在真正开始让世界“留下变化”

Canonical decision：

`architecture/world/G5_WORLD_TURN_SEMANTIC_MATERIALIZATION_V0_1_DECISION.md`

第一版只解决：

> GM 在自由 Narrative 中已经明确发生的、未来值得记住的变化，怎样进入 durable Game Reality。

核心 ordering：

```text
Player action
→ free-form visible Narrative streaming
→ durable Conversation acceptance
→ separate best-effort semantic analysis
→ optional Program-owned World Turn
→ existing atomic world mutation / Timeline
→ committed matching changes can re-enter later Context
```

保护：

> **Narrative acceptance != semantic-analysis success.**

语义分析失败不会把已经接受的玩家行动改成失败，也不会让 Narrative 承担 JSON/header/sentinel 协议。

World Turn v0.1 只是 turn-level durable consequence ledger，不是万能 entity/fact graph。Knowledge、NPC/Faction Agency、Event Evolution 仍按 G5-02/03/04 由真实 consumer 逐步拉出。

---

## 第一代产品路径

```text
Managed Source Library
→ World Pack + Character Card + optional Expansion Pack
→ explicit Composition
→ atomic Final Create
→ independent Game-local Reality
→ real AI GM play
→ durable world evolution
→ Save / reopen / Continue
```

第一代不支持无 World / 无 Character 的空白 AI 世界直接建局。

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
> **Model authors the world; Runtime makes it durable; Player owns the timeline.**
>
> **Source provides inertia; actors create history.**
>
> **Context stays bounded, not starved.**

Current runtime ordering：

```text
Model Freedom First
+
Visible Narrative First
+
Canonical Commit Behind durable infrastructure boundaries
```

Long-term distinctions：

```text
Application Lifetime != Game Session Lifetime
Source Library != Game Library
Source stable identity != exact immutable generation
Source package total != selected T0 projection != Game-local Reality
Narrative != machine semantic protocol
portrait / scene / map image != gameplay semantic authority
map image != topology / travel / current location / GIS
```

---

## Visual work deferral

Portrait / scene / authored-map Runtime Resolution remains deferred to G6. G5 semantics must not wait for art assets.

Canonical decision：

`architecture/source/G4_VISUAL_ASSET_DEFERRAL_TO_G6_DECISION.md`

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
