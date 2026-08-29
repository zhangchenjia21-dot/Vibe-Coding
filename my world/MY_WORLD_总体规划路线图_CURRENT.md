---
title: my world｜总体规划路线图
status: current-canonical-roadmap
version: 3.1
created: 2026-08-25
updated: 2026-08-29
current_phase: G4
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
- 哪些能力必须延后；
- 每个阶段为什么按这个顺序做。

不重复维护：

- 产品定义：`MY_WORLD_项目启动总纲_CURRENT.md`
- 跨阶段原则：`MY_WORLD_核心设计原则_CURRENT.md`
- 系统架构：`MY_WORLD_架构_CURRENT.md`
- 当前 Task / PASS / UAT：`MY_WORLD_CURRENT_STATUS.md`

总原则：

> **先跑通真实核心循环，再扩展外围能力。**
>
> **先建立玩家产品入口，再建立内容选择。**
>
> **先证明 World + Character 能创建并开始一局，再加入 Expansion。**
>
> **真实需求 → 最小能力 → 第一消费者 → Owner UAT → 第二消费者 → 再抽象协议。**

---

## 1. 总体关键路径

```text
G1  Foundation & Project Bootstrap
↓
G2  AI Conversation Spine
↓
G3  Persistent Game / Save / Timeline Foundation
↓
G4  Primary Source Assets & Local Game Creation
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
启动应用
→ Main Menu
→ Continue / Asset-only New Game
→ 进入 AI GM 自然语言互动
→ 世界产生 durable change
→ 退出 / 重开仍是同一 Game
→ 明确 Save
→ 继续产生未来
→ 明确 Load / Restore
→ 世界 + Context 一致恢复
→ 被回滚未来不泄漏
→ 继续新的当前进度
```

任意历史 Turn 一键 Rewind 不是第一代关键路径。

---

# G1｜Foundation & Project Bootstrap

## Outcome

证明 Godot 4.7.2 能作为独立 Host，并冻结第一代最小技术边界。

## Result

`G1-01 ... G1-06` 与 `G1-GATE`：**PASS / CLOSED**。

已证明 Windows-local Godot/Git、中文长文本/输入、真实 Provider streaming/cancel、UI 非冻结、本地 IO、filesystem images、Windows export/exported EXE，以及第一代 Godot/GDScript/same-process 技术选择。

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

## Result

G2 与 G2-GATE：**PASS / CLOSED**。

核心能力包括 Provider Adapter、Narrative Conversation View、Turn/Conversation Domain、Context Assembly v0.1，以及 Owner 产品试玩。

---

# G3｜Persistent Game / Save / Timeline Foundation

## Outcome

建立长期世界的 durable backbone，让 Save / Load / Restore 成为可靠原生能力，同时保证未来记忆隔离。

必须长期区分：

```text
Game
World State
Timeline
Save Point
Conversation
Agent Context
UI Preference
```

## Result

G3 与 G3-GATE：**PASS / CLOSED**。

已证明 SQLite authoritative persistence、atomic durable mutation、accepted Conversation durability、reopen/resume、named Save、atomic Load/Restore、future-memory isolation、Recovery Checkpoint、single-writer、verified physical backup、corruption recovery 与 real Provider continuation。

---

# G4｜Primary Source Assets & Local Game Creation

## Outcome

让产品从“只有一个自动打开的 Game”升级为真正的**本地多世界 AI RPG Host**，并冻结第一代唯一 New Game 路径：

```text
Managed Source Library
→ World Pack + Character Card + optional Expansion Pack
→ explicit Game Creation Composition
→ atomic Final Create
→ independent Game-local Reality
→ real AI GM play
```

第一代不支持无 World / 无 Character / 空白 AI 世界直接建局。

### G4 核心对象

```text
Primary Source Assets
├─ World Pack
├─ Character Card
└─ Expansion Pack

Managed Source Library
!= Game Library

Source Package Total Content
!= Selected T0 Source Projection
!= Game-local Reality
!= Runtime State
```

### 第一代 Character Card 用途

```text
Exactly 1 Player Character Card
0..N Guaranteed NPC Character Cards
```

Guaranteed NPC = 从 Final Create 起属于本局 canonical cast；不等于 opening appearance / player-known / same scene / relationship / automatic Context inclusion。

### 第一代 Expansion 数量

产品定义仍是 `0..N`。G4-05 当前 honest implementation 允许 none，真实 Expansion contract/runtime 延后到 G4-08。

---

## G4-01｜Application Shell / Main Menu + Game Session Lifecycle

### Outcome

正式拆开：

```text
Application Lifetime
!= Game Session Lifetime
```

### Result

**PASS / CLOSED**。

