---
title: my world｜总体规划路线图
status: current-canonical-roadmap
version: 1.9
created: 2026-08-25
updated: 2026-08-26
current_phase: G2
current_status_source: MY_WORLD_CURRENT_STATUS.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜总体规划路线图 CURRENT

## 0. 文档职责

本文件只拥有：

- G1–G9 阶段顺序；
- 每阶段核心 Outcome；
- 主要 Task DAG；
- Stage Gate；
- 哪些能力必须延后。

不重复维护：

- 产品定义：`MY_WORLD_项目启动总纲_CURRENT.md`
- 跨阶段原则：`MY_WORLD_核心设计原则_CURRENT.md`
- 系统架构与专题导航：`MY_WORLD_架构_CURRENT.md`
- 当前 Task / PASS / UAT：`MY_WORLD_CURRENT_STATUS.md`

原则：

> **先跑通真实核心循环，再扩展外围能力。**

---

## 1. 总体关键路径

```text
G1  Foundation & Project Bootstrap
↓
G2  AI Conversation Spine
↓
G3  Persistent Game / Save / Timeline Foundation
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
→ AI GM 自然语言互动
→ 世界产生 durable change
→ 退出 / 重开仍是同一 Game
→ 明确 Save
→ 继续产生未来
→ 明确 Load / Restore
→ 世界 + Context 一致恢复
→ 被回滚未来不泄漏
→ 继续新的当前进度
```

任意历史 Turn 一键 Rewind **不是第一代关键路径**。

---

# G1｜Foundation & Project Bootstrap

## Outcome

证明 Godot 4.7.2 能作为独立 Host，并冻结第一代最小技术边界。

## Result

`G1-01 ... G1-06` 与 `G1-GATE`：**PASS / CLOSED**。

已证明 Windows-local Godot/Git、中文长文本/输入、真实 Provider streaming/cancel、UI 非冻结、本地 IO、filesystem images、Windows export/exported EXE，以及第一代 Godot/GDScript/same-process 技术选择。

详细历史证据已下沉到 `architecture/foundation/` 与 Git history。

---

# G2｜AI Conversation Spine

## Outcome

建立第一个真正值得体验的 AI RPG Narrative 主循环：

```text
打开游戏
→ 输入自然语言
→ AI GM 真 streaming Narrative
→ 连续多回合
→ Cancel / Regenerate / Retry
→ Provider failure 后继续
```

## Tasks

### G2-01｜Application / Game Shell

正式应用入口与生命周期。状态见 `MY_WORLD_CURRENT_STATUS.md`。

### G2-02｜Provider Adapter v0.1

DeepSeek `deepseek-v4-pro` 的薄 `stream / cancel / completion / failure` seam；不做 Provider platform。

### G2-03｜Narrative Conversation View + Host Slots

第一份真正改变玩家主路径的 UI：

- Player natural-language input；
- GM Narrative；
- real streaming；
- cancel；
- regenerate/retry latest generation；
- error recovery；
- 长篇阅读友好的中央 Narrative；
- `Player Host | Narrative Host | World Surface Host` 稳定布局；
- 宽窗口三栏，窄窗口 Narrative 优先。

当前只做 fixed Godot UI + Host Slots；不做通用 Declarative Renderer、Save/Timeline、World/NPC。

### G2-04｜Turn / Conversation Domain v0.1

冻结：Player Turn、GM Turn、Conversation Entry、Generation State，以及 Retry / Regenerate / latest-turn correction 的最小正式语义。

Transcript 不等于 Timeline。

### G2-05｜Context Assembly v0.1

先实现 system/GM instructions + 当前 Conversation working set + 当前最小 game context；不做复杂 retrieval/long-memory platform。

### G2-06｜第一轮 Owner Playtest

连续真实游玩，与 The World / DSH 简单基线比较 Narrative、自由度、交互成本与“是否想继续玩”。

## G2-GATE

Engineering：多回合、stream/cancel/retry、failure recovery、UI response 可靠。

