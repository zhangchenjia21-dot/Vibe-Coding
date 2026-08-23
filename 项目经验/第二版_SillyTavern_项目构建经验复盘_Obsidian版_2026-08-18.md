---
title: 第二版 SillyTavern 项目构建经验复盘
aliases:
  - 酒馆游戏第二版工程与可玩性复盘
  - SillyTavern Second-generation Lessons Learned
  - 第二版酒馆游戏开发经验
created: 2026-08-18
last_updated: 2026-08-23
status: evergreen
project: 酒馆游戏开发
type: 项目复盘
scope:
  - G1-G8
  - G9-Owner-UAT
  - Stage-UAT
  - AI-RPG
  - Runtime
  - Product
  - Asset-Ecosystem
tags:
  - 酒馆游戏
  - SillyTavern
  - 项目复盘
  - AI游戏
  - 可玩性
  - 软件架构
  - LLM应用
  - Agent协作
  - 测试
  - UAT
  - 工程管理
related:
  - "[[第一版_SillyTavern_项目构建经验复盘_Obsidian版]]"
  - "[[G8_StageUAT_交互响应性与空壳世界阻塞发现_v1.1_2026-08-18]]"
  - "[[G8-UAT-01_PlayableRuntimeSeed与NarrativeAuthority收口规格_v1.1_2026-08-18]]"
  - "[[15_酒馆游戏_RuntimeContextOrchestration与模块化复杂度控制裁定_v1.1_2026-08-18]]"
  - "[[16_酒馆游戏_RuntimeWorldMaterialization与当局游戏资产演化裁定_v1.1_2026-08-18]]"
  - "[[核心游玩重构_产品与架构总纲_CURRENT]]"
  - "[[核心游玩重构_Turn与WorldInitiative架构规格_v1.0_2026-08-23]]"
---

# 第二版 SillyTavern 项目构建经验复盘

> [!summary] 一句话结论
> 第二版最重要的成果，是建立了**可靠的 Program Authority、原子 Formal Turn、Save / Restore、Crash Recovery、Creation Project、声明式动态 UI Host 与多 Agent 开发治理**；第二版先在 G8 Stage UAT 暴露“工程正确性不等于可玩性完整性”，又在 G9 Owner 真人试玩进一步暴露了更深层的根因：**我们没有持续把“这是一个拿来玩的游戏”作为最高产品约束，导致支撑性设计与安全边界逐渐反客为主，最终把世界主动性、剧情推进和模型创造力一起压掉。**
>
> 第一版教训是：**核心真实纵向链验证得太晚。**
>
> 第二版新增教训最终升级为：**产品目的 → 核心体验 → 核心循环 → 架构 → 能力 → 实现，这个顺序不能倒过来；工程正确只能服务产品核心，不能替代产品核心。**

---

# 1. 如何看待第二版：不是“又失败了一次”，而是工程正确性与游戏可玩性第一次真正相撞

第二版没有重演第一版的旧 Turn Engine 失败。

相反，它成功完成了大量第一版没有真正完成的基础：

- 模型不拥有正式世界状态；
- Program 决定 Formal Outcome；
- Narrative 只 realization 已批准的玩家安全结果；
- 一次提交只有一个 Formal Turn；
- Dice / RNG 归 Program；
- Save / Continue / Restore 语义明确；
- Crash / Resume / Recovery exactly-once；
- Creation Project 成为唯一新游戏权威；
- 声明式动态 UI Host 有正式 server vertical；
- 多动作可以瞬态预演后原子提交；
- Legacy 双轨被及时退休；
- GitHub 成为跨聊天动态事实源；
- Executor-owned Git Integration 减少了重复交接。

这些成果都是真实的，不应因为 Stage UAT 暴露问题而被否定。

但第二版也第一次把以下完整链真正交给项目所有者游玩：

```text
Creation Project
→ Final Create
→ Game Instance
→ Runtime
→ Product UI
→ Real Provider
→ Player free input
→ Multi-turn experience
```

这时出现的失败不是 HTTP、数据库、类型或事务失败，而是：

