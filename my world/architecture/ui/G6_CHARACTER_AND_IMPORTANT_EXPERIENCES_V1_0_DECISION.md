---
title: my world｜G6 Character + Important Experiences v1.0 Decision
status: FROZEN / CURRENT
version: 1.0
created: 2026-09-06
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
owner: Owner + GPT
parent:
  - architecture/ui/G6_SESSION_SHELL_INFORMATION_OWNERSHIP_DECISION.md
  - architecture/ui/G6_MODEL_DRIVEN_INFORMATION_CURATION_AUTHORITY_DECISION.md
supersedes_when_conflicting:
  - architecture/ui/G6_CHARACTER_AND_IMPORTANT_EXPERIENCES_SEMANTIC_AUDIT_DRAFT_V0_1.md
---

# G6 Character + Important Experiences｜FROZEN

## 1. Product outcome

G6 第一个新的真实信息 Surface 纵向冻结为两块互补能力：

```text
角色 / Character
→ evolving current Character Sheet
→ 回答“现在的我是谁”

重要经历 / Important Experiences
→ protagonist-centered milestone history
→ 回答“我是怎样走到现在的”
```

二者都随当前 Game Timeline 演化，但职责不同：

```text
Character
= current state

Important Experiences
= selected meaningful change history
```

同一个 lived change 可以同时改变 Character 当前结果并产生一条 Important Experience；这不是两份 canonical truth，而是同一当前历史的两个玩家投影。

## 2. Shell placement

继续服从：

```text
Player Status Host（左）
→ portrait + live mechanics/status HUD
→ 不承担“我是谁”

Narrative Host（中）
→ GM Narrative + Player natural-language action

World Information Host（右）
→ Character / Important Experiences / People / 事务 / 行囊 / System / Map / Save 等
```

Character Surface 上线后，MW-011 当前左栏 rich `player_profile` 视为过渡内容并迁出。若当时没有合法 portrait / mechanic contribution，左栏允许收窄、折叠或保持极简；不得继续用 biography/world/debug 信息填空。

## 3. Character Surface semantics

Character 是**当前人物状态**，不是静态开局卡，也不是 mutation log。

第一代语义组：

```text
基本资料
出身 / 来历
当前身份 / 社会角色
性格 / 价值观 / 原则
能力 / 专长说明          # 非 numeric mechanic stats
局限 / 长期特征
长期目标 / 自我方向
```

长期目标与事务正式分离：

```text
Character / 长期目标
→ 人生方向、长期追求、自我认同

事务
→ 当前尚未解决的承诺、线索、问题、计划、deadline
```

Character 默认只显示**当前最终状态**。过去的旧值、变化链和历史过程进入 Important Experiences / future history UI，不在 Character Sheet 中做审计日志。

## 4. Character does not own other domains

以下不属于 Character canonical state：

```text
HP / MP / numeric attributes / Buff / short-term status
→ Player Status HUD + System / mechanic state

Inventory / Equipment / money / consumables
→ 行囊 / Inventory

Relationship / NPC relation detail
→ 人物 / Relationship

open task / commitment / clue / current plan
→ 事务

player-known intelligence / discoveries
→ 概览或 future Knowledge / Journal

full history / recovery nodes
→ Timeline / Save semantics
```

特别冻结：

```text
起始携带物
→ 产品 IA 归行囊
→ 不放 Character Surface
```

在正式 Inventory Domain 建立前，宁可暂时不显示完整背包，也不把 Source 起始物品伪装成 dynamic Inventory。

## 5. Important Experiences semantics

Important Experiences 是**主角人生重要节点**，不是所有 durable facts、所有 Turn、Transcript、World Timeline 或事务关闭记录。

它长期保存真正改变主角人生的经历，例如：

- 身份 / 社会角色发生人生级变化；
- 获得或失去重要长期能力；
- 人生方向、核心目标、自我认同发生真实变化；
- 穿越、流亡、入仕、正式领兵、重大败亡、封爵、自立等轨迹改变；
- 结义、婚姻、至亲死亡、重大背叛等 life-scale 关系事件；
- 表面普通但在具体上下文中真正改变人生的事件。

普通战斗、一般对话、普通检定、一次买卖、短期受伤、普通新 NPC、小资源变化、一般任务完成默认通常不重要，但**这不是 Program 的硬规则表**。实际重要性由模型根据上下文判断。

Important Experiences：

