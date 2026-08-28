---
title: my world｜当前状态
status: current-project-status
version: 4.1
created: 2026-08-26
updated: 2026-08-28
phase: G4 World Pack & Local Content Foundation
current_task: G4-01 Product Entry Shell / Main Menu
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜CURRENT STATUS

## 1. 文档职责

本文件只拥有当前执行状态：Current Phase、Current Task、已完成 Gate、Owner UAT 结论、当前 blocker / waiting。

其它事实 owner：

- 产品目的：`MY_WORLD_项目启动总纲_CURRENT.md`
- 跨阶段原则：`MY_WORLD_核心设计原则_CURRENT.md`
- 架构地图：`MY_WORLD_架构_CURRENT.md`
- 阶段 / Task DAG：`MY_WORLD_总体规划路线图_CURRENT.md`

---

## 2. 当前状态

```text
G1 Foundation                         PASS / CLOSED
G2 AI Conversation Spine              PASS / CLOSED
G2-GATE                               PASS
G3 Persistent Game / Save / Timeline PASS / CLOSED
G3-GATE                               PASS

Current Phase                         G4 — World Pack & Local Content Foundation
Current Task                          G4-01 — Product Entry Shell / Main Menu
G4-GATE                               NOT YET
```

---

## 3. G3｜PASS / CLOSED

G3-01..G3-07 已完成对应 Engineering / Independent Review / Owner UAT。G3-07 Owner UAT（2026-08-28）：**PASS**。

G3-GATE：**PASS**。

第一代 persistence backbone 已成立：

```text
SQLite authoritative persistence
+ atomic durable World/Timeline mutation
+ accepted Conversation durability
+ current Game reopen/resume
+ named Save / atomic Load / Restore
+ future-memory isolation
+ internal Recovery Checkpoint / reciprocal Recover
+ immutable internal Timeline branching
+ single-writer process safety
+ SQLite-native verified physical backup
+ staged corruption recovery / quarantine
+ real Provider continuation after Restore / Recover
```

最终 Reality Test / evidence line：

```text
4529338728e7db91a2ce73b4dc8eec21c5530d0e  G3-07 reality test + central recovery action
dbc6167598ecbde3578778e638e2494bffc48244  G3-07 IR-01 real Provider B-marker evidence repair
```

明确 Deferred：任意 Turn 一键 rewind、Timeline browser、backup browser。

---

## 4. G4 Route｜已按 Owner 审核更新

历史仓库回溯与 Owner 审核后，G4 不再从 Source contract 直接开始。原因：World Pack 选择、创建新 Game、继续 Game 与多 Game 切换都需要稳定的 Application-level 产品入口；不应先做临时 Pack selector，再在后续重做正式主菜单。

新 G4 Task DAG：

```text
G4-01 Product Entry Shell / Main Menu
↓
G4-02 World Pack Source v0.1 + Contract Reality Check
↓
G4-03 Game Creation Composition v0.1 + New Game Flow
↓
G4-04 Source → Game-local Instance
↓
G4-05 Local Pack Library + Minimal Game Library
↓
G4-06 Asset Resolution
↓
G4-07 Two-Pack Playable Reality Test
↓
G4-GATE
```

旧 `docs/tasks/G4-01_WORLD_PACK_V0_1_TASK.md` **在执行前 superseded**。不得把该旧任务包发送给 Grok Build/KimiCode；World Pack Source 工作已顺延为 G4-02，并新增 lightweight Entry/T0 seed 与 two-shape Contract Reality Check。

---

## 5. Current Task｜G4-01 Product Entry Shell / Main Menu

Outcome：把 G2/G3 已有“直接进入游戏”的应用壳升级为第一代真正玩家主菜单，使后续 New Game / World Pack / multi-Game 有稳定产品入口。

第一代产品路径：

```text
Launch
→ Main Menu
├─ Continue current Game
├─ New Game
│  └─ reserved creation surface / host
└─ Quit
```

同时必须支持从正在玩的 Game 安全返回 Main Menu，并能再次 Continue。

