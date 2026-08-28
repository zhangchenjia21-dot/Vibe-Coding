---
title: my world｜AI RPG 开发路径与阶段设计经验
status: canonical-experience-reference
version: 1.0
created: 2026-08-28
updated: 2026-08-28
purpose: reusable-development-path
canonical_roadmap: ../MY_WORLD_总体规划路线图_CURRENT.md
---

# my world｜AI RPG 开发路径与阶段设计经验 v1.0

## 0. 文档定位

这份文档不是 `my world` 的 Current Roadmap，也不是某个阶段的 Task Packet。

它的目标是把 SillyTavern、The World / DSH、旧真实游戏和 `my world` 自身已经付出过的开发成本，整理成一份**未来同类项目可以直接复用的开发路径经验**。

未来如果再次开发：

- 对话式 AI RPG；
- AI GM 游戏；
- 长期持续世界；
- World Pack / Character / Mechanic 资产驱动游戏；
- 本地单人生成式 RPG；

优先阅读：

```text
MY_WORLD_项目启动总纲_CURRENT.md
MY_WORLD_核心设计原则_CURRENT.md
MY_WORLD_架构_CURRENT.md
MY_WORLD_总体规划路线图_CURRENT.md
本文件
```

旧 SillyTavern / The World 文档继续作为历史证据，不要求未来项目重新拼读全部历史才能恢复这些结论。

本文件的总原则：

> **继承被真实开发和真实试玩证明的顺序；不要继承旧宿主实现。**

---

# 1. 历史开发过程的总体统计与事实

以下不是精确的“文件数统计”，而是按真正独立的产品/架构/验证步骤归类。

## 1.1 SillyTavern 新版主体

顶层路线曾经历约 12 个主要阶段：

```text
真实模型兼容
→ 最小 Runtime
→ World capability
→ Dice / mechanics
→ Long-term continuity
→ Save / Continue / Restore
→ Crash / Resume / Recovery
→ Product UI
→ Asset ecosystem
→ Provider expansion
→ Alpha
→ Windows Release
```

资产阶段内部又经历约二十个独立设计/实现 slice，包括：

- Compatibility Audit；
- Runtime Foundation；
- Unified Asset Protocol；
- Adapter / Compiler / Binding；
- Creator Foundation；
- Shared Creator Core；
- World Creator；
- Character Creator；
- Use My Assets Game Creation；
- Expansion Creator；
- Expansion real Runtime binding；
- Primary Asset E2E；
- Owner First Playable preparation；
- 多轮 Independent Review correction。

关键事实：**资产生态不是因为 Schema 不够完整失败，而是因为真实“选资产 → 创建游戏 → 开始玩”纵向打通得太晚。**

## 1.2 G8 产品化 UAT

Engineering 看似已经完成后，Owner 真实试玩又触发两个正式修复纵切：

```text
G8-UAT-01
Playable Runtime Seed + Narrative Authority

G8-UAT-02
Game-local Semantic Materialization + Living World Convergence
```

真实问题包括：

- Creation 建出来的是空壳世界；
- Narrative 写出酒馆和人物，但 authoritative Runtime 并不存在；
- Opening 没有具体 NPC / destination / hook；
- Creation 字段没有真正 materialize 成玩家可交互内容；
- Inventory / Player profile / Information Surface 语义不一致。

因此：

> **“创建成功”必须由可玩性证明，而不是数据库 row 或 parser PASS 证明。**

## 1.3 The World / DSH

实际经历的关键步骤至少包括：

```text
Bare capability probe
→ Reality Gate A
→ Reality Gate B
→ Player panel
→ Save / Restore
→ Save Policy
→ Restore reliability
→ Long-play maintenance
→ Real playtest findings
→ successor-project lessons
```

DSH 长局最重要的产品发现不是更多技术，而是：

- Persistent World 不自动等于 Autonomous Evolving World；
- Source history 不能自动成为未来事件表；
- NPC 纸面人格不等于真实 Agency；
- 长局 Context / maintenance 会逐渐变重；
- Save / Restore 语义正确比实现漂亮更重要；
- 真实长局比短 demo 更容易发现真正产品问题。

## 1.4 旧资产库规模

历史资产不是抽象设想，已经真实维护过：

- 两类明显不同的 World corpus；
- 约 60+ Character Cards；
- 17 个左右 Expansion Packs；
- 历史/政治/战争/治理类 Expansion；
- 关系、名望、家族、战斗、身体、组织、魔法等通用/幻想 Expansion。

