# G9｜World Pack Opening Scenario 与 Creator 首轮创作验证 DRAFT v0.4

状态：`DISCUSSION DRAFT / NOT FROZEN / NOT IMPLEMENTATION AUTHORITY`
日期：2026-08-19
性质：Project Owner gameplay reflection / authoring-workflow validation
supersedes: `G9_世界包OpeningScenario与序章Runtime概念提案_DRAFT_v0.3_2026-08-19.md`

> 本文件只记录讨论方向，不修改正在执行的 G9-02A，不授权玩家端序章 Runtime 实现，也不冻结 Creator 最终字段、asset-spec 或 Scenario Runtime。下一步优先验证真实创作流程，再据经验调整产品与架构。

## 1. 新的开发顺序

Project Owner 明确：目前缺少序章创作经验，不应在没有真实作者体验的情况下提前冻结玩家端序章、Creator 完整工作流或 Runtime Contract。

因此当前推荐顺序：

```text
先定义最小 Authoring Prototype
↓
Project Owner 亲自完成第一份真实 World Pack Opening Scenario
↓
复盘：哪些信息真的需要作者填写 / 哪些应让 AI 推导 / 哪些字段阻碍创作
↓
修订 Creator authoring model
↓
再决定是否进入正式 Scenario Runtime vertical
↓
最后才冻结 G9-03 machine representation
```

### 当前 Gate

```text
玩家端“选择序章”                 DEFERRED / NOT EXPOSED
PROLOGUE_SCENARIO_MODE            DEFERRED / NOT IMPLEMENTED
Creator 完整序章编辑器             NOT FROZEN
Creator 最小序章创作实验能力       RECOMMENDED BEFORE FINAL FREEZE
G9-02A                            NO CHANGE / DO NOT INTERRUPT
```

Creator 首轮能力的目的不是 production-ready，而是让创作者真实经历一次完整创作过程。

---

## 2. Opening Scenario 继续是 World Pack 可选能力

```text
World Pack Opening Scenarios = 0..N
```

0 个完全合法；不要求所有作者制作序章。

Beat 数量不设 hard limit，也不提前规定推荐值。Beat 数应由剧情精彩度、复杂度、机制密度与事件数量自然产生。

Creator 后续可以提供软提示 / 复杂度反馈，但 Runtime / Schema 不验证“必须 X 个 Beat”。

---

## 3. 第一轮 Creator 的核心设计原则

> **让作者像设计一段开场剧情，而不是像编写状态机。**

第一轮只要求作者直接填写“真正属于创作判断”的内容；技术性 Transition / invariant / mutation metadata 尽量由 AI 辅助推导并展示给作者确认，而不是要求非开发者手工维护。

---

## 4. Scenario 顶层｜第一轮建议作者填写

### REQUIRED

1. **序章名称**
2. **一句话体验承诺**：这段序章想让玩家感受到什么？
3. **开场情景**：时间 / 地点 / 当前局面
4. **Preset Protagonist**：至少一个可选预制主角身份
5. **序章结束时的开放状态**：希望玩家完成序章后“知道什么 / 关心什么 / 可以自由去做什么”

### OPTIONAL

- 想自然展示的 Core / Expansion 机制
- 重要开场 NPC / Item / Knowledge
- 期望情绪曲线 / 氛围

不要求作者填写 machine ID、owner namespace、transition code、Program condition expression。

---

## 5. Preset Protagonist｜第一轮最小字段

作者只需要回答：

1. **这是谁 / 什么身份？**
2. **为什么这个人会合理地出现在本序章？**
3. **哪些事实必须固定，否则剧情无法成立？**
4. **哪些部分玩家可以自由定制？**

建议 Creator UI 自动整理为：

```text
Scenario-critical Locked Facts
+
Safe Customization
```

历史人物引用 Character Card，不复制人格 Definition。

---

## 6. Beat｜第一轮推荐作者只写 5 个核心内容

这是当前对“每个 Beat 应给 AI 什么”的初步答案。

### B1｜这一 Beat 的关键情景

> **这一幕发生什么？**

用自然语言描述当前场景、角色和局面。不是最终 Narrative prose，也不是程序规则。

### B2｜Action Pressure

> **为什么玩家现在需要作出回应？**

例如：NPC 正等答复、时间窗口正在消失、发生了异常、机会即将错过、当前社会压力迫使玩家表态。

### B3｜这一 Beat 想让玩家体验什么

