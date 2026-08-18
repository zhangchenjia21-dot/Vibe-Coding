# G9｜World Pack Opening Scenario / Prologue Runtime 概念提案 DRAFT v0.2

状态：`DISCUSSION DRAFT / NOT FROZEN / NOT IMPLEMENTATION AUTHORITY`
日期：2026-08-19
性质：Project Owner gameplay reflection / product-architecture exploration
supersedes: `G9_世界包OpeningScenario与序章Runtime概念提案_DRAFT_v0.1_2026-08-19.md`

> 本文件只记录讨论方向，不构成 current architecture decision，不修改正在执行的 G9-02A，不授权 Codex/Grok 实现。只有后续多轮讨论形成正式裁定后，才传播到 G9-02B/C、G9-03 与资产线。

## 1. 产品问题

AI-assisted Creation 后直接进入开放世界时，玩家可能对新世界缺少认知、情绪抓手与继续探索动力。因此讨论 World Pack 是否应提供作者精心设计的一个或多个 Opening Scenario / Prologue，用于：

1. 快速建立世界体验与情绪；
2. 给玩家明确但不等同于长期 Objective / Mainline 的短期开场抓手；
3. 可选承担教程职责；
4. 完成或中止后自然移交 NORMAL_SANDBOX_MODE。

当前 G8 Runtime 只有：

```text
Creation semantic materialization
→ Structured Opening Beat
→ authority-safe Opening Narrative
→ persisted Turn-0 prose
→ NORMAL_SANDBOX_MODE
```

尚无 multi-beat Scenario state machine、author-scripted progression、branch graph、scenario abort/exit、tutorial composition 与 explicit scripted → sandbox transition。

---

## 2. 开局双轨｜暂定高置信方向

创建 World Pack 游戏时，产品入口暂定为：

```text
选择 World Pack
↓
选择开局方式

├─ 选择序章
│  ↓
│  选择作者提供的 Opening Scenario
│  ↓
│  只能选择该 Scenario 提供的 Preset Protagonist
│  ↓
│  PROLOGUE_SCENARIO_MODE
│
└─ 自由开局
   ↓
   当前 AI-assisted Creation
   ↓
   任意合法 Player Identity
   ↓
   Dynamic Opening Beat
   ↓
   NORMAL_SANDBOX_MODE
```

这样不再要求单个 Scenario 适配 World Pack 的全部主角身份。

### 2.1 Preset Protagonist

Preset Protagonist 建议区分：

```text
Scenario-critical Locked Facts
+
Safe Customization
```

可锁定：
- 社会身份 / role family；
- 开局时间与地点；
- 与关键 NPC 的必要既有关系；
- 必要开局知识；
- 必要 Item / 文书 /职责；
- Scenario 成立所需硬事实。

可允许玩家修改：
- 姓名；
- 性别；
- 外貌；
- 语言表达；
- 不破坏 Scenario 的人格倾向。

若 Preset 本身是历史人物，应引用 Character Card，不复制第二份完整人格 Definition。

---

## 3. Strong Guided Prologue｜暂定方向

当前偏好 B｜Guided Prologue，但采用较强 Guide：

- 有明显 Action Pressure；
- 剧情持续催促玩家作出选择 /行动；
- Scenario 长度有限；
- 每个剧情 Beat 的大方向由 Creator author；
- 玩家不同输入进入少量有限分支；
- 分支允许反复收束，避免指数爆炸；
- 当前 Beat 上方 exactly five recommended inputs 由 Creator 完整 author；
- 五个推荐行动用于控制玩家行动方向；
- 玩家仍可自由输入，不构成 action whitelist。

正式区别：

```text
NORMAL_SANDBOX_MODE
AI-driven open story growth

PROLOGUE_SCENARIO_MODE
Author-defined finite branch graph
+ AI semantic interpretation / realization
+ Program-owned progression
```

---

## 4. 固定长度：Beat 而不是 Raw Input

建议固定：

```text
Beat 1 / N
...
Beat N / N
→ completion / exit
```

而不是“所有玩家输入都自动消耗一个剧情回合”。

玩家输入后：

