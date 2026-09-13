---
title: my world｜总体规划路线图
status: current-canonical-roadmap
version: 5.2
created: 2026-08-25
updated: 2026-09-13
current_phase: G7 Long-session Context & Knowledge Hardening
current_status_source: MY_WORLD_CURRENT_STATUS.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
supersedes: v5.1
---

# my world｜总体规划路线图 CURRENT

## 0. 路线原则

Owner 冻结：

> **优先保证游戏尽快完成完整游戏闭环。核心开发先做；扩展功能、体验优化、Creator、Reference 与其它外围增强后置。**

> **Internal Dynamic UI 是 V0 核心能力。**

通用方法：Vertical before platform；Consumer before Creator；真实需求 → 最小能力 → Engineering Review → Product evidence。

2026-09-10 Owner route override 继续生效：MW-032 修复后不立即复测，直接推进 G7；下一次集中 Product test 再验证 MW-032 + 足够的 G7 长局能力。Owner 仍是最终 Product PASS 的唯一裁决者。

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

# G6｜RPG Core Closure｜ENGINEERING CORE COMPLETE

## Package 0｜Correction Train + Focused Owner UAT｜PRODUCT PASS / CLOSED

Closed: MW-018 R1 People；MW-015 R1 Important Experiences；MW-019 R1 Recommendations；MW-020 Context Budget Engineering correction；MW-021 Narrative Scroll。

## Package 1｜Debug Mode / UAT Observability｜PRODUCT PASS / CLOSED

MW-022 + MW-023 established bounded read-only diagnostics and >=20px ordinary gameplay readability.

## Package 2｜Core Interaction Control｜ENGINEERING COMPLETE / CORE OUTCOME ACCEPTED

Integrated MW-024/025/026: typed Action/OOC, Character-guided recommendations, Public d20 continuity, compact recommendation UI, no internal OOC leakage.

## Package 3｜Open Threads｜ENGINEERING PASS_WITH_NOTES / INTEGRATED

MW-027 established model-curated unresolved-matters snapshot. U1 lifecycle/identity gaps were corrected by MW-032.

## Package 4｜System / Public d20｜ENGINEERING PASS_WITH_NOTES / INTEGRATED

MW-028 established accepted/current Public d20 history with no second mechanics truth.

## Package 5｜factual Inventory｜ENGINEERING PASS_WITH_NOTES / INTEGRATED

MW-029 established factual possession mutation on the existing World semantic lane. Opening bootstrap gap was corrected by MW-032.

## Package 6｜Internal Dynamic UI Host v0.1｜ENGINEERING PASS_WITH_NOTES / INTEGRATED

MW-030 established one bounded internal Host for Character / Experiences / People / Threads / Inventory / System and presentation-only hide/recover.

## Package 7｜V0 Core Closure Reality Gate U1｜COMPLETE / PASS_WITH_NOTES

U1 found four bounded issues: Opening bootstrap, People referent identity, recommendation recovery, Threads lifecycle/identity/hide. MW-032 corrected all four.

### MW-032｜ENGINEERING PASS_WITH_NOTES / REVIEWED INTEGRATION COMPLETE

Reviewed integration baseline entering G7:

`my-world/main@e876e217f0220fdc6a577cd0b52143dc8d5b6b5c`

G6 remains deliberately:

> **ENGINEERING CORE COMPLETE / PRODUCT CONFIRMATION DEFERRED**

There is no immediate G6-only re-UAT.

---

# G7｜Long-session Context & Knowledge Hardening｜CURRENT

## Package 8｜Long-session Core｜CURRENT

Primary outcomes:

- Context Orchestrator / working-set currentness and boundedness;
- Structured Output Reliability only where proven machine-schema lanes need it;
- latency / long-session reality hardening.

Principles:

> `相关 != 当前有效 != 当前有权使用`
>
> `Bounded context != starved context`

Package 8 must learn from actual G3–G6 evidence. It must not become a universal memory platform, generic agent framework, generalized schema middleware or speculative embedding system merely because those abstractions are possible.

### MW-033｜Narrative Working-Set Orchestrator v0.1｜ENGINEERING PASS_WITH_NOTES / REVIEWED INTEGRATION COMPLETE

Architecture:

`architecture/G7_NARRATIVE_WORKING_SET_ORCHESTRATOR_V0_1_DECISION.md@v1.0`

Reviewed lineage:

```text
Formal Base               e876e217f0220fdc6a577cd0b52143dc8d5b6b5c
Original Task Start       dc6c46d952ba0b63a8f713e9388896969cd71f7d
Original Implementation   d6ebe8ca2ac23ec589ca266c104470888e6daa4f
Original Candidate        32cb5325b251c81ac8d883d5921edababe0d4cbf
IR1                       73253db312c429a62bf610eed10f3a570b20b2d6
R1 Implementation         03a226396e14baf1ee780d9b145a72e148e6187e
R1 Candidate              4bd29c13be6da8e3e8c3b299c76122a1de97ba68
IR2                       80fcd17c26758f0e97ba9f9b6580f878aac13ae1
Integration Verification  651362305a7d4a2872a2da9293b9a2a77e33b7f4
```

