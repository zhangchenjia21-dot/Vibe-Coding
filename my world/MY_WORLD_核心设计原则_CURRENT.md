---
title: my world｜核心设计原则
status: current-canonical-core-design
version: 1.1
created: 2026-08-26
updated: 2026-08-26
stage: G2 AI Conversation Spine
scope: cross-stage product and runtime semantics
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜核心设计原则 CURRENT

## 0. 文档定位

这份文件记录 `my world` 跨阶段长期有效的**核心产品与 Runtime 设计原则**。

它综合以下来源中已经验证、踩坑或反复收敛过的设计资产：

- SillyTavern 新版主体时期的 World OS / FC2 / Canonical Ownership / Context / Save-Restore / Runtime World Materialization 设计；
- The World / DeepSeek Harness 的长期真实试玩、失败模式与 DSH 长局经验；
- `my world` G1 Foundation 的真实 Windows 证据；
- Product Owner 在 2026-08-26 对模型 Narrative、文本质量与篇幅、错误容忍与玩家时间线主权的最新裁定。

本文件**继承语义，不迁移旧宿主实现**。旧项目中的 TypeScript/Web/SillyTavern/DSH Workspace/Markdown Runtime 等实现形态不因此获得迁移授权。

发生冲突时：

1. 用户当前明确指令最高；
2. 本文件高于旧项目历史文档、旧经验总结和已经被本文件明确修正的 prevention-first 规则；
3. `architecture/foundation/Foundation架构决策_v1.0_2026-08-26.md` 的 Godot / GDScript / same-process / Persistence Candidate 等 G1 技术结论继续有效；
4. 本文件只修正其中“模型语义必须先成为候选、Program 必须判断所有现实是否允许成立”这一过强的产品解释。

核心总纲：

> **Model freedom first. Reversibility over prevention.**
>
> **Narrative richness over artificial brevity.**
>
> **模型尽可能自由地创造世界；Runtime 让世界可靠持续；玩家拥有时间线的最终决定权。**

---

# 1. 第一原则：不要以“永不犯错”为目标设计 AI RPG

开放式 AI RPG 的错误空间没有有限枚举。

模型可能：

- 误解玩家一句话；
- 补写玩家没有明确说出的微小动作；
- 让 NPC 偶尔说出不够严谨的信息；
- 产生设定矛盾；
- 判断错一个游戏机制；
- 把某个关系、地点或物品细节写错；
- 给出玩家不喜欢的发展；
- 在 Narrative 与结构化状态之间偶发不一致。

`my world` 不把“消灭所有这些错误”作为架构目标。

禁止默认采用以下增长路径：

```text
发现一种模型错误
→ 增加一个 Regex
→ 增加一个禁止规则
→ 增加一个 Confirmation
→ 增加一个 Narrative Validator
→ Prompt / Context / State Machine 继续膨胀
```

这种 prevention-first 路线在前代已经表现出明显副作用：

- Narrative 越来越拘谨；
- 自然语言自由度下降；
- Prompt 和状态机不断变复杂；
- 为防错引入的新机制本身制造更多 bug；
- 玩家为极少数错误承担大量操作税；
- 工程稳定性与 RPG 体验同时下降。

因此正式改为：

> **对可逆的游戏内错误，优先降低恢复成本，而不是提高模型表达成本。**

---

# 2. 玩家拥有时间线，而不是被迫接受一次生成

AI RPG 的核心容错能力应该是可逆性。

长期产品应逐步提供：

```text
Regenerate / Retry
latest-turn correction
修改上一条玩家输入后重试
explicit Save / Restore
必要的 Timeline recovery / Branch 能力
```

这些能力不是普通“便利功能”，而是 AI 生成式游戏的基础产品能力。

目标指标不应只是：

> 模型有没有犯错？

更重要的是：

> **模型或游戏犯错后，玩家回到满意状态需要付出多少成本？**

正式原则：

> **Player owns the timeline.**

玩家对自己的游戏历史拥有最终主权。

