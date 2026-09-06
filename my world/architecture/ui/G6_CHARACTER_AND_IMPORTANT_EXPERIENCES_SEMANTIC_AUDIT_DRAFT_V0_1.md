---
title: my world｜G6 Character + Important Experiences Semantic / Domain Audit Draft
status: DRAFT / FOR OWNER DISCUSSION
version: 0.2
created: 2026-09-06
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
semantic_owner: GPT + Owner
parent: architecture/ui/G6_SURFACE_INFORMATION_ARCHITECTURE_DRAFT_V0_1.md
---

# G6 Character + Important Experiences Semantic / Domain Audit｜DRAFT

> **DRAFT — NOT FROZEN — NOT IMPLEMENTATION AUTHORITY**

本文件收敛右侧 `角色 / Character` 与 `重要经历 / Important Experiences` 两个 Surface 的语义、authority、更新边界与当前实现缺口。目标是先确定“谁拥有事实、什么时候变、怎么投影”，再生成后端 / UI implementation task。

## 1. Owner-confirmed product direction

```text
Player Status Host（左）
→ portrait + live mechanics/status HUD
→ 不承担“我是谁”

Character（右）
→ evolving current Character Sheet
→ 回答“现在的我是谁”

Important Experiences（右）
→ protagonist-centered milestone history
→ 回答“我是怎样走到现在的”

Inventory（右）
→ 起始携带物 + 当前持有物的真实 Inventory
→ 起始携带物不放 Character
```

Character 默认显示**当前状态**；变化过程不塞回 Character，而进入 `重要经历`。

Owner 进一步明确：

> **“哪些信息重要、哪些信息值得进入信息栏/重要经历”的语义判断，应交给模型，而不是由游戏系统通过大量规则、关键词、打分器或启发式程序完成。宁可增加一次模型调用，也不要把开放语义判断硬编码成臃肿 Runtime。**

## 2. Core semantic rule

```text
Current Character state
!= Character mutation history
!= full World Timeline
!= Inventory
!= Relationship
!= Knowledge
!= live mechanic state
```

UI 只做 player-safe projection，不成为第二 truth source。

现有正式架构继续适用：

> **Existing Domain wins. No duplicate canonical truth for the sake of flexibility.**

当 Location / Relationship / Knowledge / Injury / Inventory / Faction / Thread / Mechanic State 等已有或未来成为正式 Domain 后，Character 不重复拥有这些事实。

新增正式讨论基线：

> **Model judges semantic importance; Program enforces authority, safety and durability.**
>
> **模型判断“这件事意味着什么、是否重要、适合进入哪个玩家信息 Surface”；程序只负责事实来源、权限、安全、幂等、持久化与时间线一致性。**

## 3. Character Sheet v0.1 information model

建议当前 Character Surface 只保留以下语义组：

| 区块 | 含义 | 是否动态 | Primary authority / source |
|---|---|---:|---|
| 基本资料 | 姓名、年龄/性别等稳定个人资料 | 低 | Frozen Character origin + legitimate game-local correction |
| 出身 / 来历 | 主角人生起点、T0 前背景 | 极低 | Frozen Character Source / T0 projection |
| 当前身份 / 社会角色 | 当前被世界承认的身份、职务、社会角色 | 高 | Game-local Character semantic；未来 Organization/Faction Domain wins where applicable |
| 性格 / 价值观 / 原则 | 当前长期人格与价值取向 | 中 | Frozen baseline + game-local Character semantic；重大自我定义受 Player ownership 保护 |
| 能力 / 专长说明 | 当前长期非数值能力与经验 | 中 | Frozen baseline + lived Character semantic；正式 mechanic/capability Domain wins |
| 局限 / 长期特征 | 当前长期限制、弱点、稳定特征 | 中 | Frozen baseline + lived Character semantic；Injury/Condition Domain wins where applicable |
| 长期目标 / 自我方向 | 当前人生层面的长期追求 | 中 | Frozen baseline + Player-originated / Player-authorized lived change |

Character Surface 不拥有：

