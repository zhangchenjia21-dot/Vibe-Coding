---
title: my world｜G6 People Surface v1.0 Decision
status: FROZEN PRODUCT SEMANTICS / IMPLEMENTATION ARCHITECTURE AUDIT ACTIVE
version: 1.0
created: 2026-09-06
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
owner: Owner + GPT
parent:
  - architecture/ui/G6_SESSION_SHELL_INFORMATION_OWNERSHIP_DECISION.md
  - architecture/ui/G6_MODEL_DRIVEN_INFORMATION_CURATION_AUTHORITY_DECISION.md
  - architecture/world/G5_STABLE_ACTOR_REGISTRY_AND_MATERIALIZATION_V0_2_DECISION.md
  - architecture/world/G5_KNOWLEDGE_PROVENANCE_V0_1_DECISION.md
---

# G6 People Surface｜PRODUCT SEMANTICS FROZEN

## 1. Product question

`人物 / People` 回答：

> **“这局游戏里，我目前知道哪些值得持续记住的人？关于他们，我最近最新了解到的是什么？”**

People 不是 omniscient NPC browser，也不是所有 stable actors 的后台目录。

`World Truth != actor Knowledge != human-player disclosure` 继续作为硬边界。

## 2. Card-first presentation

People v0.1 采用**人物卡片**形式。

默认状态：**折叠**。

折叠卡只显示最必要的识别信息，目标是让玩家快速扫一眼就知道“这是谁”。第一版产品语义为：

```text
姓名 / player-known display identity
+ 一句极短关键定位 / headline
+ 可选的一句最近已知摘要（只有真实需要时）
```

关系、详细身份、性格/特征、能力、已知近况、重要往来等不挤在折叠态。

点击展开后显示更完整的 player-known snapshot。

v0.1 不要求搜索、筛选、排序器、分页框架或持久化展开状态；只有真实规模证据出现后再加。

## 3. Expanded card content

展开卡展示的是**玩家目前最新掌握的该人物信息**，而不是该 NPC 的后台真实当前状态。

允许模型在已有玩家可知材料支持时整理例如：

```text
当前已知身份 / 社会角色
当前已知处境 / 最近已知状态
与主角的当前已知关系 / 互动关系摘要
玩家已经了解到的人物特征 / 性格印象
玩家已经了解到的能力 / 特长 / 局限
其它当前有产品价值、且确实已对玩家成立的信息
```

字段不要求永远齐全。未知就不展示或明确保持未知；不得为了卡片完整度编造内容。

### Relationship boundary

v0.1 可以展示**自然语言的玩家已知关系摘要**，例如：

```text
“曾救过你一次，目前愿意与你合作”
“对你保持戒备，但尚未公开敌对”
```

这不是正式 numeric Relationship Domain，也不是后台 NPC attitude truth。

当前没有正式 Relationship Domain 时：

- 不做好感度 / 敌对值 / 信任分数；
- 不把模型整理出的关系摘要升级成独立 world truth；
- 关系摘要只是 People current player-known snapshot 的一部分。

以后真实 Relationship Domain 出现时，Existing Domain wins，再由 People 投影其 player-safe 结果。

## 4. Latest-known snapshot semantics

Owner 明确要求：

> **People 只保存“玩家最近最新了解到的该人物的信息”。**

因此一张人物卡是**当前玩家认知快照**，不是 biography history / relationship history / event log。

语义：

```text
旧的玩家已知人物快照
+ 后续 accepted play 中玩家获得的新人物信息
↓
模型重新整理
↓
新的当前 player-known snapshot
```

规则：

- 新信息明确替代旧认知时，卡片更新为新认知；
- 新信息没有涉及某项旧认知时，可继续保留仍有依据的 last-known 信息；
- 若玩家后来得知此前认知错误，模型可修正/删除旧信息；
- 不在 People 卡里保存“以前以为 A，后来变成 B”的完整变化链；
- 真正需要回看的重大主角经历仍归 `重要经历`，世界历史仍归 Runtime/Timeline。

## 5. Last-known != omniscient current

People 卡片的“当前”指：

> **当前玩家所知的最新版本。**

它不等于 NPC 在世界后台的真实当前状态。

例如：

```text
玩家最后见到某人时：县尉
此人后来在远方升任太守，但玩家尚未获知
→ People 仍显示玩家最后知道的“县尉”相关状态
```

直到新的 player-visible accepted information 让玩家获知变化，卡片才更新。

不得因为：

- stable actor registry 已更新；
- NPC Agency 在幕后行动；
- World Evolution 发生；
- NPC 私有 Knowledge 改变；
- GM omniscient context 知道更多；

就自动刷新 People 给玩家。

## 6. Who gets a card

People 不等于所有 stable NPC。

产品语义冻结为：

> **当 accepted player-visible game history 已经让玩家实际认识、接触、明确了解到某个 distinct person，并且模型判断这个人具有持续记忆价值时，模型可以建立/维护其 People card。**

可以来自：

- 直接见面 / 互动；
- 被可信地介绍、告知或获知；
- 其它 accepted Narrative 中已经真正对玩家成立的信息。

不要求“必须见过本人”才能建卡。

但仅仅因为：

- Source 中存在这个 Character；
- Runtime 有 stable actor；
- 玩家角色原世界历史知识里知道这个历史名人；
- GM 世界后台知道这个人；