但：

> **Reversibility != frictionless arbitrary rewind.**
>
> **Save Point != Timeline Node.**

玩家拥有恢复和选择历史的最终权利，不意味着每个历史 Turn 都应该暴露成一个零成本、容易误触的一键回档点。高频低风险纠错靠近 Narrative；重大历史恢复应表达明确玩家意图。具体 UX 与底层语义从 `MY_WORLD_架构_CURRENT.md` → `architecture/persistence/时间线存档与可逆性设计.md` 深入。

---

# 3. Narrative 默认自由，不建设“故事审查器”

模型 Narrative 是 `my world` 的核心产品价值之一。

正式方向：

- 不为普通 Narrative 建立动作白名单；
- 不要求模型只能使用程序预批准的句式；
- 不因模型补写低风险的玩家动作就默认判整轮失败；
- 不因孤立的 lore / knowledge / characterization 错误就默认拒绝输出；
- 不为了证明“绝不越权”持续增加 Confirmation；
- 不把 Narrative 压成只能机械复述 Formal Outcome 的模板层。

例如：

> “你本能地后退半步，把手按在剑柄上。”

即使玩家没有逐字授权“后退半步”，也不应因为这一句自动触发硬失败。

如果玩家认为这不符合自己的角色，他更自然的权力是：取消、重生成、latest-turn correction 或明确历史恢复。

这不取消 Player Agency，而是把 Player Agency 的最高形态从：

> 模型永远不能替我补任何动作

提升为：

> **我对最终保留哪条时间线有决定权。**

重大不可逆产品操作仍可有专门确认，但不能把这种例外扩张成普通 Narrative 的默认交互模式。

---

# 3A. Narrative Quality：丰富而不灌水，篇幅由场景与 Context 决定

文字不是 `my world` 的附属说明。

> **Narrative 本身就是主要游戏内容。**

环境、人物、对白、动作、信息揭示、世界变化、后果、伏笔和节奏，大量都通过模型生成文本被玩家直接体验。因此不能把“回答越短越快”当成默认优化方向。

正式原则：

> **Narrative richness over artificial brevity.**
>
> **Scene-led length; context-enabled richness.**

## 3A.1 不人为固定模型应该写多长

默认禁止仅为了 UI、延迟、成本或“回合整齐”而增加：

```text
固定每 Turn 字数
固定最低字数
固定最高字数
“请简短回答”默认指令
为了界面方便而截断正文
无真实产品理由的 max_tokens 硬上限
```

这不意味着“越长越好”。

正确目标是：

```text
简单过渡 / 低信息密度动作
→ 可以自然简短

重要场景 / 高戏剧密度 / 多人物互动 / 大量后果
→ 可以充分展开

篇幅
→ 由模型 + 场景 + 当前相关 Context 自然决定
```

玩家一句简短输入也可能触发很丰富的世界反馈；玩家一段复杂计划也不应被强行压成几句摘要。

## 3A.2 文本质量不等于文本长度

评价 Narrative 优先看是否增加了**有价值的游戏体验与信息**，包括：

- 环境与感官细节是否建立具体场景；
- NPC 是否有具体动作、声音、态度和个体化表达；
- 对话是否自然，并真正推进人物或局势；
- 玩家行动是否产生清楚、可玩的后果；
- 世界是否发生了新的、因果上可解释的变化；
- 是否揭示了值得玩家记住或继续追踪的信息；
- 是否保持人物、地点、关系与前文连续性；
- 节奏是否匹配当前场景，而不是每轮都用同一种长度与结构；
- 是否在真正需要玩家决策时停下，而不是替玩家把重大选择全部演完。

长文本如果只是重复、空泛形容、同义改写和无关铺陈，仍然是低质量。

短文本如果准确完成一个应该快速过去的 beat，也可以是高质量。

因此：

> **Richness means useful density + immersion + causality, not filler.**

## 3A.3 不把 Narrative 降级成状态摘要或隐藏菜单

应避免默认输出变成：

