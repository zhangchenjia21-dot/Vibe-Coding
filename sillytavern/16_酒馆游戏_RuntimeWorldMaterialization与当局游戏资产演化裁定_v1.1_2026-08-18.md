---
title: 酒馆游戏｜Runtime World Materialization 与当局游戏资产演化裁定
status: current
version: 1.1
date: 2026-08-18
project: 酒馆游戏新版主体
scope:
  - game-local-assets
  - runtime-world-materialization
  - dynamic-character
  - dynamic-location
  - dynamic-item
  - open-world-growth
  - save-restore
  - g9-upstream
supersedes: 16_酒馆游戏_RuntimeWorldMaterialization与开放世界增长裁定_v1.0_2026-08-18.md
---

# 酒馆游戏｜Runtime World Materialization 与当局游戏资产演化裁定 v1.1

> [!abstract] 核心裁定
> 外部 World Pack / Character Card / Expansion 是可复用 Source Asset；创建游戏完成后，本局必须形成一套与外部资产库隔离的 **Game-local Canonical Assets｜当局游戏资产**。
>
> Source Asset 不被运行期模型改写；但模型可以在 Program / Runtime 的 typed validation、identity、authority、atomic commit 与 persistence 边界内，**创建、补全、修改当局游戏资产的真实字段**。
>
> 当局游戏资产与 Runtime State 分离：前者回答“这个世界里它是谁 / 是什么”，后者回答“它现在处于什么状态”。剧情中新出现的 NPC、地点、物品、连接等一旦被接受，应成为当局游戏资产的一部分，而不是 Narrative-only phantom。

---

## 1. 三层事实模型

正式分层：

```text
Source Asset Library
(World Pack / Character Card / Expansion)
↓ game creation snapshot / bind
Game-local Canonical Assets
(当局游戏资产)
↓ runtime instantiate / state projection
Authoritative Runtime State
```

三层不能混淆。

### 1.1 Source Asset Library

用途：
- 作者维护；
- Creator 编辑；
- 跨游戏复用；
- 版本化；
- 作为新局的来源模板 / 规则 /锚点。

运行中的模型：
- 不得直接修改；
- 不得把本局剧情反写到外部资产库；
- 不得导致其它存档 / 新游戏被当前剧情污染。

### 1.2 Game-local Canonical Assets｜当局游戏资产

创建游戏后，本局拥有独立 canonical asset graph。

包含：
- World-local definition / anchors；
- Character-local definitions；
- Place / Scene definitions；
- Item definitions；
- Relationship / Knowledge / Objective 等可长期定义性记录；
- Expansion binding / game-local configuration；
- 运行期新 materialize 的角色、地点、物品、连接和其它持久内容。

它是：

> **本局可以持续演化的世界定义层。**

### 1.3 Authoritative Runtime State

回答瞬时 / 可变运行状态，例如：
- 当前 Scene；
- Character 当前 Location；
- HP / Need / Status；
- inventory holder；
- temporary flags；
- timer / cooldown；
- 当前 relationship values；
- pending world action；
- Formal Turn / Event history。

Runtime State 不是 Source Asset，也不是 Narrative 文本。

---

## 2. “副本”是产品语义，不限定物理存储实现

创建游戏时，从玩家选中的 World Pack / Character Card / Expansion 形成当局副本。

产品语义必须满足：

- 该局绑定明确的 source asset ID / version / hash；
- 原始资产后续升级不会静默改写已有存档；
- 当前局可以独立演化；
- 当前局的修改不会反向污染 Source Asset Library。

实现可以是：

```text
full physical copy
```

或：

```text
immutable source snapshot
+
copy-on-write / local overlay
```

只要对上层表现为独立 Game-local Canonical Asset graph 即可。

---

## 3. 模型可以写真实字段，但只能写“当局游戏资产”

正式否定：

> “为了安全，模型只能提 proposal，永远不能修改真实字段。”

