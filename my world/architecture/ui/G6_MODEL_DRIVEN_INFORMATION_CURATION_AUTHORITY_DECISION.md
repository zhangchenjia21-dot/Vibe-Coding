---
title: my world｜G6 Model-driven Information Curation Authority Decision
status: FROZEN / CURRENT
version: 1.0
created: 2026-09-06
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
owner: Owner + GPT
supersedes_when_conflicting:
  - architecture/ui/G6_CHARACTER_AND_IMPORTANT_EXPERIENCES_SEMANTIC_AUDIT_DRAFT_V0_1.md v0.2 中“Program 对开放游戏语义做权限/证据判定”的保守提案
---

# G6 Model-driven Information Curation Authority｜CURRENT

## 1. Owner decision

Owner 明确要求：

> **对“什么信息重要、什么信息代表角色发生了什么变化、哪些信息应该进入角色/重要经历/其它信息栏”的开放语义判断，应信任模型完成。游戏系统不应通过关键词、规则树、打分器、证据分类器或大量 heuristic 去复刻模型的语义理解。宁可增加一次 bounded 模型调用，也不要把开放语义判断压力转移给 Runtime。**

因此正式边界修订为：

> **Model owns semantic interpretation and curation; Program owns normalized storage, temporal integrity and presentation.**
>
> **模型负责理解和决定游戏语义；程序负责把模型决定的结果稳定、可逆、规范地保存和呈现。**

这比先前“Model judges meaning; Program enforces semantic authority”的表述更进一步。Program 不再被设计成 Character / Important Experiences 的语义审批器。

## 2. What the model is authorized to decide

在 accepted current game context 内，模型有权直接判断并输出：

- 本轮是否产生值得长期保存的信息；
- 某一变化是否足够重要；
- 该变化属于 `角色` 当前状态、`重要经历`、未来 `概览 / 人物 / 事务 / 行囊 / 系统 / 地图` 等哪类玩家信息；
- Character 当前字段应新增、替换、删除还是保持；
- 某事件是否构成 protagonist milestone；
- 某段玩家行为 / 已接受 Narrative 是否已经表达了长期目标、价值取向、身份或人生方向的真实变化；
- 如何将复杂 Narrative 归纳成简洁、规范、玩家可读的信息栏文本；
- 表面普通的事件是否因为上下文与后果而具有重大意义。

Program 不为这些判断建立平行 semantic rule engine。

## 3. Player agency is preserved by model semantics, not a Program heuristic gate

既有产品原则继续成立：

> **Player owns new meaningful protagonist choices.**

但实现方式不是：

```text
Model 提出长期选择变化
→ Program 搜关键词 / evidence 类型
→ Program 决定玩家到底有没有这样选
```

而是：

```text
accepted Player input
+ accepted GM Narrative
+ current Character / world context
→ semantic curator model
→ 模型判断本轮是否真的形成了新的 Player-owned choice
→ structured Character / milestone update
```

模型的职责包括**不凭空替玩家创造未发生的重大选择**，同时也包括在上下文已经充分表达时正确识别隐含但清晰的选择。

例如：

```text
“曹操这人确实比我想象中有意思。”
```

正常高能力模型应理解这只是评价变化，不等于“终身效忠曹操”。Program 不需要为了防止这种极端误判再建立一套“效忠证据校验器”。

如果实际模型偶发产生错误 curation，应优先依靠：

- curator retry / regenerate；
- 玩家 correction；
- Timeline / Save / Restore；
- 后续模型对当前状态的再整理；

而不是把每一种潜在语义错误固化成 Runtime 规则。

这继承：

> **Model freedom first. Reversibility over prevention.**

## 4. Program responsibilities are deliberately narrow

Program 仍拥有不可替代的基础设施职责，但这些职责是**结构性 / 时间性 / 持久性**的，而不是开放语义判断。

Program 负责：

```text
accepted current-turn / current-timeline binding
stable game/entity identity
structured payload syntax / type / size normalization
atomic persistence
idempotent replay
Save / Restore / Regenerate currentness
stale-future isolation
crash / retry correctness
canonical storage location
UI-safe serialization / rendering contract
ordering / pagination / layout / empty-state presentation
```

