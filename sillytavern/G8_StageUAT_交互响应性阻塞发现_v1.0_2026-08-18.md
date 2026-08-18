# G8 Stage UAT｜交互响应性阻塞发现 v1.0

状态：`CURRENT UAT FINDING / P1 BLOCKER`
日期：2026-08-18
代码基线：`sillytavern@3ad5b419e9fb289488a18ded82c5a77f2cdd376c`

> Engineering Exit Gate 仍为 PASS；本文件记录项目所有者 Stage UAT 暴露的玩家体验 / Runtime-Narrative 纵向缺口。
> 在本阻塞收口前，G8 不得标记 PASS / CLOSED，也不得正式进入 G9-01。

## 1. UAT 症状

项目所有者使用正式 Launcher / Creation / Session 进行首轮真实游玩时发现：

- Narrative 文风流畅，但对玩家输入的实际响应性弱；
- Narrative 会创造看似可交互的路人，但后续对话不能产生正常 NPC 回应；
- 玩家询问自身经历 / 目标时，叙事只能给出泛化措辞；
- 玩家尝试前往附近地点时，Narrative 会描述“朝某方向走去”，但 Runtime 缺少真实可达位置；
- 连续对话容易重复环境锚点、避免 NPC 实质回应。

## 2. 根因 A｜Creation → Runtime 只生成空壳世界

当前 `MinimalRuntimeDefinitionAdapter` 最终生成：

```text
1 Region
1 Place
1 Scene
Player
characters = []
items = []
relationships = []
connections = []
knowledgeFacts = []
```

但 Creation Project 已经存在：

- `important_opening_characters`
- `initial_resources_and_carried_items`
- `current_goals_and_attachments`
- `important_past_experiences`
- `language_expression_style`

等玩家 / 开局语义。

当前 compiler 没有把 opening characters / items / reachable world affordances 编译为 Runtime entities。

## 3. 根因 B｜Narrative 产生 Phantom Interactable

Narrative Prompt 允许增加氛围、动作细节和 NPC 说话方式；但 Runtime Candidate Directory 只从 authoritative visible characters / items / connections 建立。

因此若 Narrative 写出一个不存在于 Runtime 的具体人物：

```text
Narrative：玩家看见一个具体路人
↓
Player：尝试与其搭话
↓
Runtime：没有 characterRef
↓
Semantic / Dialogue 无合法 target
```

形成 player-visible affordance 与 Runtime affordance 不一致。

正式原则：

> **Narrative-visible interactable affordance must have an authoritative Runtime referent.**

不得继续依赖 Narrative-only phantom entity 伪装可玩世界。

## 4. 根因 C｜Narrative Context 被裁得过薄

当前 Narrative-safe setup context 主要包含：

- world concept
- era / society
- tone
- experience focus / avoid
- public rules

但没有完整传递玩家安全的：

- player identity / public background
- current goals / attachments
- important past experiences
- language / personality style

当前 `participants` 也只提供 characterRef / displayName，缺少足够的 player-safe public description / presentation profile。

因此：

> bounded context 被错误实现成了 starved context。

目标应是：

> **Bounded != Starved.**

## 5. 根因 D｜Narrative Freedom Envelope 不清晰

当前 Prompt 同时要求：

- 可以增加 NPC 说话方式 / 语气；
- 不得新增世界事实 / Knowledge / Commitment / 玩家未表达的承诺或立场。

这一边界对普通社交回复过于模糊，模型倾向保守沉默。

需要明确区分：

### Ephemeral Narrative Realization（允许）

- NPC 的即时措辞；
- 寒暄 / 玩笑 / 拒绝 / 犹豫；
- 不持久化的表情、语气、动作；
- 不建立新世界事实的普通对话反应。

### Durable / Authoritative Consequence（仍禁止 Narrative 自建）

- 新 Knowledge truth；
- Commitment；
- Relationship state mutation；
- inventory / position / time / mechanical state；
- hidden fact；
- formal event / outcome。

同时继续保护 Player Agency：Narrative 不得替玩家新增未表达的内心感受、决定或行动。

## 6. Stage 裁定

```text
Engineering Exit Gate = PASS
Stage UAT = BLOCKED
G8 = ACTIVE
G9 = NOT AUTHORIZED
```

分类：`P1 Stage Objective Minimum / Product Critical Path`。

本问题不是 WEB-06/07 Product polish；它直接影响核心 Player Input → Runtime → Narrative 的可玩性。

## 7. 推荐收口顺序

不要先粗暴放松所有 safety prompt。

先冻结：

1. **Playable Runtime Seed**：Creation 中已有的开局人物 / 资源 / 玩家背景 / 可达世界结构如何进入 Runtime；
2. **No Phantom Interactable**：玩家可见的具体可交互对象必须有 Runtime referent；
3. **Bounded Rich Narrative Context**：补回当前任务真正需要的 player-safe character/player context；
4. **Narrative Freedom Envelope**：允许 ephemeral conversational realization，同时不放松 durable authority；
5. 使用本次真实 UAT 输入建立 responsiveness regression corpus。

在上述设计冻结前，不建议仅通过 prompt wording 修补症状。