因此 World / Character / Expansion 三类 Primary Source 有真实产品证据；但旧项目同时证明：**有证据的资产类型也不代表应该一次实现全部 Creator、版本管理、依赖管理和 UI 扩展能力。**

---

# 2. 最重要的开发顺序经验

## 2.1 先证明产品价值，再建设长期基础设施

推荐顺序：

```text
Foundation feasibility
→ real AI conversation value
→ reliable persistence
→ real content/game creation
→ living world semantics
→ richer RPG UI
→ long-session performance
→ authoring/mod platform
→ alpha/release
```

不推荐：

```text
先设计完整世界数据库
+ 完整资产协议
+ 完整 Creator
+ 完整 Mod framework
+ 完整 UI DSL
→ 最后才第一次真正玩
```

原因：生成式 RPG 的最大未知通常不是“能不能定义 Schema”，而是：

> **这些能力组合起来以后，玩家是否真的获得更好的 RPG 体验。**

---

## 2.2 Foundation Selection 必须是 spike，不是品牌选择

Foundation 阶段只证明真正影响后续路线的能力：

- 本地 Windows 启动与打包；
- 中文输入/长文本；
- streaming / cancel；
- UI responsiveness；
- 图片/字体/文件；
- persistence primitive；
- export/exe。

不要在 Foundation 阶段提前做：

- Universal ECS；
- DI framework；
- Event Bus platform；
- generic plugin runtime；
- asset marketplace；
- large schema forest。

正式原则：

> **Commodity Foundation, Owned Game Semantics.**

---

## 2.3 AI Conversation 必须在 World 系统之前先经过真实 Owner Playtest

第一个产品 Gate 应尽早回答：

```text
这个模型 + 这个 UI + 这个交互方式
是否已经比普通工程 demo 更像一个值得继续玩的 AI RPG？
```

必须真实验证：

- streaming；
- cancel；
- retry/regenerate；
- 多回合 continuity；
- Narrative 长度和阅读体验；
- Provider failure 后恢复；
- 与简单模型聊天 baseline 比较。

如果 Narrative 已经因为架构过重而明显退化，不能继续用“以后 World 做完就好了”无限推迟产品判断。

---

## 2.4 Persistence 应在复杂 World Semantics 前成立

长期 AI RPG 的核心资产是“这一局自己的历史”。

因此可靠 persistence 应早于完整 NPC/Faction simulation。

应先证明：

```text
Game identity
→ current durable state
→ reopen
→ Save
→ future
→ Restore
→ future-memory isolation
→ crash/recovery
```

这样后续任何 World semantics 都天然有 durability owner。

历史经验明确反对：

> 先把几十个 World Domain 做完，再补 Save/Restore。

---

# 3. Application Shell 必须早于正式 New Game 内容选择

一个常见遗漏是先做内容 selector，再发现产品没有真正容纳 selector 的入口。

推荐：

```text
Application Launch
→ Main Menu
→ New Game / Continue
```

而不是：

```text
启动直接进入 current Game
+ 临时弹出 Pack selector
```

更重要的是底层必须分离：

```text
Application Lifetime
!= Game Session Lifetime
```

Main Menu 不应该只是覆盖在已经打开的 Game Runtime 上。

否则 multi-Game 到来时会再次拆生命周期。

---

# 4. 第一代建局应尽量只有一条正式产品路径

旧项目曾同时考虑：

- AI-assisted blank creation；
- manual creation；
- Source asset creation；
- local player fallback；
- Draft / published asset；
- 不同 role / version / dependency 组合。

这些同时存在会显著放大状态空间。

`my world` 第一代选择更窄的路径：

```text
World Pack
→ Entry/T0
→ Expansion 0..N
→ Exactly 1 Player Character Card
→ 0..N Guaranteed NPC Character Cards
→ minimal settings
→ Compatibility Review
→ Final Create
```

这一经验可推广为：

> **早期如果存在一个明确、可信、可扩展的建局主路径，就先只支持它；其它创建方式等第一个产品纵向稳定后再加。**

这样可以避免为了支持“空白世界 AI Creation”提前建设复杂 semantic materializer / form DSL / fallback matrix。

---

# 5. Primary Source 应分类型建最小合同，不要先造万能资产协议

推荐共享：

```text
stable identity
asset type
version
exact generation / fingerprint
```

然后不同 Source 类型各自拥有最小真实语义。

例如：