```text
HP / MP / numeric stats / Buff / short-term fatigue
→ Player Status HUD + System

Inventory / Equipment / money / consumables
→ 行囊

Relationship / NPC relation detail
→ 人物 / future Relationship

open task / commitment / clue / current plan
→ 事务

player-known intelligence / discoveries
→ 概览 or future Knowledge / Journal

historical change log
→ 重要经历
```

## 4. Character update classes

这些类别是**authority / product boundary**，不是要求 Runtime 写成大型 if/else 分类器。

模型负责结合当前 accepted turn、已有 Character state、相关 Domain truth 与玩家输入做语义判断；程序只验证候选是否落在允许 authority 范围内。

### A. Objective durable Character facts

世界因果可以自然建立，不要求额外 Player confirmation：

- 获得或失去真实社会身份 / 职务；
- 被某组织正式接纳、驱逐或任命（未来 Organization/Faction Domain 存在时由其持有，Character 只投影）；
- 长期学习后真正掌握某非机制型能力；
- 长期客观人物事实发生改变。

### B. Non-voluntary long-term impacts

世界可以建立，但若已有正式 Domain 则由对应 Domain 持有：

- 永久伤残 / 长期疾病 → Injury / Condition；
- 长期社会污名 → Character/Organization 视真实 owner；
- 被强制剥夺身份 → Character/Organization。

### C. Major protagonist self-definition

必须存在 Player-originated / Player-authorized evidence：

- 决定长期效忠某人 / 某势力；
- 放弃“回到现代”等核心人生目标；
- 立下重大长期誓言；
- 决定自立、称王、归隐等重大路线；
- 根本改变核心道德原则 / 自我认同。

GM 的文学心理描写可以自由发生，但**不能仅凭一段 Narrative 自动升级为永久 Character 自我定义**。

程序不需要理解“效忠”“归隐”等语义；模型提出候选，程序只需要确认这类 Player-owned mutation 带有可验证的 Player-originated / Player-authorized evidence reference。

### D. Short-term state

不进入 Character：

- 当天心情；
- 临时恐惧；
- 临时 Buff / Debuff；
- 短期疲劳、饥饿；
- 一次性伤势在未成为长期 Condition 前。

是否已经达到“长期人物变化”由模型判断，而不是 Program 通过天数/次数阈值机械决定。

## 5. Long-term goals vs 事务

建议冻结：

```text
Character / 长期目标
→ 人生方向、长期追求、自我定义
→ “我想成为什么样的人 / 往哪里走”

事务
→ 当前尚未解决的承诺、线索、问题、计划、deadline
→ “我现在具体有什么事要处理”
```

例：

```text
Character
- 寻找是否存在回到现代的可能
- 不愿以残害平民换取个人利益

事务
- 三日内答复刘备邀请
- 去县衙解决户籍问题
- 调查昨夜袭击者
```

事务关闭不自动变成重要经历；是否构成 protagonist milestone 由模型基于实际意义判断。

## 6. Important Experiences semantic threshold

`重要经历` 不是 durable fact dump。

以下类型只是**给模型的高价值语义指南 / examples**，不是 Runtime 规则树、关键词表或打分器：

### 6.1 Identity-changing

- 获得 / 失去重要社会身份、职务、爵位；
- 加入 / 离开 / 建立重要组织或势力；
- 婚姻、师承、流亡、被俘、归附等足以改变人生身份的事件。

### 6.2 Capability-changing

- 真正掌握一种改变长期行动能力的重要能力；
- 永久失去重大能力；
- 长期身体/精神条件发生足以改变人生的变化。

### 6.3 Direction-changing

- Player 明确选择新的长期路线；
- 形成新的核心誓言 / 长期目标；
- 放弃旧的核心人生方向。

### 6.4 Trajectory-changing

- 穿越、流亡、入仕、正式领兵、重大败亡、封爵、自立等；
- 重大成功/失败真正改变以后的人生路线；
- 一次事件成为明显的人生阶段分界。

### 6.5 Relationship-changing at life scale