```text
调用都成功
+
状态都一致
+
测试都通过

但是

世界是空的
可交互物是假的
推荐是占位
叙事与权威状态矛盾
剧情没有长期增长模型
```

因此，第二版最准确的定义是：

> **工程基础已经成立，但“系统能正确运行”与“玩家面对的是一个活着的游戏世界”之间，仍缺一套可玩性与实例演化的正式 Gate。**

---

# 2. 第二版真正做对了什么

## 2.1 Program Authority 与原子世界变化是正确根基

第二版明确建立：

```text
Model interprets / proposes
↓
Program validates / resolves
↓
Formal Delta
↓
Atomic Commit
↓
Narrative realizes
```

这一方向的**持久化与正式提交原则**必须保留，但 G9 Owner UAT 证明：不能把它继续解释成“所有有意义世界变化都必须来自玩家输入授权”。

正确保留的是：

> **Model authors candidates; Program commits durable reality。**

需要退休的是：

> **Player Authorization 被扩大成 World Authorization。**

没有 Authority，世界会失控；只有 Authority、没有世界自主性，世界会变成一个可靠但不会发生事情的空壳。

## 2.2 G6 / G7 的 Save、Restore、Recovery 仍然是长期世界的必要基础

当未来新增动态 NPC、地点、物品和当局游戏资产时：

- 同一实体必须有稳定 identity；
- Restore 必须回到当时的资产与状态版本；
- Recovery 不能重复创建同一个动态角色或地点；
- Branch 应能分叉不同的世界演化。

因此，第二版此前对幂等、阶段产物、原子提交和恢复边界的投入不是过早浪费，而是未来动态世界物化的必要地基。

## 2.3 Creation Project、Host 与 Legacy Retirement 都是正确收口

第二版成功建立：

- 三 Workspace Creation Project；
- 浏览器关闭 / 服务重启后恢复；
- Contribution active / dormant；
- Final Create 确定性；
- 声明式 Extension Surface；
- safe materialized DTO；
- source identity 穿过 Host assembly；
- 旧 One Draft / Five Sections production authority 归零。

这些工作说明第二版已经具备继续演化的架构纪律。

## 2.4 Cross-chat Freshness 与 Decision Propagation 是一次重要治理升级

本项目允许多个 GPT 聊天框和 Agent 并行工作，因此仅“读到最新 GitHub 文件”不够。

第二版后期正式形成：

```text
Freshness
→ 我是否读到最新事实

Decision Propagation
→ 最新事实是否改变 Stage / Task DAG / 下一步
```

这避免了新的架构裁定只停留在某个聊天或某份文档里，而没有进入正式路线。

## 2.5 Owner-aligned Execution 减少了无价值交接

此前一度形成：

```text
Grok 实现
→ Codex 重读
→ Codex 纯代 commit / push
→ 再审核
```

后来改为：

```text
Task Executor
→ implements
→ validates
→ precise commit / push

Independent Reviewer
→ reviews canonical commit
```

这降低了上下文重载、重复劳动和无意扩 scope。

---

# 3. 第二版最根本的错误：Engineering Correctness 被误当成 Playability Completeness

第二版的自动化 Gate 可以证明：

- Turn 不重复；
- 状态不越权；
- Save / Restore 正确；
- Crash 可恢复；
- Host 可组装；
- Creation 可创建；
- Multi-action 原子；
- 类型、lint、build 全通过。

但它们没有回答：

- 玩家当前有真实 NPC 可以交谈吗？
- 玩家看到的地点真的能进入吗？
- 玩家看到的物品真的能拿取吗？
- 创建表单中的人物、资源、目标有没有进入 Runtime？
- 输入框上的“推荐”是真推荐还是占位映射？
- 连续玩 10、50、100 回合以后，世界如何产生新人、新地点、新冲突？

因此必须正式冻结：

```text
Engineering Correctness
!=
Playability Completeness
```

阶段 Gate 不能只有工程正确性，还必须单独判断：

> **玩家是否拥有足够真实、可理解、可交互、可持续演化的游戏对象与行动空间。**

