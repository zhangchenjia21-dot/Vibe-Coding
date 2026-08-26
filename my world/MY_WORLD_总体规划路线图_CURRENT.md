---
title: my world｜总体规划路线图
status: current-canonical-roadmap
version: 1.8
created: 2026-08-25
updated: 2026-08-26
current_phase: G2
current_status_source: MY_WORLD_G2_CURRENT_STATUS.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
local_project_dir: D:\AI\Projects\my-world
engine: Godot v4.7.2
engine_local_dir: D:\AI\Engine
---

# my world｜总体规划路线图 CURRENT

## 0. 路线图定位

这份文件是 `my world` 的唯一 current 总体开发路线图，负责：

- G1–G9 阶段顺序与关键依赖；
- 每阶段要证明的产品 / 工程能力；
- Task DAG 与 Gate；
- 哪些能力必须延后，避免过度建设。

当前 Task / PASS / Owner UAT 等短周期执行状态由 `MY_WORLD_G2_CURRENT_STATUS.md` 维护；本路线图不再重复承担每次小任务状态同步。

长期事实来源：

- 产品定义：`MY_WORLD_项目启动总纲_CURRENT.md`；
- 核心产品 / Runtime 原则：`MY_WORLD_核心设计原则_CURRENT.md`；
- UI Host supporting architecture：`MY_WORLD_声明式UIHost架构_CURRENT.md`；
- Foundation 技术边界：`MY_WORLD_Foundation架构决策_v1.0_2026-08-26.md`。

正式原则：

> **迁移经验，不迁移宿主债务。**
>
> **Commodity Foundation, Owned Game Semantics.**
>
> **Engine-native, not engine-semantic-coupled.**
>
> **Model freedom first. Reversibility over prevention.**
>
> **Host capability first; external asset protocol second.**
>
> **先跑通真实核心循环，再扩展外围能力。**

---

# 1. 当前冻结的第一代约束

## 1.1 产品形态

- 2D 对话式 AI RPG / 互动小说；
- AI GM Narrative + 玩家自然语言输入是中心体验；
- 长期加入角色立绘、场景图、地图、RPG 状态 UI；
- 不以自由移动 3D 世界为目标。

## 1.2 Runtime / Host

第一代采用：

```text
Godot 4.7.2
Standard / non-.NET Windows x64
GDScript
same-process Runtime
```

Godot 负责 Commodity Foundation；`Game / World / Timeline / Save / NPC / Knowledge / Agent Context / World Pack` 等产品语义由 `my world` 自己拥有。

## 1.3 世界与内容

```text
Reusable Source
→ Game-local Canonical Reality
→ Runtime State
```

- World Pack / Mod 是一级能力；
- Source 负责 T0 前的可复用世界材料和惯性；
- 游戏开始后本局现实优先；
- Source 更新不得静默改写旧局；
- 世界未来由玩家、NPC、Faction 与环境共同创造。

## 1.4 UI 长期骨架

第一代桌面产品沿用经过前代验证、并针对 Godot 重构的 Host 思路：

```text
Player Host | Narrative Host | World Surface Host
左主角栏    | 中央叙事/输入   | 右世界信息栏
```

详细架构见 `MY_WORLD_声明式UIHost架构_CURRENT.md`。

路线原则：

```text
G2  固定产品 UI + Host Slots
↓
G3–G5 真实 Domain Projection
↓
G6  Internal Declarative UI Host
↓
G8  External World Pack / Mod UI Contract
```

不得在 G2 直接建设完整通用声明式 Mod UI 平台。

---

# 2. Task / Gate 规则

阶段固定：

```text
G1 → G2 → G3 → G4 → G5 → G6 → G7 → G8 → G9
```

任务：`G<阶段>-<两位序号>`；Gate：`G<阶段>-GATE`。

规则：

1. 一个 Task 默认只有一个主要 Outcome；
2. 不把 implementation / independent review / Owner UAT 混成一个 Owner 的模糊任务；
3. 下游高耦合实现等待上游 Gate，低成本 exploration 可以提前但不得冒充 canonical commitment；
4. Product-facing Gate 区分 Engineering Acceptance 与真人 Product Value Acceptance；
5. 当前状态以阶段 current-status 文件为准，历史由 Git history 承担。