```text
你进入酒馆。
你发现一个可疑的人。
你可以：
1. 询问
2. 离开
3. 战斗
```

除非具体玩法场景确实需要明确选项，否则模型应优先维持自然语言互动小说 / TRPG 式叙事，让玩家继续以自然语言表达任意行动。

Narrative 可以自然包含：

- 连续场景描写；
- 多角色互动；
- 长对白；
- 内外部事件交错；
- 非立即可见但合理进入玩家视野的信息；
- 对行动后果的充分展开。

不要为了结构化状态、UI 卡片或机制系统，把这些内容挤压成工程摘要。

## 3A.4 Context bounded 不能变成 Context starved

`Context stays bounded` 的含义是：

> 不让 Prompt 随世界总规模和 Session Length 无限制增长。

它不意味着：

> 为了省 token，只给模型一小段贫乏上下文，然后接受干瘪、泛化的 Narrative。

正式要求：

```text
bounded
!= starved
```

Context Assembly 必须让模型获得**当前场景真正需要的世界材料**，例如必要的地点、人物、关系、最近事件、当前冲突、相关知识与状态。

如果模型输出持续过短、泛化、缺少人物和世界细节，排查顺序优先是：

1. 当前相关 Context 是否不足；
2. GM prompt 是否无意中要求简短或过度结构化；
3. 模型选择 / 参数是否影响创作质量；
4. Runtime 是否没有提供足够可写的世界事实与行动者状态；
5. 最后才判断是否需要新的专门机制。

不要用“强制至少 N 字”掩盖 Context starvation，也不要用“限制最多 N 字”掩盖 UI / streaming / latency 问题。

## 3A.5 产品与 UI 必须承受长 Narrative，而不是逼模型适配小框

如果高质量 Narrative 需要更长输出，产品层应适配它：

- 真流式显示；
- 长文本稳定滚动；
- 合理 readable width；
- 玩家可在生成中 Cancel；
- 长历史由正式 Context / Timeline 管理，而不是把所有 Transcript 永远塞回 Prompt；
- UI 不因当前可视空间不足而物理截断模型输出。

因此：

> **解决长输出的方式是把阅读、streaming、Context 和持久化做好，而不是先把模型变短。**

## 3A.6 Narrative Quality 最终是产品 Gate

自动化可以证明：

- 没有硬截断；
- streaming 正常；
- 长文本不冻结；
- Context 结构符合 contract。

但以下问题最终必须由真实 Owner gameplay 判断：

- 是否沉浸；
- 是否信息充足；
- NPC 是否像活人；
- 世界是否在推进；
- 文本是否值得继续读；
- 长度是否自然，而不是过短或灌水；
- 是否明显弱于直接使用优秀模型进行 RPG 对话。

工程指标不能替代 Narrative Product Value Acceptance。

---

# 4. 修正旧原则：从“Program owns facts”到“Runtime owns durability”

旧项目曾长期使用：

> `Program owns facts; Model writes prose.`

以及：

> `Model authors candidates; Program commits reality.`

这些原则在防止宿主状态污染、半提交和隐藏权限问题上有价值，但如果把它们解释为：

> “每一条游戏语义都必须先被 Program 判断为正确，模型只能在批准范围内叙述”

就会过度限制 AI RPG。

`my world` 当前修正为：

> **Model authors the world; Runtime makes it durable; Player owns the timeline.**

具体含义：

### Model 可以负责

- 自然语言理解；
- Narrative；
- NPC 行动与动机；
- 世界事件；
- 新人物 / 地点 / 物品创造；
- game-local 定义演化；
- 开放式后果与剧情发展；
- 语义判断与创造性裁定；
- 对游戏世界的广泛 authoring。

### Runtime / Program 必须负责

- 数据身份与稳定引用；
- 原子写入与事务边界；
- Save / Restore / Timeline 的技术正确性；
- 文件与数据库完整性；
- Secret、系统权限与外部副作用边界；
- 防止半新半旧、部分写入、物理损坏；
- 在需要结构化持久化时，把模型 authoring 可靠落成 durable state。

