---
title: my world｜总体规划路线图
status: current-canonical-roadmap
version: 5.1
created: 2026-08-25
updated: 2026-09-13
current_phase: G7 Long-session Context & Knowledge Hardening
current_status_source: MY_WORLD_CURRENT_STATUS.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
supersedes: v5.0
---

# my world｜总体规划路线图 CURRENT

## 0. 路线原则

Owner 冻结：

> **优先保证游戏尽快完成完整游戏闭环。核心开发先做；扩展功能、体验优化、Creator、Reference 与其它外围增强后置。**

> **Internal Dynamic UI 是 V0 核心能力。**

通用方法仍是：Vertical before platform；Consumer before Creator；真实需求 → 最小能力 → Engineering Review → Product evidence。

### 2026-09-10 Owner route override｜已执行

Package-7 U1 后，Owner 明确要求：

> **“我不想继续测试了，你修吧，修完了直接继续主线等下一次测试”**

该 route override 已完成传播：

- U1 = `PASS_WITH_NOTES / BOUNDED CORRECTION REQUIRED`；
- MW-032 已一次性修复 U1-F01～F04；
- MW-032 Independent Review = `ENGINEERING PASS_WITH_NOTES`；
- reviewed non-force integration 已完成；
- G6 不虚标 Product PASS，正式进入 `ENGINEERING CORE COMPLETE / PRODUCT CONFIRMATION DEFERRED`；
- 不立即 re-UAT；
- 当前直接进入 G7 Package 8；
- 下一次集中 Product test 再验证 MW-032 + G7。

此显式 Owner 指令继续 supersede v4.9 中“必须先立即复验 Package 7 才能推进 G7”的顺序要求，但不取消 Owner 对最终 Product PASS 的唯一裁决权。

## 1. 总体阶段

```text
G1 Foundation & Project Bootstrap                     PASS / CLOSED
G2 AI Conversation Spine                              PASS / CLOSED
G3 Persistent Game / Save / Timeline Foundation      PASS / CLOSED
G4 Primary Source Assets & Local Game Creation        PASS / CLOSED
G5 World Semantics & GM Runtime                       PRODUCT PASS / CLOSED
G6 RPG Core Closure                                   ENGINEERING CORE COMPLETE / PRODUCT CONFIRMATION DEFERRED
G7 Long-session Context & Knowledge Hardening         PACKAGE 8 CURRENT
G8 Product Expansion / Authoring / External Contract  QUEUED
G9 Standalone Alpha / Release Validation              QUEUED
```

Current V0 spine:

```text
Launch / New Game / Continue
→ AI GM Narrative
→ Recommendations + unrestricted natural-language action + OOC
→ durable World / actor consequences
→ Character / Important Experiences / People / Open Threads
→ Public d20 / System
→ factual Inventory
→ Internal Dynamic UI + model-curated visibility controls
→ Save / exit / reopen / Restore
→ continue play
```

# G6｜Core-first Package Axis｜ENGINEERING CORE COMPLETE

## Package 0｜Correction Train + Focused Owner UAT｜PRODUCT PASS / CLOSED

Closed:

- MW-018 R1 People;
- MW-015 R1 Important Experiences;
- MW-019 R1 Recommendations;
- MW-020 Context Budget Engineering correction;
- MW-021 Narrative Scroll.

## Package 1｜Debug Mode / UAT Observability｜PRODUCT PASS / CLOSED

MW-022 + MW-023 established bounded read-only diagnostics and >=20px ordinary gameplay readability.

## Package 2｜Core Interaction Control｜ENGINEERING COMPLETE / CORE OUTCOME ACCEPTED

Integrated MW-024/025/026:

- typed `角色行动 / OOC`;
- Character-guided recommended actions;
- Public d20 truth continuity into later GM/OOC;
- compact recommendation UI;
- no internal OOC marker leakage.

Final Product confirmation remains deferred into the later concentrated test.

