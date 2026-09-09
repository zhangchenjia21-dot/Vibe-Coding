---
title: my world｜G6 System / Public Mechanics Surface v1.0 Decision
status: FROZEN / CURRENT
version: 1.0
created: 2026-09-09
updated: 2026-09-09
phase: G6 Package 4 Core Mechanics Visibility
owner: Owner + GPT
implementation_consumer: MW-028
---

# G6 System / Public Mechanics Surface v1.0｜CURRENT

## 1. Product question

`系统 / System` v1.0 回答：

> **“本局最近真正发生、并已经公开给我的机制判定是什么？程序到底掷出了什么、怎么算、结果是什么？”**

它不是角色状态面板，不是 Debug 日志，也不是一套新 mechanics engine。

Package 4 只拿现有 **Public d20** 作为第一个真实 mechanics consumer，证明：

```text
existing Public d20 durable owner
→ current accepted mechanics truth
→ bounded player-safe structural projection
→ 系统 Surface
→ Debug mechanics terminal evidence
→ Save / reopen / Restore currentness
```

## 2. Truth ownership

Public d20 的既有 durable mechanics owner 继续是唯一机制事实源。

`系统`：

- 不保存第二份骰点/结果；
- 不重新计算 outcome；
- 不根据 Narrative 猜测是否发生过检定；
- 不从 UI 状态反写 mechanics；
- 不触发模型调用。

程序只把**当前 Timeline 中已经 accepted、已经向玩家公开、并能与 accepted Conversation 精确配对的真实 mechanics 记录**投影给 UI。

继续遵守：

> **UI projects truth; UI is not a second truth.**

## 3. v1.0 player-facing content

### 3.1 System Surface only lists actual public CHECK records

v1.0 `系统` 的核心区块是：

> **近期公开判定**

只显示实际发生的 accepted `CHECK`，最多最近 **12** 条，玩家阅读顺序默认最近优先。

每条可展示的 player-safe facts：

```text
accepted turn position
intent
DC
modifier
modifier_reason
stance
situation_reason
raw_rolls
selected_roll
total
outcome
success_intent
failure_stakes
```

这些字段来自既有 Program-owned durable d20 truth；不让模型重新总结或重写。

### 3.2 NO_CHECK does not become a persistent player card in v1.0

现有 `NO_CHECK` durable resolution 继续保留，并继续服务于：

- no-reroll / currentness；
- GM mechanics continuity；
- Debug mechanics terminal evidence。

但它**默认不进入玩家 `系统` 的长期列表**。

原因不是认为 NO_CHECK “不真实”，而是玩家表面的本轮问题是“真正发生过哪些公开判定”。普通行动大量出现的“本次无需掷骰”若全部长期堆入 `系统`，会把 Surface 退化成内部 control log。

因此：

> **CHECK is a player-facing System record; NO_CHECK is a valid mechanics terminal but not a default persistent System card in v1.0.**

未来若真实 consumer 证明某类“免检/无需判定”本身有持续玩家价值，再扩展，不在本 Package 预造规则。

### 3.3 Empty state

当当前 Timeline 没有 accepted Public d20 CHECK 时：

> `暂无公开判定记录。`

不得用虚构 HP / MP / Mana / Hunger / Money / 属性 / Buff / 等级等内容填满 `系统`。

## 4. Existing inline dice card remains

Narrative Host 中当前每次 CHECK 的即时骰点卡继续保留。

二者职责不同：

```text
Narrative inline card
= 当前回合即时理解“刚才怎么判的”

系统 Surface
= 稍后仍可回看 current Timeline 中最近的真实公开判定
```

Package 4 不删除、不替换、不重设计现有 inline dice presentation。

## 5. One mechanics-owned safe projection seam

现有 MW-026 `公开机制历史投影器` 已经拥有一套经过 currentness 验证的 mechanics history selection，用于 GM context。

Package 4 不应复制第二套“哪些 check 是 current / accepted / player-safe”的逻辑。

推荐边界：

```text
Public d20 durable truth + accepted Conversation
→ mechanics L1 validates/selects current public records once
→ L3 exposes bounded structural System projection
→ existing GM context text is derived from the same validated record set
→ System leaf UI consumes structural projection only
```

允许为 System 增加新的 L3 player-safe method / DTO；但必须保留既有 `project_context()` 行为与 MW-026 continuity regression。

System DTO 不得包含：

```text
action_id
check_id
resolution_id
control raw payload / control proposal envelope
hash / prefix / persistence node id
credentials
raw world_state
NPC-private / Knowledge-private / Agency / Evolution material
Provider reasoning
```

Leaf UI 不得接收 Runtime 后自己过滤 omniscient truth。