> **这一幕的创作目的是什么？**

可以是：认识 NPC、感受世界规则、得到 Knowledge、第一次使用 Item、体验某个机制、形成情绪或价值判断。

它不是强制 Outcome；用于帮助 AI 理解作者意图。

### B4｜五个推荐行动

每 Beat exactly five creator-authored suggestions。

要求：
- 合理；
- 可解释；
- 能明显推动当前局面；
- 大多数正常情况下可以自然通往或准备通往下一 Beat；
- 仍只是 prefill，不是行动白名单。

### B5｜下一 Beat

作者只用自然语言回答：

> **下一幕希望来到什么情景？**

例如：

```text
“玩家最终进入城内，并第一次看到城中骚乱。”
```

作者不需要手写复杂 `precondition`。

---

## 7. AI 辅助推导层｜第一轮不强迫作者手填

Creator AI 根据 B1–B5 生成一个 `Authoring Review`，至少推导：

### R1｜Next Beat Preconditions

为了让下一幕诚实成立，当前世界至少必须保持哪些事实？

### R2｜Flexible Facts

哪些内容允许根据玩家行动自由变化，而不会破坏下一 Beat？

### R3｜Breakout-sensitive Conditions

哪些玩家行动若真实成功，会让下一 Beat 无法成立，因此未来应触发 Scenario breakout？

### R4｜Persistent Consequence Candidates

本 Beat 可能自然留下哪些真实 Character / Item / Knowledge / Relationship / local-state 后果？

### R5｜Agency / Railroading Audit

是否存在：
- 为抵达下一 Beat 必须否定玩家已经成功的行动；
- 必须强迫 NPC 改变意图；
- 必须伪造巧合；
- 五个推荐行动事实上只有一个“正确答案”。

作者可以接受、修改或否定 AI 的推导。

这些 Review 在第一轮主要是创作辅助证据，不是最终 Runtime Schema。

---

## 8. 第一轮 Creator 不需要 Branch Tree

继续采用：

```text
Beat 1 → Beat 2 → ... → Beat N
```

作者不写 Branch Graph。

AI /未来 Scenario Runtime 的职责是：在不伪造现实、不覆盖玩家 Formal Outcome 的前提下，把 Beat 内合理自由输入产生的局部差异自然桥接到下一 Beat。

如果 required fact 已被玩家合法破坏，则不能为了剧情收敛修改现实；未来采用 breakout → Sandbox。

---

## 9. HARD ISOLATION 继续保留

即使玩家端序章目前 DEFER，未来若实现仍必须：

```text
PROLOGUE_SCENARIO_MODE
= only place where authored next-Beat convergence exists

NORMAL_SANDBOX_MODE
= no authored next-Beat target
= no bring-story-back-on-track authority
```

Sandbox 的 Action Pressure 只是一种适量 Pacing 工具，绝不等于剧情收敛。

---

## 10. 第一轮“真实创作流程”建议

第一次不追求完整 Creator 产品。

建议选择一个真实 World Pack，完成：

```text
1 个 Opening Scenario
+
1–2 个 Preset Protagonist
+
作者自然决定数量的 ordered Beats
+
每 Beat B1–B5
+
AI Authoring Review R1–R5
```

创作完成后专门复盘：

1. B1–B5 哪些字段自然、哪些重复？
2. 哪些信息作者总是漏填？
3. AI Review 哪些真正有帮助？
4. 哪些技术约束不应该暴露给作者？
5. 五个建议是否难写 / 是否需要 AI 辅助？
6. Preset Protagonist 的 locked/customizable 边界是否好用？
7. Tutorial Slot 是否应该在第一次就出现？
8. 一个 Beat 实际需要多少创作文字？
9. 整体创作过程有没有接近传统脚本开发的负担？

复盘结果优先于当前 Draft 假设。

---

## 11. 当前不冻结

- 玩家端序章入口；
- Scenario Runtime；
- Creator 最终 UI；
- Beat exact machine contract；
- external asset-spec fields；
- Scenario asset ownership form；
- tutorial taxonomy；
- Beat 数量；
- Preset exact customization policy；
- AI Review 是否最终保留全部 R1–R5；
- 三国具体序章。

## 12. 当前建议

> **先做真实创作验证，再做平台冻结。**

第一轮 Creator Prototype 应优先减少作者认知负担，让创作者写故事；AI 和后续 Runtime 再负责把创作意图转换成可验证的结构与执行边界。