Integrated outcome:

```text
canonical owner projections
→ one Narrative continuation working-set owner
→ P0/P1/P2 structural selection
→ validated model-capacity budget
→ whole-block / whole-Turn omission
→ safe request diagnostics
→ final Provider messages
```

Frozen tiers:

```text
P0 REQUIRED
- system/protocol
- current Player attempt/input mode
- minimum Game/World identity + World/GM instructions

P1 CURRENT CONTINUITY
- latest accepted Conversation
- current Character / Open Threads
- current World/Knowledge/Agency/Evolution
- factual Inventory / Public mechanics
- remaining accepted Conversation under budget

P2 DURABLE BACKGROUND
- Important Experiences / player-known People
- T0/source/NPC background
- literary style reference
```

R1 specifically corrected Opening/T0 source classification:

- `opening_supplement` = atomic P2;
- selected Entry `opening_seed` = atomic P2;
- selected Entry identity remains P0;
- First Opening remains unchanged/full.

Engineering evidence:

- focused `206 / 206`;
- relevant regressions `49 / 49`;
- 256k safe budget `209715` bytes;
- 1m safe budget `838860` bytes;
- ~240 KB Chinese supplement/seed omitted whole at 256k and admitted whole at 1m;
- Restore / Regenerate / reopen currentness PASS;
- G3 raw/stale leakage assertions PASS;
- Godot 4.7.2 import/export/ValidateExportOnly PASS;
- real Provider calls `0`.

Current meaning:

> MW-033 proves structural long-session Context correctness, not live long-session Product quality.

### Package-8 next gate｜GPT EVIDENCE / ARCHITECTURE AUDIT CURRENT

No next Codex task is authorized yet.

Required audit before the next flat Work ID:

1. inspect production machine-schema lanes and their actual failure/recovery patterns;
2. distinguish malformed formatting, semantic-invalid structure, transient transport, stale-currentness and permanent configuration failures;
3. determine whether one minimal Structured Output reliability primitive is truly shared or whether lane-local policies should remain separate;
4. inspect MW-033 working-set diagnostics before deciding whether source recall/semantic retrieval is actually needed;
5. select the smallest high-value next consumer/problem, not a platform title.

Do **not** pre-commit vector DB/embeddings, universal memory, generic JSON repair or all-agent Context infrastructure.

## Package 9｜Knowledge Integrity & Correction Foundation

After Package-8 evidence is sufficient:

- Provenance;
- Epistemic Status;
- Turn Freshness;
- Conflicting Evidence;
- Player correction of AI-derived information.

Only after World / Inventory / NPC / Knowledge / Mechanics / Timeline authority/currentness are sufficiently stable may Reality Correction Mode architecture be reopened.

---

# G8｜Product Expansion / Authoring / External Contract

## Package 10｜Information Surface Expansion

People Shared History；Organizations/Factions player-known Surface；World Chronicle；complete player-facing Consequence Diff。

## Package 11｜Player Utility / Personalization / Archive

Narrative Preference；Bookmark；Player Notes；Adventure Chronicle export；Game-local Frozen Manifest。

## Package 12｜Provider / Model Operations

Narrative/Background model separation；AI usage/latency/token visibility；Compatibility Preflight；Model Profiles；richer observability。

## Package 13｜Source Library / Reference / Creator

```text
Source Library productization + Composition
→ Reference Library
→ conversational Creator
→ Creator Preview Sandbox
→ Validation / Publish UX
→ only then consider external Declarative UI contract
```

---

# G9｜Standalone Alpha / Release Validation

## Package 14｜Standalone Alpha

Windows standalone packaging；onboarding/credentials/Source setup；upgrade/migration/recovery reality tests；long-play/corruption/reinstall validation；release UAT/defect closure；documentation/diagnostics/support boundary。

**G9 Exit:** independent user can install, create a game, play continuously, save/recover and understand failures.

---

## Deferred / Non-scope

Continue to defer multiplayer/cloud-account/server dependency, 3D free movement, full-universe per-NPC tick simulator, universal ECS/giant EventBus, arbitrary external code execution, giant universal Source/UI schema, automatic map generation without evidence, generic Action Intent, external declarative UI before first-party evidence, Visual Runtime before authored first-party demand, and semantic retrieval/embeddings until real MW-033 long-session evidence requires them.

Retained Engineering/Product evidence needs entering the next Package-8 slice:

- live long-session GM coherence remains unproven;
- MW-032 deferred Product confirmation remains coupled to the next concentrated G7 test;
- five bounded teardown/resource warning suites remain non-blocking debt;
- structural P2 omission is observable but intentionally non-semantic;
- Structured Output reliability must be derived from actual lane failures rather than a universal middleware assumption.