这会导致世界无法成长。

正式肯定：

> **模型可以生成当局游戏资产字段的新值，并推动它们成为 canonical truth；但写入动作必须经过 Program-controlled mutation contract。**

正确链：

```text
Model generates typed asset patch / creation proposal
↓
Program validates ownership / schema / world constraints / refs
↓
Program assigns or validates stable identity
↓
atomic Game-local Asset mutation
↓
Runtime State / projections reconcile
↓
Narrative realizes committed result
```

模型负责 authoring candidate content；Program 负责 reality commit。

---

## 4. 当局资产字段应区分可变性

不是所有字段都同等可写。

建议语义分级：

### Immutable / Anchor

例如：
- game-local asset identity；
- source provenance；
- historical canonical identity anchor；
- hard world constraint；
- schema / owner namespace。

普通剧情不得覆盖。

### Evolvable Definition Fields

模型可在剧情中通过 typed patch 更新，例如：
- Character public description；
- current role / affiliation；
- goals；
- known aliases；
- evolving personality summary；
- discovered public history；
- Place public description；
- local social / environmental description；
- Item known description；
- materialized relationship definition / labels。

### Private Canonical Fields

可由模型创建 / 修改，但必须受 Information Boundary：
- NPC private motive；
- private knowledge；
- hidden allegiance；
- secret place fact；
- undisclosed relationship truth。

这些属于 canonical truth，但不自动对玩家可见。

### Runtime State Fields

位置、数值、持有关系、timer 等继续由正式 Runtime mutation / Formal Delta owner 管理，不因为当局资产可写就让 Narrative 直接修改。

---

## 5. 新 NPC 的正式生命周期

未来剧情中新出现的 NPC 不要求存在于 Source Character Card。

正确流程：

```text
story / player need
↓
Character materialization proposal
↓
Program validation
↓
create Game-local Character Asset
  gameAssetId / semanticRef
  provenance = runtime_generated
  public profile
  optional private profile
  initial relations / knowledge seeds
↓
create / bind Runtime Character State
↓
atomic commit
↓
PlayerSession / Context Orchestrator 可引用
```

从这一刻起：
- 他是本局真实角色；
- Save / Restore 必须保留；
- 以后再次出现必须使用同一 identity；
- 可以继续被模型通过当局资产 patch 演化；
- 不会反写到外部 Character Card 库。

---

## 6. 新地点 / 物品同理

### Dynamic Place

```text
Game-local Place / Scene Asset
+
stable ref
+
public / private definition fields
+
Connection graph mutation
+
Runtime traversal semantics
```

### Dynamic Item

```text
Game-local Item Asset
+
stable ref
+
definition / known properties
+
Runtime holder / state
```

新增内容应成为当局资产，而不是只存在于 Narrative history。

---

## 7. 当局 World Pack 不是一份永远不可变的大 Markdown

“当局 World Pack”应理解为本局 canonical world asset graph / snapshot，不要求每次改动重写一个 Markdown 文件。

可以保留 Source World Pack 的结构与 provenance，同时在 Game-local 层维护：
- local entities；
- local places；
- local facts；
- local materialization records；
- local asset patches；
- provenance / createdTurn / modifiedTurn。

对用户而言，它仍属于“这局游戏自己的世界设定”。

---

## 8. Save / Restore / Branch 必须包含当局游戏资产

正式 Save 不只保存 Runtime State。

至少语义上需要：

```text
Game-local Canonical Asset revision
+
Runtime State revision
+
Formal Event / Turn boundary
```

Restore 必须恢复：
- 当时存在的动态 NPC；
- 当时已 materialize 的地点 / 连接；
- 当时的当局资产字段版本；
- 对应 Runtime State。

如果未来支持 branch：
- branch 可以从同一 Source Asset ancestry 分叉出不同的 Game-local Asset evolution；
- 两条 branch 的 NPC / 地点演化互不覆盖。

---