Runtime 的职责是**让世界可靠存在和可恢复**，不是充当一个不断扩大规则集的“创作审查委员会”。

如果需要结构化状态，允许模型同时输出或通过后续步骤生成 semantic delta / patch；但不要为了机器结构而限制 Narrative 本身的表达宽度。

---

# 5. 错误分级：允许游戏犯错，不允许基础设施不可恢复地坏掉

## 5.1 默认允许、可通过游戏 UX 修复的错误

包括但不限于：

- Narrative 不理想；
- 模型误解输入；
- NPC 个性漂移；
- 游戏规则偶发误判；
- 轻微 Player Agency 过度代演；
- 世界设定矛盾；
- NPC knowledge 偶发越界；
- Narrative 与结构化状态偶发不一致；
- 玩家不喜欢某个随机或创造性结果。

这些错误首先使用：

```text
regenerate
retry
edit-and-retry
explicit restore when appropriate
```

以及后续 Context / Prompt / 模型选择改善处理。

不要自动升级为一条新的全局禁止规则。

## 5.2 必须保持硬边界的错误

包括：

- API Key / credential 泄露；
- 模型获得任意文件系统 / OS 执行权限；
- 存档或数据库物理损坏；
- 原子提交被破坏；
- Restore 后出现半旧半新的持久状态；
- 不可恢复的数据删除；
- 真实外部系统的不可逆副作用在无授权下发生；
- UI / Cache / Transcript 静默成为第二 live truth 并导致恢复失效。

这些属于系统完整性与安全，不属于“限制模型创作能力”。

正式边界：

> **自由发生在游戏语义与创作层；强约束集中在不可逆基础设施边界。**

---

# 6. 世界事实的三层模型：Source → Game-local → Runtime

SillyTavern 后期已经形成并验证了一套应由 `my world` 承接的事实模型：

```text
Reusable Source Assets
(World Pack / Character Card / Expansion)
↓ new game snapshot / bind
Game-local Canonical Assets
↓ instantiate / current projection
Runtime State
```

## 6.1 Source Assets

Source 定义：

- T0 前的世界；
- 历史与参考资料；
- 初始人物、地点、制度；
- 世界规则与惯性；
- 可复用创作材料。

运行中的游戏不得把本局变化反写 Source。

## 6.2 Game-local Canonical Assets

一局创建后，应拥有自己的 living world definitions。

模型可以在本局中：

- 创建新 NPC；
- 创建新地点；
- 创建新物品；
- 改变人物目标、身份、描述、关系定义；
- 形成新的长期世界事实；
- 让世界偏离 Source 原轨迹。

一旦某个动态内容成为本局正式存在，它需要稳定 identity / provenance，并能被 Save / Restore / Timeline 保留。

## 6.3 Runtime State

回答“这一刻发生什么”，例如：

- 当前地点；
- 当前关系值；
- 当前物品持有；
- 状态 / cooldown；
- 当前行动与事件；
- 当前世界时间。

不要把 Source、Game-local Definition、Runtime State 混成同一份文本或同一类 Godot Resource。

---

# 7. Source 提供惯性，行动者创造历史

正式原则：

> **Source provides inertia; actors create history.**
>
> **史料提供惯性，行动者创造历史。**

历史世界包可以定义 T0 前事实和原历史参考，但 T0 之后不能把“历史上接下来会发生什么”当作自动事件队列。

新的历史应由：

```text
玩家行动
+
NPC 自主目标
+
Faction 策略
+
制度 / 资源 / 信息 / 地理约束
+
偶发事件
↓
共同产生
```

必须避免前代 DSH 暴露的 **Protagonist Causal Monopoly｜主角因果垄断**：

```text
Source 推进世界
玩家改变世界
NPC 只回应玩家
```

目标是：

> **玩家可以改变历史，但不是唯一创造历史的人。**

---

# 8. Off-screen != Inactive，但不要造全世界逐 tick 模拟器

