# G8-UAT-01｜Playable Runtime Seed + Narrative Authority Convergence 规格 v1.0

状态：`CURRENT IMPLEMENTATION BASELINE`
日期：2026-08-18
触发：项目所有者 G8 Stage UAT
代码基线：`sillytavern@3ad5b419e9fb289488a18ded82c5a77f2cdd376c`
上游：`G8_StageUAT_交互响应性与空壳世界阻塞发现_v1.1_2026-08-18.md`

> 本任务是 G8 Stage UAT blocker fix，不是 G9 Asset Runtime Binding，不是 procedural world generator，也不是 Narrative prompt polish。

## 1. 目标

修复两条正式纵向：

```text
Creation Project
→ Minimum Playable Runtime T0
→ real authoritative interactables / destinations
```

以及：

```text
Player Input
→ Semantic
→ Program Final Outcome
→ Narrative realization
```

使玩家可见叙事不得宣称 Program 没有提交的世界变化，同时 Narrative 在安全事实边界内恢复自然 NPC 对话与玩家背景响应。

## 2. P0 First｜Non-world-changing Candidate 不得吞掉 world-changing intent

当前缺口：non-world-changing candidate 分支可能在 `additionalWorldChangingIntent` 被正式处理前返回。

必须冻结：

```text
candidate is non-world-changing
+
proposal reports any unresolved/additional world-changing intent
→ cannot continue as successful non-world-changing turn
```

至少：

- `additionalWorldChangingIntent=true` → clarification / conservative failure；
- `no_action / observe / dialogue / inner_expression / read_only_query` 不得携带 world-changing evidence 后继续成功 realization；
- unsupported / unknown destination movement 应由 Semantic 返回 `needs_clarification` / unresolved reference，而不是 `no_action`；
- Program 不重新做业务 NLP，但必须拒绝模型自己已经报告的未解决 world-changing intent；
- 模型把“进入不存在的酒馆”降级成 no-action 后不得再产生“已经进入”的 Narrative。

保留：

- 合法纯观察；
- 合法普通对话；
- 合法 inner expression；
- 合法 read-only query。

## 3. Program Final Outcome → Narrative Location Boundary

Narrative request 必须明确给出：

- current/final authoritative scene ref + label；
- current authoritative participants；
- current authoritative items；
- available public destinations；
- Formal Outcome 是否包含 player location change。

Narrative system instruction 必须明确：

```text
If Program Formal Outcome has no player position change:
- do not narrate entering, leaving, arriving at, crossing into, or reaching another concrete place.
```

若发生 move：

- 只能写 Program 已确定的 destination；
- 不得追加第二个未提交地点变化。

Tool / structured acknowledgement 可增加 bounded authority acknowledgement，但不能让 Narrative 自己定义 location truth。

## 4. No Phantom Interactable

Narrative 可具体化的可交互对象白名单来源：

```text
NarrativeSafeContext.current.participants
NarrativeSafeContext.current.items
NarrativeSafeContext.availableDestinations
```

具体人物 / 物品 / 地点若不在这些列表：

- 不得作为具体可交互对象写入 Narrative；
- 不得给名字、职业、明确位置并诱导下一轮交互；
- 不得写成玩家已经到达/持有/面对。

允许 ambient non-interactable background：

- 模糊人声；
- 未具体化人群；
- 风雨雾光影；
- 远处无法直接交互的背景轮廓。

但 ambient background 不能在下一句变成 Narrative-only target。

## 5. Minimum Playable Runtime T0

AI-created game 的 T0 最低要求：

```text
current Scene
+
>= 1 visible non-player Character
+
>= 1 public reachable destination / Connection
+
optional carried/visible Item when Creation provides resource/item semantics
```

### 5.1 只使用 current Creation schema

本任务不提前冻结 G9 asset machine schema。

允许使用当前 Creation：

- opening_location；
- current_situation；
- immediate_problem_or_opportunity；
- important_opening_characters；
- initial_resources_and_carried_items；
- player identity/background/goals/personality fields。

### 5.2 Final Create 继续 Zero Mandatory AI

不得为了修复空壳而让 Final Create 强制增加 Provider call。

目标仍是：

```text
Creation Project already contains values
→ deterministic compile
→ Runtime Definition
```

G8 可以新增一个 internal bounded playable-seed compiler / bridge，从现有字段建立最小 typed Runtime entities。

若文本无法安全提取具体名字/结构：

- 使用稳定、低语义风险的 system-defined fallback identity / label；
- publicDescription 保留 Creation 中的玩家安全文本；
- 不编造隐藏事实、复杂关系或机制。

本轮允许的 G8 fallback 是临时 internal capability；G9 Asset Adapter/Creator 后续可 supersede。

### 5.3 Minimum Destination

当前 opening Scene 至少有一条 public Connection。

