---
title: my world｜Game-local Evolvable Semantics Decision
status: current-canonical-architecture-decision
created: 2026-08-29
updated: 2026-08-29
phase: G4 Primary Source Assets & Local Game Creation
owner: GPT
---

# Game-local Evolvable Semantics｜Canonical Decision

## 1. Decision

Owner 明确要求保留 The World / DSH 中已经证明有价值的一项核心能力：

> **Source schema 不是 Living World 的可能性上限。**

正式架构：

```text
Reusable immutable Source
→ Final Create / materialize
→ Game-local canonical object
→ lived history
→ model/runtime may evolve local semantics
```

因此：

> **Game-local semantic structure is evolvable.**

模型可以在某一局中修改、扩展甚至创造原始 Source 没有预见的本局语义结构；这些变化只属于该 Game，不修改原始 Source，也不修改全局 Source contract。

---

## 2. Why

`my world` 的产品目的不是运行一套预先穷举好的 RPG 表格，而是让 AI GM 与 autonomous actors 在长期世界中持续创造历史。

如果：

```text
Source schema defines all possible fields
→ world can only express what schema designer predicted
```

那么 `Model authors the world` 只是口号。

反过来，如果模型可以任意修改 physical DB schema、identity、Source contract，又会破坏 Save/Restore、exact provenance 与 durable integrity。

所以必须把：

```text
semantic freedom
!= infrastructure authority
```

分开。

---

## 3. Program-owned kernel vs evolvable semantics

每个 Game-local canonical object 都必须有一个稳定的 Program-owned kernel。概念上至少包括：

```text
game-local stable identity
entity/object type
source provenance when source-backed
exact source generation ancestry
lifecycle / timeline ownership
durable mutation identity
```

这些属于 Program/Runtime authority，模型不能随意改写其身份语义。

在 kernel 之外，本局对象允许拥有可演化 semantic facets。

例：原始 Character Card 没有 `长期心理创伤` 这个结构，但本局一场真实战争可能让角色形成长期心理变化；模型可以让这成为 durable local semantic fact。

又如：

```text
Character
→ develops a new political ethic
→ establishes a new academic school
→ school later becomes a game-local institution
```

这些不要求原始 Character Source 预先有对应字段。

---

## 4. Model may evolve local semantics

允许的本局变化包括但不限于：

- 新的长期人格层；
- 新的价值、恐惧、誓言、创伤、执念；
- 新的组织身份或社会角色；
- 新学术派别、宗教解释、政治思想；
- 新制度、新社会概念；
- 本局独有且长期有意义的对象语义；
- 由局内经历产生的 semantic decomposition / recomposition。

这不是要求 Runtime 现在就建立一个 universal generic-facet platform。当前冻结的是**能力边界与未来架构方向**，physical representation 由真实 consumer 拉出。

---

## 5. Existing Domain wins

开放 semantic facet 不能成为第二套 World DB。

若概念已经存在正式 canonical owner，例如：

```text
Location
Relationship
Knowledge
Injury / Condition
Inventory
Faction / Organization
Timeline / History
Quest / Thread
Mechanic State
```

模型产生的变化必须进入对应 Domain。

例如：

```text
刘备现在在成都
```

应该进入 Location；不是创建 `semantic_facet.current_city`。

```text
刘备与玩家关系恶化
```

应该进入 Relationship；不是 generic prose truth 与 Relationship 各存一份。

只有**当前没有正式 Domain owner、且确实是新的长期语义维度**，才适合作为开放 semantic facet。

正式原则：

> **No duplicate canonical truth for the sake of flexibility.**

---

## 6. No Source mutation

任何 Game-local evolution 都不得：

- rewrite the original Source package；
- mutate installed immutable Source generation；
- silently create a new Source version；
- make one Game's history become another Game's starting truth。

若玩家/Creator 未来希望把某局产生的内容正式发布为新 Source，这是另一个 explicit authoring/publish workflow，不是 Runtime 自动回写。

---

## 7. No model-driven physical schema mutation

模型不能：

- `ALTER TABLE`；
- 创建任意 production table；
- 修改 global validator contract；
- 重新定义 `game_id` / entity identity；
- 改变 Save/Restore authority；
- 以自由文本覆盖 durable integrity rules。

Runtime 负责把开放语义安全地表示在已经受控的 persistence mechanism 中。

> **Model owns open meaning; Program owns identity, authority and durable integrity.**

---

## 8. Timeline / Save / Restore

Game-local semantic evolution 是 lived history 的一部分，不是游离于 Timeline 之外的 metadata。

因此：

- facet creation/update/removal 必须 durable；
- Save 后 reopen 必须仍然存在；
- Restore 必须恢复到对应历史状态；
- 若 Restore 到 facet 产生之前，该 facet 必须不存在；
- 被回滚未来的 semantic knowledge 不得泄漏给模型 Context。

正式原则：

> **Player owns the timeline, including semantic evolution.**

---

## 9. Context consequence

允许 World 长期增长，不代表把全部 local semantics 每 Turn 塞进模型。

正式保持：

```text
System Total State
!= Runtime Relevant Set
!= Model-visible Working Set
```

Context Assembly 未来应选择：

- relevant Source semantic sections；
- relevant Game-local canonical Domain facts；
- relevant evolvable facets；
- relevant recent history。

富 Source 与开放 local semantics 都必须服务 Narrative richness，而不是形成 prompt tax。

---

## 10. Promotion rule｜Facet → Formal Domain

开放 facet 是未知语义的容纳方式，不是所有未来机制的永久归宿。

当某类 facet 在真实游戏中反复出现，并且出现：

- dedicated mechanic consumer；
- dedicated UI consumer；
- query/index/aggregation requirement；
- strong invariant / conflict resolution requirement；
- cross-entity behavior；

就应评估把它晋升为正式 Domain。

顺序：

```text
real repeated semantics
→ real consumer
→ minimal formal Domain
```

而不是：

```text
imagine every future concept
→ giant universal schema
```

这继承：

> **Consumer before abstraction. Reality gate before platform.**

---

## 11. Relation to G4-02R1 / G4-06+

G4-02R1 必须保证 Source contract 不阻止本决策。

G4-06 Final Create 以后 materialized Game-local Character/World definitions 不能只是 immutable Source JSON 的只读镜像；它们必须有独立 Game-local identity 与后续 lived-history owner。

但 G4-02R1 不提前实现 generic facet persistence framework。

G4-06/G5 后续在出现第一个真实 local semantic mutation consumer 时，只实现证明本决策所需的最小 vertical，并通过 Save/Restore/Context 现实测试。