重要 NPC / Faction 离开玩家视野后仍应继续拥有自己的目标和可能行动。

但：

```text
持久世界
!=
每个 NPC 每 tick 调一次模型
```

未来 G5 应优先采用有界的事件 / 优先级驱动演化，例如由：

- 时间推进；
- 重大世界事件；
- 高影响 Actor / Faction；
- 已存在的 Agenda / Obligation / Front；
- 与当前世界因果相关的触发；

决定哪些行动者需要被推进。

目标是让世界有自己的因果，而不是制造昂贵的“全宇宙模拟器”。

---

# 9. Knowledge Boundary 是世界目标，不是限制 Narrative 的无限规则源

继续保留：

> **World Truth != NPC Knowledge != Player Knowledge.**

NPC 应尽量只使用自己能够合理获得的信息：亲历、身份渠道、传闻、被告知、观察、推断或明确的特殊能力。

但是游戏世界中的一次 knowledge mistake 不再自动触发 prevention-first 扩张。

处理顺序优先为：

1. 改善 Context selection / provenance；
2. 改善模型输入；
3. 让玩家通过取消 / 重生成 / correction 纠正明显问题；
4. 只有真实高频、严重破坏玩法的问题才考虑专门机制。

真实 Secret、私人系统数据和凭据泄露仍属于硬安全问题，不在此容错范围内。

---

# 10. Context：世界可以无限增长，模型工作集必须保持有界

SillyTavern 后期的重要设计继续保留：

```text
Asset Library
!= Game Enabled Asset Set
!= Current Runtime Relevant Set
!= Current Model Visible Working Set
```

以及：

```text
Runtime Relevant != Model Visible
```

世界有数千实体，不等于一轮 Prompt 需要数千实体。

安装很多 World Pack / Mod，不等于每轮都加载全部定义。

Dependency Graph 也不等于 Prompt Inclusion Graph。

目标：

```text
Game State / Event History ↑↑↑
ordinary Turn Model Context ≈ bounded
```

Context 成本应该主要由**当前相关复杂度**决定，而不是 Session Length、数据库总规模或安装资产总量决定。

但：

> **Bounded context != starved context.**

模型必须获得足以理解当前场景、人物、关系、冲突与近期因果的相关材料。Context optimization 的目标是去掉不相关信息，不是饿死 Narrative。

---

# 11. 确定性后台推进不等于模型调用

如果 Program 可以安全、确定、低成本地推进：

- timer；
- cooldown；
- 普通资源消耗；
- routine bookkeeping；
- 简单阈值；
- 明确生命周期状态；

就不需要为了“AI 世界”而强行调用模型。

```text
World Time / State progression
→ deterministic Runtime update
→ zero model call when appropriate
```

模型用于需要开放语义、人物选择、世界创造和 Narrative 的地方。

---

# 12. Save / Restore / Timeline 是可逆性基础设施

前代已验证的长期语义应继续继承。

## 12.1 Save 发生在稳定 committed boundary

Save 保存的是一个已经成立的世界，而不是一次正在执行中的 Provider 请求。

不把 failed / cancelled / half-written execution 作为正常 Save Point。

## 12.2 Snapshot / Save material 不是第二 live truth

Snapshot 可以用于恢复，但普通运行中不能出现：

```text
Live World State
+
另一套 Live Snapshot State
```

两个并行事实源。

## 12.3 Restore 是 Runtime-owned、可验证、原子的

Restore 不应依赖模型“重新理解旧聊天”来恢复事实。

应恢复目标世界 / Timeline，并重新构建与该节点一致的 Agent Context。

必须避免：

> 世界回去了，但模型仍记得被回滚的未来。

## 12.4 Branch 代表真正的新未来

单纯浏览旧节点不应自动制造 Branch。

更自然的语义是：

```text
离开原未来
→ explicit restore / internal timeline navigation
→ 第一次从旧状态成功产生新的后续
→ 新 Branch 成立（若产品选择暴露该能力）
```