G4-01 主要边界：

- 不实现真实 Pack discovery/selection；G4-03/G4-05 再接入；
- 不修改/重建 G3 persistence semantics；Continue 必须复用已关闭的 reopen/resume truth；
- startup corruption / safe-backup recovery 不能因 Main Menu 被隐藏或降级；
- `New Game` 先建立稳定 creation surface/host，不用临时测试窗口替代正式产品入口；
- 不做 Settings framework、在线账号、云同步、商店；
- UI 仍只是 lifecycle intent / projection，不成为 Game truth；
- Windows Maximized、1280×720、960×540 必须真实验证。

由于本任务是 Godot UI + Windows 本地生命周期/导航工作，执行优先使用 **KimiCode K3**；G4-02 Source contract/cross-module semantics 再优先考虑 **Grok Build**。Agent 变化不降低 Independent Review / Windows evidence 标准。

G4-01 是 product-facing UI task：Engineering / Independent Review 通过后建议安排 Owner UAT，重点验证“启动 → 主菜单 → Continue → 返回主菜单”的自然性与恢复按钮可发现性。

---

## 6. G4 已批准的新架构结论

### Main Menu first

先建立正式应用入口，再把 World Pack / New Game 选择接进去；不创建临时 Pack selector 作为产品主路径。

### World Pack Entry / T0 seed

G4-02 World Pack v0.1 支持 optional lightweight `Entry Point`：stable entry identity + display name + authored source seed/text。它不是 Opening Scenario 状态机，也不冻结通用时间/场景/Beat DSL。

### Game Creation Composition

新 Game 不是“选了 Pack 就结束”。G4-03 明确区分：

```text
World Pack Source Generation
+ selected Entry
+ protagonist seed
→ Game Creation Composition
→ Game-local Reality
```

历史上真实使用过的 world profile、Expansion/mechanic selection、protagonist control mode 记录为候选，不默认进入 v0.1。

### Exact Source provenance

Game-local reality 至少知道创建时使用的 pack stable identity、author version、exact content fingerprint/generation 与 selected Entry。Source 后续更新不得静默改写旧 Game。

Runtime 新生成实体允许 `runtime_generated` provenance，不要求一切都来自 Source。

### Multi-Game becomes a G4 need

G3 的 one-current-Game 是有意的第一代范围；G4 现在需要多个 Pack 建立多个独立 Game，因此 G4-05 才正式升级 Local Game Library / switching，不回头把 G3 当失败重做。

### Two-pack real product proof

G4 最终验证不再只是 second fixture parse PASS；G4-07 必须用两个差异明显的 World Pack，真实 New Game、真实 DeepSeek、durable progression、reopen/switch、Source isolation，并由 Owner UAT 判断是否真的像两个不同世界。

---

## 7. 当前核心约束

- `Commodity Foundation, Owned Game Semantics.`
- `Model freedom first. Reversibility over prevention.`
- `Narrative richness over artificial brevity.`
- `Model authors the world; Runtime makes it durable; Player owns the timeline.`
- `Source provides inertia; actors create history.`
- `Off-screen != Inactive.`
- `World Truth != NPC Knowledge != Player Knowledge.`
- `Context stays bounded, not starved.`
- UI / Transcript / Prompt / Cache 不是 authoritative truth。
- `World Pack Source != Game Creation Composition != Game-local Reality != Runtime State`。
- Source character exists/materialized != Player knows character。
- 不因为旧项目曾经实现某能力就自动带入当前 G4；deferred 候选见 `experience/备选开发方向候选池_2026-08-28.md`。

---

## 8. 当前 waiting

```text
Blocking: NONE KNOWN
Current: G4-01 Product Entry Shell / Main Menu
Recommended Owner: KimiCode K3
Formal G4-01 Task Packet: TO BE REISSUED for the new route
Old G4-01 World Pack Task Packet: SUPERSEDED BEFORE EXECUTION
G4-02: HOLD until G4-01 closes
G4-GATE: NOT YET
```
