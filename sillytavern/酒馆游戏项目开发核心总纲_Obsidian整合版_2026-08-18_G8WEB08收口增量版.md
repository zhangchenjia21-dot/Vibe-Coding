---
title: 酒馆游戏项目开发核心总纲
status: current-integrated
updated: 2026-08-18
source_files: 15
supersedes: 酒馆游戏项目开发核心总纲_Obsidian整合版_2026-08-18_G8CreationMigration收口增量版.md
---

# 酒馆游戏项目开发核心总纲｜2026-08-18 G8 WEB-08 收口增量版

> 本文件是当前项目核心事实的最新索引与整合解释层。历史核心原文继续由 Git 历史与编号文件承担，不在本文件重复全文。

## 0. 当前状态

```text
G1–G7          PASS / CLOSED
G8-WEB-04      PASS / CLOSED
G8-WEB-05      PASS / CLOSED
G8-WEB-08      PASS / CLOSED
G8             EXIT GATE
G9             NOT STARTED
```

当前代码基线：

`sillytavern@3ad5b419e9fb289488a18ded82c5a77f2cdd376c`

## 1. 当前第 14 / 15 号核心来源

### 14｜Runtime-extensible UI

当前：`14_酒馆游戏_G8_RuntimeExtensibleUI产品架构裁定_v1.2_2026-08-18.md`

冻结：Core five、9 Host capabilities、Owner/Contributor、secondary View、safe component vocabulary、player-safe live materialization、Creation Contribution、source identity、durable UI preference、no arbitrary frontend/state access。

### 15｜Runtime Context Orchestration

当前：`15_酒馆游戏_RuntimeContextOrchestration与模块化复杂度控制裁定_v1.1_2026-08-18.md`

冻结：

```text
Asset Library
!= Package Included
!= Feature Enabled
!= Module Enabled
!= Runtime Relevant
!= Model Visible Working Set
```

以及：Model-first immediate routing、Program structural validation、state-mandatory augmentation、typed Handoff、bounded JIT projection、conditional dependency fail-closed、Background deterministic progression != Model Activation。

## 2. Creation 当前权威

```text
Creation Project only
→ deterministic compile
→ immutable Creation Definition
→ Game Instance
```

旧 One Draft / Five Sections production authority 已在 `61d8af56...` 退休。

## 3. Host 当前权威

```text
handwritten typed Runtime UI Definition
→ Host Assembly
→ Product Bootstrap / Server
→ player-safe projection
→ materialized DTO
→ Product UI
```

外部 asset-spec 尚未开始。

## 4. Controlled Multi-action 当前权威

WEB-08 已关闭。

```text
one Player Input
→ action_sequence(max 2)
→ Program exact authorization
→ transient state preflight
→ deterministic-only steps
→ aggregate FormalDelta
→ one commitFormalTurn
→ one Formal Turn
```

规则：

- 只允许 move / item_transfer / show_item / item_interaction / wait；
- 任一步 Resolution Required → zero-RNG fail closed；
- evidence 必须完整、有序、原文可证；
- omitted second action 不得 first-only commit；
- no partial commit；
- semantic evidence 持久化进原 DurableExecution artifact；
- semantic/formal/narrative crash recovery exactly-once；
- background progression 只在 aggregate 上 Program-owned 处理一次；
- Product Timeline 仍为一条玩家输入 / 一个 Formal Turn。

## 5. G8 当前唯一剩余工作

> **G8 Exit Gate**

不新增功能，验证：

- Creation → Session；
- Host production vertical；
- multi-action vertical；
- G5–G7 regression；
- no-key / configured-key；
- Product E2E；
- disclosure；
- build / lint / typecheck / launcher；
- desktop / narrow / keyboard critical path；
- 项目所有者 Stage UAT。

P0/P1 才开修复任务；P2/P3 进入 backlog。

## 6. G9 当前顺序

```text
G9-01 Compatibility Audit
→ G9-02 Runtime Asset Binding + Context Orchestration Foundation
→ G9-03 asset-spec vNext
→ G9-04 Game Asset Adapter / Compiler
→ G9-05 Creator rebuild
```

其中 G9-02 必须在 Schema Freeze 前证明 Feature/Module Directory pruning、disabled module fail-closed、conditional dependency、background zero-model-call 与 bounded JIT projection。

## 7. Deferred Backlog

- WEB-06 Save Center / Recovery UX；
- WEB-07 Undo / Re-input；
- deep responsive / WCAG；
- advanced Timeline / Archive；
- multi-device recovery；
- Creation Preview；
- full Semantic Review；
- full Gameplay Recommendation；
- visual/copy polish。

Alpha / G11 前重新拉回。

## 8. 当前 Next

```text
G8 Exit Gate
↓ PASS + Stage UAT
G8 PASS / CLOSED
↓
G9-01 Compatibility Audit
```
