---
title: my world｜总体规划路线图
status: current-canonical-roadmap
version: 3.3
created: 2026-08-25
updated: 2026-09-02
current_phase: G4
current_status_source: MY_WORLD_CURRENT_STATUS.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜总体规划路线图 CURRENT

## 0. 文档职责

本文件拥有：

- G1–G9 阶段顺序；
- 每阶段核心 Outcome；
- 主要 Task DAG；
- Stage Gate；
- Deferred / Non-scope；
- 为什么按这个顺序做。

其它 current owner：

- 产品定义：`MY_WORLD_项目启动总纲_CURRENT.md`
- 跨阶段原则：`MY_WORLD_核心设计原则_CURRENT.md`
- 系统架构：`MY_WORLD_架构_CURRENT.md`
- 当前执行状态：`MY_WORLD_CURRENT_STATUS.md`

总原则：

> **先跑通真实核心循环，再扩展外围能力。**
>
> **Vertical before platform. Consumer before infrastructure.**
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
→ AI GM 自然语言互动
→ 世界产生 durable change
→ Save / exit / reopen
→ Continue / Load / Restore
→ 世界 + Context 一致恢复
→ 继续产生新的当前现实
```

---

# G1｜Foundation & Project Bootstrap

## Outcome

证明 Godot 4.7.2 能作为 Windows-local 独立 Host，并冻结第一代最小技术边界。

## Result

**PASS / CLOSED**。

已证明 Godot/GDScript/same-process、中文 UI、真实 Provider streaming/cancel、本地 IO、Windows export 等基础现实。

---

# G2｜AI Conversation Spine

## Outcome

建立值得体验的自然语言 AI RPG Narrative 主循环：

```text
输入自然语言
→ AI GM 真 streaming Narrative
→ 连续多回合
→ Cancel / Regenerate / Retry
→ Provider failure 后继续
```

## Result

**PASS / CLOSED**，`G2-GATE PASS`。

---

# G3｜Persistent Game / Save / Timeline Foundation

## Outcome

建立 durable backbone，让 Save / Load / Restore 与未来记忆隔离成为可靠原生能力。

长期区分：

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

**PASS / CLOSED**，`G3-GATE PASS`。

已证明 One authoritative SQLite flow、atomic mutation、accepted Conversation durability、reopen/resume、named Save、Restore、future-memory isolation、backup/recovery 等。

---

# G4｜Primary Source Assets & Local Game Creation

## Outcome

把产品升级为真正的**本地多世界 AI RPG Host**：

```text
Managed Source Library
→ World Pack + Character Card + optional Expansion Pack
→ explicit Game Creation Composition
→ atomic Final Create
→ independent Game-local Reality
→ real AI GM play
```

核心边界：

```text
Reusable immutable Source
!= Selected T0 Source Projection
!= Game-local Canonical Reality
!= Runtime State
```

第一代：

```text
Exactly 1 World Pack
Exactly 1 Player Character Card
0..N Guaranteed NPC Character Cards
0..N Expansion Packs
```

### G4-01 Application Shell / Lifecycle

**PASS / CLOSED**。

`Application Lifetime != Game Session Lifetime`。

### G4-02R1 Source Semantic Re-audit

**PASS / CLOSED**。

Current Source contract = **v0.2-r2**：

```text
thin identity/catalog metadata
+ ordered rich semantic_sections
+ gm_reference | gm_private
+ package-local Markdown/TXT
+ World Entry-scoped material
+ Character T0-profile-scoped material
+ exact generation fingerprint over declared bytes
```

保护：

> **Do not show the model a post-T0 answer and then ask it to forget that answer.**

> **Source schema is not the possibility ceiling of the Living World.**

真实语义压力覆盖：

```text
汉末三国：天下未定
- 刘备 / 曹操 / 孙权

埃瑟维亚：诸界余辉
- 莉维娅·塞兰 / 阿德里安·维尔克 / 杜恩·石痕
```

### G4-03 Managed Local Source Library

**PASS / CLOSED**。

staged verified publish、append-only immutable generations、exact fingerprint、restart truth、missing/tamper fail-loud 已成立。

### G4-04 Multi-Game / Game Library

**PASS / CLOSED**。

正式 topology：

> **One Game = One SQLite.**

### G4-05 Asset-only New Game Wizard

**PASS / CLOSED**。

显式 selection、exact generation pinning、eligibility、Review re-resolve、Cancel no-mutation 等已成立。

### G4-06 Atomic Final Create

**PASS / CLOSED**。

```text
exact Composition
→ Program-derived create identity
→ creating intent
→ independent Game
→ exact Source provenance
→ T0 materialization
→ Setup Context ancestry
→ created
```

Final Create 不调用 Provider；exactly-once/replay-safe 与 Source isolation 已证明。

### G4-07 First Playable A｜World + Character

**PASS / CLOSED — Owner UAT A**。

证明 World + Character 本身可玩，Narrative richness、Character individuality、anti-convergence 与 Save/Continue 成立。

### G4-08 Expansion Pack v0.1

**PASS / CLOSED**。

第一款真实 Expansion：`判定与检定：公开 d20`。

证明 exact selected Expansion → runtime mechanic effect，而不是只有 manifest/binding。

### G4-09 First Playable B｜Add Real Expansion

**PASS / CLOSED — Owner UAT B**。

Owner 已确认 Public d20 玩法价值，并在 correction 后确认可靠性/响应性。

保护的 runtime principle：

```text
Model Freedom First
+
Visible Narrative First
+
Canonical Commit Behind a Turn Finalize Barrier
```

### G4-10 Runtime Asset Resolution

**DEFERRED / MOVED TO G6 — DO NOT EXECUTE IN G4**。

Owner 于 2026-09-02 裁定：当前 portrait / scene / authored-map 资源尚未成熟，且视觉资源接入不是当前核心体验，不应占用 G4/G5 critical path。

Canonical decision：

`architecture/source/G4_VISUAL_ASSET_DEFERRAL_TO_G6_DECISION.md`

原 `G4-10M1`：**SUPERSEDED / DO NOT EXECUTE**。

保留原则但不实施：

```text
authored visual presentation
!= gameplay semantic authority

