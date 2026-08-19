# G9｜World Pack Opening Scenario / Prologue Runtime 概念提案 DRAFT v0.3

状态：`DISCUSSION DRAFT / NOT FROZEN / NOT IMPLEMENTATION AUTHORITY`
日期：2026-08-19
性质：Project Owner gameplay reflection / product-architecture exploration
supersedes: `G9_世界包OpeningScenario与序章Runtime概念提案_DRAFT_v0.2_2026-08-19.md`

> 本文件只记录讨论方向，不构成 current architecture decision，不修改正在执行的 G9-02A，不授权 Codex/Grok 实现。只有后续讨论形成正式裁定后，才传播到 G9-02B/C、G9-03 与资产线。

## 1. 当前高置信产品形态

World Pack 创建游戏时暂定双轨：

```text
选择 World Pack
↓
选择开局方式

├─ 选择序章
│  → 选择作者提供的 Opening Scenario
│  → 选择该 Scenario 的 Preset Protagonist
│  → PROLOGUE_SCENARIO_MODE
│
└─ 自由开局
   → AI-assisted Creation
   → 任意合法 Player Identity
   → Dynamic Opening Beat
   → NORMAL_SANDBOX_MODE
```

Preset Protagonist 使用：

```text
Scenario-critical Locked Facts
+
Safe Customization
```

历史人物 Preset 引用 Character Card，不复制第二份完整人格 Definition。

---

## 2. Strong Guided Prologue｜作者只写关键 Beat，不写剧情分支树

本轮进一步简化：

> **取消“作者预写剧情 Branch Tree”作为默认模型。**

作者只负责设计有限的关键 Beat 序列：

```text
Beat 1
→ Beat 2
→ Beat 3
→ ...
→ Beat N
→ NORMAL_SANDBOX_MODE
```

每个 Beat 至少 author：

- Beat identity / sequence；
- 当前 authored situation；
- Action Pressure；
- exactly five creator-authored recommended inputs；
- 当前 Beat 的核心体验/教学意图；
- 下一 Beat 的 authored entry condition / required invariants；
- 本 Beat 可允许产生的 persistent consequence scope；
- breakout / abort boundary。

作者不需要穷举：

- 玩家每一种自由输入；
- 五个推荐项分别对应的完整分支剧情；
- 多层分支树；
- 所有对白和装饰性细节。

---

## 3. AI Convergence / Bridge Responsibility

PROLOGUE_SCENARIO_MODE 中，AI 的特殊职责不是开放式决定“下一段剧情去哪”，而是：

> **在不伪造玩家行动、不违反已提交世界事实、不越过作者 Beat invariant 的前提下，将当前玩家输入造成的合法局部结果自然桥接到下一 authored Beat。**

建议链：

```text
Current Beat
+ authored next-Beat contract
+ current canonical world
+ Player Input
↓
Semantic AI
↓
interpret current action / intent
↓
Program normal validation / Formal Outcome
↓
Scenario Bridge / Narrative realization
↓
check next-Beat preconditions
↓
advance Beat OR remain current Beat OR breakout
```

因为五个推荐行动由作者设计，正常玩家路径预计天然接近下一 Beat，AI 主要负责处理：

- 同一行动的自由语言变体；
- 不同态度 /说法；
- 小型局部动作；
- 轻微不同结果；
- 不影响关键 Beat 的人物/知识/关系/物品差异。

---

## 4. Beat Convergence 不是“剧情修正力”

这是本模式最重要的隔离规则。

禁止使用：

```text
next Beat 必须发生
→ Runtime / Narrative 强行改写现实
```

例如下一 Beat 需要“导师仍在场”，但玩家已经通过合法正式行动使导师离开、昏迷或死亡，则不能为了序章继续而让导师瞬移回来或忽略正式结果。

因此下一 Beat 必须有明确 precondition。

```text
Preconditions still satisfiable
→ AI 可自然 bridge / converge

Preconditions temporarily unmet but repairable without overriding player agency
→ Beat may remain / use bounded adaptation

Player action destroys a required authored invariant
→ breakout handling
```

正式世界事实、Player Agency、Formal Outcome 永远高于 Scenario convergence。

---

## 5. Beat 推进与非推进

固定的是 authored Beat 数，而不是 Raw Player Input 数。

玩家输入后建议区分：

### A｜可推进
行动完成后下一 Beat preconditions 成立。

→ commit current consequences
→ advance to next Beat

### B｜旁枝 /局部互动
合理，但没有改变当前 Beat 的核心条件。

→ 正常响应
→ Beat 保持

### C｜失败 /澄清
行动不成立或信息不足。

→ 正常 Formal Outcome /反馈
→ Beat 保持

### D｜破坏 Scenario 前提
合法玩家行为使未来 authored Beat 无法诚实成立。

→ breakout flow

---

## 6. Breakout / 脱轨暂定方案

当前暂定：

```text
Scenario Host 判断：
若继续执行当前合法行动会使序章无法继续
↓
产品提示：
“继续该行动将结束当前序章，并提前进入自由游戏。”
↓
玩家取消
→ 留在当前 Beat，不执行该脱轨动作

玩家确认
→ 正常 Runtime 执行动作
→ 所有真实 consequences 保留
→ Scenario terminated
→ NORMAL_SANDBOX_MODE
```

未来真实 UAT 若证明确认交互打断沉浸，可再寻找更自然的表达，但当前优先保护 Player Agency 与 authored Scenario integrity。