## 9. Source Asset 更新不得静默进入当前局

外部资产库升级：

```text
World Pack v1.2 → v1.3
```

不得自动改写已经进行中的游戏。

未来若支持“将新版本资产更新合入旧存档”，必须是显式 migration / compatibility operation，并处理：
- local modifications；
- dynamic entities；
- conflicts；
- provenance；
- rollback / recovery。

不属于普通 Turn。

---

## 10. 与 Narrative Authority 的关系

当局资产可写并不恢复 Narrative 的自由写权限。

禁止：

```text
Narrative text first
→ implicit DB mutation later
```

允许：

```text
Model asset mutation / materialization proposal
→ Program commit to Game-local Assets
→ Runtime / Formal Outcome commit if needed
→ Narrative realization
```

因此 G8-UAT-01 的 `No Phantom Interactable` 继续成立。

---

## 11. 与 Context Orchestration 的关系

当局资产可能长期增长到数百 / 数千实体。

仍必须：

```text
Game-local Asset Graph size ↑↑↑
Runtime State / history size ↑↑↑
ordinary Model Working Set ≈ bounded
```

Context Orchestrator 只投影当前职责相关的：
- Game-local asset definitions；
- current Runtime state；
- relevant history / knowledge。

Source Asset 全文、Game-local Asset 全图都不应每 Turn 全量加载。

---

## 12. 对 G9-02 的新增正式要求

G9-02 正式目标升级为：

```text
Runtime Asset Binding
+
Context Orchestration Foundation
+
Game-local Canonical Asset Layer
+
Runtime World Materialization Foundation
```

Schema Freeze 前必须证明：

### Existing Source Asset clone/bind

```text
Source Character / World anchor
→ game-local asset instance
→ source provenance preserved
→ current game can mutate local fields
→ source library unchanged
```

### Dynamic Character

```text
Model creates NPC proposal
→ Program creates Game-local Character Asset
→ Runtime state binding
→ subsequent Turn reuses same identity
```

### Dynamic Place

```text
Model creates Place / Scene / Connection proposal
→ Game-local asset commit
→ Runtime graph update
→ player can later navigate to same stable place
```

### Mutation

```text
existing Game-local Character field patch
→ Program validation
→ committed local revision
→ Save / Restore reproduces same value
```

至少证明新增 + 修改两类能力。

---

## 13. 对 G9-03 asset-spec 的约束

asset-spec 需要区分：

```text
Source Asset Definition
vs
Game-local Asset Instance / Provenance
vs
Runtime State
```

外部资产协议不应假设：
- Runtime 永远只读资产；
- 所有 Character 都必须来自 Character Card；
- 所有 Place 都必须在 Source World Pack 预枚举。

未来 schema 应能够表达：
- clone/bind semantics；
- immutable / mutable field policy；
- provenance；
- materialization eligibility；
- ownership / namespace；
- generation constraints；
- information boundary；
- runtime binding boundary。

最终 machine field 名仍不在本裁定冻结。

---

## 14. 当前 G8 边界不变

当前已发出的 G8-UAT-01 继续只修：
- Minimum Playable T0；
- P0 Narrative Authority；
- No Phantom；
- Bounded Rich Context；
- Dynamic Five Suggested Inputs。

不要求 Codex 在该任务中提前建设完整 Game-local Asset mutation platform。

G8 的 deterministic T0 seed 可以视为未来 Game-local Asset Layer 的前置证据，但不冻结 G9 最终实现。

---

## 15. 最终原则

最终世界模型：

```text
Reusable Source Assets
↓ snapshot / bind
Game-local Canonical Assets
↕ typed Program-owned mutation
Authoritative Runtime State
↕ Formal Event / Save / Restore
Bounded Model Authoring + Narrative
```

核心原则：

> **Source assets are templates; game-local assets are living canonical world definitions.**
>
> **Models may author and evolve game-local reality, but only through Program-owned typed commit boundaries.**
