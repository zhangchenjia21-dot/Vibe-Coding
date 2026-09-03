---
title: my world｜总体规划路线图
status: current-canonical-roadmap
version: 3.5
created: 2026-08-25
updated: 2026-09-03
current_phase: G5
current_status_source: MY_WORLD_CURRENT_STATUS.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜总体规划路线图 CURRENT

## 0. 文档职责

本文件拥有：

- G1–G9 阶段顺序；
- 每阶段核心 Outcome；
- 主要 Task DAG；
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
G5 World Semantics & GM Runtime                  ACTIVE
↓
G6 RPG Experience & Internal Declarative UI Host
↓
G7 Long-session Context & Performance
↓
G8 Mod / Authoring & External Declarative UI Contract
↓
G9 Standalone Alpha / Release Validation
```

第一条真正产品脊柱：

```text
启动应用
→ Main Menu
→ Continue / Asset-only New Game
→ AI GM 自然语言互动
→ 世界产生 durable change
→ Save / exit / reopen
→ Continue / Load / Restore
→ 世界 + Context 一致恢复
→ 世界与行动者继续产生新的当前现实
```

---

# G1｜Foundation & Project Bootstrap

**PASS / CLOSED**。

Outcome：证明 Godot 4.7.2 / GDScript / same-process Runtime / Windows-local Host / real Provider streaming / local IO / Windows export 可行。

---

# G2｜AI Conversation Spine

**PASS / CLOSED — G2-GATE PASS**。

Outcome：

```text
自然语言输入
→ AI GM 真 streaming Narrative
→ 多回合
→ Cancel / Regenerate / Retry / correction
→ Provider failure 后可继续
```

Narrative 是主要游戏内容，不是状态摘要。

---

# G3｜Persistent Game / Save / Timeline Foundation

**PASS / CLOSED — G3-GATE PASS**。

Outcome：建立 durable backbone：One authoritative SQLite flow、atomic world mutation、accepted Conversation durability、Timeline、named Save、Restore、future-memory isolation、backup/recovery。

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

**PASS / CLOSED — G4-GATE PASS**。

G4 把产品升级为真正的本地多世界 AI RPG Host：

```text
Managed Source Library
→ World Pack + Character Card + optional Expansion Pack
→ explicit exact Composition
→ Atomic Final Create
→ independent Game-local Reality
→ real AI GM play
→ Save / reopen / Continue
→ multiple Games without leakage
```

核心边界：

```text
Reusable immutable Source
!= Selected T0 Source Projection
!= Game-local Canonical Reality
!= Runtime State
```

### G4 accepted chain

```text
G4-01 Application Shell / Lifecycle   PASS / CLOSED
G4-02R1 Source semantic re-audit      PASS / CLOSED
G4-03 Managed Local Source Library    PASS / CLOSED
G4-04 Multi-Game / Game Library       PASS / CLOSED
G4-05 Asset-only New Game Wizard      PASS / CLOSED
G4-06 Atomic Final Create             PASS / CLOSED
G4-07 First Playable A                PASS / CLOSED
G4-08 Expansion Pack v0.1             PASS / CLOSED
G4-09 First Playable B                PASS / CLOSED
G4-11 Two Primary Asset Families      PASS / CLOSED
G4-11C01 Narrative Voice Soft Prompt  PASS / CLOSED
```

G4-11 Owner confirmed Han/刘备 and Afterglow/莉维娅 materially feel like different RPG worlds. Cross-world prose voice convergence remains a quality observation only; C01 added one generic Host-level soft creative instruction with no style gate.

### G4 visual deferral

```text
G4-10 Runtime Asset Resolution
DEFERRED / MOVED TO G6
```

Owner decided current portrait / scene / authored-map assets are not mature and are not part of the present core experience.

Visual runtime is therefore **not** a G4-GATE or G5 prerequisite.

Protected distinction:

```text
portrait / scene / map image
!= gameplay semantic authority

