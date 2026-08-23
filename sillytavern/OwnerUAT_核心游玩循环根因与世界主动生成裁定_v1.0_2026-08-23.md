---
title: Owner UAT｜核心游玩循环根因与世界主动生成裁定
status: current-decision-frozen
version: 1.0
updated: 2026-08-23
project: 酒馆游戏新版主体
---

# Owner UAT｜核心游玩循环根因与世界主动生成裁定 v1.0

## 0. Owner UAT 直接证据

Project Owner 在真实游玩中确认：

1. 进入游戏后漫无目的，缺少下一步引导；
2. 长时间停留在初始场景，无法自然进入第二场景，也无法遇到第二个可持续记录的角色；
3. 几乎碰不到 NPC、敌人、机会、时间压力、承诺和局势；
4. 因为没有遭遇和局势，玩家也没有机会形成有意义行动—反馈闭环；
5. 每回合结束后没有推动继续游玩的钩子。

因此当前核心产品阻塞不是资产数量不足，而是 Runtime 缺少足够的世界主动性与合法剧情生长通道。

---

## 1. 根因裁定

当前实现已经具备 Runtime World Materialization 基础，但正式运行路径主要是 **player-demand-driven materialization**：

```text
Player raw input
→ Semantic AI 判断 materializationNeed
→ place / character
→ Program typed validation
→ 新地点 / 新角色进入 Game-local Canonical Assets
→ continuation move / dialogue
```

现行 `RuntimeMaterializationNeed` 只覆盖：

```text
place → continuation=move
character → continuation=dialogue
```

并且当前 materialization validator 明确要求：没有被已授权玩家 Candidate 精确引用的生成内容不得“顺便生成”进入正式世界。

该设计适合防止 Narrative phantom，但不足以支持一个主动发展的游戏世界。

结果是：

```text
玩家没有先提出目标
→ Runtime 不生成新的可交互世界内容
→ 玩家看不到新的目标 / 人物 / 场景 / 局势
→ 玩家更不知道该提出什么目标
→ 世界继续静止
```

这是一个自锁循环。

---

## 2. 这不是推翻 Program Authority

继续冻结：

```text
Program owns formal facts / outcomes / commit
Model interprets / proposes / authors bounded content
No Phantom World Change
Player Agency violations = 0
Hidden Knowledge boundary
Atomic Commit
Save / Restore / Recovery exactly-once
Source Asset != Game-local Canonical Asset != Runtime State
```

禁止直接改成：

```text
Narrative 想到什么
→ 文字里宣布发生
→ 事后把文字当事实
```

新的方向必须是：

```text
Model proposes world initiative
→ Program validates authority / schema / refs / constraints / consistency
→ Program commits accepted world development
→ Narrative realizes committed result
```

即：扩大模型的 **authoring space**，不交出正式世界的 **commit authority**。

---

## 3. 新增独立语义：World Initiative

下一阶段应建立与“玩家行动解释”分离的世界主动生成通道。

它回答的不是：

> 玩家这句话想执行什么？

而是：

> 基于当前世界、角色、时间、历史与未解决线索，世界现在有什么合理且值得玩家回应的发展？

该通道不得要求 proposal evidenceText 必须来自玩家 raw input，因为其语义来源本来就是世界自身，而不是玩家授权。

但它仍必须有独立的 Program-owned authority / validation / commit contract。

---

## 4. 第一阶段允许的世界主动内容

首轮不建设万能剧情引擎，只支持能直接改善核心循环的最小集合：

1. **开局钩子**：新局进入 Session 后必须快速出现至少一个可回应的问题、机会、威胁、邀请、消息或未决局势；
2. **角色进入**：世界可主动让已有角色或新 materialize 的 NPC 合理进入当前场景；
3. **场景开放**：世界可揭示 / materialize 可到达地点和连接，使玩家自然获得“可以去哪里”；
4. **遭遇 / 事件**：世界可提出玩家当前能感知并回应的局部事件；
5. **机会 / 威胁**：形成有时间性、代价或取舍的可选发展；
6. **线索 / 消息**：提供能改变玩家判断的新信息，但仍遵守 player knowledge boundary；
7. **后续钩子**：一次重要行动解决或改变局势后，结果应自然产生新的问题、机会或风险。

