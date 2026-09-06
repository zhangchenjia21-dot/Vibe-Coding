---
title: my world｜总体规划路线图
status: current-canonical-roadmap
version: 4.1
created: 2026-08-25
updated: 2026-09-06
current_phase: G6
current_status_source: MY_WORLD_CURRENT_STATUS.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜总体规划路线图 CURRENT

## 0. 文档职责

本文件拥有：

- G1–G9 阶段顺序；
- 每阶段核心 Outcome；
- 当前阶段的主要能力顺序；
- Stage Gate；
- Deferred / Non-scope；
- 为什么按这个顺序做。

实时 PASS / blocker / current owner 以 `MY_WORLD_CURRENT_STATUS.md` 为准。

总原则：

> **先跑通真实核心循环，再扩展外围能力。**
>
> **Vertical before platform. Consumer before infrastructure.**
>
> **真实需求 → 最小能力 → 第一消费者 → Owner UAT → 第二消费者 → 再抽象协议。**

---

## 1. 总体关键路径

```text
G1 Foundation & Project Bootstrap                PASS / CLOSED
↓
G2 AI Conversation Spine                         PASS / CLOSED
↓
G3 Persistent Game / Save / Timeline Foundation PASS / CLOSED
↓
G4 Primary Source Assets & Local Game Creation   PASS / CLOSED
↓
G5 World Semantics & GM Runtime                  PRODUCT PASS / CLOSED
↓
G6 RPG Experience & Internal Declarative UI Host ACTIVE
↓
G7 Long-session Context & Performance
↓
G8 Mod / Authoring & External Declarative UI Contract
↓
G9 Standalone Alpha / Release Validation
```

第一条产品脊柱已经成立：

```text
Launch
→ Main Menu
→ Continue / Asset-only New Game
→ AI GM free-form Narrative
→ Player natural-language action
→ durable world / actor consequences
→ Save / exit / reopen
→ Continue / Restore
→ coherent world + context recovery
→ world and actors continue to create history
```

---

# G1｜Foundation & Project Bootstrap

**PASS / CLOSED**

Outcome：Godot 4.7.2 / GDScript / same-process Runtime / Windows-local Host / real Provider streaming / local IO / Windows export 成立。

---

# G2｜AI Conversation Spine

**PASS / CLOSED**

Outcome：自然语言输入、真 streaming Narrative、多回合、Cancel / Regenerate / Retry / provider-failure recovery 成立。

Narrative 是主要游戏内容，不是状态摘要。

---

# G3｜Persistent Game / Save / Timeline Foundation

**PASS / CLOSED**

Outcome：One authoritative SQLite flow、atomic mutation、accepted Conversation durability、Timeline、Save、Restore、future-memory isolation、backup/recovery 成立。

长期区分：

```text
Game
World State
Timeline
Save Point
Conversation
Agent Context
UI Preference
```

---

# G4｜Primary Source Assets & Local Game Creation

**PASS / CLOSED**

Outcome：

```text
Managed Source Library
→ World Pack + Character Card + Expansion Pack
→ exact Composition
→ Atomic Final Create
→ independent Game-local Reality
→ multiple Games
→ real AI GM play
```

核心边界：

```text
Reusable immutable Source
!= selected T0 projection
!= Game-local lived reality
!= Runtime current state
```

主要闭环：Application Shell、Source v0.2-r2、Managed Library、Multi-Game、Asset-only New Game、Atomic Final Create、World+Character First Playable、Expansion vertical、Two-family reality 均已关闭。

Visual Runtime 曾在 G4 被正式移动到 G6，不是 G4 blocker。

---

# G5｜World Semantics & GM Runtime

**PRODUCT PASS / CLOSED — G5-GATE PASS**

Outcome：让 Source-grounded Game 真正“活起来”，同时避免构建全宇宙模拟器。

G5 已关闭能力包括：

```text
free-form Narrative → durable semantic consequences
World Truth != actor Knowledge != human-player disclosure
stable NPC independent agency
runtime-created actor materialization
event/priority World Evolution with valid hold
meaningful choice / mechanics integration
player-safe Runtime projection
living-world integrated reality matrix
```

保护原则：