Relationship truth 仍由 Relationship/People owner 持有；但结义、婚姻、至亲死亡、重大背叛等如果足以改变主角人生，可以在 `重要经历` 中作为 milestone 投影。

默认低价值例子：普通战斗、一般对话、普通检定、一次买卖、普通任务完成、短期受伤、普通新 NPC、小资源变化。

但这些都不是硬禁止：如果某次表面普通的谈话/战斗实际上改变了主角人生，模型仍可判断其为重要经历。

正式原则：

> **Semantic significance is contextual, not rule-enumerable.**

## 7. Model-driven Information Curator

建议 G6 的最小能力不是 Program-owned “重要性判定器”，而是一个**模型驱动的 post-turn information curation step**。

可复用现有 semantic-analysis seam，或者在准确性/职责隔离更好时允许增加一次独立模型调用。Owner 明确接受“多一次模型调用换取更高准确性与更低程序复杂度”。

建议输入：

```text
accepted Player input
+ accepted GM Narrative
+ current player-safe Character state
+ recent protagonist milestones（bounded）
+ relevant formal Domain facts / authority hints
+ Surface contracts / Player-agency rules
```

模型输出 bounded semantic proposals，例如：

```text
character_updates[]
important_experiences[]
(optional later) overview/highlight suggestions[]
```

每个 proposal 只需包含最小语义材料：目标区块/Surface、player-facing text、必要的 replace/add/remove intent、以及可验证 evidence reference。

模型负责：

- 判断某变化是否长期重要；
- 判断是否值得进入 Character / Important Experiences / future Overview 等信息 Surface；
- 判断应更新、替换、移除还是保持原状；
- 生成简洁规范的玩家可见摘要；
- 在上下文中理解“普通事件是否因后果而变得重要”。

程序**不负责**：

- 关键词分类；
- “升官 +5 / 战斗 +3”重要性打分；
- 正则推断身份；
- 固定 N 回合后自动判定长期；
- 从 Narrative 中用 hard-coded parser 猜人格变化；
- 为每种未来 RPG 概念建立专用 heuristic。

## 8. Program responsibilities remain narrow and hard

把语义判断交给模型，不等于把 authority 交给模型。

程序仍必须负责：

```text
accepted turn/hash linkage
stable identity
payload shape/size validation
allowed target/surface validation
Existing Domain wins boundary
Player-owned self-definition evidence requirement
player-safe disclosure boundary
idempotent commit/replay
Save / Restore / Regenerate currentness
persistence integrity
fail-closed behavior on malformed/ambiguous proposal
```

程序只回答：

> **“这个模型提案有没有权限、格式是否有效、能不能安全 durable？”**

而不回答：

> **“这件事在人生意义上到底重不重要？”**

这保持：

> **Model authors meaning; Runtime makes it safe and durable.**

## 9. Important Experiences storage/projection principle

`重要经历` Surface 不单独维护第二份 biography truth。

长期目标：

```text
authoritative lived history
+ current-valid Character semantic change
+ relevant current-valid formal Domain event
→ model-driven protagonist milestone curation
→ bounded durable/current milestone material
→ player-safe Important Experiences projection
```

同一次 durable change 可以在两个 Surface 有不同投影：

```text
Character
→ 只显示当前结果

Important Experiences
→ 记录何时、为什么发生了重要改变
```

这不是 duplicate canonical truth；两个 Surface 都只是 projection / curated material over authoritative history。

## 10. Time / growth behavior

重要经历按 Game-world time / causally meaningful sequence 自然增长。

产品方向：

- 不设“最多 20 条”之类内容删除上限；
- 长局可按年份 / 人生阶段折叠、筛选或懒加载；
- 不为了 UI 长度删除早期重要人生；
- 排序必须 deterministic；
- 内部 Timeline Node / Turn index / hash 不作为玩家文案。

Character 页面默认只显示**当前状态**，不展示旧值链；旧变化通过 Important Experiences / future history UI 查阅。

## 11. Save / Restore / Regenerate currentness

Character 与 Important Experiences 必须服从同一 current timeline：