Program 可以拒绝 malformed payload、未知 operation kind、损坏 identity、stale timeline write 等**机器可判定的结构错误**。

Program 不负责：

```text
这件事到底重不重要？
这句话是否意味着价值观变化？
这场战斗是否值得进入重要经历？
这次谈话是否改变了人生方向？
这个身份变化应该不应该长期保存？
```

这些都属于模型语义 authority。

## 5. Domain organization remains a data organization contract, not a semantic judge

`Existing Domain wins` 仍然保留，但它的含义调整为：

> **已经存在专门数据 owner 时，模型应把对应语义写入该 owner，而不是复制第二份 truth。**

例如：

```text
当前持有物 → Inventory
关系变化   → Relationship
角色数值   → Mechanic State
玩家知识   → Knowledge
```

Program 提供这些可写入的规范化目标与结构；模型负责理解某个新事实属于哪里。

如果当前没有某个专门 Domain，则不得为了理论完美先建设巨大 Domain forest。由真实 consumer 再决定是否需要最小正式 owner。

## 6. Recommended post-turn curation model

G6 推荐形成一个统一的 **Post-turn Information Curator**，而不是为每个 Surface 建一个 Program classifier。

数据流：

```text
accepted Player input
+ accepted GM Narrative
+ current Character information
+ recent Important Experiences
+ relevant current game/domain context
+ concise information-surface semantics
↓
Information Curator model
↓
structured curation result
↓
Program normalization + atomic persistence
↓
player-safe presentation projection
↓
Character / Important Experiences / future Surfaces
```

Curator 输出可以采用 bounded operation，例如：

```text
character:
- upsert / replace / remove

important_experiences:
- append / revise / remove-current-stale

future surfaces:
- bounded structured updates when their real Domain exists
```

具体 schema 由第一个 implementation vertical 证明，不在本 Decision 预造 giant universal UI/state schema。

## 7. One semantic call may maintain multiple information surfaces

为了控制成本与职责碎片，不建议未来变成：

```text
Character 调一次模型
Important Experiences 再调一次
People 再调一次
Overview 再调一次
```

更好的长期方向是：

> **一次 post-turn semantic curation 可以同时判断所有当前已启用、需要维护的玩家信息 Surface。**

只有实际 token / latency / quality evidence 证明必须拆分时再拆。

这样新增 `人物 / 事务 / 行囊 / 系统` 等真实消费者时，优先扩展模型可表达的 bounded curation contract，而不是给 Runtime 新增一套新的 heuristic subsystem。

## 8. Non-blocking narrative rule

Information Curator 默认是后台语义维护，不成为 Narrative Finalize Gate：

```text
accepted GM Narrative
→ immediately remains playable / visible

Information Curator
→ background semantic maintenance
→ durable information update
→ UI refresh
```

Curator failure：

```text
must not invalidate accepted Narrative
must not block next Player action by default
may retry / repair later
```

除非未来某一 specific mechanic 明确要求同步状态，否则不要让信息栏整理失败阻断主玩法。

## 9. Consequence for Character + Important Experiences implementation

未来对应 backend task 不应实现：

- Character importance classifier；
- milestone score engine；
- protagonist-choice evidence heuristic；
- keyword / regex semantic router；
- per-concept Program rule forest。

应实现的是：

```text
model-driven curation call / seam
+ bounded structured curation contract
+ normalized durable current Character material
+ normalized durable protagonist milestone material
+ current-timeline binding / Save-Restore-Regenerate correctness
+ presentation-safe projection
```

Program 的复杂度应主要花在“正确保存模型决定的游戏语义”，而不是“试图比模型更懂这些语义”。

## 10. Review principle

Independent Review 必须特别防止实现者重新把开放语义偷偷硬编码回 Program。

若出现：

- 大量关键词；
- semantic score table；
- per-event-type rule branches；
- Program 判断人物心理 / 人生意义；
- 为防模型偶发误判堆叠越来越多 validator；

默认视为架构回退，需要证明存在不可由模型可靠承担的硬性基础设施理由。
