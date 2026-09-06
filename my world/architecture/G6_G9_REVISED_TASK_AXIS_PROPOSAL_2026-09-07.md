---
title: my world｜G6–G9 Core-first Revised Task Axis Proposal
status: proposed-revised-task-axis
version: 1.0
created: 2026-09-07
updated: 2026-09-07
implementation_authorization: none
roadmap_promotion: pending-owner-approval
basis_roadmap: MY_WORLD_总体规划路线图_CURRENT.md@v4.1
basis_status: MY_WORLD_CURRENT_STATUS.md@v16.21
basis_discussion: experience/SILLYTAVERN_REFERENCE_IMPROVEMENT_DISCUSSION_2026-09-06.md + experience/SILLYTAVERN_REFERENCE_IMPROVEMENT_DISCUSSION_ADDENDUM_2026-09-07.md
implementation_main_at_audit: 782daf65348f484d636d46260ac2374559cf554d
---

# my world｜G6–G9 Core-first Revised Task Axis Proposal

## 0. 结论

本轮改进讨论最终通过 34 项产品方向，但它们**不应转换成 34 个近期开发任务**。

Owner 已明确新的最高排序规则：

> **优先保证游戏尽快完成完整游戏闭环。核心开发先做；扩展功能、体验优化、Creator、Reference、模型管理、诊断与外围增强后置。**

因此本 Proposal 将全部通过项压缩为 **13 个 Capability / Stage Package**，并把真正影响 V0 游戏闭环的内容与后续成熟度增强分离。

当前正式 Roadmap / Status 仍未改变；本文件只有 Owner 批准后才能 Promotion。

---

# 1. 核心排序原则

新版路线采用以下判断顺序：

```text
没有它，V0 是否仍不能形成完整、可持续的 RPG 游玩闭环？
↓ 是
优先

只是让长期信息更丰富 / UX 更舒服 / 作者更方便 / 模型更好管理？
↓ 是
闭环之后
```

同时继续保护：

- Vertical before platform；
- Consumer before Creator；
- Model Freedom First；
- Narrative 是主要游戏内容；
- free-form Player action 永远是主输入；
- World Truth != actor Knowledge != human-player disclosure；
- Save / Restore / Timeline currentness 不可破坏；
- 不为 RPG 完整感制造 fake state；
- MW-013 Internal Declarative UI Host 继续 HOLD，直到多个真实 consumer 证明稳定模式；
- generic Action Intent 继续 Deferred；
- Visual Runtime 继续等待真实第一方视觉需求。

---

# 2. Revised Task Axis｜13 个 Package

## Package 0｜关闭当前 MW-018 + MW-019 Owner UAT Gate

**优先级：CURRENT / 最高。**

先完成当前已经集成的：

- MW-018 People；
- MW-019 Five Recommended Actions；
- Combined Owner UAT；
- 仅对真实 UAT finding 做同 lineage revision。

若 UAT 证明 fenced JSON / structured-output formatting 已经频繁影响正常使用，则只对真实受影响 lane 做最小 Structured Output 修复；不得借机先建通用 Provider framework。

**Stage Exit：** MW-018 / MW-019 分别得到 Owner Product verdict，当前 Gate 关闭。

---

## Package 1｜Core Interaction Control｜OOC + Character-guided Recommendations

**目标：让玩家既能自由扮演角色，又能明确告诉 GM“这一段怎么玩”，同时推荐行动开始真正反映当前主角是谁。**

顺序：

1. **OOC / GM Guidance**（P-17）
   - `角色行动 | OOC / GM 指导` 明确分流；
   - OOC 不成为角色行为、不直接改 World Truth、不绕过 mechanics。
2. **Character ↔ Player Action ↔ Recommendations feedback**（P-13）
   - recommendation 读取 current player-safe Character；
   - 只有最终 accepted Player action 才成为 Character 演化证据；
   - 不做 personality score / keyword classifier。

**Non-scope：** Narrative Preference、Persistent Reminder、Reality Correction 均不在本 Package。

---

## Package 2｜Core Information Continuity｜事务 / Open Threads

**目标：玩家在长一点的正常游玩中，随时知道“我现在还有什么没解决”。**

实现 P-15：

- 当前问题；
- 尚未核实线索；
- 已作承诺；
- 玩家当前计划；
- 未解决风险。

优先扩展现有 Information Curator，不另建 Quest rule engine。

**Non-scope：** Provenance、Epistemic Status、Organization、World Chronicle、Shared History 后置。

---

## Package 3｜Core Mechanics Visibility｜System Surface 第一真实消费者

