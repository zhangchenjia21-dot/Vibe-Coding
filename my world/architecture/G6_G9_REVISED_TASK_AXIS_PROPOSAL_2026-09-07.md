---
title: my world｜G6–G9 Core-first Revised Task Axis
status: owner-approved-promoted-route
version: 1.1
created: 2026-09-07
updated: 2026-09-07
owner_approved: true
implementation_authorization: staged-task-packet-required
roadmap_promotion: approved
basis_roadmap: MY_WORLD_总体规划路线图_CURRENT.md@v4.1
basis_status: MY_WORLD_CURRENT_STATUS.md@v16.21
basis_discussion: experience/SILLYTAVERN_REFERENCE_IMPROVEMENT_DISCUSSION_2026-09-06.md + experience/SILLYTAVERN_REFERENCE_IMPROVEMENT_DISCUSSION_ADDENDUM_2026-09-07.md
implementation_main_at_audit: 782daf65348f484d636d46260ac2374559cf554d
---

# my world｜G6–G9 Core-first Revised Task Axis

## 0. Owner-approved conclusion

本轮改进讨论最终通过 34 项产品方向，但它们**不转换成 34 个近期开发任务**。

Owner 冻结两条最高排序规则：

> **优先保证游戏尽快完成完整游戏闭环。核心开发先做；扩展功能、体验优化、Creator、Reference、模型管理、诊断与外围增强后置。**

> **动态 UI 是核心能力。Internal Dynamic UI Host v0.1 必须在 V0 Core Closure 前进入真实游戏闭环，而不是闭环完成后才考虑。**

因此全部通过项被压缩为 **14 个 Package（0–13）**。CURRENT Roadmap / Architecture / Status 以本轴 Promotion 后版本为 authority。

---

# 1. 排序规则

```text
没有它，V0 是否仍不能形成完整、可持续、具有 my-world 核心特色的 RPG 游玩闭环？
→ 是：Core Closure 前

只是让长期信息更丰富、作者更方便、模型更好管理、诊断更完整或体验更成熟？
→ 是：Core Closure 后
```

继续保护：

- Vertical before platform；
- Consumer before Creator；
- Model Freedom First；
- Narrative 是主要游戏内容；
- free-form Player action 永远是主输入；
- World Truth != actor Knowledge != human-player disclosure；
- Save / Restore / Timeline currentness 不可破坏；
- 不为 RPG 完整感制造 fake state；
- generic Action Intent 继续 Deferred；
- Visual Runtime 继续等待真实第一方视觉需求。

动态 UI 的例外并非违反 Consumer-before-platform：在实施前必须已经存在 Character、Important Experiences、People、Open Threads、System/mechanics、Inventory 等多个真实消费者；Host 只能从这些已证明模式中抽象。

---

# 2. Core Closure Track

## Package 0｜关闭 MW-018 + MW-019 Owner UAT Gate

**CURRENT / 最高优先级。**

先完成：

- MW-018 People；
- MW-019 Five Recommended Actions；
- Combined Owner UAT；
- 只对真实 UAT finding 做同 lineage revision。

若真实 UAT 证明 fenced JSON / Structured Output 可靠性已经频繁影响正常使用，仅修真实受影响 lane 的最小 seam；不得借机先建通用 Provider framework。

**Exit：** MW-018 / MW-019 分别得到 Owner Product verdict。

---

## Package 1｜Core Interaction Control｜OOC + Character-guided Recommendations

目标：让玩家既自由扮演角色，又能明确告诉 GM“这一段怎么玩”，同时推荐行动开始反映当前主角是谁。

包含：

1. **OOC / GM Guidance（P-17）**
   - `角色行动 | OOC / GM 指导` 明确分流；
   - OOC 不成为角色行为，不直接改 World Truth，不绕过 mechanics。
2. **Character ↔ Player Action ↔ Recommendations（P-13）**
   - recommendation 读取 current player-safe Character；
   - 只有最终 accepted Player action 才成为 Character 演化证据；
   - 不做 personality score / keyword classifier；
   - 推荐必须允许偏离、成长和自由输入。

Non-scope：Narrative Preference、Persistent Reminder、Reality Correction。

---

## Package 2｜Core Information Continuity｜事务 / Open Threads

目标：玩家随时知道“我现在还有什么没解决”。

实现 P-15：当前问题、未核实线索、承诺、玩家计划、未解决风险。

优先扩展现有 Information Curator；Program 不建立 Quest keyword/rule engine。