```text
World
→ Lore / Instructions / Entry / authored assets

Character
→ profile / private source / portrait / eligibility

Expansion
→ mechanic instructions / capability declaration / dependencies
```

不要因为三种资产都叫“Source”就要求所有资产支持同样的：

- sections；
- dependency modes；
- module graph；
- UI contribution；
- creator fields；
- runtime state。

历史教训：万能 Schema 很容易变成玩法必须迁就 Schema。

---

# 6. Source Generation 必须 immutable；旧 Game 必须 exact pin

稳定逻辑 ID 只回答：

> “这是什么资产？”

它不回答：

> “这一局当时使用的是哪一份内容？”

因此 Game Creation 必须 pin：

```text
stable identity
+ version
+ exact content fingerprint / immutable generation
```

尤其 visual assets 也必须受 generation 约束。

如果旧 Game 继续从可变 external folder 读取 portrait/map，即使文本 metadata 被 pin，视觉仍可能被 Source update 静默修改。

推荐 Managed Library：

```text
current generation
+ historical immutable generations still referenced by Games
```

第一代 New Game UI 不一定需要让玩家选择历史版本；内部保存与产品 picker 是两个概念。

---

# 7. Character Card 的正确第一代语义

Character Card 是 reusable Character Source，不应只等于 Player Character，也不应默认等于当前场景 NPC。

第一代最清楚的用途：

```text
Player Character
Guaranteed NPC Character
```

Guaranteed NPC 表达：

> **玩家明确要求这个角色属于本局。**

它不自动表达：

- opening cast；
- current scene；
- player-known；
- relationship；
- active Context。

这比把玩家暴露给 `bound_only / opening_character / player_character` 三层内部角色更简单。

如果以后真实需求需要“第一幕必须出现”，再增加 opening placement / Entry cast，而不是过早把所有生命周期都编码成 role 类型。

---

# 8. Expansion 应后于 World + Character First Playable

Expansion 与 World/Character 的复杂度不同。

World / Character 第一阶段主要解决：

```text
Source
→ exact selection
→ game-local materialization
→ Context
```

Expansion 往往还涉及：

```text
Source
→ compatibility
→ Runtime capability
→ mechanic state
→ Context
→ UI
→ Save/Restore
```

因此推荐：

```text
First Playable A
World + Character
→ Owner UAT

then

Expansion Vertical
→ real observable Runtime effect
→ Owner UAT B
```

不要第一次资产试玩就同时把三类复杂性叠在一起。

---

# 9. Expansion 的“接通”必须按真实效果判断

历史上已经出现：

```text
Expansion Source exists
Manifest exists
Binding exists
moduleRef exists
```

但游戏行为完全没有变化。

所以：

> **Binding proof != gameplay proof.**

第一代 Expansion 最少需要一个 positive/negative control：

```text
Enabled
→ formal Runtime / Context / mechanic behavior observable

Disabled
→ that effect absent
```

到 G6 再验证真实 mechanic state 的 UI consumer；到 G8 再外部化 declarative UI contract。

---

# 10. New Game 必须有 Compatibility Review 和 Atomic Final Create

Wizard 中间步骤只是“编辑 Composition”，不应边选边创建 Game。

推荐：

```text
editing composition
→ review exact selection
→ explicit Final Create
→ atomic create
```

Review 应让玩家看见真实选择：

- World；
- Entry；
- Player Character；
- Guaranteed NPC；
- Expansion；
- control mode；
- 关键设置。

### 历史 selection bug

旧项目曾出现：

- 进入 Source-player 模式就自动选择 `eligible[0]`；
- 同 stable ref 的 v1/v2 UI 同时显示 selected；
- 在 v2 卡上操作却修改了 v1 binding。

因此：

> **List visibility / chooser mode != selection.**
>
> **Authoritative selection must identify an exact source snapshot.**

### Final Create replay

旧项目还真实出现过：

```text
Game 已经创建成功
→ HTTP response 丢失
→ 客户端仍持旧 revision
→ retry 永久 stale
```

所以高价值 Final Create 应从第一天考虑：

- create identity/fingerprint；
- response-loss replay；
- double-click exactly-once；
- same intent returns same Game；
- different intent fail closed。

---

# 11. Source Library 与 Game Library 必须分开

Source Library 回答：

> 我安装了哪些可复用内容？

Game Library 回答：

> 我有哪些已经开始并拥有自己历史的游戏？

不能混成一个“资产/存档管理器”。