## Package 3｜Open Threads / 事务｜ENGINEERING PASS_WITH_NOTES / INTEGRATED

MW-027 established model-curated unresolved-matters snapshot using the existing Information Curator. Package-7 U1 lifecycle/identity findings are now corrected by MW-032.

## Package 4｜System / Public d20｜ENGINEERING PASS_WITH_NOTES / INTEGRATED

MW-028 established real accepted/current Public d20 history with no second mechanics truth and no fake RPG state.

## Package 5｜factual Inventory / 行囊｜ENGINEERING PASS_WITH_NOTES / INTEGRATED

MW-029 established exact factual possession ADD/UPDATE/REMOVE on the existing World semantic lane with Program-owned item identity and foreground grounding. Package-7 Opening bootstrap gap is now corrected by MW-032.

## Package 6｜Internal Dynamic UI Host v0.1｜ENGINEERING PASS_WITH_NOTES / INTEGRATED

MW-030 established one bounded internal Host for Character / Experiences / People / Threads / Inventory / System and presentation-only hide/recover. MW-032 extends the proven visibility-preference seam to stable Threads without expanding semantic authority.

Reviewed/integrated baseline entering U1:

`my-world/main@69ac2030b90f4165deb2ecb5302e3743422af585`

## Package 7｜V0 Core Closure Reality Gate U1｜COMPLETE / PASS_WITH_NOTES

Exact tested PCK:

`16a3aeb252eba841fedbf8a4f38d7643912764316e561d5b417b88e89085f8c4`

Formal UAT:

`docs/uat/G6_V0_CORE_CLOSURE_REALITY_GATE_U1.md@v1.2`

Final findings:

`docs/uat/G6_V0_CORE_CLOSURE_REALITY_GATE_U1_FINDINGS.md@v1.1`

U1 found four bounded issues:

```text
F01 Opening semantic/bootstrap gap for Threads / Inventory / supporting People path
F02 People eligibility over-constrained by prior stable actor identity
F03 Recommendations can remain unavailable after one transient/malformed attempt
F04 Threads need active model lifecycle review + stable identity + player hide/recover
```

### MW-032｜G6 Reality Gate U1 Correction Train｜ENGINEERING PASS_WITH_NOTES / REVIEWED INTEGRATION COMPLETE

Canonical architecture:

`architecture/G6_REALITY_GATE_U1_CORRECTION_TRAIN_V1_0_DECISION.md@v1.0`

Reviewed lineage:

```text
Formal Base              69ac2030b90f4165deb2ecb5302e3743422af585
Task Packet              30ca29f90300e876efefd083c4ef50c25d7fa4b8
Implementation           41cf4dfb87f12d9d99b9985e4e0dcf6e7f202b25
Submitted Candidate      dd51c5daef1db6f8a012f05148e49f6912bf6bd4
Independent Review       4ca34ddcb7496945cbd3648183e0d5893433d5f7
Integration Verification e876e217f0220fdc6a577cd0b52143dc8d5b6b5c
```

Integrated outcome:

```text
accepted Opening gets bounded semantic + information bootstrap
+
People supports stable player-known referents distinct from World actors
+
Recommendations get max-one same-prefix recovery retry
+
Threads get active model review + stable identity + hide/recover
```

Independent Engineering evidence:

- focused: `98 / 98`;
- three-size real-window: `484 / 484`;
- direct regression: `43 / 45`, with both failures reproduced on exact Formal Base as retained G3 Context debt;
- Godot 4.7.2 final import PASS;
- fresh Windows export + `ValidateExportOnly` PASS;
- real Provider calls: `0`.

Protected boundaries remain intact:

- no fake Opening facts;
- no Player statement → automatic World truth;
- no fame/name allowlists or display-name authority;
- no Quest engine or Program completion heuristics;
- no infinite recommendation retry/fallback choices;
- no second Provider lane for Inventory/People/Threads;
- no G7 platform work smuggled into G6 correction.