---

# 3. 总体关键路径

```text
G1  Foundation & Project Bootstrap
↓
G2  AI Conversation Spine
↓
G3  Persistent Game & Timeline
↓
G4  World Pack & Local Content Foundation
↓
G5  World Semantics & GM Runtime
↓
G6  RPG Experience & Internal Declarative UI Host
↓
G7  Long-session Context & Performance
↓
G8  Mod / Authoring & External Declarative UI Contract
↓
G9  Standalone Alpha / Release Validation
```

第一条真正产品脊柱：

```text
启动游戏
→ 进入 World Pack
→ 与 AI GM 自然语言互动
→ 世界产生 durable change
→ 退出 / 重开
→ Save
→ 继续产生未来
→ Restore / Rewind
→ 被回滚未来不泄漏
→ 玩家可以选择继续哪条时间线
```

---

# G1｜Foundation & Project Bootstrap

## 目标

证明 Godot 4.7.2 能作为独立游戏 Host，并冻结第一代最小技术边界。

## 结果

`G1-01...G1-06` 与 `G1-GATE` 已全部 PASS。

已真实证明：

- Windows-local Godot/Git 基础运行；
- 中文长文本、输入、滚动和响应；
- DeepSeek + Kimi Code 真实 HTTP streaming / cancel / post-cancel；
- UI 在网络请求期间不冻结；
- 本地 `user://` IO；
- portrait / scene / map 类 filesystem 图片加载；
- Windows export 与 exported EXE 直接运行；
- 第一代 Godot / GDScript / same-process Runtime 决策。

G1 的 Spike 实现不是后续正式 Domain / UI 设计模板。

---

# G2｜AI Conversation Spine

## 目标

建立第一个真正可玩的 AI RPG 对话主循环：

```text
打开游戏
→ 输入自然语言
→ AI GM 真 streaming Narrative
→ 连续多回合
→ cancel / retry / regenerate
→ 错误后继续
```

## 主要任务

### G2-01｜Application / Game Shell

建立正式应用入口、最小生命周期与产品壳。

当前完成状态由 `MY_WORLD_G2_CURRENT_STATUS.md` 记录。

### G2-02｜Provider Adapter v0.1

第一代只产品化一个 concrete Provider：DeepSeek `deepseek-v4-pro`。

只建立薄 `send / stream / cancel / completion / failure` seam；不建设 Provider registry、fallback mesh 或平台化路由。

### G2-03｜Narrative Conversation View + Host Slots

这是 `MY_WORLD_声明式UIHost架构_CURRENT.md` 第一次进入真实产品界面的任务。

必须完成：

- 玩家消息；
- GM Narrative；
- 真 streaming；
- cancel；
- regenerate / retry；
- 错误态；
- 长篇阅读友好的中央 Narrative；
- 玩家自然语言输入；
- **左 Player Host / 中 Narrative Host / 右 World Surface Host 的稳定布局边界**；
- 宽窗口三栏、窄窗口保持 Narrative 优先的响应式方向。

本任务只建立 Host Slots 与真实固定 UI；**不实现通用 Declarative Renderer，不创建空 World/Mod Surface，不做外部 UI schema。**

中央 Turn action 至少给后续可逆性留下明确位置；G2 当前只需要 cancel / regenerate / retry，G3 Timeline 成立后再接“回到这里 / branch”。

### G2-04｜Turn / Conversation Domain v0.1

明确：

- Player Turn；
- GM Turn；
- Conversation Entry；
- Generation State；
- Retry / Regenerate 的最小 Domain 语义；
- 哪些是 UI state，哪些是 Game/Conversation domain。

不要把聊天记录定义成 Timeline。

### G2-05｜Context Assembly v0.1

先处理最小：

- system / GM instructions；
- 当前 Conversation working set；
- 当前游戏最小 context。

不做复杂 retrieval / long-memory 平台。

### G2-06｜第一轮 Owner Playtest

真实模型连续游玩并与 The World / DSH 简单基线比较：

