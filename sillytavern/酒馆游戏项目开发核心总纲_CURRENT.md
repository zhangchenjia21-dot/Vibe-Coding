---
title: 酒馆游戏项目开发核心总纲
status: current-integrated
updated: 2026-08-18
current_path: sillytavern/酒馆游戏项目开发核心总纲_CURRENT.md
---

# 酒馆游戏项目开发核心总纲｜CURRENT

## 0. 当前状态

```text
G1–G7                         PASS / CLOSED
G8                             ACTIVE / UAT FIX
WEB-04 Host                    PASS / CLOSED
WEB-05 Migration               PASS / CLOSED
WEB-08 Multi-action            PASS / CLOSED
G8-UAT-01                      PASS / CLOSED
Stage UAT rerun                FAIL / BLOCKED
G8-UAT-02 current SHA          ce6c05ffc89f31a39400f7069c9e7503e4c86d9a
Independent Review rerun       FAIL / NARROW RETURN REQUIRED
P0                             0 confirmed
Original P1                    5
Closed P1                      4
Remaining P1                   1
Stage UAT                      NOT AUTHORIZED
Current Next                   scene-item Narrative projection narrow fix
G9                             NOT AUTHORIZED
```

`ce6c05f...` 已关闭 Save/Restore topology、AI Semantic-gated Materialization、Opening Beat authority、Creation public/private boundary 与 typed Item placement 主体问题；Independent Review 只剩一个跨层 P1：scene-present Item 在最终 Narrative-safe current projection 中仍被旧 holder 规则过滤。

---

## 1. 当前 active 正式来源

- `G8-UAT-02_GameLocalSemanticMaterialization与活世界收口规格_v1.0_2026-08-18.md`
- `G8-UAT-02_IndependentReview_阻塞发现_v1.0_2026-08-18.md`
- `G8阶段收口与G9前置裁定_v1.8_2026-08-18.md`
- `G8网页产品化启动规划_v1.8_2026-08-18.md`
- `酒馆游戏新版主体重建总路线 v2.0.md`
- `15_酒馆游戏_RuntimeContextOrchestration与模块化复杂度控制裁定_v1.2_2026-08-18.md`
- `16_酒馆游戏_RuntimeWorldMaterialization与当局游戏资产演化裁定_v1.1_2026-08-18.md`

通用治理继续使用 Skill current；Skill 最新 task-packet 标准不改变当前 Runtime 实现语义。

---

## 2. `ce6c05f...` 已独立确认关闭的四个原 P1

### Save / Restore topology

Canonical Save 已记录当前 topology / provenance / typed Item placement；Restore 在 SQLite transaction 内精确重建 snapshot 世界，删除 future-only Place/Scene/Connection/Character/Item 与 provenance，并保持 branch semantics。

### AI Semantic-gated Materialization

```text
Player Input
→ Semantic AI judges intent + optional Materialization Need
→ need = none: World Materializer 0 calls
→ need != none: World Materializer bounded call
→ Program validation / stable identity / commit
```

Program 不使用关键词、正则或通用 NLP 判断是否物化。

### Opening Beat authority

Creation Materializer 只输出 structured Opening Beat；独立 Opening Narrative Realizer 只读取 validated public Scene/Character/Item/hook，并持久化最终开场叙事。private raw source 不进入该请求。

### Creation private/public + typed Item placement

Public materialization input 与 private seed raw source 已分离；Item 支持 player / character / scene placement，并贯通 Runtime / SQLite / Product。

---

## 3. Remaining P1｜Scene-present Item Narrative projection

当前：

```text
visibleItemsInCurrentScene
→ 支持 scene-present Item

compileContinuityContext
→ 支持 scene-present Item

projectNarrativeSafeCurrentContext
→ 仍只检查 holderRef 是否属于 player / visible Character
→ 漏掉 placementKind=scene && holderRef=finalSceneRef
```

因此 authoritative/Product 可见的场景物品会在正式 Narrative realization 前消失，随后 `interactableAuthority.itemNames` 也缺少该 Item。

必须修成统一 current-scene visibility 语义，并验证移动后按 finalSceneRef 切换 scene-present items。

---

## 4. 不可回滚的 Authority

继续保持：Program Final Outcome、No Phantom、location/interactable authority、Player Agency、Bounded != Starved、exactly-five suggestions、G6 Save/Restore、G7 Recovery/Idempotency、WEB-08 controlled multi-action。

---

## 5. 当前关键路径

```text
G8-UAT-02 ce6c05f...       REVIEWED
4 original P1              CLOSED
1 Narrative projection P1  REMAINS
↓
scene-item projection narrow fix
↓
Independent Review rerun
↓
Project Owner Stage UAT
↓
G8 PASS / CLOSED
↓
G9-01 Compatibility Audit
```

G9 external asset-spec / Creator / Adapter 仍未授权。

---

## 6. 当前 Next

> **只修 scene-present Item 的 final Narrative-safe projection 与对应 regression / Real Provider proof；不重做其它 G8-UAT-02 主干。**