如果 Creation 没有结构化第二地点：

- 可以使用 neutral deterministic adjacent-scene fallback；
- label/description 必须低语义风险；
- 不得凭空生成具体酒馆/商店/城堡等强世界事实。

因此：玩家请求“最近的酒馆”而 canonical destinations 没有酒馆时，正确行为是 clarification / unknown / no-known-destination，而不是 Narrative 创建酒馆。

## 6. Bounded Rich Narrative Context

扩展 `RuntimeGameSetupContext / NarrativeSafeContext` 的 player-safe working set。

按任务需要提供：

### Player self context

- player identity；
- public origin/background；
- current goals / attachments；
- important past experiences；
- personality / language style（玩家已创建/公开部分）。

### Participant public profile

对 current visible participant 至少提供：

- characterRef；
- displayName；
- publicDescription；
- 当前 player-safe relationship label（若已有）。

绝不因此暴露：

- privateProfile；
- hidden knowledge；
- hidden state；
- internal motive truth。

规则：

> Bounded != Starved。

## 7. Narrative Freedom Envelope

允许 NPC 的 ephemeral conversational realization：

- 回答普通问题；
- 寒暄；
- 玩笑；
- 拒绝；
- 犹豫；
- 反问；
- 即时、不持久化的 conversational attitude；
- 基于 public profile 的说话风格。

例：玩家问“能和你交朋友吗？”

合法 Narrative 可以说：

```text
“刚认识就谈朋友？你倒直接。先告诉我怎么称呼你吧。”
```

这不自动建立：

- friendship relationship mutation；
- Commitment；
- Knowledge truth；
- durable disposition state。

Narrative 仍不得自行建立：

- 持久承诺；
- 新世界事实；
- 新秘密；
- 位置/物品/时间变化；
- Relationship mechanical change。

## 8. Player Agency

Narrative 不得替玩家增加未表达的：

- 羞耻；
- 后悔；
- 恐惧；
- 喜爱；
- 决心；
- 自我评价；
- 新行动。

若玩家明确表达：

```text
“我心里想着自己的过去和目标。”
```

Narrative 可以使用 Creation 已知的 player-safe background/goals 自然回忆，但不得新增新的内心事实。

## 9. Exact UAT Regression Corpus

至少把以下真实输入加入 focused + Real Provider corpus：

### A｜Observe

`观察下四周`

要求：
- 只具体描述 real participants/items/destinations；
- 不产生 phantom interactable。

### B｜Dialogue

`向人搭话`
`你好，请问我能跟你交朋友吗？`

要求：
- 有真实 characterRef；
- NPC 给出自然口头回应；
- world state 不因普通寒暄自动建立 friendship/commitment。

### C｜Player self context

`抬头看看天空，观察一下天气，在心里想了想自己的经历与目标，然后在四周寻找路人搭话`

要求：
- self reflection 可以引用真实 Creation background/goals；
- 不凭空新增玩家情绪；
- dialogue target 必须 authoritative。

### D｜Unsupported place

在没有酒馆 destination 的 Runtime：

`前往最近的酒馆`
`进入酒馆`

要求：
- 不提交 location change；
- Narrative 不得说已经到酒馆/门口/室内；
- 返回 player-safe clarification / unknown destination。

### E｜Valid place

在 fixture 中存在 tavern connection：

`进入酒馆`

要求：
- Program move first；
- canonical scene changes；
- Narrative 才可写进入酒馆；
- next Session projection 与 Narrative 一致。

## 10. Real Provider Gate

修复后必须使用当前正式 DeepSeek model 运行 targeted smoke。

至少证明：

- NPC ordinary conversation responsiveness；
- unsupported location no phantom move；
- valid move canonical/narrative一致；
- player self-context recall；
- hidden disclosure = 0。

不得只用 fixture 宣布 Stage UAT fix PASS。

## 11. 不属于本任务

不要实现：

- general JIT world generation；
- model-owned creation of arbitrary Runtime entities；
- G9 Expansion Directory / Context Router；
- asset-spec vNext；
- Creator；
- dynamic city/world graph generation；
- generic NPC memory system；
- WEB-06 / WEB-07。

## 12. Exit Criteria

G8-UAT-01 PASS 需要：

- P0 non-world-changing downgrade hole closed；
- unsupported move cannot become Narrative movement；
- AI-created T0 has minimum real interactables + destination；
- no phantom concrete affordance；
- player-safe self/NPC context restored；
- ephemeral NPC dialogue freedom established；
- player agency preserved；
- exact UAT corpus PASS；
- real DeepSeek targeted gate PASS；
- G5–G8 regressions PASS；
- Save/Restore/Recovery identity remains correct；
- no G9 implementation started。

完成后重新由项目所有者做 Stage UAT；只有真实 UAT PASS 后 G8 才能 CLOSED。