## 6. Currentness / Restore

System projection 必须服从既有 authoritative currentness：

- accepted/current Conversation pairing；
- durable check acceptance marker；
- current Timeline；
- Save / reopen；
- Restore；
- corrected/replaced history；
- displaced-future isolation；
- ambiguous/conflicting mechanics records fail-soft 不展示。

不得创建新的 System currentness owner、缓存真相或 SQLite table。

特别注意现有时序：Conversation acceptance 可能先于 d20 acceptance marker 的 durable commit 完成。因此玩家 Surface 的“本回合检定已出现”刷新点必须能够发生在**adjudication terminal / mechanics acceptance 完成之后**，不能只依赖较早的 Conversation `generation_completed`，否则可能出现一回合延迟。

## 7. World Information Host placement

Package 4 后，当前可用导航为：

```text
概览 | 角色 | 重要经历 | 人物 | 事务 | 系统 | 存档
```

`行囊` 仍等待 Package 5 的真实 Inventory owner，不创建空壳入口。

`系统` 属于 **World Information Host**。

不要为了展示近期 d20 历史而重新打开左侧 Player Status Host。Player Status Host 仍只给未来真实的 live mechanics/status contribution，而不是历史记录。

## 8. Presentation

v1.0 是有限、直接、可读的 first-party Surface，不提前建设 generic Dynamic UI。

建议每条 CHECK 使用简洁 card/section：

```text
第 N 回合 · <intent>
结果：成功 / 失败
d20: <raw roll(s)> → <selected> + <modifier> = <total> vs DC <dc>
形势：<stance / situation reason>
修正：<modifier reason>
成功意味着：<success intent>
失败风险：<failure stakes>
```

允许做纯 presentation 中文标签映射，例如 `normal / advantage / disadvantage` → `正常 / 优势 / 劣势`，但不得改变数值/结果语义。

继续遵守 MW-023：ordinary gameplay visible text >=20px。窄窗口优先换行 / 纵向滚动，不缩小正文。

## 9. Debug mechanics lane

Package 1 Debug Mode 增加一个 bounded read-only `mechanics` lane。

它从现有 Public d20 adjudication 的结构化终态获取证据，至少应能区分：

- accepted CHECK；
- accepted NO_CHECK；
- already accepted / replay no-new-change；
- degraded/no-check fallback where applicable；
- failed；
- cancelled；
- stale/currentness-rejected where existing flow can produce it。

Debug 只显示安全结构信息，例如 terminal/change/reason code 与 bounded counts；不显示原始 control payload、Provider response、ID/hash、角色私密/world hidden truth。

Debug 不创建 Provider call，不改变 adjudication，不持久化。

## 10. No model semantic curation in System v1.0

与 `角色 / 重要经历 / 人物 / 事务` 不同，Public d20 是 Program-owned mechanics truth。

因此 System v1.0 **不经过 Information Curator**，也不让模型决定“这次骰点是否应该显示”。

规则很窄：

> 已 accepted/current 的真实 Public d20 CHECK → 可投影；否则不显示。

这是机器可判定的 mechanics currentness，不是开放语义 heuristic，不违反 Model Freedom First。

## 11. Explicit non-scope

Package 4 不实现：

- 新 mechanics engine；
- d20 balance / DC / modifier / stance 规则重设计；
- HP / MP / Mana / Hunger / Money / Level / XP；
- Character stats system；
- Buff / status-effect framework；
- generic mechanics registry；
- generic System schema；
- Equipment / Inventory；
- Quest/Open Threads 改造；
- Dynamic UI Host；
- generic Action Intent；
- Narrative Preference / Reality Correction；
- Source-authored external UI contract；
- shell general refactor；
- G3 Context debt cleanup。

## 12. Acceptance principles

Engineering acceptance requires evidence that:

1. existing accepted Public d20 CHECK appears in System with exact Program facts;
2. ordinary NO_CHECK does not flood player System but is observable in Debug;
3. unaccepted/stale/displaced/conflicting records do not appear;
4. Save/reopen preserves current records;
5. Restore before a check removes it; Restore after restores it;
6. the same validated mechanics-record seam powers System and existing GM context without currentness duplication;
7. no internal identity/control/private canary reaches UI;
8. System navigation/rendering causes zero Provider calls and zero durable writes;
9. inline Narrative dice card remains intact;
10. Debug mechanics lane truthfully reflects mechanics terminal behavior;
11. 960×540 / 1280×720 / 1920×1080 remain readable and operable at >=20px;
12. no invented gameplay state is introduced.

Product confirmation may be combined with the later concentrated Owner UAT / Package 7 Reality Gate unless Owner requests earlier.
