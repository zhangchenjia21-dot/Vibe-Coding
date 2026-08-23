---
title: 酒馆游戏｜核心游玩重构 Turn 与 World Initiative 架构规格
status: current-architecture-frozen
version: 1.0
updated: 2026-08-23
project: 酒馆游戏新版主体
canonical_product_spec: 核心游玩重构_产品与架构总纲_CURRENT.md
---

# 酒馆游戏｜核心游玩重构 Turn 与 World Initiative 架构规格 v1.0

## 0. Architecture Survey Verdict

本轮审计结论：现有 Runtime 的主要问题不是缺一个提示词，而是正式主干天然假设：

```text
非空 Player Input
→ Semantic Candidate
→ Player Authorization
→ Program Outcome
→ Continuity / Narrative
→ Formal Turn Commit
```

并且：

- Runtime Materialization Request 强绑定 `playerInput + RuntimeMaterializationNeed`；
- 当前 need 主要服务 `place→move` / `character→dialogue`；
- Background Progression 只在 Program 认定玩家动作推进时间时触发；
- CommitFormalTurnRequest 强制携带 rawPlayerInput；
- Narrative Safe Outcome 只允许既有玩家安全工作集中的 concrete interactables；
- Runtime AGENTS 现行表述“只实现已提交 outcome”在旧链上被解释得过窄。

因此：

> World Initiative 必须进入正式回合主干，而不是作为旧玩家授权链之后的外围插件。

Architecture Survey = PASS。

---

## 1. 保留的底层基础

以下基础继续复用，不重写第二套：

1. SQLite Runtime Store 与 revision/CAS；
2. Formal Turn、Save / Restore / Branch；
3. Durable Execution / exactly-once Recovery；
4. Game-local Canonical Asset Layer；
5. existing dynamic Character / Place / Scene / Connection materialization primitives；
6. FormalDelta / Program Resolution；
7. player-safe projection / hidden disclosure boundary；
8. Source Asset != Game-local Canonical Assets != Runtime State；
9. Runtime Host / Product Session rails。

重构目标是改变 authority composition 和 turn orchestration，不是推翻已经证明的持久化、恢复和资产隔离。

---

## 2. Authority Model v2

### A. Player Initiative Authority

只管理“玩家本人做了什么”。

链：

```text
Player Input
→ Semantic Interpretation
→ Player Agency / Capability Validation
→ Resolution if needed
→ Player Formal Delta
```

仍禁止模型：

- 替玩家移动；
- 替玩家答应；
- 替玩家说未输入的话；
- 替玩家作不可逆选择。

### B. World Initiative Authority

管理“世界 / NPC 现在发生什么”。

链：

```text
Current World Frame
+ accepted Player Outcome（若有）
+ unresolved situations / recent consequences
↓
Model World Development Proposal
↓
Program World Validation
↓
World Formal Delta / Materialization / Canonical Patch
```

World Proposal 不需要玩家 raw input evidence，也不经过 Player Authorization。

### C. System / Rule Authority

Program deterministic rules、time、timers、RNG / contested resolution 继续保持现有 Owner。

---

## 3. Turn 主干重构

### 3.1 普通玩家回合

目标流程：

```text
Player Input
↓
1. Interpret Player Intent
↓
2. Validate Player Agency / Capability
↓
3. Resolve Player Action
↓
4. Build Provisional Post-player World Frame
↓
5. Model proposes World Development / NPC Reaction / Hook
↓
6. Validate World Proposal
   - anchors
   - identity
   - locality / topology
   - information boundary
   - no player-agency takeover
   - domain / rule constraints
↓
7. Materialize / Patch Game-local Canonical Assets as needed
↓
8. Compose one atomic commit
   Player Delta
   + World Delta
   + Materialization / Patch
   + Continuity
↓
9. Narrative realizes committed durable facts
   + free ephemeral scene texture
↓
10. Player-safe Session exposes new situation / entities / consequences
```

一个玩家输入仍最多形成一个 Formal Turn。World Reaction 默认与该 Formal Turn 原子提交，不另造第二个玩家回合。

### 3.2 T0 Opening

T0 不允许伪造玩家输入。

新增显式 Opening Initiative：

