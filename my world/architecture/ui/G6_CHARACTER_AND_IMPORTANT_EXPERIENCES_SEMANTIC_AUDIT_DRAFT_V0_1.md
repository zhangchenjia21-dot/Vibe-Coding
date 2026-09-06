---
title: my world｜G6 Character + Important Experiences Semantic / Domain Audit Draft
status: DRAFT / FOR OWNER DISCUSSION
version: 0.1
created: 2026-09-06
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

### D. Short-term state

不进入 Character：

- 当天心情；
- 临时恐惧；
- 临时 Buff / Debuff；
- 短期疲劳、饥饿；
- 一次性伤势在未成为长期 Condition 前。

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

事务关闭不自动变成重要经历；只有达到 protagonist milestone significance 才进入 `重要经历`。

## 6. Important Experiences semantic threshold

`重要经历` 不是 durable fact dump。进入经历至少应满足以下一类高价值条件：

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

默认不进入：

```text
普通战斗
一般对话
普通检定成功/失败
一次买卖
一次普通任务完成
短期受伤
每次认识新 NPC
每次小升职 / 小资源变化
```

## 7. Important Experiences storage/projection principle

`重要经历` Surface 不单独维护第二份 biography truth。

长期目标：

```text
authoritative lived history
+ current-valid Character semantic change
+ relevant current-valid formal Domain event
→ protagonist milestone selection / materialization
→ player-safe Important Experiences projection
```

同一次 durable change 可以在两个 Surface 有不同投影：

```text
Character
→ 只显示当前结果

Important Experiences
→ 记录何时、为什么发生了重要改变
```

这不是 duplicate canonical truth；两个 Surface 都只是 projection。

## 8. Time / growth behavior

重要经历按 Game-world time / causally meaningful sequence 自然增长。

产品方向：

- 不设“最多 20 条”之类内容删除上限；
- 长局可按年份 / 人生阶段折叠、筛选或懒加载；
- 不为了 UI 长度删除早期重要人生；
- 排序必须 deterministic；
- 内部 Timeline Node / Turn index / hash 不作为玩家文案。

Character 页面默认只显示**当前状态**，不展示旧值链；旧变化通过 Important Experiences / future history UI 查阅。

## 9. Save / Restore / Regenerate currentness

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

## 10. Current implementation audit

### 10.1 What exists

Current Runtime already has:

- durable `living_world.semantic_turns_by_index` tied to exact accepted GM turn/hash;
- player/NPC Knowledge provenance;
- stable actor identity/materialization;
- Timeline / Save / Restore currentness;
- frozen `world_state.player_character.source_projection.player_profile`;
- fail-closed player-safe profile projection for MW-011.

### 10.2 What does not yet exist

Current implementation does **not** yet provide a dedicated authoritative/player-safe shape for:

```text
current player Character semantic facets
current social identity / lived role
current long-term capability/limitation evolution
Player-authorized long-term goal / principle evolution
protagonist milestone history
```

`living_world.semantic_turns_by_index` currently stores bounded generic change strings. It is useful world consequence history, but it is not a typed/targeted current Character owner and cannot safely be handed to leaf UI for keyword inference.

The MW-011 `PlayerCharacterProfileProjectionDevice` intentionally reads only frozen `player_character.source_projection.player_profile`, validates it, and never falls back to raw `semantic_sections`, Source current or raw world state. Therefore it cannot satisfy an evolving Character Sheet by itself.

## 11. Minimal capability suggested by this consumer

Do **not** build a universal Character ECS/facet platform.

The smallest likely backend vertical is:

```text
existing accepted semantic lane
→ bounded player-Character semantic candidates
→ authority classification
   A objective durable fact
   B existing-domain-owned fact
   C Player-authorized self-definition
   D short-term / reject from Character
→ durable current Character semantic state
→ optional protagonist milestone materialization for significant changes
→ Save/Restore/Regenerate currentness
→ player-safe Character projection
→ player-safe Important Experiences projection
```

Hard constraints:

- no UI parsing raw Narrative to infer identity;
- no second Character truth in ViewModel;
- no generic arbitrary schema mutation;
- Existing Domain wins;
- no model authority over Player-owned major internal choices;
- no Provider call purely to render UI;
- same accepted change replay must be idempotent;
- stale/regenerated/Restored-away change must not remain current.

## 12. Likely implementation seam after Product Freeze

Because this capability changes Runtime authority and player-safe projection, implementation should be split by seam:

```text
Codex
→ minimal game-local Player Character semantic authority
→ protagonist milestone authority/materialization
→ Player-authorization boundary
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

Do not ask KimiCode to infer backend Character truth from raw Narrative or ViewModel text.

## 13. Remaining Owner decisions before freeze

建议下一轮只需要确认以下产品点：

1. `长期目标 / 自我方向` 是否正式保留在 Character，`事务` 只负责 current open work；
2. `重要经历` 是否接受上述 milestone threshold，允许同一 change 同时投影 Character current result + Important Experience history；
3. Important Experiences 是否保存完整重要人生、不设内容删除上限，仅通过折叠/分页控制长局 UI；
4. Character Surface 完成但尚无 portrait/mechanic contribution 时，左栏是否允许自动折叠/收窄；
5. `当前已知事实` 继续留在概览，还是后续建立独立 Knowledge/Journal Surface。

这些确认完成后，可以将 Character + Important Experiences semantic/domain audit 从 DRAFT 冻结，并开始 Task Shaping。