**目标：把已经存在的真实 mechanics 做成玩家看得见、能理解的游戏系统，而不是空 System Tab。**

首先以现有 Public d20 为真实 consumer，实现 P-28：

```text
System Surface
→ 当前启用的真实 mechanic
→ player-safe 当前状态 / 最近必要结果
```

Shell 不硬编码 HP / Mana / Hunger / Money。

**关键结果：** 建立第一个真正的 Expansion/mechanic-state UI consumer，为未来是否重启 MW-013 提供真实证据。

---

## Package 4｜Core Inventory Vertical｜事实型行囊

**目标：让“玩家实际拥有的东西”第一次成为正式、可持久、可恢复的 RPG state。**

实现 P-27，但必须先做最小 Inventory ownership / mutation architecture：

```text
real owned item
→ authoritative Game-local owner
→ accepted semantic/mechanic mutation
→ Save/Restore currentness
→ player-safe Inventory Surface
```

第一纵向只需要证明一个真实物品：

- 初始拥有；
- 使用 / 转交 / 失去中的至少一个真实 mutation；
- Save / Restore / reopen 一致。

**Non-scope：** equipment slots、loot system、crafting、economy、durability、复杂 stack system，除非第一 consumer 真正需要。

---

## Package 5｜V0 CORE CLOSURE GATE｜完整游戏闭环 Reality Gate

这是新版路线最重要的新 Gate。

在任何大规模体验扩展、Creator、Reference、模型管理之前，Owner 用一局连续真实游戏证明：

```text
Launch
→ New Game / Continue
→ GM opening
→ recommendation + free-form action
→ OOC Guidance
→ durable World / NPC consequence
→ Character / People / Open Threads 更新
→ Public d20 / System 可查看
→ 至少一个真实 Inventory mutation
→ Save
→ exit / reopen
→ Continue / Restore
→ 所有 player-visible state 与世界当前历史一致
→ 继续正常游玩
```

建议 Reality Run 至少覆盖：

- 20–30 个正常回合；
- 1 个新出现 NPC；
- 1 次 OOC Guidance；
- 1 次 d20；
- 1 次物品变化；
- 1 次 Save / reopen；
- 1 次 Restore；
- 1 次 free-form 行动明显偏离推荐项。

**Stage Exit：** Owner 明确认定 `V0 Core Game Loop = PRODUCT PASS`。

只有阻塞此闭环的 defect 可以在 Gate 前插队；其它已通过改进全部后置。

---

# 3. Post-closure Core Hardening

## Package 6｜Long-session Core｜Context Orchestrator + Structured Output Reliability

V0 闭环通过后，优先保护“玩久了仍然成立”。

包含：

- P-16 Context Orchestrator；
- P-03 Structured Output Reliability 在已经证明需要的 machine-schema lanes 中收敛；
- 长局 working-set / currentness / performance reality test。

原则：

```text
相关 != 当前有效 != 当前有权使用
```

不先建设通用 RAG platform；Reference retrieval 仍后置到 Content track。

---

## Package 7｜Knowledge Integrity & Correction Foundation

在增加更多信息 Surface 前，先把“玩家为什么知道、知道得有多确定、信息是否过期/冲突”统一起来。

合并：

- P-19 Provenance；
- P-20 Epistemic Status；
- P-22 Turn Freshness（只用第几回合，不维护“几天前”）；
- P-23 Conflicting Evidence；
- P-10 玩家纠正 AI 派生信息。

随后在同 Package 的后半段进行 **P-31 Reality Correction Mode Architecture Audit**；只有 owner / atomicity / Timeline / affected-domain matrix 冻结后，才允许实现世界纠错模式。

`角色行动 | OOC | 世界纠错` 三种输入语义必须保持分离。

---

# 4. Post-closure Product Experience

## Package 8｜Information Surface Expansion

在 Knowledge foundation 成立以后，批量扩展真正有语义价值的信息消费者：

- P-21 People Shared History；
- P-26 Organization / Faction player-known Surface；
- P-29 Player-known World Chronicle；
- P-32 Player-visible Consequence Diff。

这些能力尽量复用同一 player-safe information model，不为每个 Surface 建独立 semantic subsystem。

到本 Package 结束后，才重新审计 MW-013 是否已有足够真实 consumer evidence；**不是自动授权实现**。

---

## Package 9｜Player Utility / Personalization / Archive

后置的玩家体验增强统一放在这里：

- P-02 Narrative Preference；
- P-09 Bookmark；
- P-11 Player Notes；
- P-05 readable Adventure Chronicle export；
- P-30 Game-local Frozen Manifest。