不自动意味着 People 应显示该人物。

什么程度值得建卡由模型判断，不用 Program 关键词/阈值规则。

## 7. Stable identity requirement

People 卡需要绑定稳定 Game-local actor identity，不能以 display name 作为 authoritative identity。

```text
People card
→ one stable NPC local identity
→ player-facing text is a current safe snapshot
```

两个同名角色必须能保持不同卡片。

模型不 mint authoritative local ID；Program 负责验证/绑定稳定 identity。

如果 accepted play 中出现了需要长期记住、但尚未成为 stable actor 的人物，现有 Stable Actor Materialization 能力应先/同时使其拥有合法 Game-local identity；不得为 UI 建一个与 Runtime actor 平行的“人物卡身份系统”。

具体如何安全地把 player-visible narrative 中的人物语义映射到 stable actor ID，是下一步 implementation architecture audit 的核心问题，尚未授权用 display-name matching 做决定。

## 8. Model-driven curation authority

完整继承：

> **Model owns semantic interpretation and curation; Program owns normalized storage, temporal integrity and presentation.**

模型负责判断：

- 本轮是否让玩家真正认识/了解到某个人；
- 是否值得建立 People card；
- 某张卡当前哪些信息应该新增、替换、删除或保持；
- 当前玩家已知关系如何用自然语言概括；
- 哪些细节值得折叠态 headline，哪些只放展开态；
- 新信息是否纠正旧认知。

Program 不实现：

- 人名关键词识别器；
- “见面三次才建卡”阈值；
- 好感分数；
- 关系状态机；
- profile_text / Source section 自动搬运规则；
- 用 display-name equality 做 authoritative actor dedupe；
- 从 omniscient NPC state 自动刷新玩家卡片。

长期优先扩展现有 Post-turn Information Curator，使一次 bounded curation 同时维护当前已启用的信息 Surface，而不是为 People 默认增加独立第二次模型税；若 implementation evidence 证明 identity/disclosure sequencing 需要不同 placement，再单独裁定。

## 9. Player-safe input / disclosure boundary

People curation 只能根据足以支持**玩家已知**的材料输出卡片。

允许的 input direction：

```text
accepted Player input
+ accepted GM Narrative（玩家已经看到）
+ current People player-known snapshots
+ 必要的 stable identity resolution metadata
+ 其它明确 player-safe 的相关上下文
```

不得为了“让模型更聪明”把以下材料直接作为人物详情依据：

- raw omniscient stable actor material；
- GM-private Character sections；
- NPC-private Knowledge；
- Agency private plan；
- hidden World Evolution；
- Source-current 未冻结内容；
- 玩家尚未获得的后台身份/状态变化。

identity resolution metadata 即使需要看到 stable actor reference，也只能作为“这个 accepted person 对应哪个稳定身份”的机器绑定辅助，不构成玩家知道其全部 actor material 的证据。

## 10. Save / Restore / Regenerate currentness

People snapshot 必须服从当前 Timeline。

```text
accepted history 让玩家认识 A
→ A 卡出现

后续 accepted history 更新 A
→ A 卡变为新的 latest-known snapshot

Regenerate / correction 替换了对应 source history
→ stale People update 不再 current

Restore 到认识 A 之前
→ A 卡消失

Restore 到认识 A 之后、但新情报之前
→ A 卡回到当时 player-known snapshot

reopen
→ 等价 current People snapshot 重建，无 render-time Provider call
```

不通过物理删除 displaced future history来伪造 currentness；遵守现有 Timeline authority。

## 11. UI/product acceptance direction

未来实现完成后，Owner 应能在真实游戏中看到：

```text
人物
→ 多张人物卡
→ 默认全部折叠、可快速扫视
→ 展开一张才看到关系和详细 player-known 信息
→ 卡片内容随玩家获得新情报而更新
→ 玩家未获知的幕后 NPC 变化不会提前泄露
→ Restore / Regenerate 后卡片回到对应历史
```

People 不应像 debug actor registry，也不应像全知百科。

## 12. Explicit non-scope

本 Decision 不授权立即实现：

- numeric Relationship / affinity system；
- Faction page；
- People history / biography timeline；
- NPC inventory；
- NPC private knowledge viewer；
- portrait resolver / Visual Runtime；
- universal entity graph；
- search/filter/sort framework；
- MW-013 Internal Declarative UI Host；
- external declarative People schema。

## 13. Immediate architecture audit

Product semantics 已冻结；下一步只剩 implementation architecture audit：

1. 怎样让 People update 精确绑定已有 stable actor local ID，而不靠 display-name authoritative matching；
2. runtime-narrative actor 在同一 accepted Turn 才刚被 materialize 时，Information Curator 如何避免 identity race；
3. 哪些 stable identity metadata 可以安全给 curator 做 resolution，而不会把未披露 actor material 变成 player-facing evidence；
4. 当前 `information_curation` owner 是否应增加 bounded People current snapshots，还是有更小的已有 owner 可复用；
5. 如何保证 Restore / Regenerate / reopen currentness 与现有 Character / Important Experiences 共存；
6. 能否继续一次 post-turn Information Curator 维护 Character + Important Experiences + People，而不增加不必要模型调用。

这些问题冻结后，才创建 People implementation Task Packet。