Product Value：

> **作为 AI RPG 对话核心，是否已经值得继续玩，而不是工程 demo。**

---

# G3｜Persistent Game / Save / Timeline Foundation

## Outcome

建立长期世界的 durable backbone，让 Save / Load / Restore 成为可靠原生能力，同时保证未来记忆隔离。

必须区分：

```text
Game
World State
Timeline
Save Point
Conversation
Agent Context
UI Preference
```

核心产品边界：

> **Save Point != Timeline Node.**
>
> **Reversibility != frictionless arbitrary rewind.**

## Tasks

### G3-01｜Persistence Domain Architecture

用真实 fixture 评估 SQLite + durable mutation + checkpoint/snapshot 等最小组合，冻结 authoritative ownership、事务与 migration/recovery 边界。

### G3-02｜Durable World Mutation Path

模型可以广泛 author 世界；Runtime 把需要长期存在的内容可靠落成 stable identity、atomic durable state。

### G3-03｜Game Reopen / Resume

退出再开，恢复同一 Game、current World 与必要 Conversation/Context。

### G3-04｜Explicit Save / Load / Restore + Context Rebuild

- 玩家明确创建 Save Point；
- 明确 Load/Restore；
- World 与 Context 一致恢复；
- future-memory isolation；
- UI/Transcript 不成为第二 truth。

### G3-05｜Recovery / Timeline Foundation

优先解决：

- Load 旧 Save 时保留可恢复的旧 current future；
- recovery checkpoint / old-head / internal branch 等最小可靠语义；
- Retry/Regenerate/latest-turn correction 与 persistence boundary 对齐；
- 玩家理解当前 active progress。

**arbitrary per-turn rewind = Deferred**，除非后续长局 UAT 证明必须。

### G3-06｜Crash / Interrupted Write Recovery

防止半提交、物理损坏、不可恢复写入。

### G3-07｜Persistence Reality Test

真实完成：

```text
游玩
→ durable change
→ 退出 / 重开
→ Save
→ 推进未来
→ Load / Restore
→ Context 无未来泄漏
→ 必要时恢复误读档前的 current future
```

## G3-GATE

可靠 persistence、resume、Save/Load/Restore、future-memory isolation、recovery 成立；不要求任意 Turn 回档 UI。

---

# G4｜World Pack & Local Content Foundation

## Outcome

让产品不硬编码为一个世界，正式建立 `Reusable Source → Game-local Reality`。

## Tasks

### G4-01｜World Pack v0.1

只冻结当前真实需要的 metadata、world instructions、source lore、initial characters、authored map、portrait/scene assets、必要 mechanic declarations。

不冻结外部声明式 UI schema。

### G4-02｜Source → Game-local Instance

新局从 Source 建立自己的本局现实；Source 更新不得静默改写旧局。

### G4-03｜Pack Discovery / Install / Load

本地发现、安装、选择、载入；不做在线商店。

### G4-04｜Asset Resolution

portrait / scene / map 通过 World Pack 解析，不写死核心工程。

### G4-05｜Second Pack Fixture

第二个小世界证明产品不是首个世界特例。

## G4-GATE

至少两个 World Pack 能建立独立新游戏，Source/Instance 分离且旧局安全。

---

# G5｜World Semantics & GM Runtime

## Outcome

让世界真正“活起来”，同时保持模型创造力和有限工程复杂度。

## Tasks

### G5-01｜World Turn / GM Orchestration

```text
Player natural-language intent
+ relevant world context
→ Model / GM authors development
→ Runtime materializes durable consequences where needed
→ Narrative continues
```

### G5-02｜Knowledge Provenance

`World Truth != NPC Knowledge != Player Knowledge` 进入 Context/Projection，但不演化成无限 Narrative censor。

### G5-03｜NPC / Faction Agency

重要行动者拥有独立目标、风险、义务与 next move；`Off-screen != Inactive`。

### G5-04｜Event / Priority-driven World Evolution