---

# 4. Fixture Realism Gap：富测试世界掩盖了真实 Creation 空壳

此前大量 Runtime 测试使用手写 Definition：

- 有角色；
- 有物品；
- 有 Connection；
- 有 Knowledge；
- 有 Consequence；
- 有 Commitment；
- 有 Background progression。

这些测试正确证明了：

> **当 Runtime 已经有内容时，系统可以正确运行。**

但它们没有证明：

> **正式 Creation 流程创建出来的 Game Instance 自己是否有内容。**

真实 AI-created Runtime 最终一度只有：

```text
1 Region
1 Place
1 Scene
Player
characters = []
items = []
relationships = []
connections = []
knowledgeFacts = []
```

这就是典型的 Fixture Realism Gap：

```text
Rich fixture passes
!=
real product-created instance is playable
```

以后任何生成型产品都必须同时拥有两类纵向证明：

```text
Handwritten Rich Fixture Vertical
+
Real User Creation → Real Instance Vertical
```

二者不能互相替代。

---

# 5. Contract Exists != Capability Exists != Production Semantics Proven

第二版中“动态五推荐”就是典型案例。

代码已经存在：

- `SuggestedPlayerInput`；
- `PlayerRoleplayAssistanceContract`；
- `center.suggestedActions`；
- `prefill_composer`。

表面看，Contract、DTO、UI slot 都在。

但真实数据源只是：

```text
availableMoves.map(...)
```

因此老存档长期显示两个固定移动按钮，新建空壳世界则一个都没有。

必须正式区分：

```text
Contract Exists
→ 类型和接口存在

Capability Exists
→ 有真实 Owner 与实现

Production Semantics Proven
→ 正式数据源、真实状态变化、失败降级与产品消费均已验证
```

以后审核任何“已完成能力”，都必须追问：

1. 谁是生产 Owner？
2. 数据从哪里来？
3. 是否随真实状态变化？
4. 失败时怎样降级？
5. 是否经过正式入口和真实消费者？
6. 是否只是 fixture / preview / placeholder？

---

# 6. Integration of Meaning：模块都能调用，不代表整体产品语义成立

传统 Integration Test 常验证：

```text
A calls B
B returns DTO
C renders result
```

第二版证明还需要另一类集成：

> **Integration of Meaning｜语义整体集成。**

例如：

```text
Narrative 说：你进入酒馆
Runtime Session 说：你仍在码头
```

调用链可能全部成功，但意义上互相矛盾。

又例如：

```text
Creation 收集“重要开局人物”
Final Create 成功
Runtime characters = []
```

字段保存、编译、启动都成功，但产品承诺没有兑现。

因此最终纵向测试必须验证：

- 输入语义；
- Program Outcome；
- Runtime canonical state；
- Narrative realization；
- Product projection；
- 下一回合可交互能力；

在业务意义上保持一致。

---

# 7. Affordance Consistency：玩家看到的具体对象必须真的能交互

第二版 UAT 出现：

- 叙事创造裹斗篷的路人；
- 玩家尝试搭话；
- Runtime 没有 `characterRef`；
- NPC 只能沉默。

又出现：

- 叙事创造附近酒馆；
- 玩家连续要求进入；
- Runtime 没有 Connection；
- Narrative 先写进入，Session 又把玩家留在码头。

正式经验：

> **Narrative-visible concrete interactable must have an authoritative Runtime referent。**

也就是：

```text
看得见
+
被呈现成可交互
→
必须有真实 identity / capability / state
```

允许没有 Runtime identity 的内容只能是明确的 ambient background，例如：

- 雾；
- 雨；
- 风声；
- 模糊人群；
- 远处不可直接互动的轮廓。

不能让 ambient 在下一句突然变成一个 Narrative-only target。

---

# 8. Bounded Context != Starved Context

第二版为了防止信息泄露和上下文膨胀，正确建立了 Bounded Context。

但实际实现一度把 Narrative 所需的真实玩家信息也裁掉：