启动进入 Main Menu；Session 显式 open/close；返回 Main Menu 会释放 Game-owned session resources，而 Application 继续 READY。

---

## G4-02｜World Pack + Character Card Source Contracts v0.1

### Historical result

工程实现：**HISTORICAL PASS**。

v0.1 正确证明了 loader / validator / safe path / exact content fingerprint 等机制，但后来真实资产证明其语义合同过薄。

v0.1 不再是 current Source semantic authority。

---

## G4-02R1｜World / Character Source Semantic Re-audit

### Why inserted

G4-05 真实资产证据显示：`汉末三国` / `诸界余辉` 的 World 与 Character 被压缩成 compact summary，工程 PASS 没有证明真实 authored richness 被保留。

Owner 因此冻结工作分工：

> **GPT owns Meaning; Codex owns Mechanism.**

不 destructive rollback。保留 G4-03/G4-04 和 G4-05 已验证工程，向前修正 Source semantics。

### Semantic/full-fidelity result

> **PASS / FROZEN FOR MECHANISM**

Current Source contract = **v0.2-r2**：

```text
thin identity / catalog metadata
+ ordered rich semantic_sections
+ disclosure = gm_reference | gm_private
+ package-local UTF-8 Markdown/TXT
+ World Entry-scoped rich sections
+ Character T0-profile-scoped rich sections
+ exact generation fingerprint over all declared bytes
```

正式规则：

> **Do not show the model a post-T0 answer and then ask it to forget that answer.**

```text
World projection
= top-level always-safe sections
+ exact selected Entry sections

Character projection
= top-level always-safe sections
+ exact matching T0 profile sections
```

禁止 latest/nearest/later/complete-life fallback。

同时冻结：

> **Source schema is not the possibility ceiling of the Living World. Game-local semantic structure is evolvable.**

真实压力已经覆盖 2 World + 6 Character：

```text
汉末三国：天下未定
- 刘备
- 曹操
- 孙权

诸界余辉：埃瑟维亚
- 莉维娅·塞兰
- 阿德里安·维尔克
- 杜恩·石痕
```

Cross-family package shape 已停止变化。

### Remaining

G4-02R1 整体仍未关闭；必须先完成 G4-02R1M1 mechanism + GPT Independent Review。

---

## G4-02R1M1｜Source v0.2-r2 Mechanism Correction

### Current task

> **CURRENT — CODEX**

Formal packet：

`my-world/docs/tasks/G4-02R1M1_SOURCE_V0_2_R2_MECHANISM_CORRECTION_TASK.md`

### Outcome

```text
rich immutable Source package
→ deterministic validation / exact fingerprint
→ exact selected World Entry / Character T0 profile projection
→ explicit temporal compatibility state
→ no fallback / no hidden same-family restriction
```

必须使用冻结的 2 World + 6 Character full-fidelity fixtures 原样验收。

### Scope

Codex 只拥有 Mechanism：

- validator / loader / fingerprint；
- exact selected Source projection；
- temporal compatibility-state mechanism；
- localized G4-03/G4-05 repair only when real regression requires it；
- automated/failure-injection/Godot-Windows evidence。

禁止 Codex：

- 改写/压缩 frozen semantic fixtures；
- 重做 Source schema；
- 添加 same-family restriction；
- 添加 profile fallback；
- 提前做 G4-06；
- 恢复 G4-05R1。

Return ceiling：**READY FOR INDEPENDENT REVIEW**。

### Exit

Codex 返回后由 GPT Independent Review：

- assertion 是否真的覆盖 selected projection；
- full-fidelity fixture 是否真实进入 production seam；
- unselected/later/private bytes 是否仍影响 fingerprint 但不泄漏 projection；
- temporal incompatible 是否 fail as designed；
- zero-coverage cross-world route 是否未被伪造 family rule hard-block；
- G4-03 / preserved G4-05 regression 是否真实成立。

只有 IR PASS 后才恢复 G4-05 closure。

---

## G4-03｜Managed Local Source Library v0.1

### Result

**PASS / CLOSED**。

已证明：staged verified publish、append-only immutable generation、explicit current generation、restart truth、exact fingerprint identity、missing/tamper fail-loud。

Source v0.2-r2 如暴露具体 regression，只做 localized repair forward，不重开 Library 产品设计。

---

## G4-04｜Multi-Game Lifecycle / Game Library Foundation

### Result

**PASS / CLOSED**。

已正式裁定并证明：

> **One Game = One SQLite.**

Managed Game path：

`user://my-world/games/<game_id>/game.sqlite`

Application index metadata != gameplay truth。Existing-only open、identity cross-check、A-close-before-B-open、independent Game DB、metadata recovery、legacy G3 adoption 已通过。