- Narrative 质量不能明显更差；
- 自然语言自由度不因新 Runtime 降低；
- retry / cancel 容易理解；
- 三栏骨架不抢走中央 Narrative；
- UI 操作税不能明显高于简单基线。

## G2-GATE｜Core Conversation Gate

Engineering：连续多回合、stream/cancel/retry、provider failure、UI 响应均可靠。

Product Value：

> **作为 AI RPG 对话核心，是否已经值得继续玩，而不是工程 demo。**

---

# G3｜Persistent Game & Timeline

## 目标

建立长期世界脊柱，并把 **Player owns the timeline** 从产品原则落成真实能力。

必须明确分开：

```text
Game
World State
Timeline
Save Point
Conversation
Agent Context
UI Preference
```

## 主要任务

### G3-01｜Persistence Domain Architecture

冻结语义后评估 SQLite + Event Log/Snapshot 等成熟本地方案。

### G3-02｜Durable World Mutation Path

模型可以广泛 author 世界语义；Runtime 负责把需要长期存在的结果可靠落成 stable identity、atomic durable state 和可恢复 Timeline。

不要把 Runtime 做成 Narrative 审查器，也不要回到周期性大文本 consolidation。

### G3-03｜Game Reopen / Resume

关闭程序后恢复同一 Game / Timeline / World / Conversation 必要状态。

### G3-04｜Save / Restore / Timeline Context Rebuild

Restore 必须同时恢复世界和对应 Agent Context；被回滚未来不得泄漏。

### G3-05｜Rewind / Branch Foundation

建立：

- 回到稳定节点；
- 从旧节点继续后形成新未来；
- regenerate / edit-and-retry 与 Timeline 边界对齐；
- 中央 Turn action 与右侧 Timeline Surface 未来共用同一 Domain 能力。

### G3-06｜Crash / Interrupted Write Recovery

防止半提交、物理损坏和不可恢复写入。

### G3-07｜Persistence Reality Test

真实完成：

```text
游玩
→ durable change
→ 退出 / 重开
→ save
→ 推进未来
→ restore / rewind
→ 从旧节点继续
```

## G3-GATE

- durable truth 不依赖周期性模型归并；
- Save / Restore / Rewind 正确；
- branch 语义可靠；
- future-memory isolation 成立；
- UI projection / snapshot / transcript 不成为第二 live truth。

---

# G4｜World Pack & Local Content Foundation

## 目标

让游戏不硬编码为一个世界，并建立 Source → game-local reality。

## 主要任务

### G4-01｜World Pack v0.1

只冻结当前需要的 metadata、world instructions、source lore、initial characters、authored map、portrait / scene assets、必要 mechanic declarations。

**不在 G4 冻结通用外部声明式 UI schema。** World Pack 若需要影响 UI，只保留未来 capability / metadata 需求证据，等 G6 Host 真实证明后由 G8 外部化。

### G4-02｜Source → Game-local Instance

`Reusable Source → game-local canonical reality`，旧局不被 Source 更新静默污染。

### G4-03｜Pack Discovery / Install / Load

本地发现、安装、选择和载入；不做在线商店。

### G4-04｜Asset Resolution

portrait / scene / map 通过 World Pack 解析，不写死核心工程。

### G4-05｜Second Pack Fixture

第二个小世界包证明产品不是首个世界特例。

## G4-GATE

至少两个 World Pack 能建立独立新游戏，Source/Instance 分离，旧局安全，换世界不需要改核心代码。

---

# G5｜World Semantics & GM Runtime

## 目标

让世界真正“活起来”，同时保持模型创造力。

## 主要任务

### G5-01｜World Turn / GM Orchestration

围绕：

```text
Player natural-language intent
+ relevant world context
→ Model / GM authors development
→ Runtime materializes durable consequences where needed
→ Narrative continues
```

不建立 prevention-first Narrative whitelist。

### G5-02｜Knowledge Provenance

`GM / Source / System knows X != NPC knows X`。知识边界进入 Context / Projection，但普通偶发知识错误优先通过 Context、retry、rewind 改善，而不是无限加审查层。

### G5-03｜NPC / Faction Agency

