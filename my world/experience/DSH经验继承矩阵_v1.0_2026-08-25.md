---
title: my world｜DSH 经验继承矩阵
status: active-experience-reference
version: 1.0
created: 2026-08-25
updated: 2026-08-26
source_project: The World on DeepSeek Harness
canonical_map: ../MY_WORLD_架构_CURRENT.md
---

# DSH 经验继承矩阵 v1.0

## 0. 定位

本文件是历史产品 / 工程经验参考，不与顶层 Product / Core Design / Architecture CURRENT 并列为默认 Authority。

它回答：

> **The World / DSH 哪些经验值得继承，哪些应重新设计，哪些必须留在旧 Host。**

如果本文件与当前 `MY_WORLD_核心设计原则_CURRENT.md`、`MY_WORLD_架构_CURRENT.md` 或其专题设计冲突，以 current 核心文档为准。

正式总原则：

> **迁移经验，不迁移宿主债务。**

分类：`KEEP / RETHINK / DROP`。

---

## 1. KEEP｜长期值得继承

### K-01｜模型自由优先

The World 的成功首先来自优秀模型能自由主持、创造人物、组织戏剧并自然推进世界。独立版不能把 GM 降级成封闭状态机播放器。

### K-02｜Recovery First

对可逆错误优先提供低成本恢复，不为理论风险给所有行为加重型审批。

当前进一步收敛为：**Reversibility 不等于任意历史节点零摩擦回退；局部 Retry 低摩擦，重大 Save/Load 明确表达意图。**

### K-03｜开放尝试

```text
Player owns Attempt
World owns Consequence
GM owns Playability of the Consequence
```

玩家可以尝试任何行动，不代表世界必须让它成功。

### K-04｜Game-local Reality > Source

World Pack / Source 定义开局参考；游戏开始后的历史由本局产生，不能为贴回原著/历史静默修正分歧。

### K-05｜Knowledge Provenance

```text
GM / Source / System knows X != NPC knows X
```

### K-06｜NPC / Faction Agency

重要行动者拥有自己的 Agenda、Fear/Cost、Red Line、Obligation、Independent Next Move，可以拒绝、拖延、自行行动并形成第三方关系。

### K-07｜Meaningful Choice Risk Structure

不同选择应在可行性、难度、优势/劣势、失败代价、暴露、资源与长期后果上真正不同。

> **Dice decides uncertainty. Dice does not erase character.**

### K-08｜Failure Forward

失败应改变局面并产生新信息、代价、敌意、机会、暴露或路线，而不是“失败后原地再掷”。

### K-09｜Pacing Elasticity

```text
World Loop = 局势 → 事件 → 后果 → 时间推进
Life Loop  = 自由活动 → 日常 → 人物互动 → 关系 / 人格积累
```

> **Compress dead time; stop at meaningful choice.**

### K-10｜Player Agency

重大承诺、阵营和不可逆行为不能由 GM 从宽泛意图随意扩张；但当前核心原则也明确：低风险 Narrative 插值不需要 prevention-first 硬 Gate，玩家可通过 Retry 等纠正。

### K-11｜Persistent World != every-NPC tick

已经形成 durable identity 的人物、地点、组织、承诺与冲突不能因暂时离屏而消失；模拟资源按重要性与因果相关性分配。

### K-12｜UI 是 Projection

> **UI is a projection of game truth, not a second truth source.**

### K-13｜Narrative First

玩家主阅读流优先叙事、角色、场景与游戏状态，不暴露后台 Agent 工程噪音。

### K-14｜Save Point != Persistent State

必须区分 current world state、Timeline、Save Point、Conversation、Agent Context。当前又进一步明确：**Save Point != Timeline Node**。

### K-15｜World Pack / Mod First-class

可复用世界内容是核心能力，不是产品完成后的补丁。

---

## 2. RETHINK｜语义保留，实现重做

### R-01｜Persistence