---

## G4-05｜Asset-only New Game Wizard v0.1

### State

**REWORK / HOLD**。

Implementation candidate `145c3e1192b443f6284da7f36aee74619adad5bf` 保留为 provisional accepted Wizard/Composition engineering evidence。

保留的已验证 seam：

- chooser visibility/focus != selection；
- explicit click pins exact generation；
- World change clears dependent Entry；
- Player eligibility；
- same exact Character cannot be Player + Guaranteed NPC；
- Review exact re-resolve；
- missing/tamper fail-loud；
- Wizard→Review creates no Game DB / Game Library mutation / Provider call；
- Cancel returns Main Menu / no Session。

旧 `G4-05R1_REAL_ASSET_FIDELITY_CORRECTION_TASK.md`：**SUPERSEDED / DO NOT EXECUTE**。

G4-05 只有在 G4-02R1M1 + GPT IR PASS 后才恢复 closure；不允许绕过当前 Source correction。

---

## G4-06｜Atomic Final Create + World/Character Game-local Materialization

### HOLD until prerequisites pass

Outcome：

```text
exact Composition
→ Program-derived create identity / fingerprint
→ persist creating intent
→ create independent Game
→ pin exact Source generations
→ materialize selected T0 World + Character game-local definitions
→ establish Setup Context ancestry
→ created
```

Provider calls = 0 during Final Create。Exactly-once/replay-safe、Source isolation、Game-local provenance 必须成立。

---

## G4-07｜First Playable A：World + Character

### Purpose

在引入真实 Expansion 前，证明 World + Character 纵向本身值得玩。

```text
real World
+ real Player Character
+ optional real Guaranteed NPCs
+ Expansion = none
→ New Game
→ Compatibility Review
→ Final Create
→ real DeepSeek Opening
→ continuous free play
→ Save / exit / reopen / Continue
→ Owner UAT A
```

必须同时做：

- Narrative richness；
- Character individuality；
- anti-convergence pressure；
- Context not starved。

Engineering PASS 不替代 Owner Product PASS。

---

## G4-08｜Expansion Pack v0.1 + First Real Runtime Vertical

### Outcome

在 First Playable A 成立后才加入第三类 Primary Source。

```text
Expansion Source
→ exact selected binding
→ real observable Runtime / Context / mechanic effect
```

不接受 manifest/binding exists 就宣称 Expansion 工作。

第一代 `0..N`；不做 arbitrary code、复杂 feature toggle tree 或 generic external UI contribution schema。

---

## G4-09｜First Playable B：Add Real Expansion

```text
已通过的 World + Character 组合
+ 1 real Expansion
→ New Game
→ exact binding
→ real DeepSeek play
→ observable Expansion effect
→ Save / reopen / Continue
→ Owner UAT B
```

Gate question：Expansion 是否真的增加玩法，而不是只增加数据库记录。

---

## G4-10｜Runtime Asset Resolution

让 portrait / scene / authored map 绑定 exact Source generation，并通过 safe path / missing fallback / Windows export / real Godot load 验证。

不升级成完整地图 topology / travel / GIS / procedural map system。

---

## G4-11｜Two Primary Asset Families Reality Test

使用历史/低魔与高魔/幻想两组真实 Primary Source，分别完成独立 Game、Provider play、durable progression、Save/reopen/switch、exact visual asset generation 与 Source update isolation。

Owner 必须明显感到是两个不同世界，而不是只看到不同 `asset_id`。

---

## G4-GATE

至少要求：

```text
Application / Game Session lifecycle separation
+
Managed Source Library
+
World / Character v0.2-r2 real full-fidelity Source
+
T0-scoped projection / temporal compatibility mechanism
+
Multi-Game Library / One Game = One SQLite
+
asset-only New Game Wizard
+
Atomic Final Create / exact provenance
+
World + Character Owner UAT A PASS
+
0..N Expansion Source + real Runtime effect
+
Expansion Owner UAT B PASS
+
Runtime asset resolution
+
Two-family real Provider proof
+
Owner UAT PASS
```

G4 不要求 Creator、Reference Library、Opening Scenario Runtime、Map gameplay engine、任意外部 UI plugin、online store 或无资产建局。

---

# G5｜World Semantics & GM Runtime

## Outcome

让 G4 创建出的资产世界真正“活起来”，同时保持模型创造力和有限工程复杂度。

## Tasks