重要行动者拥有自己的目标、风险、义务和 Independent Next Move；`Off-screen != Inactive`。

### G5-04｜Player Agency / Delegation

玩家拥有最终时间线主权；具体 Full Control / Light Delegation 等模式用真实 UAT 收敛，不把普通 Narrative 代演自动视为硬失败。

### G5-05｜Meaningful Choice / Checks

风险结构、DC、advantage/disadvantage、failure stakes 与 deterministic RNG 在真正有不确定性时使用。

### G5-06｜Pacing / Time / World-Led Events

World Loop / Life Loop；压缩无意义时间；在 meaningful choice 停下；不做 universal per-NPC tick。

### G5-07｜Runtime → UI Projection v0.1

真实 Character / Relationship / Inventory / Faction / Objective 等 Domain 出现后，建立 player-safe projection 进入 G2 已存在的 Host Slots。

可以出现极小 handwritten internal contribution object，但**不冻结 external asset UI contract**。

### G5-08｜NPC / GM Owner Playtest

验证 NPC 是否会主动创造历史、人物是否有差异、失败是否推动局面、模型表达是否保持自由。

## G5-GATE

Engineering：关键世界语义可持久化并投影到 UI。

Product Value：GM 自由、人物质感、世界自主性至少不明显差于 The World / DSH 基线。

---

# G6｜RPG Experience & Internal Declarative UI Host

## 目标

把 AI RPG 核心循环变成真正的 2D 游戏，并完成第一版**内部声明式 UI Host**。

本阶段不是“页面越多越好”，而是：

```text
真实 Domain truth
→ player-safe projection
→ Host composition
→ Godot presentation
```

## 主要任务

### G6-01｜Narrative UX Polish

阅读层、输入层、Turn action、streaming、历史、状态提示、响应布局。

### G6-02｜Player Host / Character Portrait

左栏正式承载主角立绘、身份、高频状态和 Character Detail projection。

### G6-03｜Scene Art

场景图 / 插图；美术是 presentation，不是第二世界状态源。

### G6-04｜Authored Map

World Pack 手工地图、缩放/拖动、canonical current location marker；不做自动地图生成 / GIS。

### G6-05｜World Surface Host / RPG State Panels

右栏按真实需求逐步承载：Character、Relationship/Faction、Threads/Objectives、Inventory/Economy、Map、Timeline/Save、Settings 等。

具体 Tab 数量和命名通过真实 UAT 收敛，不提前冻结。

### G6-06｜Internal Declarative UI Host v0.1

用 handwritten internal definitions 完成真实 vertical：

```text
internal UI definition
→ Host assembly
→ Runtime player-safe projection
→ bounded contribution materialization
→ Godot Control rendering
```

第一版只实现真实需要的安全组件，例如：

- section；
- card；
- badge；
- meter；
- status_list；
- fact_list；
- action_list。

证明：

- Player Status / Character Detail / Core Surface 可以通过 internal definition 进入正确 Host Slot；
- 至少一个 live value 在 Runtime 变化后正确更新；
- UI 不读取任意 state path，不执行任意 GDScript；
- stable surface / contribution / source identity 成立；
- bounded Action Intent 可以请求 Application/Domain 行为而不是直接 mutation；
- fixed handwritten core UI 与 declarative contribution 可以共存。

### G6-07｜Extension Surface / Responsive / Preference

按真实需求证明：

- 至少一个 internal Extension Surface；
- 宽窗口三栏、窄窗口 Narrative 优先；
- 必要的玩家 UI preference（如 Surface order）与 Game Timeline 分离。

没有真实需求时不要提前建设完整 preference framework。

### G6-08｜Experience UAT

真人连续游玩判断：

> **它是否已经明显像一个长期 RPG，而不是换皮聊天工具或工程 Workspace。**

## G6-GATE｜RPG Experience / Internal UI Host Gate

必须同时满足：

- art / map / state UI materially improve 游戏体验；
- 中央 Narrative 仍然是主体验；
- UI 不建立第二事实源；
- internal declarative Host 有真实 vertical proof；
- 扩展性没有让界面变成工程工具；
- 玩家不承担文件 / Workspace / schema 心智。

---

