---
title: my world｜G6 Factual Inventory Vertical v1.0 Decision
status: FROZEN / CURRENT
version: 1.0
created: 2026-09-09
updated: 2026-09-09
phase: G6 Package 5 Core Inventory Vertical
owner: Owner + GPT
implementation_consumer: MW-029
---

# G6 Factual Inventory Vertical v1.0｜CURRENT

## 1. Product question

`行囊 / Inventory` v1.0 回答：

> **“我现在真正随身拥有、可以在接下来的行动里使用或失去的东西是什么？”**

它不是 Character 描述，不是 Open Threads，不是 GM 随口提到过的物件列表，也不是通用物品/装备/economy engine。

Package 5 只证明一个最小但真实的 possession vertical：

```text
accepted Narrative 明确建立玩家获得/持有一个实体物品
→ existing World semantic lane 同一次调用提取 factual Inventory mutation
→ Program-owned stable item identity + durable Inventory event record
→ current Inventory projection
→ 行囊 Surface
→ 后续 Narrative / mechanics 能看到当前行囊事实
→ 一次使用后消耗 / 转交 / 丢失 / 状态更新
→ Save / reopen / Restore / Regenerate currentness
→ Debug inventory changed / no-change / failed
```

## 2. Inventory is factual gameplay truth

Inventory 与 Character / Important Experiences / People / Open Threads 不同。

这些后者允许模型做长期语义整理；Inventory 表示的是更窄的事实：

> **当前 Timeline 上，玩家角色现在实际拥有/携带的物品。**

因此：

- 不经过 Information Curator；
- 不由 UI 自行从 Narrative 关键词猜物品；
- 不把 `world changes` prose 当第二份 Inventory；
- 不把 Character profile 中“可能拥有/常用”的东西直接视为当前持有；
- 不根据现实常识自动补齐衣服、钱、武器、食物等默认装备。

模型可以在现有 World semantic lane 中根据 **accepted Player Action + accepted GM Narrative** 判断“本回合是否已经明确改变当前 possession”；Program 负责结构、identity、currentness、持久化和投影。

继续遵守：

> **Model owns semantic interpretation; Program owns normalized factual state integrity.**

这里的模型自由只用于理解 accepted Narrative 已经确立了什么，不授权模型在后台另写一套未叙述发生的物品剧情。

## 3. No invented initial inventory

当前 first-party Character Source `character_card.v0.2` 没有独立、结构化、可验证的 initial inventory contract；现有 T0 profile 主要是身份、人格、能力、关系、知识等语义材料。

因此 Package 5 明确不做以下错误捷径：

- 不从 Character prose 中抽取/猜测“起始装备”；
- 不给所有新 Game 硬塞默认钱币、衣服、武器或食物；
- 不为了测试方便修改 Source 外部 contract，提前建设 G8 Creator / external item authoring schema。

v1.0 的合法基线是：

```text
没有 authoritative Inventory event
= 当前结构化行囊为空
```

第一个真实物品可以在正常 accepted Narrative 中首次被明确建立为玩家当前持有，并由 World semantic lane 物化。这样得到的“第一件物品”是真实 Game-local gameplay fact，而不是 Program invented filler。

未来若真实产品需求批准 Source-authored initial inventory，可在 G8 / 相应 Source contract work 中增加明确 factual seed；不得在 MW-029 暗中预造。

## 4. Canonical owner and event-sourced currentness

### 4.1 Owner

Inventory 使用 Game-local durable World document 中一个独立、窄的 owner，例如：

```text
player_inventory
  schema_version: player_inventory.v0.1
  turns_by_index: {...}
```

具体内部命名由实现者决定，但 ownership 必须保持：

```text
accepted semantic turn version
→ durable Inventory event record
→ fold current accepted records
→ current Inventory snapshot projection
```

不新增 SQLite table。现有 Timeline World snapshot 持久化继续是底层 durable owner。

### 4.2 Why event records, not a mutable list only

不得只把 `current_items=[]` 作为无版本 mutable list 写进 World，然后让 UI 直接读取。

原因：Regenerate / Correction 可能替换已经 accepted 的 Conversation 版本，而旧 World mutation 仍可能存在于当前/历史 Timeline snapshot。Inventory 必须像其它 currentness-sensitive domain 一样能识别：

- source turn index；
- exact accepted GM hash/version；
- current accepted Conversation；
- Restore / displaced future。

因此每个 Inventory mutation record 必须绑定产生它的 exact accepted turn version；当前 Inventory 由 **只折叠仍与 current accepted Conversation 匹配的记录** 得到。

这样：

```text
旧版本获得物品
→ Regenerate 替换该回合
→ 旧 Inventory event 自动不再 current
→ 物品不会残留
```

Program 不为 UI 建第二套 currentness owner。

## 5. Minimal item identity and shape

### 5.1 Stable identity is Program-owned

每件持久 Inventory item 有内部 stable `item_id`，由 Program 生成。

模型不得提供或决定 authoritative item ID。

