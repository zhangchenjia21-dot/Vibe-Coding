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

当前真人主路径已成立：

```text
Launcher
→ Main Menu
→ Creation Project
→ Final Create
→ Game Instance
→ Product Session
```

当前 Host 正式纵向也已成立：

```text
handwritten typed Runtime UI Definition
→ Host Assembly
→ Real Product Server
→ player-safe Projection
→ materialized declarative UI DTO
→ Formal Product UI consumer
```

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
- Declarative Structure ≠ Live Data；
- player-safe live contribution materialization；
- current Creation Contribution；
- sourceDefinitionId / ownerGameplayId；
- durable Product UI Preference；
- Host-before-protocol。

正式实现基线：

`zhangchenjia21-dot/sillytavern@5c76f4302152a7598b54d7f9d5774616b1fd618d`

### 15｜Runtime Context Orchestration 与模块化复杂度控制裁定

`15_酒馆游戏_RuntimeContextOrchestration与模块化复杂度控制裁定_v1.0_2026-08-18.md`

冻结：

```text
Asset Library
!= Game Enabled Asset Set
!= Current Runtime Relevant Set
!= Current Model Visible Working Set
```

以及：

- Enabled Expansion 不自动进入 Prompt；
- Dependency Graph != Context Inclusion Graph；
- Model-first Semantic Routing；
- Program structural validation + state-mandatory augmentation；
- bounded JIT Runtime Projection；
- typed Handoff 同时是 Context Complexity Boundary。

## 2. 当前权威顺序

1. 项目所有者当前明确裁定；
2. 当前最新编号核心裁定；
3. 当前最新路线 / 阶段规划 / frozen product spec；
4. `sillytavern/main` 当前实现与测试事实；
5. 历史版本 / Legacy Reference；
6. 聊天记忆 / 旧附件。

动态事实源：

- 治理 / 路线 /核心裁定 → `Vibe-Coding/main`
- 当前实现 → `sillytavern/main`
- 资产 → `sillytavern-assets/main`
- Skill → `Skill/main`

## 3. G8 当前解释

### WEB-01
PASS / CLOSED。

### WEB-02
PASS / absorbed。

### WEB-03
PASS / CLOSED。

### WEB-04 Final Host Convergence
**PASS / CLOSED。**

独立审核确认：

- production typed composition root；
- default Core-only behavior；
- player-safe materializer；
- no arbitrary state path / selector / expression；
- live public state after Formal Turn 自动更新 Contribution；
- Creation bridge 已对齐 `expansion_settings / character / opening`；
- Host source / gameplay owner identity 显式；
- gameplay disable/enable dormant preservation；
- durable UI Preference 跨 application restart；
- Restore 不回滚 UI Preference；
- invalid definitions fail closed。

### WEB-05
玩家主路径 PASS；当前只剩 **Technical Migration Closure**。

目标：

```text
Creation Project
=
唯一 production New Game authority
```

退休 legacy One Draft / Five Sections production path。

### WEB-06 / WEB-07
**DEFERRED / NON-BLOCKING FOR G9**。

G6/G7 authority 已 PASS；Alpha/G11 前重新拉回。

### WEB-08
**REQUIRED Core Slice**。

只冻结会影响未来 Expansion Action Intent / Resolution 的最小 multi-action semantics。

### G8 Exit Gate
WEB-05 与 WEB-08 后直接进行 regression / E2E / narrow / keyboard / Stage UAT；只有 P0/P1 再开修复任务。

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

禁止：

- arbitrary JS / React / DOM / CSS / eval；
- arbitrary state path；
- general expression / query DSL；
- direct Game State mutation；
- hidden/private state 进入 browser DTO。

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

Host Creation Contribution 当前合法 placement：

- `expansion_settings`；
- `character`；
- `opening`。

AI Fill values, never invent schema。

## 7. G8 → G9 边界

G8 必须先证明 Game Host / Creation / Core Action semantics。

G9 才做 External Asset machine protocol。

当前 Decision Propagation 后的 G9 顺序：

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

G9-02 先用 handwritten/internal Expansion runtime profiles 证明：

```text
Player Input
+ Enabled Expansion Directory
+ minimal scene summary
→ Router Model
→ relevant Expansion / Intent IDs
→ Program mechanical validation
→ state-mandatory augmentation
→ Current Relevant Set
→ bounded JIT Runtime Projection
```

然后 asset-spec 才声明这些已被证明的能力。

## 8. 延后 Backlog

不阻塞当前 G9：

- Save Center / Recovery Product UX；
- Undo / Re-input Product UX；
- Creation Preview；
- Real DeepSeek Creation Semantic Review 完整接线；
- 完整 Gameplay Recommendation；
- UI copy / preset / chip polish；
- 深度 mobile / WCAG；
- advanced Timeline / Archive GC / multi-device Recovery。

这些不是取消，进入 Alpha / G11 前重新拉回。

## 9. 当前正式文件

- 第 14 号核心：`14_酒馆游戏_G8_RuntimeExtensibleUI产品架构裁定_v1.2_2026-08-18.md`
- 第 15 号核心：`15_酒馆游戏_RuntimeContextOrchestration与模块化复杂度控制裁定_v1.0_2026-08-18.md`
- G8 规划：`G8网页产品化启动规划_v1.4_2026-08-18.md`
- 当前总路线：`酒馆游戏新版主体重建总路线 v1.8.md`

## 10. 当前 Next

> **G8-WEB-05｜Technical Migration Closure**

当前不得开始正式 G9 machine schema / Creator / Asset Adapter 实现。