这些语义服务于玩家时间线主权，但不要求把任意 Timeline Node 做成一键公开回档点。

---

# 13. UI 继续是 Projection，不是第二真相

继续保留：

> **UI is a projection of game truth, not a second truth source.**

模型自由不意味着 UI 可以偷偷维护另一套：

- Relationship；
- Inventory；
- Location；
- Quest；
- Character state；
- Save metadata。

UI 可以发起 mutation / retry / restore，但最终显示应从当前游戏事实与 Timeline 派生。

---

# 14. No Phantom 从“硬 Narrative Gate”降级为“一致性目标”

旧项目的 `No Phantom World Change` 对发现 Narrative / state 脱节很有价值。

但 `my world` 不再把它解释为：

> Narrative 中出现一个程序尚未批准的世界变化，就必须拒绝整段输出。

新的解释是：

> **Narrative 与 durable world state 应尽量保持可解释的一致性；出现偏差时优先 reconcile / retry / restore，而不是不断收紧模型。**

如果一个 Turn 需要持久化世界变化，应让 Runtime 尽可能把模型创作落成结构化、原子、可恢复的状态；如果失败，应保持上一个稳定状态并允许玩家重试，而不是留下物理半提交。

因此：

- `No Phantom` 仍是质量观测项；
- 不再是默认 Narrative censorship gate；
- 高发偏差应推动更好的模型上下文 / state materialization；
- 偶发偏差允许通过玩家可逆操作处理。

---

# 15. Unlimited Attempt：机制不是玩家输入白名单

继续继承：

```text
Player owns Attempt
World owns Consequence
GM owns Playability of the Consequence
```

玩家自然语言可以提出规则库没有预枚举的尝试。

技能、机制、Domain、Router、Mod Capability 都不能被错误实现为：

> “只有列表中的行为才允许输入。”

程序结构用于帮助世界持续、一致和可恢复，不用于把 AI RPG 降级成隐藏菜单树。

---

# 16. World Loop + Life Loop

长期世界同时需要：

```text
World Loop
局势 → 事件 → 后果 → 时间推进

Life Loop
日常 → 自由活动 → 人物互动 → 关系 / 人格积累
```

只做 World Loop 会让世界像事件模拟器；只做 Life Loop 会让世界缺少历史与外部压力。

继续保留：

> **Compress dead time; stop at meaningful choice.**
>
> **压缩无意义时间，停在有意义选择。**

“充分展开 Narrative”与这条原则不冲突：重要场景可以写长，无意义时间不应为了凑长度而铺陈；真正需要玩家做重大决定时，应给玩家行动空间。

但“停下”也不应演变成每个潜在错误都先弹一次确认。

---

# 17. 四层产品架构与模块内部 L3→L0 同时成立

`my world` 的宏观产品层级：

```text
RPG Experience Layer
↓
The World Runtime
↓
Engine Adapter
↓
Mature Game Foundation / Godot
```

这是 ownership / responsibility boundary，不要求制造四套进程或四套空目录。

业务模块内部继续使用：

```text
L3 → L2 → L1 → L0
```

低层不反向依赖高层，跨模块通过稳定公开边界。

两套“四层”不是同一概念：

- 宏观四层回答“游戏体验、Runtime、引擎适配、通用 Foundation 谁负责什么”；
- L3→L0 回答“单个业务模块内部怎样控制依赖与复杂度”。

都不应被形式主义地扩张成大量空 interface / wrapper / service。

---

# 18. 对阶段路线的直接要求

## G2｜AI Conversation Spine

优先证明：

- 高质量模型输出不被新壳限制；
- streaming / cancel / retry 自然；
- Narrative 不因工程约束明显弱于简单模型聊天；
- 不默认要求短回答，不为了 UI / latency convenience 新增无真实必要的 output cap；
- UI 能稳定呈现和滚动较长的真实 Narrative；
- 不为预防普通模型错误建立复杂 Confirmation / Regex / Narrative Validator；
- regenerate / retry 是第一批可逆能力。