```text
A. 命中当前 authored branch intent
→ Formal Turn
→ Beat/Branch progression

B. 合理旁枝但不改变 Beat 条件
→ 响应
→ Beat 保持

C. 不成立 / 无意义 /需澄清
→ 正常反馈
→ Beat 保持

D. 合法但破坏 Scenario 前提
→ breakout handling
```

因此固定的是作者设计的有效剧情 Beat 数，而不是用户敲了多少次 Enter。

---

## 5. Creator-authored Dynamic Five Replacement

PROLOGUE_SCENARIO_MODE 中，普通 Sandbox Dynamic Five 暂停，改为当前 Beat 自带：

```text
fiveAuthoredSuggestions[5]
```

这些 Suggestion：
- exactly five；
- 由 Creator author；
- 强烈引导当前 Beat 的有效行动方向；
- 继续遵守 `click = prefill, not auto-submit`；
- Suggestion Text != Branch ID。

真正的 authored graph 是 Branch Intent / Transition。AI Semantic 将自由输入映射到 branch intent；Program 不用关键词 / regex 做自然语言判断。

---

## 6. Converging Branch Graph

禁止按 `5^N` 独立铺满所有分支。

推荐：

```text
Beat A
├─ Branch 1
├─ Branch 2
└─ Branch 3
   ↓
保留局部 consequence / flags / knowledge / relationship
   ↓
在后续 Beat 重新汇合
```

汇合不代表选择无意义；差异可通过真实 canonical consequence 持续存在。

---

## 7. Action Pressure｜序章与正式游戏的双层设计

### 7.1 序章

Action Pressure 是 authored core mechanic。

每个重要 Beat 应尽量给出明确的当前张力与待响应问题，让玩家自然产生：

> “我现在要怎么处理？”

而不是：

> “所以我要干什么？”

它可以表现为：
- NPC 正等待答复；
- 当前时间压力；
- 必须处理的小型冲突；
- 明显的未知与机会；
- 即将失去的窗口；
- 当前局面迫使玩家选择态度 / 行动。

### 7.2 Normal Sandbox

Project Owner 进一步提出：Action Pressure 不应只属于序章，在正式 Sandbox 也应适量存在，以提高剧情推进能力与玩家体验。

但正式游戏必须保持节奏起伏：

```text
Pressure /紧凑推进
↔
Neutral /自由探索
↔
Breathing /休息、闲聊、生活、反思
```

因此：

> **Action Pressure is a pacing tool, not an every-turn requirement.**

正式 Sandbox 不应每回合强制“出事”；持续高压会让世界失去生活感、社交空间和情绪对比。

当前只冻结为讨论原则，不冻结：
- Pressure meter；
- 固定频率；
- Program pacing score；
- Planner API；
- AI 调用次数。

主观节奏判断原则上应由 AI/未来 Story-Pacing responsibility 完成；Program 负责既有正式事实、时间、事件与约束，不应靠关键词猜“现在该不该制造剧情压力”。

---

## 8. Breakout / 脱轨处理｜暂定方案

当前暂定：

### Level 1｜语义变体
玩家输入与已有 authored branch 意义相同。

→ AI Semantic 映射 branch，正常推进。

### Level 2｜旁枝但不破坏前提
例如观察无关细节、补充一句话、短暂互动。

→ 正常响应；Beat 可保持不变。

### Level 3｜合法但破坏 Scenario 前提
例如剧情要求接受城门盘查，玩家却选择发动严重暴力并成功造成不可逆后果。

当前暂定产品行为：

```text
System detects Scenario continuation would become invalid
↓
提示玩家：该行动将导致当前序章无法继续，并提前进入自由游戏
↓
Player confirms
↓
正常 Runtime resolves action
↓
已发生 canonical consequences 保留
↓
Scenario terminates safely
↓
NORMAL_SANDBOX_MODE
```

若玩家取消，则保持当前 Beat，不替玩家执行该脱轨行动。

未来若出现更优体验方案可再调整；当前只作为 provisional direction。

`Scenario Authority != Player Action Whitelist` 继续成立。

---

## 9. AI / Creator / Program Authority

建议：