- 玩家身份；
- 公开背景；
- 当前目标与牵挂；
- 重要往事；
- 初始人格；
- 语言风格；
- NPC public description。

于是玩家明确输入：

> “想了想自己的经历与目标。”

Narrative 只能输出泛化空话。

因此必须区分：

```text
Bounded Context
=
只给当前职责所需的最小高信号事实

Starved Context
=
连完成当前职责所必需的事实都没有
```

诊断 Context 时不能只看 token 数量，还要问：

> 模型当前被要求完成的任务，所需事实是否完整？

---

# 9. 防模型越权不能演变成禁止世界成长

第二版长期强调：

```text
Model does not own reality
```

这是正确的。

但一度形成了错误的隐含推论：

```text
Model cannot directly mutate authority
↓ 被错误理解为
Model should never create or evolve canonical world content
```

如果坚持这一错误理解：

- 所有 NPC 必须开局预写；
- 所有地点必须开局预写；
- 所有物品必须开局预写；
- Narrative 不能安全引入新人；
- 长期剧情没有新的落脚点；
- 资产作者被迫枚举整个未来世界。

正确原则应是：

> **Model authors candidates; Program owns reality。**

也就是：

```text
Model generates typed content / patch
↓
Program validates schema / ownership / constraints / refs
↓
Atomic commit
↓
Candidate becomes canonical truth
```

Proposal 是事务输入，不是“永远只能停留在建议状态”。

---

# 10. 第二版遗漏的关键数据层：Game-local Canonical Assets｜当局游戏资产

第一版已经形成过“人物卡进入游戏后形成实例副本”的方向，但第二版 Runtime 实现中没有足够早地把它提升为正式三层模型。

正确模型是：

```text
Source Asset Library
(World Pack / Character Card / Expansion)
↓ snapshot / bind
Game-local Canonical Assets
(当局游戏资产)
↓ instantiate / runtime projection
Authoritative Runtime State
```

三层职责：

## Source Asset

- 作者与 Creator 维护；
- 跨游戏复用；
- 不被当前存档反写；
- 作为模板、规则和锚点。

## Game-local Canonical Asset

- 本局独立世界定义；
- 可以新增 NPC、地点、物品、连接；
- 可以演化已有角色的目标、身份、别名、公开描述、私有动机；
- 保存 provenance 与 stable identity；
- 不污染外部资产库。

## Runtime State

- 当前 Location；
- HP / Need / Status；
- inventory holder；
- timer / cooldown；
- 临时关系值；
- pending action；
- Turn / Event history。

必须分别回答：

```text
这个对象是谁 / 是什么？
vs
这个对象现在怎样？
```

缺少当局游戏资产层，会导致“模板不可改、状态又不适合承载定义性演化”的结构性空洞。

---

# 11. 长期剧情必须在架构层回答“新内容从哪里来”

AI RPG 的阶段评审不能只看前 1–2 回合。

必须做 Long-session Thought Experiment：

```text
10 回合后？
50 回合后？
100 回合后？
```

至少回答：

- 新 NPC 从哪里来？
- 新地点从哪里来？
- 新物品和线索从哪里来？
- 已生成内容如何保持 identity？
- 动态内容如何 Save / Restore？
- Source Asset 更新是否影响旧局？
- 世界增长后 Prompt 是否仍 bounded？
- 新内容如何进入 Context Router？
- Narrative 什么时候可以把新对象写给玩家？

长期目标应是：

```text
Authored / Created Anchors
+
Game-local Canonical Asset Graph
+
Authoritative Runtime State
+
Bounded Model-generated Proposals
+
Program-validated JIT Materialization
+
Persistent Continuity
```

并继续保持：

```text
World Growth != Prompt Growth
```

---

# 12. Narrative Authority 与 Narrative Freedom 必须同时存在

第二版一度把 NPC 普通口头回应也理解成可能产生 durable state，导致模型倾向保守沉默。

应明确区分：

## Ephemeral Narrative Realization

允许：

- 寒暄；
- 玩笑；
- 拒绝；
- 犹豫；
- 反问；
- 表情与语气；
- 不持久化的即时态度。

