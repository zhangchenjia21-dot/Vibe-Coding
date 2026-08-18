---
title: 酒馆游戏项目开发核心总纲
status: current-integrated
updated: 2026-08-18
source_files: 15
supersedes: 酒馆游戏项目开发核心总纲_Obsidian整合版_2026-08-18_G8Host收口增量版.md
---

# 酒馆游戏项目开发核心总纲｜2026-08-18 G8 Migration 收口增量版

> 本文件是当前项目核心事实的最新索引与整合解释层。历史核心原文继续由 Git 历史与对应编号文件承担，不在此重复复制。

## 0. 当前阶段摘要

```text
G1–G7          PASS / CLOSED
G8             ACTIVE
WEB-04 Host    PASS / CLOSED
WEB-05 New Game / Creation Migration PASS / CLOSED
当前任务        G8-WEB-08 Controlled Multi-action Core Slice
G9             NOT STARTED / blocked by G8 Exit Gate
```

正式实现基线：

- Host Final Convergence：`sillytavern@5c76f4302152a7598b54d7f9d5774616b1fd618d`
- Creation Migration Closure：`sillytavern@61d8af56bd52e127de29f113c524681f0eea819a`

## 1. 当前 15 份核心来源

前 13 号职责保持不变。

### 14｜G8 Runtime-extensible UI 产品架构

Current：

`14_酒馆游戏_G8_RuntimeExtensibleUI产品架构裁定_v1.2_2026-08-18.md`

冻结 Core five、9 类 Host Capability、Owner/Contributor、pre-game conflict、safe declarative UI、player-safe live materialization、Creation Contribution、source identity、durable UI preference 与 Host-before-Protocol。

### 15｜Runtime Context Orchestration 与模块化复杂度控制

Current：

`15_酒馆游戏_RuntimeContextOrchestration与模块化复杂度控制裁定_v1.1_2026-08-18.md`

v1.1 supersedes v1.0。

冻结：

```text
Asset Library
!= Game Enabled Asset Set
!= Runtime Relevant
!= Model Visible Working Set
```

大型模块化 Package 进一步区分：

```text
Package Included
!= Feature Enabled
!= Module Enabled
!= Runtime Relevant
!= Model Visible
```

并冻结：

- Model-first Semantic Routing；
- Router 只判断 immediate relevance；
- Program structural validation + state-mandatory augmentation；
- typed Handoff = Ownership + Context Complexity Boundary；
- bounded JIT Runtime Projection；
- disabled Module fail-closed；
- `Background deterministic progression != Model Activation`。

## 2. G8 当前正式解释

### WEB-01～04

PASS / CLOSED / absorbed。

### WEB-05 New Game / API / Creation

**PASS / CLOSED。**

正式 New Game authority：

```text
Creation Project only
```

Migration Closure 已确认：

- legacy `GameCreationDraft` production authority = 0；
- Product client Draft methods = 0；
- Product HTTP Draft routes = 0；
- `GameCreationPublicApi` = Creation Project only；
- Provider current generation contract 单轨；
- `GAME_CREATION_ASSET_STEP_ORDER` 移入独立 current L0 New Game method contract；
- Creation Project / Host Contribution / Final Create 语义保持。

### WEB-06 / 07

DEFERRED / NON-BLOCKING FOR G9；Alpha / G11 前重新拉回。

### WEB-08

**NEXT / REQUIRED Core Slice。**

只冻结 bounded ordered multi-action、Program authority、one-input-one-Formal-Turn、atomic/no-partial-commit 语义；不建设万能 Planner。

### G8 Exit Gate

WEB-08 后直接执行 regression / Product E2E / Desktop+narrow / keyboard/focus / GitHub-first Review / Stage UAT。只有 P0/P1 才开修复任务。

## 3. G8 当前最短关键路径

```text
WEB-04 Host                         ✓ CLOSED
↓
WEB-05 Creation Migration           ✓ CLOSED
↓
WEB-08 Controlled Multi-action      ← NEXT
↓
G8 Exit Gate
↓
G8 PASS / CLOSED
```

## 4. Creation 当前权威

```text
Creation Project
├─ World Draft
├─ Gameplay Composition Draft
└─ Character & Opening Draft
→ Final Review
→ deterministic compile
→ immutable Creation Definition
→ Game Instance
```

Host Creation Contribution placement：`expansion_settings / character / opening`。

AI Fill values, never invent schema。

## 5. G8 → G9 当前边界

G8 先证明 Game Host / Creation / Core Action semantics；G9 才实现 External Asset machine protocol。

当前 G9 Task DAG：

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
```

原则：

> **Runtime Context Orchestrator Before Asset Context Schema。**

## 6. G9-02 current Exit Criteria

先用 internal handwritten runtime profiles 证明：

```text
Player Input
+
pruned Enabled Expansion / Feature / Module Directory
+
minimal scene / active context
↓
Router Model
↓
immediate relevant capability / intent
↓
Program structural validation
+
state-mandatory augmentation
↓
Runtime Relevant Set
↓
bounded JIT Runtime Projection
```

同时：

```text
Background deterministic progression
→ Program-only
→ zero model call
```

```text
Module OFF
→ no routing profile
→ no conditional dependency
→ no module state / surface
```

Asset/Dependency Graph 不等于 Context Inclusion Graph。

## 7. 当前动态事实源

- 治理 / 路线 / 项目裁定 → `Vibe-Coding/main`
- 当前代码 / tests → `sillytavern/main`
- Assets → `sillytavern-assets/main`
- Skills → `Skill/main`

多聊天正式任务前必须执行 Freshness + Decision Propagation。

## 8. Deferred Backlog

不阻塞当前 G9：Save Center / Recovery UX、Undo/Re-input、Creation Preview、完整 Semantic Review、完整 Gameplay Recommendation、深度 mobile/WCAG、高级 Timeline/Archive、多设备 Recovery、小型 UI polish。

## 9. 当前正式路线

- G8 Plan：`G8网页产品化启动规划_v1.5_2026-08-18.md`
- G8→G9 Decision：`G8阶段收口与G9前置裁定_v1.4_2026-08-18.md`
- Rebuild Roadmap：`酒馆游戏新版主体重建总路线 v1.9.md`
- Core 15：`15_酒馆游戏_RuntimeContextOrchestration与模块化复杂度控制裁定_v1.1_2026-08-18.md`

## 10. 当前 Next

> **G8-WEB-08｜Controlled Multi-action Core Slice**

当前不得开始 G9 Router / asset-spec / Creator / external Asset Adapter 实现。