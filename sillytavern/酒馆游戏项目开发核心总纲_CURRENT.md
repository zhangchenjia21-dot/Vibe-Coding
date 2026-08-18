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
G8-UAT-02 implementation SHA   2c7e6a4cd85e1f3c52350c1b85ae70c99864b940
G8-UAT-02 Independent Review   FAIL / RETURN REQUIRED
P0                             0 confirmed
P1                             5
Stage UAT                      NOT AUTHORIZED
Current Next                   G8-UAT-02 narrow return fix
G9                             NOT AUTHORIZED
```

`2c7e6a4...` 已完成 G8-UAT-02 主体实现，并正确建立 Creation Semantic Audit、typed materialization、concrete Game-local seed、Product Profile / Information IA 与 Runtime JIT Place/NPC 的主要纵向；但 Independent Review 发现 5 个 P1，会破坏 Save/Restore、Narrative Authority、Context Activation 或 Creation 信息边界，因此当前不能进入 Project Owner Stage UAT。

---

## 1. 当前 active 正式来源

### 当前阶段 / UAT

- `G8_StageUAT_交互响应性与空壳世界阻塞发现_v1.1_2026-08-18.md`
- `G8-UAT-01_PlayableRuntimeSeed与NarrativeAuthority收口规格_v1.1_2026-08-18.md`
- `G8_StageUAT_语义物化与活世界阻塞发现_v1.0_2026-08-18.md`
- `G8-UAT-02_GameLocalSemanticMaterialization与活世界收口规格_v1.0_2026-08-18.md`
- `G8-UAT-02_IndependentReview_阻塞发现_v1.0_2026-08-18.md`
- `G8阶段收口与G9前置裁定_v1.8_2026-08-18.md`
- `酒馆游戏新版主体重建总路线 v2.0.md`

### 编号核心

- `11_酒馆游戏_G5长期连续性与玩家授权收口裁定_v1.0_2026-08-15.md`
- `12_酒馆游戏_G6_SaveContinueRestore收口裁定_v1.0_2026-08-15.md`
- `13_酒馆游戏_G7_CrashResumeRecovery收口裁定_v1.0_2026-08-16.md`
- `14_酒馆游戏_G8_RuntimeExtensibleUI产品架构裁定_v1.2_2026-08-18.md`
- `15_酒馆游戏_RuntimeContextOrchestration与模块化复杂度控制裁定_v1.2_2026-08-18.md`
- `16_酒馆游戏_RuntimeWorldMaterialization与当局游戏资产演化裁定_v1.1_2026-08-18.md`

### 通用治理

- `项目经验/第二版_SillyTavern_项目构建经验复盘_Obsidian版_2026-08-18.md`
- `Skill/skill/gpt/lifecycle-dev-process/SKILL.md`：current v2.0
- `Skill/skill/gpt/lifecycle-templates/SKILL.md`：current v1.8

---

## 2. 已确认正确并保留的 G8-UAT-02 主干

`2c7e6a4...` 已经真实完成以下上游改造，返修时不整体重做：

1. Creation Field Semantic Audit；
2. configured AI strict Creation Semantic Materializer；
3. Program-side cardinality / ref / placeholder / identity validation；
4. manual / no-key path 不再伪造 `附近的人 / 附近区域 / 随身物品`；
5. concrete multi-role / multi-item / place materialization；
6. Product Player Profile 结构化投影；
7. Information Surface 与 Journal/Event 分离；
8. Runtime Game-local provenance；
9. JIT Place / NPC stable identity 基本纵向；
10. Runtime materialization 已进入 Formal Turn SQLite transaction。

因此当前是 narrow return，不是 G8-UAT-02 推倒重做。

---

## 3. Independent Review P1 blocker cluster

### P1-01｜Save / Restore 必须覆盖 Game-local topology revision

当前动态 Character / Place / Scene / Connection 会改变实体集合，但旧 Canonical Save Snapshot / Restore 仍按当前实体集合做 exact-ref 校验，且旧存档无法语义上删除“存档之后才 materialize”的世界内容。

必须证明：

```text
Save before materialization
→ materialize Tavern/NPC
→ Restore old save
→ later-created topology disappears
```

以及 save-after-materialization restore、branch divergence、provenance ledger 一致性。

### P1-02｜World Materializer 必须 need-gated，不得每 Turn 前置调用

当前 configured Runtime 会在普通输入先调用 World Materializer，再调用 Semantic。必须改为：

```text
Semantic / Router detects unresolved local materialization need
→ only then World Materializer
→ Program validation
→ bounded continuation
```

普通 observe / dialogue / inner / read-only / wait 不需要未知实体时，World Materializer calls 必须为 0；materializer failure 不得阻塞 unrelated Turn。

这与 #15 v1.2 的 `Runtime Relevant != Model Visible` / outcome-gated activation 一致。

### P1-03｜Opening Beat 不得绕过 Narrative Authority

当前 Creation Materializer 可直接生成 `openingBeat.narrative` 并作为 Turn 0 显示；测试自身已经出现 narrative 提到“导师手里的信”，但没有对应 canonical Item。

必须改为 structured Opening Beat semantic plan → Program refs validation → Narrative Provider realization；Turn 0 同样遵守 No Phantom / Player Agency / disclosure / interactable authority。

### P1-04｜Creation private/public semantic class 必须成为真正信息边界

当前 `private_seed` 只是 audit metadata，Materializer 仍收到 flat raw `creatorAuthoredContext`。必须改成 purpose-built / partitioned materialization context，使 private-only source 不得被 publicDescription / opening / Product / Narrative 回显。

### P1-05｜Creation Item 必须有 holder / placement 语义

当前 materialized Item 没有 holder/location，Adapter 把全部 Item 都写给 player。必须支持至少 player / character / scene（或等价现有 Runtime 可表达结构），Program 验证 ref，Inventory 只显示 player-held items。

---

## 4. G8-UAT-01 authority 继续不可回滚

必须继续保持：

- Program Final Outcome authority；
- non-world-changing downgrade fail-closed；
- locationAuthority / interactableAuthority；
- No Phantom Interactable；
- Bounded != Starved；
- Ephemeral NPC Dialogue Freedom；
- Player Agency；
- server-side grounded exactly-five suggestions；
- controlled multi-action；
- G6 Save / Restore；
- G7 Crash / Recovery / idempotency。

新的 Game-local world growth 不能以破坏这些 CLOSED 能力为代价。

---

## 5. G8-UAT-02 返修 Gate

至少新增并通过：

1. save-before-materialization → materialize → restore-old → topology rollback；
2. save-after-materialization → restore → same stable identity；
3. old-save branch → different future materialization；
4. ordinary non-materialization turns → World Materializer call count 0；
5. materializer failure does not poison unrelated turn；
6. structured Opening Beat → authority-safe Turn 0 Narrative；
7. unique private Creation marker → zero public disclosure；
8. player-held / NPC-held / scene-held Item placement；
9. G5/G6/G7/G8/full + typecheck/lint/build/launcher/disclosure/diff-check；
10. targeted Real DeepSeek rerun。

---

## 6. 当前关键路径

```text
G8-UAT-01                       PASS / CLOSED
Stage UAT rerun                 FAIL
G8-UAT-02 implementation        REVIEWED / RETURN REQUIRED
↓
G8-UAT-02 narrow return fix
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

## 7. G9 DAG 传播

G8-UAT-02 继续只证明第 16 号裁定的最小 internal production vertical；G9-02 后续负责 Source Asset bind / full Game-local layer，并与 #15 v1.2 Context Orchestration 汇合。

```text
G9-01 Compatibility Audit
→ G9-02 Runtime Asset Binding
      + Context Orchestration
      + full Game-local Asset Layer
      + World Materialization Foundation
→ G9-03 asset-spec vNext
→ G9-04 Adapter / Compiler
→ G9-05 Creator
```

Wave 4 继续冻结：

```text
Hard Dependency != Transitive Prompt Inclusion
Runtime Relevant != Model Visible
Formal upstream Outcome / Trigger → downstream continuation activation
```

---

## 8. 当前 Next

> **G8-UAT-02 narrow return fix at/after `2c7e6a4...`，随后重新 Independent Review。**

当前不授权 Project Owner Stage UAT；用户无需继续测试该实现。