G6 current meaning:

> **ENGINEERING CORE COMPLETE / PRODUCT CONFIRMATION DEFERRED**

There is no immediate Owner build/UAT. The next concentrated Product test will cover the MW-032 corrections together with G7.

---

# G7｜Long-session Context & Knowledge Hardening｜CURRENT

## Package 8｜Long-session Core｜CURRENT / TASK SHAPING

Primary outcomes:

- Context Orchestrator;
- Structured Output Reliability for the machine-schema lanes that real gameplay has proven need it;
- working-set / currentness / latency / long-session reality hardening.

Principle:

> `相关 != 当前有效 != 当前有权使用`
>
> `Bounded context != starved context`

Package 8 must learn from actual G3/G5/G6 evidence rather than build a universal memory platform. It must preserve existing World/Knowledge/disclosure authority, accepted Narrative primacy, free-form Player action, Game/Timeline currentness and domain ownership.

Current execution meaning:

```text
Package route = AUTHORIZED / CURRENT
Executable implementation task = NOT YET DISPATCHED
Current owner = GPT task shaping / architecture audit
Next gate = bounded G7 task architecture + executable Task Packet
```

The two exact-baseline G3 Context assertions retained through MW-032 are direct Package-8 evidence, not a reason to reopen G3 as a separate repair train.

Structured Output Reliability must be pulled only from production machine-schema lanes with demonstrated failure/recovery pressure; do not create a generalized universal model protocol layer merely because several JSON responses exist.

## Package 9｜Knowledge Integrity & Correction Foundation

- Provenance;
- Epistemic Status;
- Turn Freshness;
- Conflicting Evidence;
- Player correction of AI-derived information.

Only after World / Inventory / NPC / Knowledge / Mechanics / Timeline authority and currentness are sufficiently stable may Reality Correction Mode architecture be reopened.

---

# G8｜Product Expansion / Authoring / External Contract

## Package 10｜Information Surface Expansion

- People Shared History;
- Organization / Faction player-known Surface;
- Player-known World Chronicle;
- complete player-facing Consequence Diff.

## Package 11｜Player Utility / Personalization / Archive

- Narrative Preference;
- Bookmark;
- Player Notes;
- readable Adventure Chronicle export;
- Game-local Frozen Manifest.

## Package 12｜Provider / Model Operations

- Narrative / Background model separation;
- AI usage / latency / token visibility;
- Compatibility Preflight;
- Model Profiles;
- richer observability dashboard.

## Package 13｜Source Library / Reference / Creator

```text
Source Library productization + Composition
→ Reference Library
→ conversational Creator
→ Creator Preview Sandbox
→ human-readable Validation / Publish UX
→ only then consider external Declarative UI contract
```

---

# G9｜Standalone Alpha / Release Validation

## Package 14｜Standalone Alpha

- Windows standalone packaging;
- onboarding / credentials / Source setup;
- upgrade / migration / recovery reality tests;
- long-play / corruption / reinstall validation;
- release UAT / defect closure;
- documentation / diagnostics / support boundary.

**G9 Exit:** independent user can install, create a game, play continuously, save/recover and understand failures.

---

## Deferred / Non-scope

Continue to defer multiplayer/cloud-account/server dependency, 3D free movement, full-universe per-NPC tick simulator, universal ECS/giant EventBus, arbitrary external code execution, giant universal Source/UI schema, automatic map generation without evidence, generic Action Intent, external declarative UI before first-party evidence, and Visual Runtime before authored first-party demand.

Retained engineering debt entering G7:

- exact-baseline G3 Context assertions — now evidence for Package 8, not a standalone G3 reopening;
- bounded teardown/resource warnings;
- layer-boundary/Shell decomposition debt;
- live-model semantic People/Thread quality confirmation deferred to the next concentrated Product test.
