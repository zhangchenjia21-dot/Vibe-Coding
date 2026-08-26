---
title: 酒馆游戏｜DEC-P04｜Model Freedom & Recovery Philosophy
status: current-product-architecture-decision
version: 1.0
updated: 2026-08-23
project: 酒馆游戏新版主体
decision_id: DEC-P04
canonical_product_spec: 核心游玩重构_产品与架构总纲_CURRENT.md
---

# DEC-P04｜Model Freedom & Recovery Philosophy

## 0. 决策

Project Owner 正式将以下原则提升为一级产品 / 架构原则：

> **Freedom Before Prevention；Prefer Recovery over Prevention。**
>
> 对于低成本、可感知、可撤回、可重新生成、可回档的模型错误，默认优先让模型正常发挥，并把工程投入放在“容易发现、容易撤回、容易恢复”上，而不是为了理论上阻止一次错误，提前建设大规模审批、白名单、Schema、状态机和强制校验体系。

该原则不是“模型写什么都算真”，也不是取消 Player Agency、持久化和安全边界。它改变的是**默认风险哲学和证明责任**：

```text
旧默认：
模型可能犯错
→ 先预防
→ 再允许行动

新默认：
模型应完成核心职责
→ 若错误低成本且可恢复，先允许正常发挥
→ 发现错误后撤回 / 修正 / 重生成 / 回档
→ 只有风险足够高时才把预防机制放到前面
```

---

## 1. 风险成本必须对称

任何会限制模型主持能力、世界主动性、创作空间、上下文使用或自然交互的防错机制，在进入核心主干前必须回答：

1. 它防止的具体错误是什么；
2. 这个错误真实发生一次的用户成本是多少；
3. 错误是否容易被玩家 / 系统发现；
4. 是否可以撤回、重生成、修改、回档或从最近安全点恢复；
5. 预防机制本身会损失多少核心体验；
6. 预防机制会增加多少实现复杂度、上下文负担、工具调用、状态传播和新 bug surface；
7. 防错成本是否明显高于错误本身。

若：

```text
Prevention Cost > Expected Error Cost + Recovery Cost
```

则默认不建设该预防机制，或把它收窄到真正高风险的边界。

---

## 2. 默认采用 Recovery-first 的错误

以下错误在单人 AI RPG 中通常属于低成本、可感知、可恢复错误，默认不值得用大规模前置限制消灭：

- 一段叙事写得不好；
- NPC 某句话不符合玩家预期；
- 一次剧情发展无聊或方向不佳；
- 一处普通世界细节前后轻微不一致；
- 一个新 NPC / 地点候选质量不高；
- 模型误解了一个非不可逆意图；
- 一次低影响的局势推进不理想；
- 模型生成了用户不喜欢的创意内容但尚未造成高成本外部副作用。

默认恢复手段：

```text
撤回最后响应
重新生成
修改 / 重写
回到最近存档
恢复最近 canonical checkpoint
必要时人工纠正世界事实
继续游戏
```

产品应优先让这些恢复操作低摩擦，而不是让模型为了避免它们而失去主持能力。

---

## 3. 仍然必须 Prevention-first 的高风险边界

以下风险不因 DEC-P04 被放开：

### 3.1 Player Agency

模型不得替玩家作出玩家本人未授权的不可逆决定，包括但不限于：

- 替玩家答应承诺；
- 替玩家选择阵营 / 立场；
- 替玩家主动攻击 / 放弃 / 交付重要物品；
- 替玩家移动到明确改变局势的位置；
- 替玩家说出未表达的重要台词。

### 3.2 隐私 / 信息边界

hidden / private information 不得因为模型自由度增加而自动向玩家泄露。

### 3.3 外部不可逆副作用

真实支付、删除外部数据、向第三方发送信息、发布内容、修改 Source Asset 或其它局外持久资源，不属于“几十秒可恢复错误”。必须由独立授权和安全边界保护。

### 3.4 持久化完整性与恢复能力本身

Save / Restore / Recovery、identity、atomic commit、存档完整性等能力继续保留，因为它们正是 Recovery-first 哲学能够成立的基础设施。

### 3.5 高影响规则结果