# G7｜Long-session Context & Performance

## 目标

正面解决 DSH 长局逐渐不可用的问题。

## 主要任务

### G7-01｜Context Budget & Assembly Architecture

区分 authoritative truth、current relevant state、conversation history、retrieval/summaries 与 model working set。

### G7-02｜Incremental Memory / Retrieval

只给模型当前相关信息，`World Growth != Prompt Growth`。

### G7-03｜Conversation Compression / Historical Recall

压缩和 retrieval 是 working context，不是 live truth。

### G7-04｜Background Work Scheduling

后台 summary/index/world maintenance 不冻结主 UI，不要求玩家等待后台“维护世界”。

### G7-05｜Performance Instrumentation

至少测：context size、TTFT、generation throughput、stream responsiveness、persistence/load/restore、background cost、UI frame responsiveness。

### G7-06｜Long-session Evolution Test

真实几十到上百回合，验证性能、状态一致性和 GM 质量。

## G7-GATE

无 DSH 式持续恶化趋势；Context 有预算；UI 始终响应；长期恢复正确；GM 质量不因过度压缩明显下降。

---

# G8｜Mod / Authoring & External Declarative UI Contract

## 目标

把 World Pack / Mod 从开发者 fixture 提升成真实作者生态，并把 G6 已证明的 Internal UI Host **外部化**，而不是重新发明 Host 能力。

## 主要任务

### G8-01｜World Pack Author Contract

冻结作者真正需要表达的 Source / content / dependency / identity；区分 required / optional。

### G8-02｜External Declarative UI Contract

只把 G6 已经真实证明的 Host capabilities 暴露给 World Pack / Mod。

外部 Definition 可声明：

- placement / Host capability；
- safe component kind；
- Core Surface contribution；
- Extension Surface ownership；
- explicit dependency；
- bounded Action Intent；
- 必要的 recommended order。

禁止：

- arbitrary Godot Scene / GDScript；
- arbitrary NodePath / state path；
- arbitrary query / expression DSL；
- arbitrary filesystem / OS；
- direct authoritative mutation；
- 未经 G6 Host proof 的新执行能力。

正式顺序：

```text
G6 Host capability proof
→ G8 external schema
→ validator / adapter
```

### G8-03｜Pack / UI Definition Validation

提供窄而实用的 manifest、refs、version、identity、dependency、surface ownership、component support、obvious incompatibility 验证。

### G8-04｜Local Mod Manager

本地安装 / 启用 / 禁用 / 查看 World Pack 与内容 Mod。

### G8-05｜Source Upgrade Semantics

明确 Source 新版本对新游戏、旧游戏、asset fix 与 optional migration 的影响。

### G8-06｜Authoring Support

按真实作者需求增加 template、preview、map helper、asset checker、UI contribution preview 或 lightweight editor。

### G8-07｜External-style Third Pack Test

由不依赖核心开发者特殊知识的第三内容包证明：

- 新世界不改核心代码即可运行；
- 若 Pack 有特殊 RPG 状态 / Surface，可以通过受控 declaration 出现；
- Host 仍控制视觉和交互；
- 多个 Definition 的 identity / dependency / ownership 不靠加载顺序猜测。

## G8-GATE｜Modability Gate

- World Pack / Mod 可本地安装并运行；
- 外部 UI declaration 只能使用已验证 Host capabilities；
- 作者错误有可理解反馈；
- 旧局不被 Source 升级静默污染；
- 多个内容包证明不是单一世界特例。

复杂脚本沙箱继续 Deferred，除非未来真实产品价值证明必须存在。

---

# G9｜Standalone Alpha / Release Validation

## 目标

形成第一版真正可以长期玩的 standalone alpha，而不是“技术栈完成”。

## 主要任务

### G9-01｜Reference World Alpha

完整但受控的参考世界端到端运行。

### G9-02｜Onboarding / New Game / Continue

无需开发者知识即可配置 Provider、选择 Pack、新建、继续、Save/Restore。

### G9-03｜Windows Packaging / Update Baseline

稳定 Windows 发布包。

### G9-04｜Recovery / Corruption / Error UX

