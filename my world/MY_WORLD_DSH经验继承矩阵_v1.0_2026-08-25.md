---
title: my world｜DSH 经验继承矩阵
status: current-reference
version: 1.0
created: 2026-08-25
updated: 2026-08-25
source_project: The World on DeepSeek Harness
---

# my world｜DSH 经验继承矩阵 v1.0

## 1. 文档目的

这份文档只回答一个问题：

> **The World / DSH 里哪些东西值得带进 `my world`，哪些应该重新设计，哪些必须留在旧项目里？**

正式原则：

> **迁移经验，不迁移宿主债务。**

分类：

```text
KEEP
经过真实试玩验证，应继承为产品 / 领域原则

RETHINK
目标与语义正确，但独立版应重新设计实现

DROP
纯 DSH workaround、临时 Host seam 或已确认不值得迁移的实现
```

---

## 2. KEEP｜必须继承

### K-01｜优秀模型自由度优先

The World 的真实成功首先来自：优秀模型能自由主持、创造人物、组织戏剧并自然推进世界。

独立版不能为了“更像游戏引擎”把 GM 降级为只能执行封闭状态机的剧情播放器。

> **Freedom Before Prevention.**

### K-02｜Recovery First

低成本错误优先恢复、重生成、回档，不为理论风险给所有行为加重型前置审批。

> **Prefer recovery over prevention.**

### K-03｜玩家拥有尝试权

```text
Player owns Attempt
World owns Consequence
GM owns Playability of the Consequence
```

玩家可以尝试任何行动，不代表世界必须让其成功。

### K-04｜Game-local Reality > Source

World Pack / Source 决定开局参考世界。

游戏开始后的正式历史由本局产生：

> **game-local reality > source default trajectory**

不能为了“贴回原著 / 历史 / 世界包”静默修正已经发生的分歧。

### K-05｜Knowledge Provenance

> **GM / Source / System knows X != NPC knows X.**

NPC 的知识必须能从世界内来源解释。

### K-06｜NPC Agency

重要 NPC 不是等待玩家触发的功能按钮，而是拥有：

- Current Agenda；
- Fear / Cost；
- Red Line；
- Obligation；
- Independent Next Move。

NPC 可以不同意玩家、拖延、拒绝、自行行动、与第三方建立关系。

### K-07｜Meaningful Choice Risk Structure

有意义的选择不仅叙事方向不同，也应在风险结构上不同：

- 可行性；
- 难度；
- 优势 / 劣势；
- 失败代价；
- 暴露、资源、关系与长期后果。

> **Dice decides uncertainty. Dice does not erase character.**

### K-08｜失败改变局面，而不是关闭游戏

失败应带来：

- 新信息；
- 新代价；
- 新敌意；
- 新机会；
- 时间损失；
- 暴露；
- 关系改变；
- 路线转折。

而不是“失败 → 原地再掷一次”。

### K-09｜Pacing Elasticity

```text
World Loop
局势 → 事件 → 后果 → 时间推进

Life Loop
自由活动 → 日常 → 人物互动 → 关系 / 人格积累
```

> **Compress dead time; stop at meaningful choice.**

### K-10｜Player Agency = Authorization Boundary

可提供不同主角操控粒度，但 GM 不得把玩家的宽泛意图扩展成未授权的重大承诺、阵营、路线或不可逆行为。

### K-11｜Persistent World，不等于全世界逐 tick 模拟

> **重要性决定注意力与模拟资源，不决定实体是否存在。**

已经形成 durable identity 的人物、地点、组织、承诺与冲突不能因为暂时不重要而消失。

### K-12｜UI 是世界真相的投影

> **UI is a projection of game truth, not a second truth source.**

玩家 UI 按玩家问题组织，不按底层存储布局组织。

### K-13｜Narrative First

玩家首先消费叙事和游戏状态，不应被后台维护日志、工具调用与工程工作噪音占据主阅读流。

### K-14｜Save Point != Persistent State

世界持续状态与显式恢复点是两个概念。

独立版必须继续区分：

- 当前世界状态；
- Timeline；
- Save Point；
- Conversation / Agent Context。

### K-15｜World Pack / Mod

可复用世界内容是产品核心能力之一，不是后期补充。

World Pack 应允许不同世界拥有不同：

- Source；
- 人物；
- 地图；
- 立绘；
- 场景；
- 规则与机制组合。

---

## 3. RETHINK｜语义保留，实现重做

### R-01｜Persistence

#### DSH 方案

Markdown Workspace + Owner 文件 + DELTAS + checkpoint consolidation。

#### 保留价值

- 世界事实必须 durable；
- 要有明确 ownership；
- 长局恢复必须稳定。

#### 独立版重做目标

一次 durable mutation 应即时更新 authoritative state，并让 UI / Agent Context 立即读取同一真相。

不再接受“等若干回合 consolidation 才收敛”为正常 Runtime 机制。

具体数据库 / 文件格式暂不冻结。

### R-02｜Agent Context

#### DSH 方案

从 CURRENT / RECENT / DELTAS / COMPOSITION 等文件恢复和动态注入。

#### 独立版重做目标

Agent Context 成为 Runtime 的一等派生物：

```text
World Truth
+
Player-visible / GM-relevant context
+
Conversation context
↓
Context Assembly
```

不能等同于“把所有状态文件塞给模型”。

### R-03｜Timeline / Restore

#### DSH 方案

文件 snapshot + fresh DSH Session。

#### 独立版重做目标

Restore 是原生游戏生命周期动作：

