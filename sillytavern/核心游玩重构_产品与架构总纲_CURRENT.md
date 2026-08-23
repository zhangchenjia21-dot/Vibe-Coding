---
title: 酒馆游戏｜核心游玩重构产品与架构总纲
status: current-canonical-product-spec
version: 0.2
updated: 2026-08-23
project: 酒馆游戏新版主体
supersedes_narrow_interpretations:
  - 玩家授权是所有世界变化前置条件
  - 叙事只能复述既有正式结果且不得产生任何新世界内容
  - 世界主动发展必须先有玩家输入中的直接证据
---

# 酒馆游戏｜核心游玩重构产品与架构总纲 CURRENT

## 0. Product Reset

Project Owner 已明确：当前最大问题不是缺少更多外围系统，而是进入游戏后不好玩。即使其它能力暂缓，酒馆游戏的核心必须实现：

> 玩家进入一个会主动发展的世界，在其中做有意义的选择，推动剧情与世界持续演化，并不断产生新的局势、人物、机会、风险与后果。

因此本阶段属于重大产品方向重置。旧工程正确性继续作为基础，但任何会实质阻止上述核心体验的历史限制都必须重新审查，不因“已冻结”而自动保留。

当前状态：

```text
Product Definition Reset = PASS
Architecture / Capability Survey = AUTHORIZED / NEXT
Implementation = NOT YET AUTHORIZED until architecture survey closes
Owner UAT = BLOCKED / NEEDS CORE PLAYABILITY REBUILD
```

---

## 1. Product Promise

酒馆游戏不是安全聊天壳，也不是等待玩家自己编故事的空白沙盒。

核心承诺：

```text
世界主动提出局势
+
玩家自由决定如何回应
+
选择产生正式后果
+
NPC / 世界继续反应
+
世界持续增长与变化
+
玩家自然产生下一步动机
```

最终体验必须让玩家感到：

- 我进入的是一个正在发生事情的世界；
- 我不需要先知道“正确指令”才能遇到内容；
- 我可以自由行动，而不是被按钮或候选列表锁死；
- 我的选择会改变后续局势；
- 世界中的人物、地点和事件会继续存在并演化；
- 我愿意主动继续下一回合。

### 1.1 最低体验基准：不得劣于直接 AI 主持

Project Owner 明确增加最低产品基准：

> 如果玩家直接在通用网页版 AI 中说“我要玩酒馆游戏，请你主持”，得到的主动剧情、角色互动、场景推进、创意回应和继续游玩的欲望明显优于正式产品，则酒馆游戏核心产品目标视为失败，不得以长期状态、安全性、资产协议、存档恢复或工程复杂度为理由宣布成功。

因此正式产品必须至少保留普通优秀 AI 主持体验中的核心优势：

- 主持人主动开场，而不是等待玩家自编剧情；
- 世界会主动发生事情；
- NPC 会主动行动和说话；
- 模型可以合理创造尚未预枚举的人物、地点、事件和细节；
- 玩家可以用自然语言自由尝试，而不是学习隐藏命令；
- 主持人能够承接玩家意图并创造有后果的后续；
- 每个重要发展都应自然产生下一步可回应的局势或钩子。

在此基础上，酒馆游戏才有资格通过自身架构获得额外优势：

```text
优秀 AI 主持体验
+
长期 canonical world
+
可持续 NPC / 地点 / 关系 / 局势
+
正式规则与判定
+
Save / Restore / Recovery
+
资产生态与可复用世界
```

架构复杂度本身不构成产品价值。

---

## 2. 核心错误：把 Player Authorization 扩大成 World Authorization

必须废除以下全局假设：

```text
没有玩家当前输入的授权
→ 世界不得发生新的有意义变化
```

正确模型改为按行为主体分权。

### 2.1 Player Agency

只有“玩家本人做了什么”需要玩家当前输入授权。

例如：