---

## 7. Creator-authored Five

PROLOGUE_SCENARIO_MODE 中：

```text
普通 Sandbox Dynamic Five
→ 暂停

current Beat
→ exactly five creator-authored recommended inputs
```

规则：

- exactly five；
- 作者控制；
- 强 Action Pressure；
- click = prefill, not auto-submit；
- 不是 action whitelist；
- 玩家仍可自由输入。

五个建议本身应尽量确保：

> 按推荐行动游玩时，正常情况下都能够以合理方式推进或准备推进下一个 Beat。

这样 AI 的 convergence 工作量会保持可控。

---

## 8. Action Pressure

### 8.1 序章

Action Pressure 属于 authored core design。

每个重要 Beat 应让玩家明显感到：

> “当前有一个值得我现在回应的问题。”

### 8.2 Normal Sandbox

Action Pressure 也可以作为正式游戏的 pacing tool，但必须适量：

```text
Pressure /紧凑推进
↔
Neutral /自由探索
↔
Breathing /休息、闲聊、生活、反思
```

正式游戏不得每 Turn 强制造事。

```text
Action Pressure
!= Objective
!= mandatory quest
```

---

## 9. HARD ISOLATION｜剧情收敛权只能存在于 Prologue

正式冻结为本 Discussion Draft 的最高风险约束：

> **AI 为了抵达作者指定的 next Beat 而进行剧情收敛，只能存在于 `PROLOGUE_SCENARIO_MODE`。绝对不得复用到普通 `NORMAL_SANDBOX_MODE`。**

两种 Runtime 语义必须显式不同：

```text
PROLOGUE_SCENARIO_MODE
- has scenarioInstanceRef
- has currentBeatRef
- has nextBeat authored contract
- Scenario Host may request bounded convergence/bridge

NORMAL_SANDBOX_MODE
- no authored nextBeat target
- no convergence target
- no “bring story back on track” authority
- future story must emerge from current world causes / player action / NPC agency
```

禁止在 Sandbox 中出现：

- “主线要求下一幕发生 X，所以强制世界收敛到 X”；
- 忽略玩家已改变的现实以恢复作者轨道；
- NPC 因剧情需要强行改变意图；
- 为抵达某预设剧情点伪造 Coincidence / teleport / mandatory event；
- 把 Action Pressure 误用成 hidden railroading。

普通 Sandbox 继续遵守：

```text
Past / Current Canonical Truth
+ Player Action
+ NPC Agency
+ World Rules
→ Future
```

而不是：

```text
Desired Future Plot
→ reverse-engineer current reality
```

该隔离必须未来通过 Runtime contract / mode guard / negative tests 证明，不能只依赖 Prompt 约定。

---

## 10. Tutorial Composition 进一步简化

取消复杂 Branch Variant 后，作者工作量进一步下降。

World Pack 作者可以在某些 Beat 声明 Tutorial Slot / capability intent；Expansion 提供 generic tutorial semantics。

最终每局在进入序章前根据 Enabled Feature / Module 编译：

```text
Base authored Beat sequence
+ compatible optional tutorial requirements
→ Game-local Prologue Plan
```

但仍不要求作者为每个 Expansion 组合写独立完整分支。

原则：

- World Pack owns dramatic Beat；
- Expansion owns mechanism teaching semantics；
- AI 在 Beat 约束内自然实现具体教学过程；
- Program / Domain Runtime 继续拥有正式机制结果。

Tutorial Composition 细节仍未冻结。

---

## 11. Persistence / Real Consequences

序章发生在真实 Game-local world，不是 disposable simulation。

合法 commit 的：

- Character；
- Item；
- Knowledge；
- Relationship；
- local definition mutation；
- Runtime state；

完成/中止序章后继续保留。

但 Scenario 默认不主动 author 宏观 mainline / Politics / War / History trend mutation。

---

## 12. G9 关系

### G9-02A

`NO CHANGE / DO NOT INTERRUPT`。

### G9-02B

若后续正式冻结本方向，需要评估最小：

- Scenario Runtime Owner / Host；
- Game-local Scenario Instance；
- currentBeat / nextBeat contract；
- scenario-only convergence/bridge capability；
- hard mode isolation guard；
- breakout transition；
- tutorial contribution seam。

### G9-02C

Scenario working set 只加载：

- current Beat；
- next Beat minimal contract；
- current required entities/state；
- enabled tutorial slices。

不得加载全部 Scenario Library。

同时必须有 negative proof：

```text
NORMAL_SANDBOX_MODE
→ scenario convergence capability unreachable / unavailable
```

### G9-03

只有 Runtime vertical 证明后才决定 external machine representation。

### G9-05 Creator

Creator 应让 World Pack 作者主要编辑：

- Scenario；
- Preset Protagonists；
- ordered Beats；
- Action Pressure；
- exactly five suggestions；
- next Beat requirements；
- tutorial slots；
- breakout-sensitive invariants。

不要求手工维护大型 Branch Tree。

---

## 13. 当前仍未冻结

- Opening Scenario 是否 World Pack mandatory；
- 推荐 Beat 数量；
- Preset customization exact policy；
- exact Beat transition contract；
- tutorial taxonomy / composition wire；
- Scenario asset ownership form（embedded / child / standalone）；
- exact confirmation UI；
- external asset-spec fields；
- Creator UI；
- 三国具体序章内容。
