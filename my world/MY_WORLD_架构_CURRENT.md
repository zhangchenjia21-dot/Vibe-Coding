---
title: my world｜架构总览
status: current-canonical-architecture-map
version: 1.1
created: 2026-08-26
updated: 2026-08-26
current_phase: G2
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜架构 CURRENT

## 0. 文档职责

本文件拥有 `my world` 的**当前架构地图与专题导航**。

它回答：

> **整个系统现在如何分层、核心边界是什么、需要深入某个领域时去哪里读。**

它不重复产品总纲、不重复阶段路线图，也不承载每个专题的全部 trade-off。详细设计下沉到 `architecture/`；历史经验下沉到 `experience/`；当前 Task / PASS / UAT 只看 `MY_WORLD_CURRENT_STATUS.md`。

正式文档结构原则：

> **Root is map; subfolders are depth.**
>
> **顶层负责快速理解全局，专题目录负责深入。**

---

## 1. 产品 / 系统四层

`my world` 的宏观架构：

```text
RPG Experience Layer
玩家看到、阅读和操作的游戏体验
↓
The World Runtime
Game / World / Timeline / Save / Conversation / NPC / Faction /
Knowledge / Relationship / Agent Context / World Evolution
↓
Engine Adapter
把领域语义连接到 Godot 的 UI / IO / Network / Assets / Lifecycle
↓
Godot / Mature Game Foundation
Window / Control / 2D / Input / Font / Image / Audio / Packaging / Debug
```

原则：

> **Commodity Foundation, Owned Game Semantics.**
>
> **Engine-native, not engine-semantic-coupled.**

因此：

```text
World != SceneTree
NPC != Node
Save != Resource dump
Timeline != Scene history
```

Godot 提供成熟通用能力；`my world` 自己拥有游戏语义。

---

## 2. 第一代 Foundation 冻结结果

当前第一代采用：

```text
Host            Godot 4.7.2
Distribution    Standard / non-.NET Windows x64
Language        GDScript
Runtime         Godot same-process Runtime
Provider        DeepSeek deepseek-v4-pro
Persistence     JSON/files for config/source + SQLite as G3 preferred evaluation candidate
```

同时保持：

- Domain 不依赖 Scene / Node / Resource 生命周期；
- Provider Adapter 保持极薄；
- Persistence 与 Domain / UI 分离；
- UI、Transcript、Markdown、Godot Resource 不作为 authoritative gameplay database；
- 当前不建设 IPC、通用 Provider 平台或通用 persistence framework。

详细证据与 trade-off：

`architecture/foundation/Foundation架构决策_v1.0_2026-08-26.md`

---

## 3. Runtime 真相与世界演化

长期事实模型：

```text
Reusable Source Assets
World Pack / Character / Expansion
↓ new game / bind
Game-local Canonical Assets
↓ current runtime
Runtime State
```

原则：

- Source 提供 T0 前的世界材料与惯性；
- 开局后 game-local reality 权威；
- 动态 NPC / 地点 / 物品 / 长期事实可以被模型创造并进入本局现实；
- durable 内容需要 stable identity / provenance；
- UI 只投影世界真相，不拥有第二份真相。

世界演化原则：

> **Source provides inertia; actors create history.**
>
> **Off-screen != Inactive.**

玩家不是唯一历史创造者；NPC / Faction / 环境也要形成自己的因果。但持久世界不等于“每个 NPC 每 tick 调一次模型”。

---

## 4. Model / Runtime 边界

当前核心方向：

> **Model freedom first. Reversibility over prevention.**
>
> **Model authors the world; Runtime makes it durable; Player owns the timeline.**

模型可以广泛 author Narrative、NPC 行为、事件、新实体与开放式后果。

Runtime 的强职责集中在：

- stable identity / references；
- atomic durability；
- Save / Restore / Timeline 技术正确性；
- filesystem / database integrity；
- secrets / OS authority / irreversible external side effects；
- reliable state recovery。

Runtime 不是 Narrative 审查器。普通模型 / 游戏语义错误优先通过更好 Context、Cancel、Regenerate、Retry、明确 Save/Restore 等方式恢复，而不是无限增加 Regex / Confirmation / whitelist / validator。

---

## 5. Context 架构

必须保持：

```text
Asset Library
!= Game Enabled Set
!= Runtime Relevant Set
!= Model Visible Working Set
```

以及：

```text
Runtime Relevant != Model Visible
```

目标：

```text
Game State / Event History ↑↑↑
ordinary Turn model context ≈ bounded
```

能由 Runtime 确定性处理的 timer / cooldown / bookkeeping / simple progression 不需要调用模型。

---

## 6. UI Host 架构

第一代桌面长期骨架：

```text
Player Host | Narrative Host | World Surface Host
左主角栏    | 中央叙事/输入   | 右世界信息栏
```

含义：

```text
Left
→ 主角立绘 / 身份 / 高频状态 / 属性摘要

Center
→ AI GM Narrative / 玩家自然语言输入 / streaming / low-risk turn actions

Right
→ 概览 / 人物 / 关系 / 任务 / 物品 / 地图 / Save / Timeline / extensions
```

Center 永远是视觉和交互重心，但：

> **Narrative First != Narrative Only.**

