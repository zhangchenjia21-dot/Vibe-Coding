---
title: 酒馆游戏项目开发核心总纲
aliases:
  - 酒馆游戏核心文件
  - Tavern Project Master Spec
  - World OS 项目总纲
type: project-master-spec
status: current-integrated
created: 2026-08-14
updated: 2026-08-18
source_files: 15
supersedes: 酒馆游戏项目开发核心总纲_Obsidian整合版_2026-08-18_G8收口增量版.md
---

# 酒馆游戏项目开发核心总纲｜2026-08-18 G8 Host 收口增量版

> 本文件是当前项目核心事实的最新索引与整合解释层。历史核心原文继续保留；本文件只维护 current interpretation，避免复制整套历史正文。

## 0. 当前阶段

```text
G1–G7          PASS / CLOSED
G8             ACTIVE
WEB-04 Host    PASS / CLOSED
当前任务        G8-WEB-05 Technical Migration Closure
G9             NOT STARTED / blocked by G8 Exit Gate
```

当前真人主路径：

```text
Launcher
→ Main Menu
→ Creation Project
→ Final Create
→ Game Instance
→ Product Session
```

当前 Host 正式纵向：

```text
handwritten typed Runtime UI Definition
→ Host Assembly
→ Real Product Server
→ player-safe Projection
→ materialized declarative UI DTO
→ Formal Product UI consumer
```

---

## 1. 当前 15 份核心来源

前 13 号继续保持既有职责。

### 14｜G8 Runtime-extensible UI 产品架构裁定

当前版本：

`14_酒馆游戏_G8_RuntimeExtensibleUI产品架构裁定_v1.2_2026-08-18.md`

状态：**current / WEB-04 PASS-CLOSED baseline**。

冻结：

- Core five permanent Surfaces；
- 九类 Host Capability；
- Surface Owner / Contributor；
- pre-game unique Owner conflict；
- secondary View / safe component vocabulary；
- controlled Action Intent；
- Declarative Structure != Live Data；
- player-safe live contribution materialization；
- current Creation Contribution；
- sourceDefinitionId / ownerGameplayId；
- durable Product UI Preference；
- Host-before-protocol。

正式实现基线：

`sillytavern@5c76f4302152a7598b54d7f9d5774616b1fd618d`

### 15｜Runtime Context Orchestration 与模块化复杂度控制裁定

当前版本：

`15_酒馆游戏_RuntimeContextOrchestration与模块化复杂度控制裁定_v1.1_2026-08-18.md`

v1.1 当前冻结：

```text
Asset Library
!= Game Enabled Asset Set
!= Runtime Relevant
!= Model Visible Working Set
```

对于大型 Feature / Module Package：

```text
Package Included
!= Feature Enabled
!= Module Enabled
!= Runtime Relevant
!= Model Visible
```

并冻结：

- Enabled Expansion 不自动进入 Prompt；
- Full Asset Definition != Model Prompt Payload；
- Dependency Graph != Context Inclusion Graph；
- Model-first Semantic Routing；
- Program structural validation + state-mandatory augmentation；
- Router 只判断 immediate relevance；
- typed Handoff 是 Ownership + Context Complexity Boundary；
- bounded JIT Runtime Projection；
- Router Directory 只暴露当前真正 Enabled 的 Feature / Module profile；
- `Background deterministic progression != Model Activation`；
- timer / need / cooldown / routine automation 等 deterministic progression 可 Program-only；
- 长期状态 / enabled asset / Package capability 数量增长不得自动造成普通 Turn Prompt / model-call 数线性增长。

Wave 2 的 Survival / Traveler-System 是上述两条增量规则的真实资产证据。

---

## 2. 当前权威顺序

1. 项目所有者当前明确裁定；
2. 当前最新编号核心裁定；
3. 当前最新路线 / 阶段规划 / frozen product spec；
4. `sillytavern/main` 当前实现与测试事实；
5. 历史版本 / Legacy Reference；
6. 聊天记忆 / 旧附件。

动态事实源：治理/路线/核心裁定 → Vibe-Coding；实现 → sillytavern；资产 → sillytavern-assets；Skill → Skill。

多聊天正式任务前执行 Freshness；发现新 current decision 后继续做 Decision Propagation，不得只读到新文件却沿用旧 Task DAG。

---

## 3. G8 当前解释

### WEB-01
PASS / CLOSED。

### WEB-02
PASS / absorbed。

### WEB-03
PASS / CLOSED。

### WEB-04 Final Host Convergence
**PASS / CLOSED。**