对于本回合新获得物品，Program 应基于 exact accepted turn version + candidate ordinal/material deterministic mint identity，或采用等价的 replay-safe Program-owned方法；同一 accepted version 重新处理不得制造第二件重复物品。

Stable ID：

- 可以用于内部 event fold / update / remove；
- 不需要显示给玩家；
- 不进入 Debug prose；
- 不作为模型可自由伪造的输入。

### 5.2 Player-facing v0.1 item material

v1.0 每件当前物品只需要非常小的 factual material：

```text
name
summary
```

其中：

- `name` = 玩家可理解的物品名称；
- `summary` = 当前物品事实状态的简短描述，例如“封口仍完整的书信”“已空的水囊”。

不在 v1.0 建：

- numeric item stats；
- rarity；
- price/value；
- weight；
- durability number；
- equipment slot；
- stack system；
- loot table；
- crafting tags；
- ownership history UI。

如果 Narrative 明确涉及“三枚果子”之类数量，v1.0 可以把数量作为自然语言 factual material 的一部分；Program 不建立通用 stack arithmetic engine。

## 6. Minimal mutation vocabulary

World semantic lane 的 Inventory 子结果只需要表达三类事实变化：

```text
ADD
UPDATE
REMOVE
```

语义：

- `ADD`：accepted Narrative 明确建立玩家现在获得/携带一个此前不在当前 Inventory 的物品；
- `UPDATE`：同一 stable item 仍归玩家所有，但 accepted Narrative 明确改变了它的当前 factual state；
- `REMOVE`：accepted Narrative 明确建立玩家已经不再持有该物品，例如消耗、交给别人、丢失、遗弃、被夺走。

`使用` 本身不是固定 Inventory mutation：

- 用过但仍持有 → 可 no-change；
- 使用后状态改变 → UPDATE；
- 使用后消耗 → REMOVE。

Program 不通过关键词把“喝/给/丢/用”等词机械映射到 operation；由模型根据 accepted Narrative 的实际含义决定。

## 7. Model references existing items through opaque request refs

模型在 semantic extraction 时需要能够准确指向“当前已有的哪件物品”，但不得获得/伪造 authoritative stable IDs。

采用与 People identity bridge 相同的原则：

```text
current Inventory internal item_id
→ request-only opaque item_ref
→ model UPDATE / REMOVE 引用 item_ref
→ Program resolve back to exact current stable item
```

Rules：

- `item_ref` 每次请求临时生成；
- 不持久化；
- 不显示给玩家；
- 不允许模型提交 raw `item_id`；
- unknown / duplicate / stale ref fail-soft drop，不猜名称匹配；
- display name 永远不是 authoritative identity。

新物品由模型提供 bounded `name + summary` candidate material，Program mint stable identity。

## 8. Same World semantic call; no extra Provider lane

Package 5 复用现有 `WorldTurn / Semantic Materialization` 的同一次后台语义调用。

当前 lane 已经从 accepted Narrative 提取：

- durable world changes；
- actor knowledge events；
- new stable actor candidates / identity bindings。

Inventory 是同一 accepted Narrative 的另一类 factual consequence，因此增加为一个**独立、fail-soft 的可选子字段**，而不是新增第二个模型调用。

要求：

- Inventory 子字段 invalid/oversized 不得破坏 otherwise-valid existing world/knowledge/identity result；
- existing world/knowledge/identity 字段错误也不得把已经合法提交的 Inventory 子字段部分提交成违反原子性的新第二 mutation；
- 本回合 semantic commit 仍使用现有 single durable World mutation seam；
- Inventory events 与同回合其它合法 semantic material 在同一 candidate World snapshot 中提交。

不得新增 Inventory Curator、Inventory Provider adapter 或后台轮询。

## 9. Current Inventory projection

新增一个 Inventory-owned player-safe L3 seam，输入 current durable World + current accepted Conversation（或直接输入 current Runtime），输出 detached current items：

```text
[
  {"name":"...", "summary":"..."},
  ...
]
```

可在内部保留 stable IDs 用于 model-ref mapping / fold，但 leaf UI 不需要它们。

Fold 必须：

1. 只读合法 Inventory records；
2. 只接受与 current accepted turn/hash 精确匹配的 record；
3. 按 accepted turn 顺序重放 ADD / UPDATE / REMOVE；
4. UPDATE / REMOVE 只作用于当时 current 的 exact stable ID；
5. ambiguous/corrupt event fail-soft，不制造物品；
6. 返回 detached copy。

## 10. GM / mechanics grounding

结构化 Inventory 若只显示在 UI，却不进入后续 GM request，会产生明显产品矛盾：玩家看到自己有物品，但 GM 下一回合可能不知道。

因此 Package 5 必须让**当前 player-safe Inventory snapshot**进入后续 foreground gameplay context。

至少覆盖：

- ordinary continuation / OOC GM guidance；
- Public d20 control + narrative stages。

Grounding 应是 bounded、player-safe 的 factual block，例如：

```text
Current Player Inventory
- 封口书信｜封口仍完整
- 火折子｜可继续使用
```

Rules：

