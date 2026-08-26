# my world｜项目治理入口

`my world` 是基于 SillyTavern / The World / DSH 多轮开发与长期真实试玩经验启动的独立单人 AI RPG。

## Start Here｜五份核心文档

默认只需要先读下面五份。专题文件只有任务真实涉及对应领域时再深入。

1. [`MY_WORLD_项目启动总纲_CURRENT.md`](./MY_WORLD_项目启动总纲_CURRENT.md)  
   **产品是什么、为什么做、核心体验与范围。**
2. [`MY_WORLD_核心设计原则_CURRENT.md`](./MY_WORLD_核心设计原则_CURRENT.md)  
   **跨阶段不能丢失的产品 / Runtime 原则。**
3. [`MY_WORLD_架构_CURRENT.md`](./MY_WORLD_架构_CURRENT.md)  
   **当前系统架构地图，以及所有专题设计导航。**
4. [`MY_WORLD_总体规划路线图_CURRENT.md`](./MY_WORLD_总体规划路线图_CURRENT.md)  
   **G1–G9 的先后顺序、Task DAG 与 Stage Gate。**
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
Phase       G2 — AI Conversation Spine
G2-01       PASS — Owner UAT
G2-02       PASS — Engineering
Current     G2-03 — Narrative Conversation View
G2-GATE     NOT YET
```

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
> **Reversibility ≠ frictionless arbitrary rewind.**
>
> **Model authors the world; Runtime makes it durable; Player owns the timeline.**
>
> **Save Point != Timeline Node.**
>
> **Source provides inertia; actors create history.**
>
> **Context stays bounded.**

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

## 经验资料

```text
experience/
└─ DSH经验继承矩阵_v1.0_2026-08-25.md
```

经验文件用于回答“过去验证过什么 / 踩过什么坑”，不自动覆盖 current 产品与架构裁定。

---

## 历史 / 归档

已关闭阶段过程、旧 handoff、Preflight、被 supersede 的 current 等，统一进入仓库根的：

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

不要默认“阅读整个治理目录”。只有 `MY_WORLD_架构_CURRENT.md` 指向某个专题且当前 Task 真正触及时，再进入对应 `architecture/` 或 `experience/` 文件。