左右 Host 不是装饰边条，必须拥有足以承载真实 RPG 信息的可用宽度。

### 6.1 宽屏伸缩

宽屏 / 最大化窗口下，三个 Host **都参与横向扩张**，不能只让 Narrative 吃掉新增宽度。

第一代 UAT 基线目标约为：

```text
Player Host      ~18%
Narrative Host   ~60%
World Host       ~22%
```

这是布局调优基线，不是长期不可修改的像素合同。右侧通常可略宽于左侧，因为 World Surface 的信息密度更高。

同时保留最低可用宽度，第一版建议量级：

```text
Player Host min  ~250 px
World Host min   ~310 px
Narrative        使用剩余弹性空间并保持最大份额
```

具体值可由 Owner UAT 微调；原则是不允许侧 Host 被压缩成无法承载文字 / 卡片 / 列表的信息细条。

### 6.2 响应式折叠

响应式不是固定 breakpoint 崇拜，而是由三 Host 的最低可用空间决定：

```text
space sufficient
→ three hosts visible and proportionally expandable

space insufficient
→ Narrative remains primary
→ Player / World collapse, hide, drawer or overlay
```

因此：

> **先保证 Host 可用性；放不下时折叠，不靠无限压窄侧栏维持三栏。**

### 6.3 默认桌面启动形态

G2 起默认玩家启动应优先使用 **Maximized Window**，而不是 Exclusive Fullscreen：

- 更接近长期桌面游玩状态；
- 方便测试真实宽屏信息架构；
- 保留 Windows 标题栏、Alt+Tab、最小化与还原。

回归基线继续保留：

```text
Maximized desktop  → primary Owner UAT
1280x720           → normal windowed regression
960x540            → narrow responsive regression
```

### 6.4 Narrative 可读宽度

`NarrativeHost` 可以很宽，但长篇正文不应无限拉长单行长度。

后续/当前低成本可实现时，正文列应拥有独立的 readable-width 约束并在 Host 内合理居中；Host 多余空间未来可承载场景、立绘、Narrative contextual UI 与氛围表现，而不是只把文字一行拉得越来越长。

演化顺序：

```text
G2    fixed Godot UI + stable Host Slots
↓
G3–G5 real Domain / player-safe projections
↓
G6    Internal Declarative UI Host
↓
G8    external World Pack / Mod declarative UI contract
```

原则：

> **Definition declares what should be expressed; Host owns how it is rendered.**
>
> **Host capability first; external asset protocol second.**

详细设计：

`architecture/ui/声明式UIHost设计.md`

---

## 7. Save / Timeline / Reversibility 架构

必须区分：

```text
Cancel / Regenerate
= 高频、低风险、靠近 Narrative

Save / Load
= 明确玩家意图、长期恢复点

Timeline
= 首先是 Runtime 内部历史 / 恢复基础设施
```

核心不变量：

> **Reversibility ≠ frictionless arbitrary rewind.**
>
> **Save Point != Timeline Node.**

因此第一代不默认把每个历史 Turn 暴露成一个 `回到这里` 按钮；arbitrary per-turn rewind 当前为 Deferred。

G3 优先证明：可靠当前游戏 → reopen/resume → explicit Save → explicit Load/Restore → Context future isolation → 误读档后的恢复能力。

详细设计：

`architecture/persistence/时间线存档与可逆性设计.md`

---

## 8. 业务模块内部 L3 → L0

宏观四层之外，业务模块内部使用另一套依赖分层：

```text
L3 外交层
↓
L2 流程层
↓
L1 器件层
↓
L0 公理层
```

规则：

- 向下跳层允许；
- 向上依赖禁止；
- 跨业务模块只通过对方公开 L3；
- Bootstrap 是 composition root；
- 不为形式完整创建空层、空类、空 interface 或事件总线。

这套 L0–L3 与“Experience / Runtime / Adapter / Godot”四层不是同一个概念。

---

## 9. 专题架构导航

只有任务真实触及对应领域时才深入读取。

```text
architecture/
├─ foundation/
│  └─ Foundation架构决策_v1.0_2026-08-26.md
├─ ui/
│  └─ 声明式UIHost设计.md
└─ persistence/
   └─ 时间线存档与可逆性设计.md

experience/
└─ DSH经验继承矩阵_v1.0_2026-08-25.md
```

后续新增专题也优先放入现有领域目录，而不是继续增加顶层 `*_CURRENT.md`。

---

## 10. 架构文档维护规则

默认：

```text
新架构事实
→ 更新本 MY_WORLD_架构_CURRENT.md 的当前结论
→ 如果需要深度，再更新对应 architecture/<domain>/ supporting doc
```

只有当一个主题满足至少一个条件时才新建 supporting doc：

- 内容足够复杂，会明显污染架构总览；
- 可以独立演化；
- 会被多个 Task 重复引用；
- 必须保存详细 trade-off / contract / migration / evidence。

同一个事实不要在多份文件中人工维护成多套正文。

当前任务与 Gate 状态只看：

`MY_WORLD_CURRENT_STATUS.md`

开发顺序只看：

`MY_WORLD_总体规划路线图_CURRENT.md`

产品目的只看：

`MY_WORLD_项目启动总纲_CURRENT.md`

跨阶段原则只看：

`MY_WORLD_核心设计原则_CURRENT.md`
