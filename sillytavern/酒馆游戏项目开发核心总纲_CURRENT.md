---
title: 酒馆游戏项目开发核心总纲
status: current-integrated
updated: 2026-08-18
current_path: sillytavern/酒馆游戏项目开发核心总纲_CURRENT.md
---

# 酒馆游戏项目开发核心总纲｜CURRENT

## 0. 当前状态

```text
G1–G8                         PASS / CLOSED
WEB-04 Host                    PASS / CLOSED
WEB-05 Migration               PASS / CLOSED
WEB-08 Multi-action            PASS / CLOSED
G8-UAT-01                      PASS / CLOSED
G8-UAT-02 final SHA            cdbd9cd7ff0b5b9a5672156066478b57f732307c
G8-UAT-02 Independent Review   PASS / CLOSED
Project Owner Stage UAT        PASS WITH NON-BLOCKING UX FINDINGS
P0                             0
P1                             0
G9                             ACTIVE / G9-01
Current Next                   G9-01 Compatibility Audit
```

Project Owner 最终 UAT 确认：游戏已经可以持续运行；剩余问题属于体验、后续 Runtime capability 或产品管理，不再构成 G8 blocker。G8 正式 PASS / CLOSED，G9-01 Compatibility Audit 获授权。

---

## 1. 当前 active 正式来源

- `G8_StageUAT_最终收口与体验Backlog_v1.0_2026-08-18.md`
- `G8-UAT-02_IndependentReview_阻塞发现_v1.0_2026-08-18.md`（RESOLVED / PASS）
- `G8阶段收口与G9前置裁定_v1.8_2026-08-18.md`
- `酒馆游戏新版主体重建总路线 v2.0.md`
- `15_酒馆游戏_RuntimeContextOrchestration与模块化复杂度控制裁定_v1.2_2026-08-18.md`
- `16_酒馆游戏_RuntimeWorldMaterialization与当局游戏资产演化裁定_v1.1_2026-08-18.md`

---

## 2. G8 最终完成能力

### Creation / Playability

- Creation Field Semantic Audit；
- configured AI strict typed materialization；
- concrete opening NPC / Place / Item；
- proactive authority-safe Opening Narrative；
- Player Profile；
- Dynamic Five；
- manual/no-key 不伪造 generic placeholder。

### Runtime / Living World

```text
Player Input
→ Semantic AI judges intent + optional Materialization Need
→ need only: bounded World Materializer
→ Program validation / stable identity
→ atomic canonical commit
→ Narrative / Product / Suggestions
```

Program 不做开放式 NLP；World Materializer 不成为普通 Turn 固定前置调用。

### Durable World / Authority

- dynamic topology Save / Restore / Branch；
- G7 Crash / Recovery / Idempotency；
- Program Final Outcome authority；
- No Phantom Interactable；
- Player Agency；
- location / interactable authority；
- Creation public/private boundary；
- typed Item player / character / scene placement；
- Runtime / Product / Continuity / Narrative scene-item consistency；
- Information Surface = Knowledge，不混入 generic event bookkeeping。

---

## 3. Owner UAT 非阻塞体验发现

正式记录于：

`G8_StageUAT_最终收口与体验Backlog_v1.0_2026-08-18.md`

### UX-01 Item dossier evolution

拆分为：

```text
carried Item publicDescription 展示遗漏
→ P3 / G11 Product polish

inspection/discovery → durable Item known/public description patch
→ P2 / G9-02 Existing Game-local Asset Mutation Reference Case
```

第 16 号裁定已经冻结 `Item known description` 等为 Evolvable Definition Fields；G9-02 应证明 existing local asset typed patch → Product refresh → Save/Restore → source unchanged。

### UX-02 Objective / Goal

当前 Objective Surface 刻意等待未来 Runtime Objective authority，不从 Narrative / Commitment 伪造目标。

```text
G9-01：检查 Objective 未来可作为 Game-local canonical record / extension
完整 Objective Engine：后续 dedicated vertical，最迟 Alpha/G11 前
```

### UX-03 Game deletion

`G11 Alpha / Game Library lifecycle management`。

### UX-04 DeepSeek model selection

`G10 Provider Expansion`；当前 fixed model / `modelEditable=false` 不阻塞 G9。

---

## 4. 当前 G9 DAG

```text
G9-01 Compatibility Audit
↓
G9-02 Runtime Asset Binding
      + Context Orchestration Foundation
      + full Game-local Canonical Asset Layer
      + Runtime World Materialization Foundation
↓
G9-03 asset-spec vNext Machine Contract
↓
G9-04 Game Asset Adapter / Compiler
↓
G9-05 Creator rebuild
```

### G9-01 新增真实 UAT 输入

Compatibility Audit 必须显式检查：

1. `Objective / Task` 等长期记录未来可进入 Game-local canonical extensibility，不被 Source / Runtime schema 提前封死；
2. Existing Game-local Item / Character / Place 的 evolvable definition field patch 具备明确 ownership / provenance / persistence / visibility 边界；
3. Item inspection 后的 durable known-description evolution 可作为 G9-02 reference case。

不把 Game Delete、Provider model selector 或完整 Objective Engine 提前塞入 G9-01/02。

---

## 5. 当前 Next

> **G9-01 Compatibility Audit。**

G8 不因上述非阻塞体验问题再次 reopen；只有 G9-01 若发现会导致错误 asset protocol / Game-local ownership / Context boundary 的 P0/P1 架构缺口，才调整 G9 DAG。