G3 one-current-Game 并不是错误；在真正出现多个 Game 需求之前保持简单是合理的。

但一旦 New Game 需要同时保留 A/B 世界，就应正式增加 multi-Game lifecycle，而不是继续覆盖 `current-game.sqlite`。

---

# 12. Creation Success 必须经过 Minimum Playable Reality Gate

最危险的假成功：

```text
row created
parser PASS
manifest PASS
Runtime boot PASS
```

但玩家进入后：

- 没有人；
- 没有地点可去；
- 没有具体问题；
- Narrative 只能自己凭空编交互对象。

因此至少要有一个现实 Gate 回答：

> **创建出的 Game 是否已经提供具体、可继续玩的 Opening？**

在 `my world` 中：

- G4 First Playable 先证明 Source/Composition 能给真实 DeepSeek 足够具体的 Opening；
- G5 再负责 authoritative semantic materialization、JIT NPC/Place/Item 和真正 Living World。

避免把完整 World Materializer 又提前塞回 New Game Creation。

---

# 13. Owner UAT 应插在风险边界，而不是只放阶段最后

推荐产品 UAT 节点：

```text
AI Conversation first playable
Persistence reality test
World+Character asset-created first playable
Expansion first playable
Living World stage UAT
RPG UI stage UAT
Long-session UAT
Authoring UAT
Alpha UAT
```

不要等全部模块都完成以后才第一次让 Owner 看 New Game Wizard。

### Owner 做什么

Owner 判断：

- 好不好用；
- 像不像游戏；
- 是否愿意继续玩；
- UI 是否自然；
- Narrative /角色/世界是否对；
- 恢复流程是否可信。

### Agent 做什么

Agent 负责：

- fixture；
- logs；
- DB proof；
- export；
- script；
- regression；
- exact hash；
- crash injection；
- Windows automation。

不要把工程验证劳动转嫁给 Owner。

---

# 14. Independent Review 的价值：专门找“测试天然会 PASS”的证据漏洞

Independent Review 不只是再跑一遍测试。

它应该寻找：

- assertion 是否真覆盖目标语义；
- marker 是否真的进入被排除的 future；
- mock 是否绕过正式 Runtime path；
- proof module 是否只是测试专用；
- UI projection 是否和 authoritative selection exact 对齐；
- crash/retry 是否使用真实原请求身份；
- test fixture 是否比 production contract 更宽松。

`my world` G3-07 IR-01 就发现过：B-marker 根本没有进入 B future，导致 `not contains(marker)` 是 vacuous assertion。

这类错误代码可能没有 bug，但会污染最终 Gate 证据链。

---

# 15. Real Provider Gate 应只放在它真正证明价值的地方

不是每个数据库单元测试都要调用真实 Provider。

但以下能力如果目标包含模型语义，就不能永久用 mock 代替：

- Narrative Quality；
- Context isolation after Restore；
- Source-created Game Opening；
- World/Character identity continuity；
- Living World materialization；
- Expansion 对真实 GM behavior 的影响；
- long-session context quality。

原则：

> **Deterministic harness proves semantics; real Provider proves product reality.**

两者不能互相替代。

---

# 16. Context 必须 bounded，但不能 starved

长期项目最容易从一个极端摆到另一个极端：

```text
把整个世界都塞进 Prompt
```

变成：

```text
为了省 token 只给模型极少信息
```

两者都不对。

必须长期区分：

```text
Source Library
Game selected source
Game-local entity set
Player-known set
Runtime relevant set
Model-visible working set
```

真正目标：

> 世界越来越大，但每个普通 Turn 的 Context 由当前相关复杂度决定。

Narrative 越来越泛化时，应先检查 Context starvation，而不是用“最低字数”补救。

---

# 17. World Agency 应是事件/优先级驱动，不是全 NPC tick

DSH 长局暴露 Protagonist Causal Monopoly：

```text
Source 推大势
Player 创造新历史
NPC 主要回应玩家
```

长期目标：

> **Source provides inertia; actors create history.**

重要 NPC/Faction 有：

- Agenda；
- Fear / Cost；
- Red Line；
- Obligation；
- Independent Next Move。

但不要因此建立：

```text
每个 NPC
每个 tick
都调用模型
```

更好的方向：重大时间推进、World Event、高影响 Actor、Agenda/Front、因果触发驱动有界演化。

---

# 18. UI 应在真实 Domain consumer 后抽象

推荐顺序：