Non-scope：Provenance、Epistemic Status、Organization、World Chronicle、Shared History。

---

## Package 3｜Core Mechanics Visibility｜System Surface

目标：把已经存在的真实 mechanics 做成玩家看得见的游戏系统，而不是空 System Tab。

第一真实 consumer 使用 Public d20：

```text
real mechanic owner
→ bounded player-safe mechanic contribution
→ System Surface
```

Shell 不硬编码 HP / Mana / Hunger / Money；没有真实 state 就不显示。

这也是 Dynamic UI Host 的真实 mechanic consumer 证据之一。

---

## Package 4｜Core Inventory Vertical｜事实型行囊

目标：让“玩家实际拥有的东西”第一次成为正式、可持久、可恢复的 RPG state。

最小链：

```text
real owned item
→ authoritative Game-local owner
→ accepted semantic/mechanic mutation
→ Save/Restore currentness
→ player-safe Inventory Surface
```

第一纵向只需证明：至少一个真实物品初始拥有，并发生一次使用 / 转交 / 失去中的真实 mutation，Save / Restore / reopen 一致。

Non-scope：装备槽、复杂 loot、crafting、economy、durability、通用 stack framework。

---

## Package 5｜Internal Dynamic UI Host v0.1｜核心动态 UI

**Owner 明确要求：V0 Core Closure 前必做。**

进入条件：Package 0–4 已经形成足够真实 consumers，至少包括：

- Character；
- Important Experiences；
- People；
- Open Threads；
- System / Public d20；
- Inventory。

目标：让不同 Game-local 实际信息与 mechanics 能通过统一的**内部、受控、player-safe 动态 UI Host**组织和呈现，而不是每增加一种内容都重新手写整套页面。

v0.1 只从已经重复出现的内部模式抽象，候选 vocabulary 限于真实需要，例如：

- section / group；
- text / labeled field；
- card / list；
- collapsed / expanded region；
- bounded status / mechanic contribution；
- safe navigation where already proven。

关键边界：

- Dynamic UI = presentation host，不是 World Truth owner；
- leaf renderer 不接收 omniscient `world_state` 后自行过滤；
- Restore / Regenerate 后动态 UI 必须随安全 projection currentness 回退；
- 不允许 arbitrary GDScript callback、NodePath execution、OS/filesystem command、任意 authoritative mutation；
- **generic Action Intent 仍不在 v0.1**；
- **外部 Source / Expansion 作者声明 UI 仍不在 v0.1**，G8 才能基于成熟内部 vocabulary 决定是否开放；
- 旧 `MW-013` Task Shaping 只能作为历史材料，不能直接执行；必须基于 Package 2–4 新增 consumer evidence 重新 Task Shape。

**Exit：** 多个真实 Surface / mechanic contribution 已通过同一 Internal Dynamic UI Host 正确呈现，且没有第二事实源、泄密或 currentness 回归。

---