已证明 production typed composition root、Core-only default、player-safe materializer、no arbitrary state path / selector / expression、live contribution、Creation bridge、source identity、gameplay dormant preservation、durable UI preference 与 fail-closed invalid definition。

### WEB-05
玩家主路径 PASS；当前只剩 **Technical Migration Closure**。

最终：

```text
Creation Project = 唯一 production New Game authority
```

退休 legacy One Draft / Five Sections production path。

### WEB-06 / WEB-07
**DEFERRED / NON-BLOCKING FOR G9**；G6/G7 authority 已 PASS，Alpha/G11 前重新拉回。

### WEB-08
**REQUIRED Core Slice**；只冻结会影响未来 Expansion Action Intent / Resolution 的最小 multi-action semantics。

### G8 Exit Gate
WEB-05 与 WEB-08 后直接进行 regression / E2E / narrow / keyboard / Stage UAT；只有 P0/P1 再开修复任务。

---

## 4. G8 当前最短关键路径

```text
WEB-04 Final Host Convergence        ✓ PASS / CLOSED
↓
WEB-05 Technical Migration Closure   ← NEXT
↓
WEB-08 Controlled Multi-action Core Slice
↓
G8 Exit Gate
↓
G8 PASS / CLOSED
```

---

## 5. Declarative UI 当前权威模型

```text
Definition / future Asset
→ 声明安全 UI 结构与受控数据需求

Runtime / Projection
→ authoritative state + player-safe live values

Host
→ validate / assemble / materialize / layout / render

Player
→ Surface order preference
```

禁止 arbitrary JS / React / DOM / CSS / eval、arbitrary state path、general expression/query DSL、direct Game State mutation、hidden/private state 进入 browser DTO。

---

## 6. Creation Project 当前权威

```text
Creation Project
├─ World
├─ Gameplay Composition
└─ Character & Opening
→ deterministic Final Create
→ immutable Creation Definition
→ Game Instance
```

Host Creation Contribution placement：`expansion_settings / character / opening`。

AI Fill values, never invent schema。

---

## 7. G8 → G9 边界

G8 先证明 Game Host / Creation / Core Action semantics；G9 才做 External Asset machine protocol。

当前 G9 顺序：

```text
G9-01 Compatibility Audit
↓
G9-02 Runtime Asset Binding + Context Orchestration Foundation
↓
G9-03 asset-spec vNext Machine Contract
↓
G9-04 Game Asset Adapter / Compiler
↓
G9-05 Creator rebuild
↓
Golden Asset Family migration
```

关键原则：

> **Runtime Context Orchestrator Before Asset Context Schema。**

G9-02 必须先用 handwritten/internal runtime profiles 证明：

```text
Player Input
+ pruned Enabled Expansion / Feature / Module Directory
+ minimal scene summary
→ Router Model
→ immediate relevant Expansion / Intent IDs
→ Program mechanical validation
→ state-mandatory augmentation
→ Current Relevant Set
→ bounded JIT Runtime Projection
```

同时证明：

```text
background deterministic progression
→ Program only
→ no model call
```

以及：

```text
Module OFF
→ no routing profile
→ no conditional dependency
→ no module state/surface
```

再由 asset-spec 声明这些已经被 Runtime 证明的能力。

---

## 8. 延后 Backlog

不阻塞当前 G9：Save Center / Recovery Product UX、Undo / Re-input、Creation Preview、完整 DeepSeek Creation Semantic Review、完整 Gameplay Recommendation、UI polish、深度 mobile / WCAG、advanced Timeline / Archive / multi-device Recovery。

这些不是取消，Alpha / G11 前重新拉回。

---

## 9. 当前正式文件

- 第 14 号核心：`14_酒馆游戏_G8_RuntimeExtensibleUI产品架构裁定_v1.2_2026-08-18.md`
- 第 15 号核心：`15_酒馆游戏_RuntimeContextOrchestration与模块化复杂度控制裁定_v1.1_2026-08-18.md`
- G8 规划：`G8网页产品化启动规划_v1.4_2026-08-18.md`
- 当前总路线：`酒馆游戏新版主体重建总路线 v1.8.md`

---

## 10. 当前 Next

> **G8-WEB-05｜Technical Migration Closure**

当前不得开始正式 G9 machine schema / Creator / Asset Adapter 实现。

资产侧 Semantic Context Retrofit 可以并行继续，为 G9-02 提供真实 handwritten requirements corpus；但不得反向发明 Host 能力。