```text
accepted current history
→ Character current state / milestone may exist

Regenerate replaces source turn
→ stale Character change / stale milestone must disappear from current projection

Restore to before the change
→ Character reverts
→ corresponding Important Experience disappears

reopen current Game
→ equivalent current Character + milestone projections reconstruct durably
```

不为 UI 物理删除 displaced history；currentness/projection 决定当前玩家看到什么。

## 12. Current implementation audit

### 12.1 What exists

Current Runtime already has:

- durable `living_world.semantic_turns_by_index` tied to exact accepted GM turn/hash;
- player/NPC Knowledge provenance;
- stable actor identity/materialization;
- Timeline / Save / Restore currentness;
- frozen `world_state.player_character.source_projection.player_profile`;
- fail-closed player-safe profile projection for MW-011.

### 12.2 What does not yet exist

Current implementation does **not** yet provide a dedicated authoritative/player-safe shape for:

```text
current player Character semantic facets
current social identity / lived role
current long-term capability/limitation evolution
Player-authorized long-term goal / principle evolution
protagonist milestone history
```

`living_world.semantic_turns_by_index` currently stores bounded generic change strings. It is useful world consequence history, but it is not a targeted current Character owner and cannot safely be handed to leaf UI for keyword inference.

The MW-011 `PlayerCharacterProfileProjectionDevice` intentionally reads only frozen `player_character.source_projection.player_profile`, validates it, and never falls back to raw `semantic_sections`, Source current or raw world state. Therefore it cannot satisfy an evolving Character Sheet by itself.

## 13. Minimal capability suggested by this consumer

Do **not** build a universal Character ECS/facet platform or Program-owned significance engine.

The smallest likely backend vertical is:

```text
accepted turn
→ model-driven Character / milestone curation
→ bounded semantic proposals
→ narrow Program authority/safety validation
→ durable current Character material + milestone material
→ Save/Restore/Regenerate currentness
→ player-safe Character projection
→ player-safe Important Experiences projection
```

Hard constraints:

- no UI parsing raw Narrative to infer identity;
- no second Character truth in ViewModel;
- no generic arbitrary schema mutation;
- no Program keyword/score/heuristic significance classifier;
- Existing Domain wins;
- no model authority over Player-owned major internal choices without evidence;
- no Provider call purely to render UI;
- same accepted change replay must be idempotent;
- stale/regenerated/Restored-away change must not remain current.

Model curation call is semantic maintenance, not rendering. It may run as a bounded post-turn/background step and must not make the accepted Narrative wait for completion or failure.

## 14. Likely implementation seam after Product Freeze

Because this capability changes Runtime authority and player-safe projection, implementation should be split by seam:

```text
Codex
→ model-driven Character / milestone curator seam
→ minimal game-local Player Character semantic storage/authority
→ Player-authorization evidence validation
→ Save/Restore/Regenerate/currentness
→ safe L3 projection + backend tests

then

KimiCode
→ right-side Character Surface
→ right-side Important Experiences Surface
→ migrate transitional biography/profile out of left Player Status Host
→ empty/collapsed left behavior when no portrait/mechanic contribution
→ UI/interaction/layout tests
```

Do not ask KimiCode to infer backend Character truth from raw Narrative or ViewModel text。

## 15. Remaining Owner decisions before freeze

此前五项产品点已有明确方向：

1. `长期目标 / 自我方向` 保留在 Character；`事务` 只负责 current open work；
2. 同一 change 可同时投影 Character current result + Important Experience history；
3. Important Experiences 保存完整重要人生，不设内容删除上限，仅通过折叠/分页控制长局 UI；
4. Character Surface 完成但尚无 portrait/mechanic contribution 时，左栏允许自动折叠/收窄；
5. `当前已知事实` 暂留概览，未来由真实 Knowledge/Journal consumer 决定是否拆分。

新增 Owner direction：

6. 信息重要性 / Surface 收录判断默认由模型完成；Program 不建设大型 semantic classifier / heuristic engine；允许为准确性增加一次独立模型 curation 调用。

在确认第 6 项作为正式架构原则后，本 Audit 即可进入 FREEZE，并开始 Task Shaping。
