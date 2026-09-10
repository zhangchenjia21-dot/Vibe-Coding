---
title: my world｜总体规划路线图
status: current-canonical-roadmap
version: 5.0
created: 2026-08-25
updated: 2026-09-10
current_phase: G6 correction → G7 handoff
current_status_source: MY_WORLD_CURRENT_STATUS.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
supersedes: v4.9
---

# my world｜总体规划路线图 CURRENT

## 0. 路线原则

Owner 冻结：

> **优先保证游戏尽快完成完整游戏闭环。核心开发先做；扩展功能、体验优化、Creator、Reference 与其它外围增强后置。**

> **Internal Dynamic UI 是 V0 核心能力。**

通用方法仍是：Vertical before platform；Consumer before Creator；真实需求 → 最小能力 → Engineering Review → Product evidence。

### 2026-09-10 Owner route override

Package-7 U1 后，Owner 明确要求：

> **“我不想继续测试了，你修吧，修完了直接继续主线等下一次测试”**

因此：

- U1 = `PASS_WITH_NOTES / BOUNDED CORRECTION REQUIRED`；
- MW-032 一次性修 U1-F01～F04；
- MW-032 Engineering PASS/integration 后**不立即 re-UAT**；
- G6 不虚标 Product PASS，而记为 `ENGINEERING CORE COMPLETE / PRODUCT CONFIRMATION DEFERRED`；
- 直接进入 G7 Package 8；
- 下一次集中 Product test 再验证 MW-032 + G7。

此显式 Owner 指令 supersede v4.9 中“必须先立即复验 Package 7 才能推进 G7”的顺序要求，但不取消 Owner 对最终 Product PASS 的唯一裁决权。

## 1. 总体阶段

```text
G1 Foundation & Project Bootstrap                     PASS / CLOSED
G2 AI Conversation Spine                              PASS / CLOSED
G3 Persistent Game / Save / Timeline Foundation      PASS / CLOSED
G4 Primary Source Assets & Local Game Creation        PASS / CLOSED
G5 World Semantics & GM Runtime                       PRODUCT PASS / CLOSED
G6 RPG Core Closure                                   CORRECTION TRAIN CURRENT
G7 Long-session Context & Knowledge Hardening         NEXT AFTER MW-032
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

# G6｜Core-first Package Axis

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

Final Product confirmation remains deferred into later concentrated testing.

## Package 3｜Open Threads / 事务｜ENGINEERING PASS_WITH_NOTES / INTEGRATED

MW-027 established model-curated unresolved-matters snapshot using the existing Information Curator. U1 later found lifecycle/identity gaps now assigned to MW-032.

## Package 4｜System / Public d20｜ENGINEERING PASS_WITH_NOTES / INTEGRATED

MW-028 established real accepted/current Public d20 history with no second mechanics truth and no fake RPG state.

## Package 5｜factual Inventory / 行囊｜ENGINEERING PASS_WITH_NOTES / INTEGRATED

MW-029 established exact factual possession ADD/UPDATE/REMOVE on the existing World semantic lane with Program-owned item identity and foreground grounding. U1 opening bootstrap gap is assigned to MW-032.

## Package 6｜Internal Dynamic UI Host v0.1｜ENGINEERING PASS_WITH_NOTES / INTEGRATED

MW-030 established one bounded internal Host for Character / Experiences / People / Threads / Inventory / System and presentation-only hide/recover for People + Important Experiences.

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

### MW-032｜G6 Reality Gate U1 Correction Train｜CURRENT

Canonical architecture:

`architecture/G6_REALITY_GATE_U1_CORRECTION_TRAIN_V1_0_DECISION.md@v1.0`

Outcome:

```text
accepted Opening gets bounded semantic + information bootstrap
+
People supports stable player-known referents distinct from World actors
+
Recommendations get max-one same-prefix recovery retry
+
Threads get active model review + stable identity + hide/recover
```

Protected:

- no fake opening facts;
- no Player statement → automatic World truth;
- no fame/name allowlists or display-name authority;
- no Quest engine or Program completion heuristics;
- no infinite recommendation retry/fallback choices;
- no second Provider lane for Inventory/People/Threads;
- no G7 platform work smuggled into G6 correction.

After MW-032 Engineering PASS + reviewed integration:

> **G6 = ENGINEERING CORE COMPLETE / PRODUCT CONFIRMATION DEFERRED**

No immediate Owner build/UAT is required.

---

# G7｜Long-session Context & Knowledge Hardening

## Package 8｜Long-session Core｜NEXT AFTER MW-032

Primary outcomes:

- Context Orchestrator;
- Structured Output Reliability for the machine-schema lanes that real gameplay has proven need it;
- working-set / currentness / latency / long-session reality hardening.

Principle:

> `相关 != 当前有效 != 当前有权使用`
>
> `Bounded context != starved context`

Package 8 should learn from real G6 failures rather than build a universal memory platform. It must preserve existing World/Knowledge/disclosure authority and free-form Narrative primacy.

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

Retained engineering debt that MW-032 must not absorb merely because U1 ended:

- exact-baseline G3 Context assertions;
- bounded teardown/resource warnings;
- layer-boundary/Shell decomposition debt;
- general long-session Context Orchestrator and generalized Structured Output reliability work reserved for G7.