```text
fixed real UI
→ stable host slots
→ real Domain projections
→ second/third consumer
→ internal declarative vocabulary
→ external mod UI contract
```

不要：

```text
先设计万能 UI DSL
→ 后来才找 gameplay surface 使用
```

Expansion UI 尤其应该遵守这个顺序：G4 先证明 Runtime effect，G6 再做 Internal UI，G8 再外部化。

---

# 19. Creator 应晚于“资产真的能被使用”

历史上最大的资产路线教训之一：Creator 可以越做越漂亮，但如果 published Source 还不能：

```text
select
→ create Game
→ play
→ save/reopen
```

Creator 只是生产孤岛。

所以推荐：

```text
hand-authored / fixture Source
→ Source contract
→ install/library
→ create/play vertical
→ prove real consumption
→ then Creator
```

Creator 后来再补：

- Draft；
- Import；
- Publish；
- revision；
- AI-assisted typed editing；
- ChangeSet / Undo；
- preview。

---

# 20. 失败时什么时候继续 patch，什么时候重新设计 seam

建议采用 correction budget：

```text
correction-01
→ focused root fix

correction-02
→ inspect root-neighbor / sibling boundary / crash window

仍失败
→ stop patching
→ redesign seam
```

不要无限叠 if / compatibility / special case。

尤其在未发布阶段，若 v0.1 contract 明显错误：

> **优先直接修正确模型，不要为了不存在的用户立即建设 v1/v2/v3 compatibility forest。**

---

# 21. 每个任务的推荐工程流程

高价值 Task 默认：

```text
S0 Freshness / takeover
↓
S1 Read current Authority
↓
S2 Historical/relevant seam audit
↓
S3 Contract + failure matrix before code
↓
S4 Minimal implementation
↓
S5 Self validation
↓
S6 Automated regression
↓
S7 Real Windows / Provider / crash evidence where relevant
↓
S8 Independent Review
↓
S9 Owner UAT when product-facing
↓
S10 Decision Propagation / closeout
```

### Matrix-before-code 特别有价值的领域

- persistence；
- single-writer；
- migration；
- asset version selection；
- Final Create；
- multi-Game switching；
- Save/Restore；
- Expansion binding；
- source update isolation。

它能迫使团队在编码前回答：

> “失败发生在每个边界时，系统应该留下什么真相？”

---

# 22. 高频反模式清单

未来项目看到以下信号应立即审计：

1. **Platform before product**：已经有大量协议/Creator/validator，但还没有第一条真实建局试玩。
2. **Schema drives gameplay**：玩法被迫迁就已经写好的字段。
3. **Parser PASS = product PASS**：没有真实 Runtime/Provider/Owner evidence。
4. **Binding = behavior**：机制只有注册信息，没有可观察作用。
5. **Main Menu as overlay**：底层仍然自动打开 Game，Application/Game Session 未分离。
6. **Mutable Source path**：旧 Game 继续读取会变化的外部目录。
7. **Stable ref guessing latest**：没有 exact generation。
8. **Chooser auto-select**：打开列表就擅自选择第一项。
9. **Version UI aliasing**：同 logical identity 的多个版本共享 selected state。
10. **Draft becomes runtime**：未发布资产直接进入 Game。
11. **Creator before consumer**：资产编辑器先于正式资产建局。
12. **Late Owner UAT**：几十个任务完成后才第一次真实体验。
13. **Empty playable shell**：Game 创建成功但没有具体可玩局面。
14. **Phantom affordance dependency**：Narrative 靠临时编出对象才能继续玩。
15. **Context starvation disguised by formatting**：用字数规则掩盖模型不知道世界。
16. **Every NPC tick**：为了世界自主性创建不可控模型调用量。
17. **UI second truth**：界面维护自己的关系/位置/物品状态。
18. **Restore only DB**：世界回档但 Agent 仍记得未来。
19. **Recovery mixed with Save**：系统保护点污染玩家存档列表。
20. **Infinite correction chain**：同一 seam 不断打补丁而不重新设计。

---

# 23. 未来类似项目可直接复用的阶段模板

如果重新启动一个类似 AI RPG，默认可以先采用：

```text
P0 Product Definition / Baseline
↓
P1 Foundation Feasibility Spike
↓
P2 AI Conversation First Playable
↓
P3 Persistence / Save / Recovery
↓
P4 Application Shell / Source / New Game / Multi-Game
↓
P5 World Semantics / Living World
↓
P6 RPG UI / Internal UI Host
↓
P7 Long-session Context / Performance
↓
P8 Authoring / Mod / External Contract
↓
P9 Alpha Stabilization
↓
P10 Packaging / Release（若真实风险需要独立拆出）
```