map image
!= topology / travel / current location / GIS
```

### G4-11 Two Primary Asset Families Reality Test

**NEXT / ACTIVE PREP**。

目的不是美术，而是对核心产品做最后一次跨 family reality pressure：

```text
历史 / 低魔：汉末三国
+
高魔 / 幻想：埃瑟维亚
```

分别验证：

```text
real full-fidelity Source
→ independent Game
→ real Provider Opening / play
→ world-specific narrative/causality
→ Character individuality
→ durable progression
→ Save / reopen / Continue
→ switch Games without leakage
→ exact semantic Source provenance / update isolation
```

**不要求：** portrait / scene / authored map、exact visual generation、视觉 polish。

Product Gate：Owner 必须明显感觉这是两个不同的 RPG 世界，而不是同一聊天模板换 `asset_id`。

### G4-GATE

至少要求：

```text
Application / Game Session lifecycle separation
+
Managed Source Library
+
World / Character v0.2-r2 full-fidelity Source
+
T0-scoped projection / temporal compatibility
+
Multi-Game / One Game = One SQLite
+
asset-only New Game Wizard
+
Atomic Final Create / exact provenance
+
World + Character Owner UAT A PASS
+
0..N Expansion + real Runtime effect
+
Expansion Owner UAT B PASS
+
Two-family real Provider proof
+
Two-family Owner Reality UAT PASS
```

**Runtime visual asset resolution is no longer a G4-GATE requirement.**

G4 不要求 Creator、Reference Library、Opening Scenario Runtime、Map gameplay engine、visual asset runtime、外部 UI plugin、online store 或无资产建局。

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

Visual independence invariant：

```text
portrait / scene / map image
!=
World / Character / Knowledge / Event / Location authority
```

G5 不等待美术资产完成。

## G5-GATE

玩家不再是唯一因果源；世界有选择性自主演化；Guaranteed NPC 可成为真正行动者；Expansion 机制语义进入世界，而没有发展成全宇宙模拟器。

---

# G6｜RPG Experience & Internal Declarative UI Host

## Outcome

把已经成立的 Runtime Truth 做成真正的 RPG 产品界面，并让**真实 presentation consumer 拉出视觉资源能力**。

## Recommended order / Tasks

- Runtime projection → ViewModel → real UI consumer；
- **re-audit + implement Runtime Asset Resolution only for actual G6 visual consumers**；
- portrait / scene / authored-map presentation；
- 三栏 RPG Experience 完整化；
- Character / Relationship / Inventory / Faction / Map / Save 等真实 Surface；
- Expansion mechanic state 的真实 UI consumer；
- Internal Declarative UI Host v0.1；
- bounded Action Intent；
- responsive / Theme / navigation；
- Owner UAT 与视觉 polish。

G6 re-entry 时必须重新裁定：旧 Game 是否允许 presentation-only override 使用后来补充的视觉，而不改变 semantic Source ancestry。G4/G5 不预造该机制。

## G6-GATE

UI 明显增强理解与沉浸；真实 visual/UI consumer 证明 Host/asset resolution 是被需求拉出来的，而不是预造平台。

---

# G7｜Long-session Context & Performance

## Outcome

长局增长时，working set、UI responsiveness 与 background work 仍可控。

## Tasks

- bounded Context Assembly；
- relevant subgraph / working set selection；
- deterministic background progression 与 model work 分离；
- TTFT / throughput / context size / persistence latency 真实长局证据；
- Source / Game semantics / history 增长时不线性塞满 Prompt；
- long-session recovery/performance test。

## G7-GATE

`Game State / History ↑↑↑` 时 ordinary Turn Context 不线性爆炸，且 bounded 不等于 Narrative starved。

---

# G8｜Mod / Authoring & External Declarative UI Contract

## Outcome

在内部 Runtime/UI consumer 已证明后，才建立 authoring / external extension contract。

## Tasks

- Source authoring/import workflow；
- safe external declarative UI vocabulary from proven internal Host；
- compatibility/versioning/migration；
- bounded extension capabilities；
- real third-party-like package tests。

## G8-GATE

Creator/extension 能力由真实 consumer 与稳定内部协议拉出，不形成 arbitrary-code plugin platform。

---

# G9｜Standalone Alpha / Release Validation

## Outcome

把完整纵向整理为可独立安装、恢复、诊断的 Alpha 产品。

## Tasks

- standalone Windows packaging；
- onboarding / credentials / Source setup；
- upgrade/migration/recovery；
- long-play / corruption / reinstall reality tests；
- release UAT / defect closure；
- documentation / diagnostics / support boundary。

## G9-GATE

独立用户能安装、建局、长期游玩、保存恢复，并在真实失败后有可理解的恢复路径。