- 玩家移动；
- 玩家攻击；
- 玩家承诺；
- 玩家交付物品；
- 玩家说了某句话；
- 玩家主动等待到某时间。

模型不得替玩家补做、补说、补答应。

### 2.2 World / NPC Agency

NPC、组织、环境、事件和时间推进拥有独立世界行动权，不需要玩家输入授权。

允许：

- NPC 主动来到当前场景；
- NPC 主动说话、请求、欺骗、威胁、离开、追踪；
- 世界发生消息、机会、危机、冲突、环境变化；
- 已有承诺、目标和背景进程产生后续；
- 新人物、新地点、新连接在合理剧情需要下产生。

这些内容由 Model author proposal，Program / Domain Owner 校验并提交，不走 Player Authorization Gate。

### 2.3 System / Rule Agency

时间、计时器、确定性资源变化、规则后果继续由 Program 直接推进；需要开放语义创作时再调用模型。

---

## 3. 新总原则：Model Freedom + Typed Reality Commit

旧的两个极端都禁止：

```text
A. Model 写什么就自动成为世界事实
B. Model 几乎不能创造任何会进入世界的新内容
```

新的总原则：

> **Model 可以大胆创造候选世界内容；Program 只负责判断这些内容是否可以成为本局现实。**

Program 的价值不再是替模型决定“故事接下来应该发生什么”，而是：

- player agency；
- schema / identity；
- immutable anchors；
- world constraints；
- dependency / capability；
- RNG / contested resolution；
- atomic commit；
- persistence / recovery；
- information boundary。

不得把 Program validation 重新演化成一套僵硬的剧情白名单。

---

## 4. Narrative Freedom 分层

旧的“所有 Narrative 新内容都危险”解释废除。

### 4.1 Ephemeral Narrative Detail｜自由

模型可直接创造不会形成长期权威事实的氛围与感官细节，例如：

- 天色、气味、杂音；
- 不具持续身份的路人；
- 房间中的普通装饰；
- 即时表情、语气、动作描写；
- 不会被后续依赖的局部纹理。

这些无需先创建数据库实体。

### 4.2 Durable / Interactable Content｜同回合物化

一旦内容满足任一条件：

- 有稳定身份；
- 玩家可以继续互动；
- 会影响后续选择或规则；
- 需要 Save / Restore 后继续存在；
- 后续剧情会引用；

则必须在被当作持续现实之前，通过 typed materialization / mutation commit 成为 Game-local Canonical truth。

规则从：

```text
No Phantom World Change
```

收窄为：

```text
No Phantom Player Action
+
No Phantom Durable Consequence
+
No Phantom Interactable Identity
```

纯叙事纹理不再被误判为正式状态污染。

---

## 5. Core Turn Architecture 目标

下一架构不再以“玩家输入解释 → 固定结果 → 叙事复述”作为完整回合。

目标应收敛为：

```text
Current World Frame
+
optional Player Input
↓
Model authors Turn / World Proposal
  - player intent interpretation（若有）
  - NPC / world reactions
  - new beat / hook
  - optional materialization
  - optional canonical patch
  - narrative plan / scene texture
↓
Program partitions authority
  - Player slice → player authorization
  - World slice → world authority / constraint validation
  - Rule slice → deterministic / contested resolution
↓
Atomic Canonical Commit
↓
Narrative realization with ephemeral freedom
↓
Player-safe projection
↓
next hook / next decision
```

T0 允许没有玩家输入：世界必须能够主动形成 Opening Situation。

不得要求 world proposal 的 evidenceText 来自玩家 raw input。

---

## 6. World Initiative 是核心，不是附属事件系统

World Initiative 必须能够：

- 开局主动点火；
- 在局势停滞时生成合理发展；
- 让 NPC 主动行动；
- 让已有事件 / 承诺 / 目标继续推进；
- 根据时间和前序后果产生反应；
- 自然引入新 NPC / place / clue / threat / opportunity；
- 在玩家重要选择后形成新的局面。

但它不得：