- 按 Game-world time / causally meaningful sequence 排序；
- 不设置“最多 N 条”内容删除上限；
- 长局可以折叠、分页、懒加载，但不为 UI 长度删除早期重要人生；
- 内部 Turn index / hash / Timeline node ID 不作为玩家文案。

## 6. Model semantic authority

本 Decision 完整继承：

`G6_MODEL_DRIVEN_INFORMATION_CURATION_AUTHORITY_DECISION.md`

正式原则：

> **Model owns semantic interpretation and curation; Program owns normalized storage, temporal integrity and presentation.**

对于 Character / Important Experiences，模型直接有权决定：

- 本轮是否出现长期人物变化；
- Character 哪些信息新增 / 替换 / 删除 / 保持；
- 某一变化是否值得成为 Important Experience；
- 玩家是否已经真实表达了新的长期目标、价值取向、身份或人生方向；
- 某事件适合进入哪个当前已启用的信息 Surface；
- 如何生成规范、简洁、玩家可读的摘要。

Program 不实现平行的：

- keyword / regex semantic classifier；
- importance score table；
- per-event-type rule tree；
- protagonist-choice evidence heuristic；
- “N 回合后才算长期”之类机械阈值；
- 人物心理 / 人生意义判定器。

## 7. Program responsibilities

Program 只负责适合程序确定性保证的基础设施边界：

```text
current Game / current Timeline binding
accepted turn/version binding
stable identities
bounded structured payload syntax/type/size normalization
atomic durability
idempotent replay
Save / Restore / Regenerate currentness
stale-future isolation
crash/retry correctness
canonical storage location
player-safe serialization/projection
ordering/pagination/presentation contract
```

Program 可以拒绝 malformed payload、未知 operation、无效 identity、stale timeline write 等机器可判定错误，但不对模型的开放剧情语义做第二次“是否合理”审批。

## 8. Post-turn Information Curator

G6 第一个实现纵向采用统一的 **Post-turn Information Curator** 思路。

推荐数据流：

```text
accepted Player input
+ accepted GM Narrative
+ current Character information
+ recent Important Experiences（bounded context only）
+ relevant player-visible / protagonist-owned current game context
+ concise Surface semantics
↓
Information Curator model
↓
bounded structured curation result
↓
Program normalization + durable currentness
↓
player-safe projection
↓
Character / Important Experiences
```

允许增加一次独立模型调用。Owner 明确接受“多一次 bounded model call 换取更高语义准确性、更低 Runtime 复杂度”。

长期可以由一次 curation call 同时维护多个已启用信息 Surface；G6 v0.1 只证明 Character + Important Experiences，不提前冻结 universal information schema。

## 9. Non-blocking Narrative

Information Curator 是后台语义维护，不是 Narrative Finalize Gate：

```text
accepted Narrative
→ immediately visible/playable

Curator
→ background curation
→ durable information update
→ UI refresh
```

Curator failure 不得使已接受 Narrative 失效，也不得默认阻断玩家下一行动。允许 retry / later repair。

## 10. Currentness / reversibility

Character 与 Important Experiences 永远服从当前 Timeline：

```text
accepted current history
→ current Character change / milestone visible

Regenerate replaces source turn
→ stale curation no longer current

Restore to before change
→ Character reverts
→ corresponding milestone disappears

reopen current Game
→ equivalent current projections reconstruct durably
```

不需要为了 UI 物理删除 displaced future history；currentness 决定玩家当前看到的世界。

## 11. First implementation vertical

第一个后端任务只证明：

```text
Model-driven post-turn curation
→ normalized durable Character current material
→ normalized durable protagonist milestone material
→ Save/Restore/Regenerate/reopen correctness
→ player-safe projections
```

不实现完整右侧 UI；UI consumer 在后续独立任务通过稳定 seam 接入。

不建设：

- universal Character ECS；
- giant generic semantic-facet framework；
- universal Event/History platform；
- Inventory / Relationship / Thread / Map 等尚无真实 owner 的新系统；
- Declarative UI Host（MW-013 继续 HOLD）。

## 12. Acceptance philosophy

Engineering tests 证明：模型输出能被规范保存、恢复、重放、回滚和安全投影。

模型对“什么重要、角色是否真的改变”的实际产品质量，最终由真实 Provider + Owner UAT 判断，不用 Program heuristic 的测试数量冒充语义质量证明。