```text
Game created / first playable session
↓
World Opening Proposal
↓
Program validation
↓
optional materialization / local canonical patch / opening situation
↓
non-turn authoritative commit
↓
Opening Narrative
```

Opening commit 可以推进 revision，但不增加 turnNumber。现有 revision 已正式允许独立于 turnNumber 单调推进，因此不需要重新把 revision 等同为回合数。

Opening 必须 idempotent / exactly-once；重复进入 Session 不得重复生成第二份开局。

### 3.3 Wave 1 不做异步自由跑世界

首轮不实现玩家离线期间实时模拟、后台定时模型调用或无输入自动连续多步世界推进。

World Initiative 首轮触发点：

- T0 Opening；
- 每个成功 Formal Turn 的 post-outcome world reaction；
- 玩家显式 Wait / 时间推进时允许更强世界发展。

这样先证明“世界会主动发展”，不引入无界后台自主系统。

---

## 4. World Development Proposal 语义

不在本规格冻结最终 JSON 字段名，但语义至少支持：

### 4.1 Beat / Situation

- arrival / encounter；
- request / invitation；
- threat / pressure；
- opportunity；
- message / clue；
- reveal；
- consequence / reaction；
- follow-up / escalation；
- quiet continuation（允许无强事件）。

World Initiative 不是强制每回合制造转折；模型可以返回“无新 durable beat”，仅叙事延续。

### 4.2 Existing Entity Actions

可引用当前 canonical NPC / place / item，让其：

- 移动；
- 到达 / 离开；
- 发起互动；
- 推进已有 commitment / situation；
- 造成需要 Program Resolution 的争议行为。

### 4.3 Dynamic Materialization

World Proposal 可以请求创建：

- Character；
- Place / Scene / Connection；
- 后续再按需要扩 Item。

不再要求新实体被当前 Player Candidate 精确引用。

但一旦实体要持续存在或可交互，必须在 Narrative 将其作为 durable reality 使用前同回合物化。

### 4.4 Canonical Patch

允许对 Game-local evolvable fields 做 bounded patch；不得修改 Source、immutable anchor 或 Program-owned identity。

---

## 5. Active Situation｜最小核心玩法原语

引入轻量 Runtime `Active Situation` 语义，避免“钩子只存在一段文字里”。

它不是传统 Quest，也不强制玩家接受目标。

语义至少包括：

```text
stable situation identity
player-safe summary
status = active | changed | resolved
origin = opening | world_reaction | consequence
created / updated revision
optional related refs
```

用途：

- 给玩家明确“现在世界里有什么值得回应”；
- 给下一回合 World Initiative 一个可持续对象；
- 让选择能改变 / 解决 / 升级局势；
- 为 Product UI 的目标 / Journal / contextual presentation 提供统一上游。

首轮同时活跃的 Situation 应保持很少，默认 1–3，不建设任务清单游戏。

---

## 6. Narrative Freedom v2

### 6.1 Narrative 可自由 author 的内容

无需 formal commit：

- 环境气氛；
- 感官细节；
- 非持续背景人物；
- 无后续依赖的细节；
- NPC 的即时语气 / 表情 /动作表现（不改变 durable state 时）。

### 6.2 Narrative 必须使用 committed reality 的内容

以下必须已在本回合 World/Player commit 或既有 canonical truth 中：

- 玩家完成的行动；
- durable NPC / place / item identity；
- 位置变化；
- 资源 / state 变化；
- commitments；
- durable clues / knowledge；
- contested outcome；
- 会成为后续因果依据的重大事件。

因此旧 No Phantom 收窄为：

```text
No Phantom Player Action
No Phantom Durable Consequence
No Phantom Interactable Identity
```

而不是“禁止 Narrative 创造任何新细节”。

---

## 7. 现有接口的迁移方向

### 7.1 `FormalTurnSubmissionFlow`

从“完整世界回合 Owner”降为 Player Initiative 主入口，并新增 post-outcome World Initiative 阶段。

不得继续假设所有正式世界变化都来自 Player Authorization。

### 7.2 `NarrativeProvider.decideContinuity`

现有阶段已经位于 Player Outcome 与 Narrative realization 之间，是最适合升级的 seam。

迁移方向：

```text
decideContinuity
→ broader proposeWorldDevelopment
```

继续包含 continuity proposal，但增加 world reaction / situation / materialization / canonical patch semantics。