map image
!= topology / travel / current location / GIS
```

Formal closeout:

`my-world/docs/g4_11/G4_GATE_CLOSEOUT.md`

---

# G5｜World Semantics & GM Runtime

## Outcome

让 G4 创建出的 Source-grounded Game 真正“活起来”，但不把 AI RPG 变成一个僵硬的全宇宙模拟器。

核心原则：

> **Model authors the world; Runtime makes it durable; Player owns the timeline.**

以及：

```text
Model Freedom First
+
Visible Narrative First
+
Canonical Commit Behind durable infrastructure boundaries
```

G5 semantics 不等待美术资源。

## G5 task order

```text
G5-01 Minimum Playable T0 + World Turn / Semantic Materialization  PASS / CLOSED
↓
G5-02 Knowledge Provenance                                    ACTIVE
↓
G5-03 NPC / Faction Agency
↓
G5-04 Event / Priority-driven World Evolution
↓
G5-05 Meaningful Choice / Mechanics Integration
↓
G5-06 Runtime → UI Projection
↓
G5-07 World Product Tests
↓
G5-GATE
```

---

## G5-01｜Minimum Playable T0 + World Turn / Semantic Materialization

**PASS / CLOSED**。

Canonical decision：

`architecture/world/G5_WORLD_TURN_SEMANTIC_MATERIALIZATION_V0_1_DECISION.md`

Implementation：

`my-world/docs/tasks/G5-01M1_WORLD_TURN_SEMANTIC_MATERIALIZATION_TASK.md`

Closeout：

`my-world/docs/g5_01/G5-01_CLOSEOUT.md`

### Outcome

第一次让 free-form Narrative 中已经明确成立的世界变化进入 durable Game Reality：

```text
Player action
→ free-form visible GM Narrative
→ durable Conversation acceptance
→ separate best-effort semantic analysis
→ 0..N newly-established durable consequences
→ Program-owned World Turn
→ existing atomic world mutation / Timeline
→ committed matching changes may re-enter later Context
```

Protected distinction：

> **Narrative acceptance != semantic-analysis success.**

G5-01 没有为了结构化世界语义重新把 Narrative 变成 JSON / sentinel / parser protocol。

Semantic analysis 是独立 machine lane；malformed / transport / empty analysis fail-soft，不把已经接受的玩家行动判失败，也不创建 fake world mutation。

World Turn v0.1 是 turn-level durable consequence ledger，不是 permanent universal ontology。

Owner 在 Engineering PASS 后明确选择跳过额外的专门 G5-01 UAT：此前实际游玩已经给了“世界会记住玩家选择后果”的足够产品信心，因此直接推进。此前 dedicated real-provider vertical 因 Kimi K3 timeout 未能完成，历史证据状态不被伪造为 PASS，但不再作为额外 progression gate。

`G5-01M1C02 Restore Timeline Isolation` 已取消；它针对 Restore 后相同 turn index + byte-identical GM Narrative hash 的极窄 exact-replay edge，等待未来真实 branch/result-reuse consumer 再裁定。

---

## G5-02｜Knowledge Provenance

**ACTIVE**。

Canonical decision：

`architecture/world/G5_KNOWLEDGE_PROVENANCE_V0_1_DECISION.md`

First implementation：

`my-world/docs/tasks/G5-02M1_KNOWN_ACTOR_KNOWLEDGE_PROVENANCE_TASK.md`

### Outcome

正式建立：

```text
World / Game Truth
!= Actor Knowledge
!= Human-player disclosure
!= Omniscient GM model Context
```

核心原则：

> **GM omniscience must not become actor omniscience.**

当前 Source 的 `disclosure: gm_reference` 只表示 GM/reference visibility，不能被解释成“所有 NPC 都知道这些信息”。

### G5-02M1 first slice

只让**游戏开始后新产生的知识获取**变成 durable provenance，而且仅覆盖已经有稳定 Game-local identity 的：

```text
Player Character
+
Guaranteed NPCs
```

不把所有 T0 Source 转成 Knowledge Graph，不提前建立 incidental/emergent NPC identity，也不建立 Faction/shared knowledge。

G5-02M1 复用 G5-01 每回合已有的一次后台 semantic-analysis request；不增加第二次 Provider tax。概念响应：

```json
{
  "changes": ["durable world consequence"],
  "knowledge_events": [
    {
      "knower_id": "stable-local-character-id",
      "fact": "actor now has grounds to know this post-T0 fact",
      "basis": "witnessed|told|discovered|participated"
    }
  ]
}
```

Knowledge failure 不能拖累 G5-01：

```text
valid changes
+ absent/invalid knowledge data
→ valid changes still eligible to commit
→ knowledge fails soft
```

Knowledge provenance 使用既有 world snapshot / atomic mutation，不新增 SQLite schema/table。

第一 consumer 是 later GM Context：GM 仍可看 broad World truth，但额外拿到 bounded Actor Knowledge Provenance，提醒它不能让角色仅因为 GM 知道就自动知道 post-T0 事实。

这是 soft semantic guidance，不是 Narrative output classifier/gate。

Explicitly deferred：

- universal Entity/Knowledge Graph；
- false belief / deception resolution；
- confidence/reliability scoring；
- rumor propagation；
- Faction/shared knowledge；
- emergent NPC identity platform；
- separate Player Knowledge UI；
- long-session retrieval platform。

---

## G5-03｜NPC / Faction Agency

Outcome：NPC / Faction 不再只是玩家触发器，而能依据当前目标、资源、知识、关系、风险与世界压力采取行动。

> **Source provides inertia; actors create history.**

Guaranteed NPC 可成为真正行动者，但不自动等于 opening appearance / player-known / current Context membership。

---

## G5-04｜Event / Priority-driven World Evolution

Outcome：世界可在选择性、相关、可控的范围内演化；Player 不再是唯一因果源。

不要模拟每个 NPC 每分钟的全部生活。优先由真实 pressure / priority / event consumer 拉出最小世界推进能力。

---

## G5-05｜Meaningful Choice / Mechanics Integration

Outcome：把已证明的 Expansion mechanic/world semantics 接入 living-world consequences，而不重新打开 Public d20 已关闭的协议设计。

---

## G5-06｜Runtime → UI Projection

Outcome：把已成立的 Runtime truth 投影成 player-safe consumer，为 G6 RPG Experience 拉出真实 UI 需求。

不在这里预造完整 G6 visual host。

---

## G5-07｜World Product Tests

至少包括：

```text
Player Absence
Counterfactual Propagation
Independent Actor
Knowledge Boundary
Save/Restore world consistency
```

Engineering evidence 不能代替 Owner 对“世界是否真的活起来”的产品判断。

## G5-GATE

至少要求：

- free-form Narrative 能产生 durable world consequences；
- 世界语义在 Save/Restore/Continue 后一致；
- World Truth / actor knowledge / Player knowledge 有真实边界；
- NPC/Faction 能成为独立行动者；
- 世界可以选择性地在 Player 之外演化；
- Expansion mechanics 能进入世界语义而不是孤立 UI 特效；
- 没有发展成全宇宙模拟器或模型格式 gate 森林。

---

# G6｜RPG Experience & Internal Declarative UI Host

## Outcome

把已成立的 Runtime Truth 做成真正的 RPG 产品界面，并让**真实 presentation consumer 拉出视觉资源能力**。

Recommended order：

- Runtime projection → ViewModel → real UI consumer；
- re-audit + implement Runtime Asset Resolution only for actual visual consumers；
- portrait / scene / authored-map presentation；
- Character / Relationship / Inventory / Faction / Map / Save 等真实 Surface；
- Expansion mechanic state consumer；
- Internal Declarative UI Host v0.1；
- bounded Action Intent；
- responsive / Theme / navigation；
- Owner UAT / visual polish。

G6 re-entry 时重新裁定旧 Game 是否允许 presentation-only override 使用后来补充的视觉，而不改变 semantic Source ancestry。

---

# G7｜Long-session Context & Performance

## Outcome

长局增长时，working set、UI responsiveness 与 background work 仍可控。

Tasks：

- bounded Context Assembly；
- relevant subgraph / working-set selection；
- background progression 与 model work 分离；
- TTFT / throughput / context size / persistence latency 真实长局证据；
- Source / Game semantics / history 增长时不线性塞满 Prompt；
- long-session recovery/performance test。

> **Bounded context != starved context.**

---

# G8｜Mod / Authoring & External Declarative UI Contract

## Outcome

在内部 Runtime/UI consumer 已证明后，再建立 authoring / external extension contract。

Tasks：

- Source authoring/import workflow；
- safe external declarative UI vocabulary from proven internal Host；
- compatibility/versioning/migration；
- bounded extension capabilities；
- real third-party-like package tests。

不建设 arbitrary-code plugin platform。

---

# G9｜Standalone Alpha / Release Validation

## Outcome

把完整纵向整理为可独立安装、恢复、诊断的 Alpha 产品。

Tasks：

- standalone Windows packaging；
- onboarding / credentials / Source setup；
- upgrade/migration/recovery；
- long-play / corruption / reinstall reality tests；
- release UAT / defect closure；
- documentation / diagnostics / support boundary。

## G9-GATE

独立用户能安装、建局、长期游玩、保存恢复，并在真实失败后有可理解的恢复路径。