```text
Creator / World Pack Author
owns:
Scenario Definition
Beat graph
Branch intents
Action Pressure
Five Suggestions
Authored invariants
Completion/Breakout semantics

AI Semantic
owns:
开放式玩家输入理解
自由文本 → current branch intent / side action / breakout candidate

Program / Scenario Runtime Host
owns:
current Scenario/Beat/Branch state
structural validation
transition legality
Formal Outcome / canonical commit
completion / breakout state transition
Save / Restore / Recovery

Narrative AI
owns:
在已确定 Scenario state + Formal Outcome 下进行自然对白与叙事 realization
```

Narrative AI 不拥有下一剧情节点选择权。

---

## 10. Mainline / Trend Neutrality

Opening Scenario 默认遵守：

```text
local hook / onboarding
!=
mainline trend mutation
```

避免赤壁、官渡、政权覆灭、关键人物必死等宏观未来节点作为必经开场。

Scenario 可产生真实 Game-local：
- NPC；
- Item；
- Knowledge；
- Relationship；
- low-impact Event；
- local Place / Connection。

完成后这些真实事实继续保留。

若玩家主动产生足以影响宏观历史的行为，则按 Breakout 进入 Sandbox，由正式 Runtime authority 接管。

---

## 11. Tutorial Composition｜仍未冻结

不能假设所有 Expansion 都启用。

暂时保留：

```text
Base Opening Scenario
+ optional Tutorial Inserts / Variants
```

潜在设计仍包括：
1. World Pack author optional tutorial beats；
2. Expansion 提供 generic tutorial contribution；
3. World Pack owns Scenario，Expansion contributes capability-specific tutorial units，然后做 world-specific binding。

当前倾向 3，但尚未冻结。

---

## 12. Runtime Isolation

如果最终采用，应至少区分：

```text
PROLOGUE_SCENARIO_MODE
↓ completion / confirmed breakout
NORMAL_SANDBOX_MODE
```

Prologue Mode 可能需要：
- scenario instance identity；
- preset protagonist binding；
- current beat / branch；
- authored invariant refs；
- five authored suggestions；
- allowed persistent consequence scope；
- breakout pending/confirmed state；
- save / restore / recovery；
- deterministic transition to sandbox。

不能仅靠 System Prompt 告诉模型“照着剧本演”。

---

## 13. G9 Propagation Boundary

### G9-02A

`NO CHANGE / DO NOT INTERRUPT`。

Source Binding + Game-local Revision 是未来把 Source Scenario / Preset Protagonist bind 为 Game-local Instance 的基础。

### G9-02B

在正式 Task Freeze 前必须完成本议题核心裁定；若采用，评估：
- built-in Scenario Runtime Owner / Domain Module；
- Game-local Scenario record；
- beat / transition / breakout contract；
- preset protagonist binding seam；
- authored five suggestion seam；
- tutorial contribution seam。

### G9-02C

证明只加载 current Scenario + current Beat + required entities / enabled tutorial unit，不加载整个 Scenario Library。

### G9-03

Runtime vertical 证明后才决定 external representation。

### G9-05 Creator

未来 Creator 才决定完整 Scenario authoring UI / graph editor / preset protagonist editor / compatibility preview。

---

## 14. 当前高置信但仍未正式冻结的方向

- World Pack creation 提供 `选择序章 | 自由开局`；
- 序章只允许 Scenario-specific Preset Protagonists；
- Guided Prologue 使用强 Guide；
- finite authored beat graph；
- creator-authored exactly-five suggestions per Beat；
- AI interprets free input within authored graph；
- Program owns progression；
- Narrative cannot freely invent next plot direction；
- severe valid derail → warn → confirm → preserve consequences → Sandbox；
- Action Pressure 在序章中高密度使用，在正式 Sandbox 中适量使用并保留高低节奏变化。

仍不冻结：
- Opening Scenario 是否所有 World Pack mandatory；
- Scenario 默认是否开启；
- exact beat count；
- exact branch schema；
- Tutorial taxonomy / composition contract；
- Story pacing implementation；
- asset-spec fields；
- Creator UI；
- 三国具体 Scenario 内容。