> **Model authors the world; Runtime makes it durable; Player owns the timeline.**
>
> **Source provides inertia; actors create history.**
>
> **Persistent != fully simulated.**

---

# G6｜RPG Experience & Internal Declarative UI Host

## Outcome

把已经成立的 Runtime Truth 做成真正可读、可查询、可操作的 RPG 产品界面，并让真实 Surface / mechanic consumer 反向拉出 Host 与 visual capability。

G6 不是“把所有传统 RPG 页面一次做齐”，而是：

> **玩家问题先于页面数量；真实 Domain 先于空 Tab；重复 consumer 先于抽象 renderer。**

## G6 canonical order

```text
A. Runtime projection → ViewModel → first real UI consumer
B. Visual Runtime re-entry audit
C. portrait / scene / authored-map presentation when real demand exists
D. grounded real Surfaces + concrete Narrative interaction consumers
   - Character / Important Experiences / People
   - Five Recommended Actions (fixed first-party composer prefill consumer)
E. Expansion mechanic-state consumer
F. Internal Declarative UI Host v0.1
G. generic bounded Action Intent only after proven consumers
H. responsive / Theme / navigation / UI preference as actually justified
I. Owner UAT / visual polish
```

### G6-A｜First real consumer

**DONE — MW-011 PRODUCT PASS / CLOSED**

已成立：

```text
Player-safe projection
→ presentation-only RPG Host ViewModel
→ Player Host + World Overview
```

张琛 `player_profile` 证明 rich Character Source 可以通过独立 fail-closed player-facing projection 进入 UI，而不泄漏 raw `semantic_sections` / GM-private material。

MW-012 张琛 Character Card 已集成，当前 accepted generation 为 `0.1.1`。

### G6-B / C｜Visual Runtime re-entry

**AUDITED — IMPLEMENTATION DEFERRED**

当前 Player / Narrative / World 三个 Host 已有潜在 portrait / scene / map placement，但仍没有成熟第一方 authored visual demand。

因此：

```text
Runtime Asset Resolution implementation = DEFERRED
portrait / scene / authored-map implementation = DEFERRED
```

Re-entry trigger：Owner 提供/批准真实第一方视觉资产，或某个 Surface 的产品 Outcome 明确依赖 visual。

### G6-D｜Real Surfaces / Narrative interaction consumers

**CURRENT**

历史 The World 已验证的玩家 IA 作为 evidence 母版：

```text
概览 / 角色 / 人物 / 行囊 / 事务 / 系统 / 存档
```

`my world` 当前母版：

```text
概览 / 角色 / 重要经历 / 人物 / 事务 / 行囊 / 系统 / 地图 / 存档
```

当前已完成/集成：

```text
Character + Important Experiences
People（Engineering PASS / Owner UAT pending）
```

一个 Surface 只有同时满足以下条件才进入实现：

```text
real player question
+ real domain owner
+ player-safe projection
+ non-trivial product value
```

禁止为了“像 RPG”而创建 fake HP / location / inventory / faction / quest state 或空标签页。

#### MW-019｜Five Recommended Actions — CURRENT PRIORITY

Owner 于 2026-09-06 明确要求先实现推荐行动，再与 MW-018 一起做 Owner UAT。

Canonical product/architecture decision：

`architecture/ui/G6_FIVE_RECOMMENDED_ACTIONS_V1_0_DECISION.md`

产品目标：

```text
accepted GM Narrative
→ dedicated player-safe background Action Recommender
→ exactly five model-generated suggested actions
→ recommendation click PREFILLS existing composer
→ player may edit freely
→ normal Send / Ctrl+Enter path remains authoritative
```

保护边界：

```text
five recommendations != five allowed actions
free-form input always remains available
click != auto-send
recommendations are ephemeral derived UI, not Game truth
no hidden Runtime/actor/private data in recommendation input
recommendation failure never blocks play
```

MW-019 is a concrete fixed first-party consumer. It does **not** pull generic G6-G Action Intent infrastructure forward.

MW-018 remains Owner UAT pending; after MW-019 Engineering PASS/integration, one fresh Owner build should be used for combined UAT of People + Recommendations.

### G6-E｜Expansion mechanic-state consumer