- 替玩家决定回应；
- 为制造戏剧性而无视世界锚点；
- 随机轰炸事件；
- 每回合强行转折；
- 为“有内容”而无限生成无记忆实体。

---

## 7. 开放世界增长

继续采用：

```text
Source Asset
↓
Game-local Canonical Assets
↓
Runtime State
```

但必须真正实现“living world”语义：

- 新 NPC 不要求预先存在于 Character Card；
- 新 Scene / Place 不要求预先枚举在 Source World；
- 世界关系图可以在剧情中增长；
- 本局角色与地点可以通过正式 patch 演化；
- Source 不被反写；
- Save / Restore / Branch 保留本局演化。

Runtime World Materialization 不再只是“玩家说出未知对象后的补洞器”，而成为世界生长的正式基础设施。

---

## 8. Choice / Consequence 要求

核心玩法不以“每回合有多少文字”为衡量标准。

重要玩家选择至少应能改变以下之一：

- 可访问地点；
- NPC 态度 / 关系；
- 信息与认知；
- 资源 / 物品；
- 风险 / 敌意；
- 承诺 / 义务；
- 机会窗口；
- 当前局势；
- 后续可选项；
- 世界中谁会采取下一步行动。

如果两个不同选择长期只产生不同措辞、后续世界几乎一样，则核心循环失败。

---

## 9. Success Gate

下一轮真人验收至少必须证明：

1. 新局进入后不靠玩家自编剧情即可出现具体局势；
2. 数回合内自然遇到第二个有持续身份的角色；
3. 数回合内自然获得或进入第二个有意义场景；
4. NPC / 世界至少一次主动发起玩家可回应的发展；
5. 玩家至少一次重要选择显著改变后续局势；
6. 一次后果会自然产生新的问题、机会或风险；
7. 玩家不需要学习隐藏命令或先知道实体名称；
8. 模型叙事明显更自由，但不会替玩家做决定；
9. Save / Restore 后动态世界增长保持一致；
10. 与“直接让通用网页版 AI 主持同一酒馆游戏”的对照体验相比，正式产品在主动性、创意承接、场景推进和继续游玩欲望上不得明显更差；
11. Project Owner 明确给出“我愿意继续玩”的体验结论。

工程测试通过只能证明实现可用，不能替代第 10–11 条。

---

## 10. 保留 / 废除 / 重审

### 保留

- Player Agency；
- Source != Game-local != Runtime；
- canonical identity；
- atomic commit；
- Save / Restore / Recovery；
- hidden knowledge boundary；
- contested outcome / RNG 的 Program ownership；
- Model authors candidates / Program commits durable reality。

### 废除或收窄

- 所有世界变化都必须由玩家输入授权；
- world materialization 必须被当前 Player Candidate 精确引用；
- 叙事只能输出已经枚举过的全部实体与细节；
- No Phantom 被解释成“模型不能自由增加任何环境内容”；
- Candidate Directory 被当成世界可发生内容的白名单。

### 必须重审

- `RuntimeMaterializationNeed` 只允许 place/move 与 character/dialogue；
- `materializationUsedByCandidate()` 的全局适用范围；
- Narrative Provider “只能实现 Program 已冻结 Outcome”的边界是否过窄；
- Continuity / Background Progression 是否只能消费预先存在的状态；
- T0 静态启动；
- Suggested Actions 与玩家自由输入关系；
- Formal Turn 是否只能由 Player Initiative 驱动；
- 世界主动行为和玩家回合之间的事务边界。

---

## 11. 当前任务顺序

```text
Canonical Product Reset = PASS
↓
Runtime / Narrative / Materialization / Authority 全边界审计
↓
新 Turn + World Initiative 架构规格
↓
Shared Foundation implementation
↓
最小真实刘备纵向
↓
Independent Review
↓
Owner 真人连续试玩
↓
Playability PASS 才继续其它路线
```

在此 Gate 关闭前：Library、更多通用核心扩展、G10、G11、G12 均保持后置。