```text
restore timeline point
↓
restore world state
↓
rebuild matching agent context
↓
continue on new future branch
```

### R-04｜Game Session

DSH Agent Session 不等于 RPG Game / Timeline。

独立版必须让 Game、Timeline、Conversation、Agent Context 有独立身份。

### R-05｜Background Maintenance

#### DSH 方案

模型在回合后读写 Markdown，周期性 consolidation。

#### 独立版目标

- 确定性状态变化由 Runtime 可靠提交；
- 模型继续负责语义判断与高价值内容；
- 后台任务不冻结 UI；
- 不要求玩家看到后台维护过程。

### R-06｜Mechanics

DSH 阶段大量机制以自然语言 Expansion Pack 运行，这是重要成功经验。

独立版不要立刻把全部机制改写成代码。

原则：

```text
语义与创造性强的部分
→ 优先模型 / 内容规则

随机、存档、资源结算、关键不变量等窄可靠边界
→ 确定性 Runtime
```

具体哪个机制值得代码化，继续由真实游玩推动。

### R-07｜Map

DSH 计划中的 authored map 方向保留：

- World Pack 作者提供地图；
- 游戏负责展示与当前位置投影。

独立版拥有更强 2D Host 后，再根据实际游玩决定地图层级、地点系统、路线与自动生成。

### R-08｜RPG UI

The World Panel 已验证 RPG UI 有明显玩家价值。

独立版不复制 Panel 的文件解析实现，只继承玩家信息架构：

- 角色；
- 人物关系；
- 地图；
- 事务；
- 机制；
- Save；
- 势力；
- 物品等。

### R-09｜Provider

DSH 已提供通用 Provider Runtime。

独立版先做极薄 Provider Adapter，不重建完整 Agent Harness。

第一阶段优先：

```text
send
stream
cancel
```

### R-10｜角色 / 世界内容资产

The World 已积累世界包、人物卡、机制包的内容创作经验。

独立版可以继承创作原则与语义，但应重新评估：

- 文件组织；
- manifest；
- 安装 / 加载；
- asset references；
- Mod 兼容；
- 编辑器 / 作者体验。

不要把 DSH Markdown 路径当作新格式标准。

---

## 4. DROP｜不得照搬

### D-01｜周期性模型 consolidation 作为主状态一致性机制

原因：长局已经明确出现 edit 越来越慢、Owner 更新滞后。

这是当前 DSH Host 的可接受债务，不是新项目设计目标。

### D-02｜Markdown 作为运行时数据库

Markdown 可以继续作为：

- Source 内容；
- 作者文档；
- 可读导出；
- Debug / Inspect。

但不预设它继续承担整个独立 Runtime 的权威状态数据库职责。

### D-03｜WeakMap / DSH turn lifecycle workaround

所有围绕 DSH `agent/turn-stopping`、Session 生命周期建立的计数、pending 状态和维护时序，不迁移。

### D-04｜fresh DSH Session Restore workaround

独立版直接拥有 Timeline / Agent Context，不能再靠创建通用 Agent Session 消除未来历史。

### D-05｜DSH fs.watch / Windows Restore 兼容补丁

这些是旧 Host 的具体文件句柄问题，不属于游戏语义。

### D-06｜Plugin UI seam

The World 为了把 RPG UI 嵌进 DSH 使用的 client plugin / panel seam 不迁移。

独立版 Godot UI 本身就是正式游戏界面。

### D-07｜通用 Agent Workspace IA

独立版不要求玩家产品围绕工程 Workspace 组织。

### D-08｜为了 DSH 限制而接受的 Restore 延迟

独立版将 Save / Restore 作为原生生命周期能力，应重新建立性能目标。

### D-09｜把所有模型工作过程暴露在主游戏流

think / read / write / tool 的工程 trace 不属于玩家主阅读体验。

### D-10｜从零重造已有成熟游戏基础设施

独立项目不是“所有底层都自己写”。

Rendering、2D、UI、Input、Audio、Font、Animation、Asset Pipeline、Packaging 等优先由成熟 Engine / Foundation 提供。

---

## 5. 新项目新增原则

这些不是 DSH 直接验证出的旧规则，而是从 DSH 宿主经验抽象出的独立项目工程原则。

### N-01｜Commodity Foundation, Owned Game Semantics

> **通用基底尽量复用，游戏核心语义必须掌握在自己手里。**

### N-02｜Engine-native, not engine-semantic-coupled

使用 Godot 不代表：

```text
World = SceneTree
NPC = Node
Save = Resource dump
Timeline = Scene history
```

引擎负责通用游戏能力，`my world` Runtime 负责领域语义。

### N-03｜Foundation Selection Gate

正式大规模实现前必须用真实 spike 验证 Host，而不是按喜好决定技术栈。

### N-04｜Local-first / Single-player-first

第一代架构不背服务器与多人同步复杂度。

### N-05｜World Pack / Mod First-class

第一 Vertical 就需要一个最小 World Pack，而不是产品做完以后才加 Mod。

---

## 6. Migration Checklist

未来从 `the-world` 借鉴任何设计 / 代码前，先问：

```text
1. 这是产品语义，还是 DSH workaround？
2. 它有真实试玩证据吗？
3. 独立 Host 还存在同一个问题吗？
4. 成熟 Foundation 是否已经解决？
5. 如果重做，能否更简单地表达同一产品价值？
6. 直接复制会不会把 DSH 的状态 / Session / Workspace 假设带进新项目？
```

只有通过这六个问题后，才决定复用、改写或丢弃。