敌人、组织、承诺、任务、长期剧情线等只有在已有正式 Owner/contract 足够时才复用；不得为了首轮“好玩”一次性新建大而全系统。

---

## 5. 核心游玩循环

当前目标循环冻结为：

```text
World presents a concrete situation / hook
↓
Player understands plausible choices but remains free-form
↓
Player acts
↓
Program validates and commits meaningful outcome
↓
World / characters react
↓
Visible state, relationship, place, knowledge, opportunity or risk changes
↓
A new question / hook emerges
↓
Player wants another turn
```

Suggested Actions 可以辅助发现选择，但不得成为唯一玩法入口，也不得自动替玩家做决定。

---

## 6. T0 与连续回合要求

### T0

创建游戏成功后，不应只给一个静态场景说明然后等待玩家自行发明剧情。

T0 应产生一个经 Program 接受的 Opening Situation。它可以：

- 使用 Source / Game-local 已有角色与地点；
- 或在允许边界内 materialize 首个 NPC / place / local event；
- 给出可感知的即时局势；
- 至少暴露一个值得回应的钩子。

### 连续回合

不要求每回合都随机刷新事件。

World Initiative 的触发必须有节奏控制，例如：

- 当前没有 active hook；
- 玩家刚完成一个显著行动；
- 时间推进达到合理阈值；
- 未解决事件应继续推进；
- 当前场景长期无变化；
- 某个正式世界状态形成新的反应条件。

目标是世界有主动性，不是强制剧情轰炸或导演式抢夺玩家控制权。

---

## 7. 对现有 World Materialization 的修正方向

现有 player-demand materialization 继续保留，仍服务于：

```text
玩家主动说想去一个当前未物化的合理地点
玩家主动找一个当前未物化的合理人物
```

新增 world-initiative materialization 不应复用“必须由玩家 Candidate 精确引用”的授权规则。

应该形成两条合法来源：

```text
A. Player Initiative
Player Candidate
→ materialization need
→ Program validate
→ commit

B. World Initiative
World Beat Proposal
→ world-initiative authority
→ Program validate
→ commit
```

两者最终都进入同一 Game-local Canonical Asset / Runtime / Save-Restore 基础，不建立第二套世界事实源。

---

## 8. 最小可验证乐趣假设

下一实现波次不以“功能很多”为成功标准，而验证：

1. 玩家进入新局后很快能看见一个具体局势，而不是空等；
2. 玩家无需主动要求“生成一个 NPC”，就能自然遇到人物；
3. 玩家无需先知道地点名，世界也能合理开放第二场景；
4. 至少一次玩家选择能明显改变后续局势或可选项；
5. 世界能在玩家行动后产生反应，而非只生成总结文字；
6. 一段连续试玩中至少自然出现一次“我想看看接下来会怎样”的下一回合动机；
7. 上述改善不得制造 Narrative-only phantom，也不得绕过 Program commit。

最终仍由 Project Owner 真人试玩决定可玩性是否通过。

---

## 9. 当前阶段顺序

```text
Root Cause = CLOSED / DECIDED
↓
World Initiative + Core Play Loop Product Spec
↓
最小实现波次
↓
Independent Review
↓
Project Owner 真人连续试玩
↓
Playability PASS / 下一轮体验修正
```

在核心循环关闭前继续保持：

```text
Library Product = DEFERRED
通用核心大扩展 = DEFERRED
G10 Provider Expansion = NOT AUTHORIZED
G11 Alpha = NOT AUTHORIZED
G12 Release = NOT AUTHORIZED
```