G2 当前没有完整 World Context 时，短输出本身不代表最终 Narrative 目标；真正质量判断要随着 G2-05 Context Assembly 与后续 World Semantics 继续进行。

## G3｜Persistent Game & Timeline

G3 不只是“存档功能”，还是**AI 游戏容错基础设施**。

优先证明：

- reliable current persistence / reopen-resume；
- explicit Save / Load / Restore；
- future-memory isolation；
- 误读档后的恢复能力；
- latest-turn correction 与 durable state 正确衔接。

任意历史 Turn 一键回退不是第一代默认交付要求。

## G4｜World Pack

正式落实 Source → Game-local Canonical Assets → Runtime State。

允许模型在本局创造和演化人物、地点、物品等，不要求全部内容必须预先存在于 Source。

## G5｜World Semantics & GM Runtime

重点是：

- 模型能自由创造有生命力的世界；
- NPC / Faction 自主行动；
- 世界不是玩家因果附属物；
- Runtime 提供 durability，而不是用规则淹没模型；
- 玩家相关 Context 能支撑具体、连续、信息丰富的 Narrative；
- Player Agency 主要通过自然输入 + 时间线主权保护，而非不断 Confirmation。

## G6｜RPG Experience

把 Cancel、重生成、Save / Restore / Timeline 等能力设计成自然的玩家操作，而不是工程调试面板；UI 必须为长 Narrative 与多行自然语言行动提供舒适空间。

## G7｜Long-session Context

目标不是积累更大的 Prompt，而是让世界规模增长时 ordinary Turn working set 仍保持 bounded。

同时必须避免 Context starvation：长期运行不能以“Context 很省”为代价，让 Narrative 越来越泛化、遗忘人物或失去世界细节。

---

# 19. 已明确降级 / 废弃的旧默认模式

以下旧模式不再作为 `my world` 的默认设计方向：

1. **Program owns facts; Model writes prose** —— 不再作为全局创作限制；Program/Runtime 重点拥有 durable / integrity boundary。
2. **No Phantom = Narrative hard rejection** —— 降级为一致性质量目标。
3. **玩家未逐字授权的任何动作都是硬 Player Agency failure** —— 不再成立；低风险叙事补全允许，玩家通过时间线主权最终裁定。
4. **每发现一种误解就增加 Regex / Confirmation** —— 明确反对。
5. **用巨大规则集换取模型“永远正确”** —— 明确反对。
6. **Dependency = Prompt inclusion** —— 明确反对。
7. **Persistent world = every NPC every tick model call** —— 明确反对。
8. **Source history = future event scheduler** —— 明确反对。
9. **恢复世界 = 重发聊天让模型猜** —— 明确反对。
10. **Narrative quality 可以为了架构整洁长期牺牲** —— 明确反对。
11. **为了 UI、延迟或工程方便默认把 Narrative 压短** —— 明确反对；应先解决 streaming、阅读、Context 与性能本身。
12. **用固定最低字数制造“丰富感”** —— 明确反对；丰富来自相关信息、人物、因果与沉浸，不来自灌水。

---

# 20. 最终设计口号

`my world` 的长期方向可以压缩为：

> **模型自由创造世界。**
>
> **Narrative 丰富但不灌水，篇幅由场景决定。**
>
> **Runtime 让世界持续。**
>
> **行动者共同创造历史。**
>
> **Context 只携带当前真正需要、但足够丰富的世界。**
>
> **玩家拥有时间线。**

英文速记：

> **Model freedom first.**
>
> **Narrative richness over artificial brevity.**
>
> **Runtime owns durability.**
>
> **Source provides inertia; actors create history.**
>
> **Context stays bounded, not starved.**
>
> **Player owns the timeline.**

任何新的架构、规则、Prompt、Validator、UI 或流程，如果明显削弱这些原则，必须证明它解决的是一个已经真实发生且价值高于新增复杂度的问题，而不能仅凭“理论上可能出错”获得建设授权。