按时间、重大事件、高影响 Actor/Faction、Agenda/Front 和因果关联选择需要推进的世界部分；不做每 NPC 每 tick 模型调用。

### G5-05｜Meaningful Choice / Mechanics Integration

骰子处理真正不确定性；身份、已知事实、人格底线与明显不可能性不能被万能 d20 抹平。

### G5-06｜Runtime → UI Projection

Character / Relationship / Item / Faction / World 等真实数据进入 Host Slots，UI 只读 player-safe projection。

### G5-07｜World Product Tests

至少包括 Player Absence、Counterfactual Propagation、Independent Actor 等测试，防止 **Protagonist Causal Monopoly**。

## G5-GATE

玩家不再是唯一因果源；世界有选择性自主演化且没有发展成全宇宙模拟器。

---

# G6｜RPG Experience & Internal Declarative UI Host

## Outcome

把已成立的 Runtime Truth 做成真正的 RPG 产品界面，并证明内部声明式 Host 能力。

## Tasks

- 三栏 RPG Experience 完整化；
- Player portrait/status/detail；
- Character / Relationship / Inventory / Faction / Map / Save 等真实 Surface；
- scene/portrait/map presentation；
- Internal Declarative UI Host v0.1；
- 小型 safe vocabulary：section/card/badge/meter/status_list/fact_list/action_list 等，仅按真实需求增长；
- Runtime projection → ViewModel → Host → Godot Control；
- bounded Action Intent；
- responsive / Theme / navigation；
- Owner UAT 与视觉 polish。

## G6-GATE

UI 明显增强游戏理解与沉浸；声明式能力改善可扩展性但没有让产品变成工程工具。

---

# G7｜Long-session Context & Performance

## Outcome

长局持续增长时，模型 working set、UI responsiveness 和 background work 仍可控。

## Tasks

- bounded Context Assembly；
- relevant subgraph / working set selection；
- deterministic background progression 与 model work 分离；
- TTFT / throughput / context size / persistence latency 的真实 long-play evidence；
- long-session recovery/performance test。

## G7-GATE

`Game State / History ↑↑↑` 时 ordinary Turn Context 不线性爆炸，游戏仍可玩。

---

# G8｜Mod / Authoring & External Declarative UI Contract

## Outcome

把 G4/G6 已证明的内容与 Host capability 外部化给 World Pack / Mod 作者。

顺序：

```text
proven internal capability
→ external schema / naming
→ validator / adapter
→ authoring helper / preview
```

## Tasks

- World Pack / Character / Expansion authoring contracts；
- external UI contribution schema；
- Surface ownership/dependency；
- safe component declarations；
- validation/migration；
- creator/authoring UX；
- second/third real pack proof。

复杂任意代码沙箱仍默认 Deferred。

## G8-GATE

Mod 能扩展内容和受控 UI，而不会获得任意 Runtime/OS 权限或制造第二事实源。

---

# G9｜Standalone Alpha / Release Validation

## Outcome

证明第一代可以作为独立长期 AI RPG 使用，而不是只通过单次演示。

覆盖：

- Windows build / startup / upgrade；
- long-play stability；
- Save/Load/Recovery；
- Context/performance；
- World Pack install/use；
- Provider failure；
- UI usability；
- Product Value UAT；
- 与 The World / DSH simple baseline 的真实比较。

## G9-GATE

> **玩家愿意把它当一个独立 AI RPG 长期玩，而不是一个技术样品。**

---

## 文档 / 任务纪律

- 当前短周期状态只更新 `MY_WORLD_CURRENT_STATUS.md`；
- 架构结论只更新 `MY_WORLD_架构_CURRENT.md`，深度从其导航；
- 默认不因新 Stage 新建新的 status 文档；
- 正式 implementation/review 仍使用 repository-native Task Packet；
- 小 bug / polish 不必强行升级成完整架构事件；
- Product-facing Task 的 Engineering Acceptance 不替代 Owner Product UAT。