- context block 从同一 Inventory L3/current fold 派生；
- 不直接注入 raw Inventory event records / IDs；
- 不改变 Public d20 mechanics truth；
- 不让 Inventory 反向成为 Character personality evidence；
- `project_world_only()` / off-screen World Evolution 默认不需要玩家私人行囊，不应因此扩大 World-only authority。

World semantic extraction 自身也获得 current Inventory 的 request-only refs，才能对 UPDATE / REMOVE 做 exact identity resolution。

## 11. 行囊 Surface

Package 5 后，World Information Host navigation 为：

```text
概览 | 角色 | 重要经历 | 人物 | 事务 | 行囊 | 系统 | 存档
```

`行囊` 是 read-only current snapshot。

v1.0：

- 标题/区块：`当前行囊`；
- 每项显示 `name + summary`；
- 空态：`当前没有已记录的随身物品。`；
- ordinary gameplay text >=20px；
- 多物品使用现有右侧纵向滚动；
- 切换 tab 不触发 Provider、不写 Runtime。

不在本 Package 提供：

- checkbox；
- drag/drop；
- 手动添加/删除；
- 装备按钮；
- 物品使用按钮；
- 排序/筛选/搜索；
- 容量/负重 UI。

玩家仍通过自然语言行动使用、给予、丢弃物品；模型/World semantic lane根据 accepted Narrative 后果更新 factual Inventory。

## 12. Refresh timing

Inventory change 是 World semantic commit 的结果，不是 Conversation acceptance 同步结果。

因此正确玩家刷新点是：

```text
accepted Narrative
→ World semantic terminal / durable commit
→ Inventory L3 current projection
→ 行囊刷新
```

现有 Shell 已在 World semantic terminal 调用 player-safe panel refresh；Package 5 应复用该 seam，不增加轮询。

在 semantic lane 仍处理中时，旧 Inventory snapshot 保持可见，不把模型“正在分析”当作已经发生的新 factual state。

## 13. Debug inventory lane

Package 1 Debug Mode 增加 `inventory` lane。

建议从 World semantic terminal 的安全结构 counts 派生：

```text
added
updated
removed
total (optional current safe count)
```

至少区分：

- committed changed；
- committed no-change；
- failed；
- cancelled；
- stale/currentness-rejected（若现有 semantic flow产生）。

Debug：

- 不显示物品 prose；
- 不显示 item_id/item_ref；
- 不显示 raw semantic response；
- 不新发 Provider 请求；
- Restore 清旧 diagnostic epoch。

## 14. Failure behavior

Inventory lane 延续现有 semantic background / fail-soft 原则：

- Narrative 已 accepted 后，Inventory extraction failure 不回滚 Narrative；
- 不允许 partial invalid item operation 制造虚假物品；
- unknown/stale item_ref fail-soft drop；
- semantic Provider failure → Inventory 保持上一 current snapshot；
- persistence failure → 新 Inventory event 不成立；
- Restore / Regenerate 后旧 event 不得泄漏回 current inventory；
- reopen 不做历史 backfill Provider call。

## 15. Explicit non-scope

Package 5 不实现：

- Source-authored initial inventory external contract；
- Creator inventory authoring；
- equipment slots；
- weapon/armor stats；
- generic item schema/platform；
- loot generation；
- crafting；
- shops/economy/currency system；
- weight/capacity；
- numeric durability；
- stack arithmetic；
- NPC inventories；
- containers/storage/chests；
- ownership provenance UI；
- direct item-use buttons；
- Dynamic UI Host；
- Generic Action Intent；
- G3 Context debt cleanup；
- Shell general refactor。

## 16. Acceptance principles

Engineering acceptance requires evidence that:

1. current structured Inventory starts empty when no authoritative event exists; no default/fake item appears;
2. a controlled accepted Narrative that explicitly establishes player possession yields one real ADD in the same existing World semantic call;
3. the new item has Program-owned stable identity internally but UI/Debug never expose it;
4. a later accepted Narrative can exact-ref that current item and UPDATE or REMOVE it without display-name identity matching;
5. ordinary use with no possession/state change can leave Inventory no-change;
6. invalid/unknown/stale refs do not mutate Inventory and do not corrupt existing World/Knowledge/Identity materialization;
7. Regenerate replacing the source accepted version removes the superseded Inventory consequence from current projection;
8. Save/reopen preserves current Inventory; Restore before/after exact events removes/restores items correctly;
9. current Inventory is grounded into later ordinary/OOC and Public d20 requests through player-safe context only;
10. World-only evolution does not gain player Inventory private authority accidentally;
11. 行囊 UI shows exact current safe item material, has an empty state, >=20px typography and no horizontal overflow at 960×540 / 1280×720 / 1920×1080;
12. tab/render causes zero Provider calls and zero durable mutation;
13. Debug inventory lane reports safe changed/no-change/failure/currentness evidence;
14. existing Character / Experiences / People / Threads / System / d20 inline card / Narrative behavior regressions remain intact;
15. no new Provider call, SQLite table, Source contract or fake RPG/item framework is introduced.

Product confirmation may remain deferred and be combined with the later concentrated Owner UAT / Package 7 Reality Gate unless Owner requests earlier.