例如 NPC 可以说：

> “刚认识就谈朋友？你倒直接。先聊两句吧。”

这不自动建立 Friendship、Commitment 或 Knowledge。

## Durable / Authoritative Consequence

仍需 Program commit：

- Relationship mutation；
- Commitment；
- Knowledge truth；
- Location / Item / Time；
- Formal Event；
- hidden fact；
- mechanical state。

同时，Player Agency 要比 NPC 即时表现更严格：

> Narrative 不替玩家新增未表达的羞耻、后悔、恐惧、喜爱、决心、自我评价或行动。

---

# 13. 正确的诊断顺序：先找 authoritative referent，最后才调 Prompt

当玩家反馈“AI 不响应”“NPC 不说话”“移动不生效”时，不应首先调 temperature、文风或 Prompt。

推荐诊断顺序：

```text
1. 玩家看到的对象是否有 authoritative referent？
2. Runtime 是否有当前 capability？
3. Semantic 是否选中了正确 Candidate？
4. Authorization / Program Outcome 是否允许并提交？
5. Product Session 是否投影了新状态？
6. Narrative Context 是否足够且 player-safe？
7. Narrative validator 是否误杀合法表达？
8. 最后才检查 Prompt / model style / sampling。
```

这可以避免用 Prompt 修补数据模型或 Authority 漏洞。

---

# 14. UAT 不应只在“工程全部完成”后第一次真实游玩

真人 UAT 不应该承担数据库竞态、Crash 注入或幂等调试；这些仍属于自动化。

但真实玩家路径也不能等到阶段几乎 CLOSED 时才首次运行。

更合理的节奏：

```text
每个核心 Product Vertical 成立
↓
短 UAT：1–3 回合

Creation → Game Instance 成立
↓
Real Creation UAT：5–10 回合

Stage Exit 前
↓
Meaning UAT：10–20 回合

长期架构 Freeze 前
↓
100-turn Thought Experiment / stress corpus
```

UAT 重点不是“帮工程师测内部实现”，而是验证：

- 世界是否像真实世界；
- 玩家看到的 affordance 是否可信；
- 输入是否被理解；
- NPC 是否自然回应；
- 地点移动是否一致；
- 推荐是否有用且动态；
- 游戏是否有继续玩的动力。

项目所有者本轮真实输入应永久进入 regression corpus，而不是只留在聊天记录中。

---

# 15. Agent 与 Review 流程的新增教训

## 15.1 Freshness 之后必须有 Decision Propagation

读到新文件不等于更新了工作方向。

每个重大决策必须检查：

- Stage Gate；
- Current Task；
- Prerequisite；
- Task DAG；
- obsolete plan；
- Protocol Freeze timing；
- Owner / Boundary；
- next recommendation。

## 15.2 Independent Review 不能只审核文件范围和测试数量

还要追问：

- 数据源是否真实；
- capability 是否生产实现；
- 正式用户路径是否消费；
- 下一回合是否还能继续；
- 真实 Game Instance 是否与 fixture 同等有意义。

## 15.3 不要把“可能还要做的事”全部塞进当前 Agent 任务

此前曾出现 Grok 已完成 UI，Codex 指令却被扩成再次开发 Provider、Preview、Legacy 清理等大任务。

正确做法：

- 当前 Task 只收当前 blocker；
- Independent Review 后再按 Owner 分派；
- 当前安全任务不因未来 P2 设计被打断；
- 新架构影响后续 DAG 时，传播影响而不是机械提前实现。

## 15.4 Contract / Fixture / Placeholder 必须在报告中明确标注成熟度

建议报告统一写：

```text
Contract only
Fixture proven
Production vertical proven
Product UAT proven
```

禁止笼统写“已支持”。

---

# 16. 可复用的 AI 游戏阶段收口检查表

每次准备关闭较大阶段，至少回答以下问题。

## 16.1 工程正确性

- Authority 是否唯一？
- Turn / transaction 是否原子？
- Save / Restore / Recovery 是否一致？
- hidden / player-safe 边界是否正确？

