---
title: my world｜当前状态
status: current-project-status
version: 4.0
created: 2026-08-26
updated: 2026-08-28
phase: G4 World Pack & Local Content Foundation
current_task: G4-01 World Pack v0.1
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
Current Task                          G4-01 — World Pack v0.1
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

G3-GATE 已证明：可靠 reopen/resume、Save/Load/Restore、future-memory isolation、误读档前 future Recovery、crash/interrupted-write correctness、single-writer、physical corruption recovery，以及 recovered durable history 上的真实 DeepSeek continuation。玩家标准恢复路径不需要理解 SQLite/WAL 或手工修文件。

明确 Deferred：任意 Turn 一键 rewind、Timeline browser、backup browser。

---

## 4. Current Phase｜G4 World Pack & Local Content Foundation

G4 Outcome：让产品不硬编码为一个世界，正式建立：

```text
Reusable Source
World Pack / Character / Expansion
↓ new Game materialization
Game-local Reality
↓ Runtime
Current Game World
```

核心边界：

> **Source 定义开始前的参考世界；游戏开始后，game-local reality 优先。**
>
> **Source provides inertia; actors create history.**

G4 Task DAG：

```text
G4-01 World Pack v0.1
↓
G4-02 Source → Game-local Instance
↓
G4-03 Pack Discovery / Install / Load
↓
G4-04 Asset Resolution
↓
G4-05 Second Pack Fixture
↓
G4-GATE
```

G4-GATE 要求至少两个 World Pack 能建立独立新 Game，Source / Instance 分离，Source 后续变化不得静默改写已有 Game。

---

## 5. Current Task｜G4-01 World Pack v0.1

Outcome：冻结并实现第一代**可复用 Source 内容包契约**，让 Runtime 能从显式本地 pack root 读取一个结构明确、安全、版本化的 World Pack definition；本任务不把 Source materialize 进当前 Game。

第一代 contract 只覆盖当前真实需要的 Source 能力：

```text
pack metadata / stable identity / contract version
world / GM instructions
ordered source lore entries
initial character source seeds
authored map declaration
portrait / scene / map asset declarations
necessary mechanic declarations
```

边界：

- `World Pack Source != Game-local Instance != Runtime World State`；
- G4-01 只定义/加载 Source；G4-02 才负责 Source → game-local materialization；
- 不冻结 G5 NPC/Faction/Knowledge/Relationship gameplay schema；initial character 只是 Source seed，不是完整 NPC runtime row；
- authored map 在 G4-01 只冻结 Source entry / asset reference，不冻结完整 geography/topology gameplay schema；
- asset declaration 不等于 G4-04 runtime Asset Resolution；
- mechanic declaration 不执行任意代码；第一代 World Pack 是 content/data package，不获得 Runtime/OS authority；
- 不定义外部声明式 UI schema；该能力仍延后到 G8；
- 不做 Pack discovery/install/catalog；该能力属于 G4-03；
- 不修改 G3 production persistence schema，除非出现明确 blocker 并先重开架构。

Execution Owner：**Grok Build**。当前 Codex quota 不作为阻塞；用户已授权后续使用 Grok Build 或 KimiCode。G4-01 偏 Source contract / cross-module semantics，因此优先 Grok Build；后续偏 Windows/Godot 实际装载与资源路径验证的任务可优先 KimiCode。

G4-01 为工程/契约任务，默认不要求 Owner UAT；Independent Review PASS 后才进入 G4-02。

---

## 6. 当前核心约束

- `Commodity Foundation, Owned Game Semantics.`
- `Model freedom first. Reversibility over prevention.`
- `Narrative richness over artificial brevity.`
- `Model authors the world; Runtime makes it durable; Player owns the timeline.`
- `Source provides inertia; actors create history.`
- `Off-screen != Inactive.`
- `World Truth != NPC Knowledge != Player Knowledge.`
- `Context stays bounded, not starved.`
- UI / Transcript / Prompt / Cache 不是 authoritative truth。
- Source 内容文本不做 Narrative censorship；硬校验只围绕 contract integrity、stable identity、safe local paths、version/readability 与禁止任意代码执行。

---

## 7. 当前 waiting

```text
Blocking: NONE KNOWN
Current: G4-01 repository-native Task Packet / implementation
Owner: Grok Build
G4-02: HOLD until G4-01 Independent Review PASS
G4-GATE: NOT YET
```