- G5-01 Minimum Playable T0 + World Turn / Semantic Materialization；
- G5-02 Knowledge Provenance：`World Truth != NPC Knowledge != Player Knowledge`；
- G5-03 NPC / Faction Agency；
- G5-04 Event / Priority-driven World Evolution；
- G5-05 Meaningful Choice / Mechanics Integration；
- G5-06 Runtime → UI Projection；
- G5-07 World Product Tests：Player Absence / Counterfactual Propagation / Independent Actor。

开放 Game-local semantics 可以由 lived history 产生，但已有 canonical Domain wins，不能建立 duplicate generic truth。

## G5-GATE

玩家不再是唯一因果源；世界有选择性自主演化；Guaranteed NPC 可以成为真正行动者；Expansion 机制语义进入正式世界，而没有发展成全宇宙模拟器。

---

# G6｜RPG Experience & Internal Declarative UI Host

## Outcome

把已成立的 Runtime Truth 做成真正的 RPG 产品界面，并证明内部声明式 Host 能力。

## Tasks

- 三栏 RPG Experience 完整化；
- Character / Relationship / Inventory / Faction / Map / Save 等真实 Surface；
- scene/portrait/map presentation；
- Expansion mechanic state 的真实 UI consumer；
- Internal Declarative UI Host v0.1；
- 小型 safe vocabulary，只按真实需求增长；
- Runtime projection → ViewModel → Host → Godot Control；
- bounded Action Intent；
- responsive / Theme / navigation；
- Owner UAT 与视觉 polish。

## G6-GATE

UI 明显增强游戏理解与沉浸；至少一个真实 Expansion/UI consumer 证明 Internal Host 是被需求拉出来的，而不是预造平台。

---

# G7｜Long-session Context & Performance

## Outcome

长局持续增长时，模型 working set、UI responsiveness 和 background work 仍可控。

## Tasks

- bounded Context Assembly；
- relevant subgraph / working set selection；
- deterministic background progression 与 model work 分离；
- TTFT / throughput / context size / persistence latency 的真实 long-play evidence；
- Source Library / Game-local semantics / history 增长时不把全部内容塞入 Prompt；
- long-session recovery/performance test。

## G7-GATE

`Game State / History ↑↑↑` 时 ordinary Turn Context 不线性爆炸，游戏仍可玩，且 Context bounded 不等于 Narrative starved。

---

# G8｜Mod / Authoring & External Declarative UI Contract

## Outcome

只在 G4/G6 已证明内部能力后，把内容与 Host capability 外部化给作者。

```text
proven internal capability
→ external schema / naming
→ validator / adapter
→ Creator / authoring helper / preview
```

## Tasks

- World Pack Creator；
- Character Card Creator；
- Expansion Pack Creator；
- Draft / Published Source 分离；
- Import / revision / append-only publish；
- AI-assisted authoring：typed scope / visible ChangeSet / Undo；
- external UI contribution schema；
- validation/migration；
- second/third real authored asset proof。

复杂任意代码沙箱仍 Deferred。

---

# G9｜Standalone Alpha / Release Validation

## Outcome

证明第一代可以作为独立长期 AI RPG 使用，而不是只通过单次演示。

覆盖 Windows build/startup/upgrade、long-play stability、Save/Load/Recovery、Context/performance、Source install/use、multi-Game、Provider failure、UI usability、Product Value UAT，以及与 The World / DSH simple baseline 的真实比较。

## G9-GATE

> **玩家愿意把它当一个独立 AI RPG 长期玩，而不是一个技术样品。**

---

## 10. 跨阶段开发纪律

每个高价值产品阶段默认采用：

```text
Freshness / historical preflight
→ failure / contract matrix
→ minimal production vertical
→ automated validation
→ real Provider / real Windows evidence when relevant
→ Independent Review
→ Owner UAT when product-facing
→ Decision Propagation
→ next task
```

长期反模式：

- 先造完整平台，再等玩法消费；
- Creator 先于 consumer；
- parser/schema PASS 替代真实建局/试玩；
- binding/proof module 存在就宣称机制真正工作；
- Owner UAT 太晚；
- stable display identity 猜 exact generation；
- Source update 静默改旧 Game；
- 全量 Source / full history dump 进 Prompt；
- 为避免开放语义而预造 giant schema forest。

---

## 11. 文档 / 任务纪律

- Current Task / PASS 只更新 `MY_WORLD_CURRENT_STATUS.md`；
- 架构结论更新 `MY_WORLD_架构_CURRENT.md`，深度从其导航；
- Task DAG / stage order 改变时更新本 Roadmap；
- 正式 implementation/review 使用 repository-native Task Packet；
- Product-facing Engineering Acceptance 不替代 Owner Product UAT；
- Active current only；历史依赖 Git history / archive；
- G4-05R1 remains superseded；不得从旧 packet 恢复已被新 Source 语义取代的路线。