这些都很有价值，但**不允许阻塞 V0 Core Closure**。

---

# 5. Post-closure AI Operations

## Package 10｜Provider / Model / Observability

统一收敛，不建设多套平行配置系统：

- P-01 player-safe generation status / diagnostics；
- P-08 Narrative / Background model separation；
- P-18 AI usage / latency / token visibility；
- P-24 Provider / Model Compatibility Preflight；
- P-33 Model Profiles；
- P-34 Debug Mode。

顺序建议：

```text
shared terminal/status evidence
→ Compatibility Preflight
→ model split
→ profiles
→ usage UI
→ Debug Mode presentation
```

如果前面的 Core 阶段出现真实 debugging blocker，只拉出最小观测 seam，不把整个 Package 10 前移。

---

# 6. Content / Creator Track｜严格后置

## Package 11｜Source Library / Reference / Creator

这个 Package 明确在完整游戏闭环之后。

内部继续遵守 Consumer before Creator：

### 11A｜Source Library 作品化 + Composition

- P-04 Source Library 作品化 / discovery；
- P-12 推荐 Composition / 作品套装。

### 11B｜Reference Library

- P-06 大型参考资料层；
- retrieval 只提供候选背景，不成为当前 Game Truth。

### 11C｜Creator Loop

- P-14 对话式 Creator；
- P-07 Creator Preview Sandbox；
- P-25 人话化 Validation / Publish UX。

明确不建设 arbitrary-code plugin platform、在线商店、云账号或 speculative universal package manager。

---

# 7. Release Track

## Package 12｜Standalone Alpha / Release Validation

保留现有 G9 的正式产品职责：

- Windows standalone packaging；
- onboarding / credentials / Source setup；
- upgrade / migration / recovery reality tests；
- long-play / corruption / reinstall validation；
- release UAT / defect closure；
- documentation / support boundary。

Package 10 的 diagnostics / compatibility / Debug Mode 可以在此被真正消费，但 G9 不反过来要求提前实现所有未来增强。

**G9 Exit：** 独立用户能安装、建局、持续游玩、保存恢复，并在真实失败时得到可理解的路径。

---

# 8. 明确不阻塞 V0 Core Closure 的内容

即使已经通过，以下能力全部后置：

- Narrative Preference；
- Bookmark / Notes / Chronicle export；
- Provenance / epistemic / conflicting evidence 的完整成熟版；
- Organization / World Chronicle / Shared History；
- Reality Correction；
- Model Profiles / model split / usage dashboard / Debug Mode；
- Source Library 作品化；
- Reference Library；
- Creator / Preview / Publish UX；
- visual runtime / authored map；
- MW-013 Internal Declarative UI Host；
- generic Action Intent。

如果其中某个能力在真实 Core UAT 中成为 blocker，只拉出解决 blocker 的**最小必要切片**，不整体前移所属 Package。

---

# 9. 本轮讨论中未批准的方向

未被 Owner 通过的旧提案与原提案 56–60 不进入本 Task Axis；不因为相邻功能被批准就静默复活。

未来只有出现新的真实用户问题或 Owner 明确重新开启，才重新讨论。

---

# 10. 与当前 Roadmap v4.1 的关键变化

新版提案不是推翻 G6–G9，而是改变内部优先级：

```text
旧：G6 中持续扩展 Surface / Host / UI maturity

新：先插入明确 V0 CORE CLOSURE GATE
    → 只做闭环必须的 OOC / Open Threads / System / Inventory / Character-guided recommendations
    → Owner 真正长一点试玩 PASS
    → 再做 Knowledge / richer Surfaces / diagnostics / Creator
```

因此：

- `MW-013` 继续 HOLD，不再被“G6 要完整”推着提前做；
- G7 Context 紧跟 Core Closure，优先保护长局；
- G8 Creator / Reference 保持严格后置；
- 产品体验增强不再与“核心完成”混为同一 Gate。

---

# 11. Promotion Gate

本 Proposal 当前仍是：

```text
PROPOSED REVISED TASK AXIS
NOT Route Freeze
NOT CURRENT Roadmap
NOT implementation authorization
```

若 Owner 批准本轴，下一步由 GPT：

1. Promotion 到 `MY_WORLD_总体规划路线图_CURRENT.md`；
2. 只在必要处传播到 `MY_WORLD_架构_CURRENT.md`；
3. 更新 `MY_WORLD_CURRENT_STATUS.md`；
4. 保持当前 MW-018 + MW-019 UAT 为 immediate Gate；
5. UAT 关闭后，按新版 Package 1 开始 Task Shaping / Codex dispatch。