争议性战斗结果、稀缺资源消耗、随机判定、不可逆规则效果等，在存在正式规则 Owner 时继续由 Program / Domain Owner 裁定；但不得因此把所有剧情与世界创作也一并程序化。

---

## 4. Program 的新定位

Program 不再以“尽可能防止模型犯错”为设计目标。

Program 的核心职责收敛为：

```text
保护 Player Agency
+ 保护高成本 / 不可逆边界
+ 提供 stable identity 与 durable world
+ 提供 Save / Restore / Recovery
+ 在真正需要规则裁定时 adjudicate
+ 让模型主持出来的世界可靠地存在
```

Program 不负责：

- 预先枚举故事允许发生的内容；
- 用剧情关键词白名单代替模型判断；
- 因为模型理论上可能出错就禁止它创造 NPC / 地点 / 事件；
- 把所有语义变化都拆成大量审批和机械 Tool Call；
- 为低成本可恢复错误建立高成本状态机。

---

## 5. 架构设计证明责任反转

从本决策起，新增限制模型能力的机制必须承担证明责任。

旧问题：

> “如果不加这个限制，模型会不会有可能犯错？”

新问题：

> “这个错误发生一次到底有多贵？能否恢复？这个限制会不会比错误本身更伤害产品？”

只有满足以下至少一项，才默认允许 Prevention-first：

- 错误不可逆或难以恢复；
- 错误难以被用户 / 系统发现；
- 错误涉及玩家自主权；
- 错误涉及隐私、安全或外部副作用；
- 错误可能系统性污染长期 canonical state 且无法可靠回滚；
- 错误单次成本明显高于限制带来的体验与复杂度成本。

否则默认选择较自由的模型路径 + 明确恢复能力。

---

## 6. 对当前 World Initiative 重构的直接影响

### 保留

- Player / World / System 三种 Authority 分离；
- `Model authors → Program commits durable reality`；
- Source != Game-local != Runtime；
- Save / Restore / Recovery；
- stable identity；
- hidden disclosure boundary；
- contested resolution / RNG 的正式 Owner。

### 进一步收窄

- Program 对 World Proposal 的校验应优先检查结构完整性、identity、Player Agency、高风险规则边界与信息边界；
- 不应因为剧情内容“未预枚举”“不在 Candidate Directory”“模型可能创造得不好”而拒绝；
- Narrative / World Initiative 的普通创意错误优先交给重生成、回档和后续纠正处理；
- 不应继续为低影响世界内容增加新的强制审批层。

### 需要后续产品能力支持

Recovery-first 要真正成立，产品后续应优先拥有低摩擦的：

- 重生成最近叙事；
- 撤回最近回合；
- 从最近 Save / checkpoint 恢复；
- 必要时对最近世界事实做受控纠正；
- 清楚显示当前 canonical 结果与可恢复边界。

这些能力优先级高于继续增加更多防错白名单。

---

## 7. 与 PLAY-01 correction-01 的关系

本决策不撤销当前 `PLAY-01 correction-01`。

当前返修解决的是：

- 真实 Provider 指令与新 Authority 语义自相矛盾；
- Active Situation 没有进入模型工作集；
- Opening 仍携带旧“不得创造未枚举实体”限制；
- quiet continuation 契约不一致。

这些问题本身正违反 DEC-P04，因为它们让模型在本应自由主持的区域无法正常发挥。

因此 correction-01 继续有效；不得在返修中借 DEC-P04 扩 scope 去重写整个 Runtime。

---

## 8. 一级原则

最终冻结为：

> **DEC-P04｜Freedom Before Prevention / Prefer Recovery over Prevention**
>
> The World 默认信任模型完成主持、世界推进与开放式内容创作。对于低成本、可感知、可恢复的模型错误，优先通过撤回、修正、重生成、回档和恢复来处理，而不是提前建设会明显限制核心体验的大规模防错体系。
>
> 只有当错误不可逆、难以发现、成本高，或涉及玩家自主权、隐私、安全、外部副作用、持久化完整性与正式高影响规则时，才默认采用前置预防。

一句话：

> **让模型先成为一个好的 GM；程序首先负责让它主持出来的世界可持续、可恢复，而不是把它变成一个不敢主持的状态机客户端。**