这不是不可变模板。它提供的是**风险顺序**：

1. 先证明基础能跑；
2. 再证明 AI 核心值得玩；
3. 再保证历史不会丢；
4. 再建立正式建局与内容来源；
5. 再让世界活起来；
6. 再增加更丰富 UI；
7. 再解决长局规模；
8. 最后才把内部成熟能力外部化成 Creator / Mod 平台。

---

# 24. 如何判断一个新能力该放在哪个阶段

问四个问题：

```text
A. 没有它，当前真实产品纵向是否无法继续？
B. 它依赖的下游事实是否已经存在？
C. 是否已经有一个真实 consumer？
D. 如果现在做错，后面会不会被大量模块依赖？
```

规则：

- A=是：优先当前阶段解决；
- B=否：通常后置；
- C=否：通常先做更薄 seam；
- D=是：先 Reality Check / 第二消费者，再冻结协议。

这就是为什么：

- Main Menu 要早于 Pack selector；
- Source Library 要早于 asset-only Wizard；
- World+Character First Playable 要早于 Expansion；
- Expansion Runtime effect 要早于 Expansion UI framework；
- Internal UI Host 要早于 external UI contract；
- 资产正式消费要早于 Creator。

---

# 25. 文档治理经验

长期项目最大的隐性成本之一是“事实散落”。

推荐：

> **Root is map; subfolders are depth.**

顶层只维护少数长期 Owner：

- Product；
- Principles；
- Architecture Map；
- Roadmap；
- Current Status。

详细设计进入 architecture；经验进入 experience；Task/evidence 进入阶段/实现仓库；旧过程进入 archive/Git history。

未来 AI 接手时默认只读最小 current set，不把几百份历史文件全部塞进 Context。

但经验不能只留在旧项目历史里，因此本文件承担“跨项目可复用开发路径”的收敛职责。

---

# 26. 主要历史证据来源

本文件综合但不取代以下历史证据：

### SillyTavern

- `current/新版主体重建总路线.md`
- G8 Stage UAT 的空壳世界 / Authority findings
- `G8-UAT-02_GameLocalSemanticMaterialization与活世界收口规格`
- G9-05E Use My Assets Game Creation 与 correction-01 / correction-02
- G9-05G 真实 Expansion 行为缺口与 Primary Asset E2E
- G9-05H Owner First Playable UAT 准备
- Owner UAT 核心游玩优先级重排 / 世界主动生成裁定
- 第一版 / 第二版 SillyTavern retrospectives

### The World / DSH

- `DSH_GAME_TEST_LESSONS_CORE.md`
- `FUTURE_STANDALONE_BACKLOG.md`
- `PLAYTEST_FINDINGS_2026-08-25_RISK_NPC_AGENCY_DSH_DEBT.md`
- `games/luan-shi-sanguo-2/COMPOSITION.md`
- Save / Restore / Gate A / Gate B evidence

### Asset corpus

- 汉末三国 Character Card batches
- 诸界余辉 Character Cards
- 汉末三国 Expansion Packs
- 通用 / 魔法族 Expansion Packs

### my world

- G1 Foundation real Windows evidence
- G2 Owner Playtest
- G3-01..G3-07 persistence / reality / Owner UAT
- G3-07 IR-01 vacuous marker evidence repair
- 2026-08-28 G4 historical path audit and Owner decisions

---

# 27. 最终可复用口诀

> **先证明能跑，再证明值得玩。**
>
> **先保证历史不会丢，再让世界变复杂。**
>
> **先有正式产品入口，再放内容选择。**
>
> **先有正式 Source consumer，再做 Creator。**
>
> **先 World + Character 试玩，再叠 Expansion。**
>
> **先证明 Runtime effect，再抽象 UI / Mod contract。**
>
> **Source 要 exact pin，Game-local history 不回写 Source。**
>
> **自动测试证明语义，真实 Provider 证明现实，Owner UAT 证明产品。**
>
> **第一消费者拉出最小能力，第二消费者再帮助抽象。**
>
> **如果同一 seam 连续返修，停止补丁，重新设计边界。**

英文速记：

> **Vertical before platform.**
>
> **Consumer before creator.**
>
> **Exact source before mutable convenience.**
>
> **Reality gate before abstraction.**
>
> **Owner UAT before stage complacency.**