## 16.2 可玩性完整性

- 当前真实实例至少有什么可做？
- 是否存在真实 NPC、地点、物品或其它互动对象？
- 玩家是否能得到有意义的反馈？
- 游戏是否能连续进行多个回合而不暴露空壳？

## 16.3 Real Creation Vertical

- 正式用户创建出来的实例是什么样？
- Creation 字段是否真的进入 Runtime / Product？
- 是否只在富 fixture 中成立？

## 16.4 Affordance Consistency

- 玩家看见的具体对象是否都有 ref？
- UI 显示的能力是否真的可执行？
- Narrative 与 Session 是否一致？
- 推荐是否只引用当前真实对象？

## 16.5 Capability Provenance

- Contract、Owner、Implementation、Data Source、Consumer 分别是谁？
- 功能是否只是 placeholder？
- 失败时是否安全降级？

## 16.6 Instance Evolution

- Source Template 进入本局后如何独立演化？
- 新实体写到哪里？
- 哪些字段 immutable / evolvable / private / runtime-owned？
- Source 是否保持不变？

## 16.7 World Growth

- 10 / 50 / 100 回合后新内容从哪里来？
- 新内容是否有 stable identity？
- Save / Restore 是否保留？
- Context 是否仍 bounded？

## 16.8 Context Sufficiency

- 模型当前职责所需事实是否完整？
- 是否把 bounded 错做成 starved？
- 不同模型职责是否使用不同 working set？

## 16.9 Product Assistance

- 推荐、预览、提示是否是真能力还是静态映射？
- 是否随真实状态更新？
- 是否只辅助玩家，而不替玩家执行？

## 16.10 Integration of Meaning

- Player Input、Semantic、Program Outcome、Runtime State、Narrative、UI、下一回合 Capability 是否在语义上闭环？

## 16.11 Primary Purpose / Core Value

- 玩家最主要拿这个产品来干什么？
- 这个核心用途在当前真实产品中真的成立了吗？
- 我们有没有把支撑性能力误当成核心价值？
- 某个安全/工程约束是否已经伤害了核心体验？
- 与最简单的现实替代方案相比，核心体验是否至少不明显更差？

任一关键项只能回答“Contract 有”“Fixture 有”或“工程测试通过”时，不应直接判 Stage Closed。

---

# 17. 第二版经验相对第一版经验的新增层次

第一版主要教会我们：

```text
不要在核心纵向链不稳定时提前横向扩展
不要让模型拥有 Program 权威
不要为未发布旧实现背永久兼容债
```

第二版先新增：

```text
纵向链工程正确
!=
整体产品语义正确

Program Authority 正确
!=
世界内容足够可玩

Bounded Context
!=
Starved Context

Contract Exists
!=
Capability Exists

Rich Fixture Passes
!=
Real Creation Instance Playable

Source Asset Immutable
!=
Game-local World Cannot Evolve
```

G9 Owner UAT 再进一步新增：

```text
局部架构正确
!=
产品路线正确

Guardrail 正确
!=
Guardrail 可以无限扩大

长期可靠
!=
值得长期玩

功能 / 设计很多
!=
产品核心成立
```

两版经验必须一起使用。

第一版防止我们再次建出一个复杂但不可靠的 Turn Engine。

第二版现在还必须防止我们建出一个**可靠、完整、可扩展，却连核心用途都没有兑现的产品**。

---

# 18. 后续建议：把这篇复盘当成“核心价值 + 可玩性 + 世界演化检查表”

每当酒馆游戏或其它 AI 互动产品准备：

- 关闭阶段；
- 冻结协议；
- 构建资产生态；
- 引入新的模型职责；
- 宣称 dynamic / intelligent / personalized；
- 设计新的安全或控制边界；

都应先问：

### 问题 1

这个产品最主要是拿来干什么的？用户为什么要用它？

### 问题 2

我们证明的是“代码可调用”，还是“用户获得了核心价值”？

### 问题 3

正式用户创建出来的实例，与测试 fixture 一样有意义吗？

### 问题 4