DSH 的 Markdown Workspace + DELTAS + periodic consolidation 不迁移。

独立版目标：durable mutation 即时进入 authoritative state，UI/Context 立即读同一真相。

### R-02｜Agent Context

从文件恢复工作区改为 Runtime 一等派生物：

```text
World Truth
+ player/GM relevant context
+ Conversation context
→ Context Assembly
```

不能等于“把所有状态文件塞给模型”。

### R-03｜Timeline / Restore

DSH 的 file snapshot + fresh Agent Session 改成原生 Game 生命周期：恢复 world、恢复/重建匹配 Context、隔离未来。

当前产品优先 explicit Save/Load/Recovery；arbitrary per-turn rewind Deferred。

### R-04｜Game / Session Identity

通用 Agent Session 不等于 RPG Game / Timeline。Game、Timeline、Conversation、Agent Context 必须有独立身份。

### R-05｜Background Maintenance

确定性状态变化由 Runtime 可靠提交；模型处理开放语义与高价值内容；后台工作不冻结 UI，也不进入玩家主 Narrative。

### R-06｜Mechanics

不要把所有自然语言机制一次代码化：

```text
语义/创造性强 → 模型 / 内容规则
随机/存档/资源结算/硬不变量 → 窄确定性 Runtime
```

### R-07｜Map

保留 authored map 方向；具体层级、路线和生成能力由独立 Godot 产品真实 UAT 决定。

### R-08｜RPG UI

继承玩家信息架构价值，不迁移 DSH Panel/file parser 实现。当前已经收敛为 Player Host / Narrative Host / World Surface Host。

### R-09｜Provider

只继承真实 Provider stream/cancel 经验；独立版用薄 Adapter，不重造 Harness。

### R-10｜内容资产

继承 World Pack、Character、Mechanics 的创作经验，但重新设计 manifest、安装、asset refs、Mod compatibility 与 authoring UX；不把 Markdown 路径当新格式标准。

---

## 3. DROP｜不得照搬

- D-01：周期性模型 consolidation 作为主状态一致性机制；
- D-02：Markdown 作为 authoritative runtime database；
- D-03：DSH turn/session lifecycle workaround；
- D-04：fresh DSH Session Restore workaround；
- D-05：DSH `fs.watch` / Windows restore compatibility patch；
- D-06：为了嵌入 DSH 的 plugin UI seam；
- D-07：通用 Agent Workspace IA 作为玩家产品结构；
- D-08：因 DSH Host 限制而接受的 Restore 延迟；
- D-09：把模型 think/read/write/tool trace 暴露在主游戏流；
- D-10：从零重造 Rendering / 2D / UI / Input / Audio / Font / Animation / Packaging 等成熟游戏基础设施。

---

## 4. 独立版新增工程原则

### N-01｜Commodity Foundation, Owned Game Semantics

通用基底尽量复用，核心游戏语义自己拥有。

### N-02｜Engine-native, not engine-semantic-coupled

```text
World != SceneTree
NPC != Node
Save != Resource dump
Timeline != Scene history
```

### N-03｜Foundation Selection Gate

技术栈靠真实 Spike 证据，而不是偏好冻结。

### N-04｜Local-first / Single-player-first

第一代不背服务器、多人同步与账户系统复杂度。

### N-05｜World Pack / Mod First-class

真实 Vertical 中尽早出现最小 World Pack，但外部 UI/Mod 协议必须晚于真实 Host capability。

---

## 5. 继承检查

未来从 SillyTavern / The World / DSH 借鉴任何东西前，先问：

```text
1. 这是产品语义还是宿主 workaround？
2. 有真实试玩 / 工程证据吗？
3. 独立 Host 还存在同一个问题吗？
4. Godot / 成熟 Foundation 是否已解决通用部分？
5. 能否更简单地表达同一产品价值？
6. 直接复制是否会带入 Session / Workspace / Markdown Runtime 等旧假设？
```

通过后再决定继承、重写或丢弃。