网络、模型、Save、Pack、局部损坏均有恢复路径；可逆优先于惩罚玩家。

### G9-05｜Regression / Migration

只为真实发布契约建立版本升级 / 存档兼容，不预造多年兼容层。

### G9-06｜Long-play Product UAT

真实 World Pack + 真实模型长局，对照：

```text
The World / DSH + same model
vs
my world standalone + same model
```

## G9-GATE

Engineering：核心循环、持久化、World Pack、Timeline、长局、Windows packaging 可靠。

Product Owner 判断：

1. Want to Continue；
2. GM Quality Preserved；
3. Long-session Advantage；
4. Native Game Value；
5. Persistence / Timeline Confidence；
6. World Pack / Mod Value；
7. Player Plays, Runtime Maintains。

---

# 4. 跨阶段不变量

## INV-01｜核心体验优先

工程完整性不能以明显损害 GM 输出质量、自然语言自由度与沉浸为代价。

## INV-02｜成熟基底优先

Godot 等成熟 Foundation 能解决的通用问题不从零重写。

## INV-03｜游戏语义自有

Godot / Provider / SQLite / UI Host / Mod Schema 都不能自动成为 World / Timeline / NPC / Save 的产品定义者。

## INV-04｜本地单人优先

Server / multiplayer / cloud 不反向复杂化第一代核心架构。

## INV-05｜Source ≠ Game Reality

World Pack 是可复用 Source；开局后的 game-local reality 是本局真相。

## INV-06｜Model authors; Runtime makes durable; Player owns timeline

模型拥有尽可能宽的世界与 Narrative authoring 空间；Runtime 负责 stable identity、durability、persistence、Timeline integrity 与不可逆系统边界；玩家通过 retry / rewind / restore / branch 决定最终保留的时间线。

## INV-07｜UI 是 Projection

UI / Declarative Definition / Host / ViewModel 可以聚合和呈现，但不能维护第二套长期世界真相。

## INV-08｜Host capability before external schema

先用真实产品和 internal definitions 证明 Host capability，再允许 World Pack / Mod 外部协议声明它；外部 schema 不得反向发明未验证的任意 UI 执行能力。

## INV-09｜Timeline ≠ Conversation

Save / Restore / Rewind / Branch 与 Conversation history 必须分离。

## INV-10｜产品 Gate 需要真人

游戏性、UI 感受、GM 质量和“是否愿意继续玩”不能只由自动测试宣布 PASS。

---

# 5. 当前阶段与 Decision Propagation

当前执行状态以：

`MY_WORLD_G2_CURRENT_STATUS.md`

为准。

本次声明式 UI Host 决策的传播结果：

- 当前 G2-02 Provider Adapter 任务保持有效，不返工；
- G2-03 scope 增加稳定三 Host Slot / 三栏响应式骨架，但不增加通用 declarative renderer；
- G4 明确不冻结外部 UI schema；
- G5 增加真实 Runtime → UI Projection 路径；
- G6 增加 Internal Declarative UI Host vertical proof；
- G8 在 G6 proof 后外部化 World Pack / Mod UI declaration contract。

---

# 6. 当前 Non-scope

在相应阶段前不建设：

- Multiplayer / Server backend / Cloud account；
- 3D 自由移动；
- automatic map generation / GIS；
- universal per-NPC tick；
- Universal ECS；
- giant generic schema / protocol；
- arbitrary Mod scripting sandbox；
- arbitrary UI GDScript / arbitrary state query；
- Steam Workshop；
- Local LLM hosting；
- TTS / STT；
- 自动生成 World Pack；
- 与当前 Gate 无关的未来基础设施。

---

# 7. Roadmap 变更规则

本文件为 rolling current。

以下情况必须重新评估并更新：

- Product Spec / 核心原则改变；
- 真实 Spike / UAT 推翻 Foundation 或产品假设；
- Gate FAIL 导致阶段重排；
- 新证据改变 Task DAG；
- supporting architecture（如 UI Host）改变下游职责；
- 阶段 PASS 并进入下一阶段。

不得因为“已经写了很多代码”拒绝重排路线；也不得因为未来可能需要就提前建设当前无真实消费者的通用平台。