玩家看到的每一个具体 affordance，都有 authoritative referent 吗？

### 问题 5

如果连续运行 100 回合，新人物、新地点、新关系和新剧情从哪里来？

### 问题 6

模型是否被错误地完全禁止写入世界，或反过来被允许绕过 Program 直接宣布世界事实？

### 问题 7

我们是不是把 Bounded Context 做成了 Starved Context？

### 问题 8

某个“已完成能力”到底处于 Contract、Fixture、Production Vertical 还是 Product UAT 哪一层？

### 问题 9

某项安全、可靠性或扩展性设计是在保护产品，还是已经开始替产品做主？

### 问题 10

如果用户直接使用一个更简单的替代方案，核心体验反而更好吗？如果是，我们为什么还认为复杂方案成功？

如果这些问题没有答案，应该先补产品定义、架构或真实纵向证明，而不是继续增加更多外围功能。

---

# 19. G9 Owner 真人试玩追加复盘：最深层错误是没有抓住产品核心

2026-08-23 的真实 Owner 试玩把问题从“世界空壳 / 可玩性不足”进一步升级为产品路线问题。

Owner 的直接反馈包括：

1. 进入游戏后漫无目的，不知道下一步做什么；
2. 长时间停留在初始场景，碰不到第二个场景与第二个可持续角色；
3. NPC、敌人、机会、时间、承诺、局势等设计几乎没有机会被触发；
4. 因为世界不主动发生事情，玩家没有机会体验选择与后果；
5. 回合结束没有自然钩子；
6. 直接让通用网页版 AI “主持一个酒馆游戏”，反而更像一个会主动推进剧情的游戏主持人。

这说明早先“工程正确 ≠ 可玩性”仍然只是中层诊断。

更深层根因是：

> **我们没有在项目最上游持续冻结：酒馆游戏首先是用来玩的。世界、资产、Runtime、长期一致性、安全边界、扩展机制，都只是为了让“玩”这件事更丰富、更可靠、更持久。**

当这个最高坐标系不明确时，局部设计会自然优化自己：

```text
Authority 更严格
Persistence 更完整
Schema 更清晰
边界更安全
模块更多
协议更丰富
```

但没有人持续追问：

```text
这些东西叠加以后，游戏是不是更想让人继续玩？
```

最终就可能得到：

> 一个很难坏掉、很难越权、很容易恢复，但也几乎不会主动发生事情的系统。

这不是“少几个功能”的问题，而是产品目的没有成为架构最高约束。

---

# 20. 为什么这个错误能活到这么晚

## 20.1 项目启动时，全生命周期流程还没有 Stage 0 Product Definition

第二版酒馆主体最早设计时，当前的 `Product Definition & Canonical Product Spec Gate` 尚未进入开发流程。

因此当时实际顺序更接近：

```text
已有愿景
→ 大量架构讨论
→ Runtime / Authority / State / Recovery
→ 功能与阶段逐步扩展
→ 很晚才由真实 Owner 体验验证“到底好不好玩”
```

缺少了一个强制环节来明确：

- 产品最主要拿来干什么；
- 核心体验是什么；
- 哪些东西只是支撑核心体验；
- 哪个体验一旦失败，后面的工程正确全部失去意义。

因此这次事故也反向验证：后来把 Stage 0 加进生命周期流程是必要的，但 Stage 0 还需要继续强化为 **Primary Purpose / Core Value Gate**。

## 20.2 产品负责人不应该靠“全程不走神”来防止 AI 方案偏航

Product Owner 对产品方向有最终裁定权，但流程不能建立在一个不现实的假设上：

> 只要 Owner 在每次长讨论里都足够专注，就能发现 GPT 提出的所有错误方案。

GPT / AI 提出的方案即使逻辑完整、术语专业、文档详尽，也仍然只是待验证假设。

因此以后 AI 在产品定义和重大架构提案时，应主动暴露：

- 我认为产品的 Primary Purpose 是什么；
- 这套方案如何增强核心价值；
- 哪些约束可能损害核心体验；
- 更简单的替代方案是什么；
- 什么现象一出现就说明当前方案错了；
- 最早什么时候让 Owner 真人体验来证伪。

