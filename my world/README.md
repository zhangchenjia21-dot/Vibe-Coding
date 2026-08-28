# my world｜项目治理入口

`my world` 是基于 SillyTavern / The World / DSH 多轮开发与长期真实试玩经验启动的独立单人 AI RPG。

## Start Here｜五份核心文档

默认只需要先读下面五份。专题文件只有任务真实涉及对应领域时再深入。

1. [`MY_WORLD_项目启动总纲_CURRENT.md`](./MY_WORLD_项目启动总纲_CURRENT.md)  
   **产品是什么、为什么做、核心体验、第一代建局方式与范围。**
2. [`MY_WORLD_核心设计原则_CURRENT.md`](./MY_WORLD_核心设计原则_CURRENT.md)  
   **跨阶段不能丢失的产品 / Runtime 原则。**
3. [`MY_WORLD_架构_CURRENT.md`](./MY_WORLD_架构_CURRENT.md)  
   **当前系统架构地图、Source/Game/Runtime 边界，以及专题设计导航。**
4. [`MY_WORLD_总体规划路线图_CURRENT.md`](./MY_WORLD_总体规划路线图_CURRENT.md)  
   **G1–G9 的先后顺序、Task DAG、Stage Gate 与为什么这样排。**
5. [`MY_WORLD_CURRENT_STATUS.md`](./MY_WORLD_CURRENT_STATUS.md)  
   **现在做到哪里、当前 Task、PASS / UAT / blocker。**

原则：

> **Root is map; subfolders are depth.**
>
> **顶层让人快速理解整个项目；专题深度下沉子目录。**

以后默认更新已有核心文档，不因每次讨论继续增加新的顶层 `*_CURRENT.md`。

---

## 当前状态

```text
G1 Foundation                         PASS / CLOSED
G2 AI Conversation Spine              PASS / CLOSED
G3 Persistent Game / Save / Timeline PASS / CLOSED
G3-GATE                               PASS
Current Phase                         G4 — Primary Source Assets & Local Game Creation
Current Task                          G4-01 — Application Shell / Main Menu + Game Session Lifecycle
G4-GATE                               NOT YET
```

G4 当前正式采用 **asset-only first-generation creation**：

```text
World Pack
+ Character Card
+ optional Expansion Pack
→ explicit Composition
→ atomic Final Create
→ independent Game-local Reality
```

第一代不支持无 World Pack / 无 Character Card / AI 空白世界直接建局。真实 World + Character 先做 First Playable A；通过后再加入真实 Expansion 做 First Playable B。

实时状态只以 [`MY_WORLD_CURRENT_STATUS.md`](./MY_WORLD_CURRENT_STATUS.md) 为准。

---

## 项目事实

```text
本地项目       D:\AI\Projects\my-world
实现仓库       zhangchenjia21-dot/my-world
治理仓库       zhangchenjia21-dot/Vibe-Coding
Godot          4.7.2 Standard / non-.NET Windows x64
第一代语言     GDScript
Runtime        Godot same-process Runtime
当前 Provider  DeepSeek deepseek-v4-pro
Persistence    SQLite via godot-sqlite v4.9
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
> **Save Point != Timeline Node.**
>
> **Source provides inertia; actors create history.**
>
> **Context stays bounded, not starved.**

G4 新增长期边界：

> **Application Lifetime != Game Session Lifetime.**
>
> **Source Library != Game Library.**
>
> **Source stable identity != exact immutable generation.**
>
> **Guaranteed NPC != Opening NPC != Player-known NPC.**
>
> **Expansion binding != real gameplay effect.**

---

## 第一代 New Game 路径

```text
Main Menu
→ New Game
→ Exactly 1 World Pack
→ Entry / T0
→ Expansion 0..N（可 none）
→ Exactly 1 Player Character Card
→ 0..N Guaranteed NPC Character Cards
→ Game name / Control Mode / optional opening supplement
→ Compatibility Review
→ Atomic Final Create
```

Character Card 是 reusable Character Source，不是“主角专用卡”。玩家选择某个 NPC Character Card，代表该角色从建局起属于本局 canonical cast，但不保证第一幕出现，也不代表玩家已经认识他。

---

## 架构专题

统一从 [`MY_WORLD_架构_CURRENT.md`](./MY_WORLD_架构_CURRENT.md) 导航。

```text
architecture/
├─ foundation/
│  └─ Foundation架构决策_v1.0_2026-08-26.md
├─ ui/
│  └─ 声明式UIHost设计.md
└─ persistence/
   └─ 时间线存档与可逆性设计.md
```

这些文件保存深度、trade-off 和阶段实施细节；它们不与顶层五份核心文档并列成为默认读取集。

---

## 经验 / 跨项目复用

```text
experience/
├─ DSH经验继承矩阵_v1.0_2026-08-25.md
├─ AI_RPG开发路径与阶段设计经验_v1.0_2026-08-28.md
└─ 备选开发方向候选池_2026-08-28.md
```

### `AI_RPG开发路径与阶段设计经验`

这是**未来开发类似 AI RPG 时优先参考的完整经验入口**。它已经把旧项目的重要开发步骤、返修模式、资产路线、Owner UAT、Independent Review、Source pinning、Final Create、Expansion、Creator、Living World 等经验收敛到 `my world`。

未来新项目不需要先拼读旧 SillyTavern / The World 的大量历史过程文档；旧文档继续承担历史证据角色。

### `DSH经验继承矩阵`

用于回答 The World / DSH 真实长局验证过什么、哪些宿主债务不能迁移。

### `备选开发方向候选池`

只保存当前**尚未授权**的能力和 revisit trigger。Character Card、Expansion Pack、Protagonist Control Mode、Managed Source Library、Multi-Game 已经进入 CURRENT，不再属于 Deferred。

经验文件不自动覆盖 current 产品与架构裁定，也不得因为“过去做过”自动生成 Task。

---

## 历史 / 归档

已关闭阶段过程、旧 handoff、Preflight、被 supersede 的 current 等，统一进入仓库根：

`99_归档/my world/`

Git history 继续承担完整版本历史。

---

## AI 读取规则

默认初始工作集：

```text
Vibe-Coding/AGENTS.md
+ 本目录五份核心文档中的任务相关最小集合
+ 实现仓库 AGENTS.md / 当前代码 / 当前 Task Packet
```

如果任务是：

- **规划新阶段 / 审计任务顺序 / 启动未来同类项目**：额外读 `experience/AI_RPG开发路径与阶段设计经验_v1.0_2026-08-28.md`；
- **讨论延后功能**：读 `experience/备选开发方向候选池_2026-08-28.md`；
- **涉及 DSH 长局经验**：读 `experience/DSH经验继承矩阵_v1.0_2026-08-25.md`。

不要默认“阅读整个治理目录”。只有 `MY_WORLD_架构_CURRENT.md` 指向某个专题且当前 Task 真正触及时，再进入对应 `architecture/` 或 `experience/` 文件。