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
G8                             ACTIVE / AWAITING OWNER UAT
WEB-04 Host                    PASS / CLOSED
WEB-05 Migration               PASS / CLOSED
WEB-08 Multi-action            PASS / CLOSED
G8-UAT-01                      PASS / CLOSED
G8-UAT-02 current SHA          cdbd9cd7ff0b5b9a5672156066478b57f732307c
G8-UAT-02 Independent Review   PASS / CLOSED
P0                             0
Remaining P1                   0
Project Owner Stage UAT        AUTHORIZED / NEXT
Current Next                   Project Owner Stage UAT
G9                             NOT AUTHORIZED
```

G8-UAT-02 已完成 Creation semantic materialization、Game-local concrete seed、bounded JIT Place/NPC、dynamic topology Save/Restore、Semantic-gated World Materialization、Opening Narrative authority、Creation private/public boundary、typed Item placement、Player Profile、Information IA，以及 Runtime/Product/Narrative scene-item visibility 收口。

---

## 1. 当前 active 正式来源

- `G8-UAT-02_GameLocalSemanticMaterialization与活世界收口规格_v1.0_2026-08-18.md`
- `G8-UAT-02_IndependentReview_阻塞发现_v1.0_2026-08-18.md`（已 RESOLVED / PASS）
- `G8阶段收口与G9前置裁定_v1.8_2026-08-18.md`
- `G8网页产品化启动规划_v1.8_2026-08-18.md`
- `酒馆游戏新版主体重建总路线 v2.0.md`
- `15_酒馆游戏_RuntimeContextOrchestration与模块化复杂度控制裁定_v1.2_2026-08-18.md`
- `16_酒馆游戏_RuntimeWorldMaterialization与当局游戏资产演化裁定_v1.1_2026-08-18.md`

---

## 2. G8-UAT-02 已证明

### Creation Semantic Materialization

- Creation Field Semantic Audit；
- configured AI strict typed materialization；
- multi-role / multi-item semantic cardinality；
- generic placeholder rejection；
- manual / no-key 不伪造 `附近的人 / 附近区域 / 随身物品`；
- Player Profile 完整 player-safe projection。

### Living World

```text
Player Input
→ Semantic AI judges existing target / optional Materialization Need
→ need only: World Materializer
→ Program validation / stable identity
→ atomic canonical commit
→ Narrative / Product / Suggestions
```

Program 不做开放式 NLP；World Materializer 不成为所有 Turn 固定前置调用。

### Durable World

Save / Restore 覆盖动态 Game-local topology / provenance / typed Item placement：

- save-before-growth → restore-old → future entities disappear；
- save-after-growth → restore → stable refs preserved；
- branch divergence / rollback / provenance consistency maintained。

### Narrative / Information Boundary

- Structured Opening Beat → independent authority-safe Opening Narrative；
- public materialization input 与 private raw seed 隔离；
- No Phantom / Player Agency / location & interactable authority 保留；
- Information Surface 只显示 Knowledge，不混入 generic event bookkeeping。

### Item placement / visibility

Item 支持：

```text
player-held
character-held
scene-present
```

并统一：

```text
Runtime current/final Scene items
== Product
== Continuity
== final NarrativeSafeContext
== interactableAuthority
```

`cdbd9cd...` 关闭了最后一个 scene-present Item final Narrative projection seam。

---

## 3. 不可回滚的既有 Authority

继续保持：

- Program Final Outcome authority；
- No Phantom Interactable；
- Player Agency；
- Bounded != Starved；
- exactly-five grounded suggestions；
- G6 Save / Restore / Branch；
- G7 Crash / Recovery / Idempotency；
- WEB-08 Controlled Multi-action。

---

## 4. 当前关键路径

```text
G8-UAT-01                 PASS / CLOSED
G8-UAT-02                 PASS / CLOSED
Independent Review        PASS
↓
Project Owner Stage UAT   AUTHORIZED / NEXT
↓
G8 PASS / CLOSED
↓
G9-01 Compatibility Audit
```

G9 external asset-spec / Creator / Adapter 仍未授权。

---

## 5. 当前 Next

> **Project Owner 使用真实新建游戏重新执行 Stage UAT。**

用户只做产品体验验收，不承担技术 QA；发现任何“不像正常 AI RPG”的现象，按真实玩家体验记录即可。