这样错误不会依赖 Owner 在长文本中“自己抓出来”。

## 20.3 真人体验验证得仍然太晚

如果在世界主循环刚能跑的时候，就让 Owner 连续玩 10–20 分钟，以下问题会很早暴露：

- 世界不主动发生事情；
- 不知道下一步做什么；
- NPC / 新地点不会自然进入；
- 玩家必须自己替系统编剧情；
- 没有继续玩的欲望。

这些现象不需要数据库诊断，也不需要代码 Review。

所以以后应执行：

```text
最小核心循环一旦可运行
→ 立即真人体验
→ 若核心用途不成立，先停外围建设
```

而不是：

```text
先把系统做得越来越完整
→ 最后才问好不好用 / 好不好玩
```

---

# 21. 永久开发顺序：先冻结更高层，再建设更低层

本项目已经连续两次出现“低层先做、上层后补”导致返工。

资产开发阶段的教训是：

```text
通用核心引擎
→ 通用能力
→ 专用扩展
```

而本次核心游玩事故说明，还有更高的一层：

```text
产品目的
↓
核心价值 / 核心体验
↓
核心循环 / 核心用户旅程
↓
产品与架构边界
↓
共享基础
↓
具体能力
↓
实现与外围扩展
```

越上层的错误，越会造成下层成倍返工。

因此以后任何重要系统、架构或功能，如果不能明确说明：

> **它如何服务产品 Primary Purpose / Core Value？**

就不得仅因为“设计更完整”“以后可能有用”“工程上更漂亮”进入高成本实现。

---

# 22. Guardrail 只能保护游戏，不能替游戏做主

这次最典型的架构漂移是 Player Agency。

原始正确目标：

```text
模型不能替玩家决定玩家本人做什么
```

逐渐扩大成：

```text
没有玩家输入授权
→ 世界也不应该发生新的重要事情
```

于是用于保护玩家自主权的 Guardrail，最终剥夺了 NPC / 世界的自主性。

今后所有安全 / 权限 / 一致性设计都必须额外问：

```text
这个限制保护了什么？
这个限制误伤了什么？
能否缩窄到真正危险的边界？
```

对于 AI 游戏，至少要明确区分：

```text
Player Agency
!=
World / NPC Agency
!=
System / Rule Authority
```

“模型不能直接提交正式世界事实”不能再次被误读为“模型不能创造事情发生”。

---

# 23. 简单替代方案是必要的产品反证

以后不只做内部工程 Gate，还要保留一个外部基线：

> 用户不用我们的复杂系统时，会怎么完成同一核心任务？

酒馆游戏当前最直接的反证就是：

```text
直接告诉通用网页版 AI：
“我要玩酒馆游戏，你负责主持。”
```

如果正式产品在以下核心维度长期明显更差：

- 开局吸引力；
- 世界主动性；
- NPC 活跃度；
- 创意承接；
- 场景推进；
- 选择与后果；
- 下一步钩子；
- 继续游玩的欲望；

那么不能回答：

> “但我们的存档更严谨、事务更正确、协议更完整。”

这些应该成为建立在优秀核心体验之上的额外优势，而不是拿核心体验交换来的优势。

---

## Related Notes

- [[第一版_SillyTavern_项目构建经验复盘_Obsidian版]]
- [[G8_StageUAT_交互响应性与空壳世界阻塞发现_v1.1_2026-08-18]]
- [[G8-UAT-01_PlayableRuntimeSeed与NarrativeAuthority收口规格_v1.1_2026-08-18]]
- [[15_酒馆游戏_RuntimeContextOrchestration与模块化复杂度控制裁定_v1.1_2026-08-18]]
- [[16_酒馆游戏_RuntimeWorldMaterialization与当局游戏资产演化裁定_v1.1_2026-08-18]]
- [[核心游玩重构_产品与架构总纲_CURRENT]]
- [[核心游玩重构_Turn与WorldInitiative架构规格_v1.0_2026-08-23]]