## Package 6｜V0 CORE CLOSURE GATE｜完整游戏闭环 Reality Gate

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
→ 多种真实内容通过 Internal Dynamic UI Host 呈现
→ Save
→ exit / reopen
→ Continue / Restore
→ Dynamic UI 与所有 player-visible state 同步回到当前历史
→ 继续正常游玩
```

Reality Run 建议至少覆盖：

- 20–30 个正常回合；
- 1 个新出现 NPC；
- 1 次 OOC Guidance；
- 1 次 d20；
- 1 次物品变化；
- 1 次 Save / reopen；
- 1 次 Restore；
- 1 次 free-form 行动明显偏离推荐项；
- 至少 3 类由 Dynamic UI Host 承载的真实 Surface / contribution。

**Exit：Owner 明确认定 `V0 Core Game Loop = PRODUCT PASS`。**

只有阻塞此闭环的 defect 可以在 Gate 前插队；其它已通过改进全部后置。

---

# 3. Post-closure Core Hardening

## Package 7｜Long-session Core｜Context Orchestrator + Structured Output Reliability

V0 闭环通过后，优先保护“玩久了仍然成立”。

包含：

- P-16 Context Orchestrator；
- P-03 Structured Output Reliability 在已经证明需要的 machine-schema lanes 中收敛；
- 长局 working-set / currentness / performance reality test。

原则：`相关 != 当前有效 != 当前有权使用`。

不先建设通用 RAG platform；Reference retrieval 后置。

---

## Package 8｜Knowledge Integrity & Correction Foundation

合并：

- P-19 Provenance；
- P-20 Epistemic Status；
- P-22 Turn Freshness（只用第几个回合 / accepted-history node）；
- P-23 Conflicting Evidence；
- P-10 玩家纠正 AI 派生信息。

后半段进行 P-31 Reality Correction Mode Architecture Audit；只有 owner / atomicity / Timeline / affected-domain matrix 冻结后才实现世界纠错模式。

`角色行动 | OOC | 世界纠错` 三种输入语义始终分离。

---

# 4. Post-closure Product Experience

## Package 9｜Information Surface Expansion

在 Knowledge foundation 后扩展：

- P-21 People Shared History；
- P-26 Organization / Faction player-known Surface；
- P-29 Player-known World Chronicle；
- P-32 Player-visible Consequence Diff。

这些新 consumer 应优先复用已成立的 Dynamic UI Host 和统一 player-safe information model，不为每个 Surface 再建平行 UI/semantic subsystem。

---

## Package 10｜Player Utility / Personalization / Archive

统一后置：

- P-02 Narrative Preference；
- P-09 Bookmark；
- P-11 Player Notes；
- P-05 readable Adventure Chronicle export；
- P-30 Game-local Frozen Manifest。

这些能力有价值，但不阻塞 V0 Core Closure。

---

# 5. Post-closure AI Operations

## Package 11｜Provider / Model / Observability

统一收敛：

- P-01 player-safe generation status / diagnostics；
- P-08 Narrative / Background model separation；
- P-18 AI usage / latency / token visibility；
- P-24 Compatibility Preflight；
- P-33 Model Profiles；
- P-34 Debug Mode。

若 Core 阶段出现真实 debugging blocker，只拉出最小观测 seam，不整体前移本 Package。

---

# 6. Content / Creator Track｜严格后置

## Package 12｜Source Library / Reference / Creator

### 12A Source Library 作品化 + Composition
- P-04 Source Library discovery / presentation；
- P-12 推荐 Composition / 作品套装。

### 12B Reference Library
- P-06 大型参考资料层；
- retrieval 只提供候选背景，不成为当前 Game Truth。

### 12C Creator Loop
- P-14 对话式 Creator；
- P-07 Creator Preview Sandbox；
- P-25 人话化 Validation / Publish UX。

未来若开放外部 Declarative UI contract，必须从 **Package 5 已证明的内部 Dynamic UI vocabulary** 派生，而不是设计第二套 UI schema。

不建设 arbitrary-code plugin platform、在线商店、云账号或 speculative universal package manager。

---

# 7. Release Track

## Package 13｜Standalone Alpha / Release Validation

保留 G9 正式职责：Windows standalone packaging、onboarding / credentials / Source setup、upgrade / migration / recovery reality tests、long-play / corruption / reinstall validation、release UAT、documentation / support boundary。

**G9 Exit：** 独立用户能安装、建局、持续游玩、保存恢复，并在真实失败时得到可理解路径。

---

# 8. 不阻塞 V0 Core Closure 的已通过方向

除 Package 0–5 外，其它增强默认不允许插队。尤其包括：

- Narrative Preference；
- Bookmark / Notes / Chronicle export；
- 完整 Provenance / epistemic / conflicting evidence；
- Organization / World Chronicle / Shared History；
- Reality Correction；
- Model Profiles / model split / usage dashboard / Debug Mode；
- Source Library 作品化；
- Reference Library；
- Creator / Preview / Publish UX；
- Visual Runtime / authored map；
- generic Action Intent。

Dynamic UI 不在此列表：**Package 5 已被 Owner 定为 Core Closure 前必做。**

---

# 9. 未批准方向

本轮未被 Owner 通过的旧提案与原提案 56–60 不进入 Task Axis；不因相邻能力批准而静默复活。

---

# 10. Promotion / execution

本轴已获 Owner 批准并允许 Promotion 到 CURRENT Product / Architecture / Roadmap / Status。

当前执行顺序仍从 Package 0 开始：MW-018 + MW-019 Combined Owner UAT 不被跳过。

新独立 implementation outcome 仍需：

```text
CURRENT route
→ Task Shaping
→ flat MW Work ID
→ Task Packet
→ Codex implementation
→ GPT Independent Review
→ integration
→ Owner UAT（player-facing outcome）
```

本轴授权的是**顺序和阶段**，不是允许一次性把所有 Package 直接交 Codex。