在真实 durable mechanic state 已存在后，让玩家能查看当前机制状态。

原则继承 The World：

```text
no durable mechanic state
→ no fake System page
```

### G6-F｜Internal Declarative UI Host v0.1

`MW-013` 已完成 Task Shaping，但因路线复核被判定过早：

```text
MW-013 = HOLD / NOT AUTHORIZED YET
```

只有在多个真实 Surface / mechanic consumer 已经重复出现稳定组件模式后，才 re-authorize。

Internal Host 只来自 proven internal consumers；G8 之前不接受 World/Character/Expansion 外部 UI schema。

### G6-G｜Generic Bounded Action Intent

在 Host vocabulary 经过多个真实消费者验证后，再考虑通用受控 UI intent，例如 open surface / prefill composer / Save navigation。

MW-019 的“推荐按钮 → prefill composer”是固定第一方交互，可作为未来抽象证据，但不授权提前建设 generic intent schema / dispatcher。

不允许 arbitrary GDScript callback、NodePath execution、OS/filesystem command 或资产直接 mutation authoritative state。

### G6-H / I｜Responsive / Theme / Navigation / UAT

持续以 desktop 最大化为主体验，保留 normal/narrow regression；Narrative First != Narrative Only。

最终 Owner UAT 必须判断：

- 信息是否按玩家需求组织；
- Narrative 是否仍是重心；
- Side Surfaces 是否真的降低认知负担；
- 推荐行动是否提供灵感而没有把自由叙事变成选项制；
- UI 是否像游戏而不是工程 inspector；
- 没有为完整度制造假状态。

## G6-GATE（draft acceptance direction）

至少要求：

- Player Host / Narrative / World Host 的职责清晰；
- 至少若干真实 RPG Surface 投影真实 state，而非空壳；
- Save/Restore player experience 保持可靠；
- free-form player action remains primary even when recommendation guidance exists；
- real mechanic state 有合理 consumer；
- Internal Declarative Host 仅从 proven patterns 抽象；
- player-safe disclosure boundary 在 UI 层保持；
- responsive/navigation/theme 达到长期桌面游玩可接受水平；
- Owner 明确认为 UI materially improves RPG experience。

---

# G7｜Long-session Context & Performance

## Outcome

长局增长时，working set、UI responsiveness 与 background work 仍可控。

Tasks：

- bounded Context Assembly；
- relevant working-set / subgraph selection；
- background progression 与 foreground model work 分离；
- TTFT / throughput / context size / persistence latency 长局证据；
- Source / Game / history 增长时不线性塞满 Prompt；
- long-session recovery/performance reality test。

> **Bounded context != starved context.**

---

# G8｜Mod / Authoring & External Declarative UI Contract

## Outcome

在内部 Source / Runtime / UI consumers 已经证明后，再建立外部 authoring / extension contract。

Tasks：

- Source authoring/import workflow；
- Character / World / Expansion batch authoring tooling；
- external safe declarative UI vocabulary derived from G6；
- compatibility/versioning/migration；
- bounded extension capability；
- real third-party-like package tests。

不建设 arbitrary-code plugin platform。

---

# G9｜Standalone Alpha / Release Validation

## Outcome

把完整纵向整理为可独立安装、恢复、诊断的 Windows Alpha 产品。

Tasks：

- standalone packaging；
- onboarding / credentials / Source setup；
- upgrade / migration / recovery；
- long-play / corruption / reinstall reality tests；
- release UAT / defect closure；
- documentation / diagnostics / support boundary。

## G9-GATE

独立用户能安装、建局、长期游玩、保存恢复，并在真实失败后有可理解的恢复路径。

---

## Deferred / Non-scope reminders

继续不提前建设：

- multiplayer / cloud account / server dependency；
- 3D free-movement world；
- full-universe per-NPC tick simulator；
- universal ECS / giant EventBus；
- arbitrary external code execution；
- giant universal Source/UI schema；
- automatic map generation before authored-map evidence；
- Creator before real consumers；
- external declarative UI before G6 internal patterns prove out。

Current Task / Owner / PASS 始终以 `MY_WORLD_CURRENT_STATUS.md` 为准。