避免额外叠一个重复的“剧情导演 Provider”。

### 7.3 `RuntimeWorldMaterializer`

从：

```text
playerInput + RuntimeMaterializationNeed
```

升级为可接受两类 source：

```text
player_need
world_initiative
```

底层 identity / validation / apply 逻辑尽量复用。

### 7.4 `materializationUsedByCandidate()`

只继续适用于 Player Initiative materialization。

不得应用到 World Initiative。

### 7.5 `RuntimeStore`

Formal Turn commit 继续存在。

新增一个明确的 Opening / World Initiative non-turn commit path，至少用于 T0；该路径：

- revision 单调推进；
- turnNumber 不增加；
- exactly-once；
- Save / Restore 可恢复；
- dynamic topology 与 Game-local assets 同事务一致。

### 7.6 Background Progression

现有 deterministic due-action 推进保留，但不再承担“世界全部主动性”。

它负责确定性状态推进；World Initiative 负责开放式、语义性世界发展。

---

## 8. 需要同步退休的历史约束

实现完成时必须同步更新或退休：

- Runtime AGENTS 中“普通 Turn 不应无条件调用 World Materializer”的旧解释；
- Narrative “只能实现 Program 已冻结 Outcome”被误读为“不得 author ephemeral detail / new world proposal”的部分；
- `RuntimeMaterializationNeed` 作为唯一 materialization source；
- world materialization 必须被 Player Candidate 精确引用的全局规则；
- 任何把 Candidate Directory 当作世界可发生内容白名单的实现 / 测试；
- T0 只能静态等待玩家输入的假设。

保留其真正目的：player agency、durable reality consistency、stable identity、atomicity、recovery、disclosure。

---

## 9. Implementation Waves

### PLAY-01｜World Initiative Shared Foundation + First Vertical

一个主要 Outcome：

> 在不伪造玩家输入、不绕过 Program commit 的前提下，让真实游戏能够生成一次 durable T0 Opening，并在玩家正式行动后产生经校验、可物化、可保存恢复的 World Reaction。

必须包含：

- authority separation；
- Opening Initiative exactly-once；
- world development proposal seam；
- dynamic NPC / Place materialization from world source；
- Active Situation primitive；
- narrative freedom split；
- Save / Restore / Recovery；
- real product session integration；
- deterministic / fixture tests；
- authorized real Provider playability probe。

### PLAY-02｜Owner 刘备连续试玩收敛

只根据 PLAY-01 真人反馈处理：

- opening quality；
- reaction quality；
- pacing；
- situation visibility；
- confirmation friction；
- choice consequence clarity。

不得预先假定 PLAY-02 必须增加更多系统。

---

## 10. PLAY-01 Acceptance

工程与产品纵向至少证明：

1. 无 Player Input 的首次 Session 可以 exactly-once 产生 Opening Situation；
2. Opening 可以自然 materialize 至少一个新 NPC 或一个新地点，而不是要求玩家先点名；
3. 玩家 Formal Turn 后，World Initiative 可以使已有 NPC 主动行动或产生新局势；
4. World Initiative 不经过 Player Authorization，但不能写玩家行动；
5. newly materialized durable interactable 在 narrative 前已有 authoritative identity；
6. ephemeral detail 无需预注册；
7. Active Situation 可在连续回合变化 / 解决 / 升级；
8. Save before / after dynamic growth 与 Restore 保持正确 topology / identity；
9. Crash / replay 不重复 Opening、NPC、Place、Situation 或 world reaction；
10. hidden/private facts 不泄露；
11. 旧 Player Agency negative tests 继续通过；
12. 真人刘备纵向在数回合内自然出现第二角色、第二场景或同等级有意义世界扩展；
13. 至少一次玩家选择导致后续 World Reaction 明显不同；
14. 最终 Fun / Playability 仍由 Project Owner 裁定，不由自动测试代替。

---

## 11. Stage Decision

```text
Core Playability Product Reset = PASS
Architecture Survey = PASS
Turn + World Initiative Architecture = FROZEN
PLAY-01 Implementation = AUTHORIZED / NEXT
```

Library、通用核心大扩展、G10、G11、G12 继续后置，直到 Owner 对核心游玩给出 PASS。
