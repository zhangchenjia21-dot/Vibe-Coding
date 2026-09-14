---
title: my world｜总体规划路线图
status: current-canonical-roadmap
version: 5.4
created: 2026-08-25
updated: 2026-09-14
current_phase: G7 Long-session Context & Knowledge Hardening
current_status_source: MY_WORLD_CURRENT_STATUS.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
supersedes: v5.3
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
G7 Long-session Context & Knowledge Hardening         PACKAGE 8 POST-MW-034 EVIDENCE AUDIT CURRENT
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

G6 packages 0–7 are Engineering complete. MW-032 corrected Package-7 U1 findings for Opening bootstrap, People referent identity, recommendation recovery and Threads lifecycle/identity/hide.

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

Reviewed/integrated baseline before MW-034:

`my-world/main@651362305a7d4a2872a2da9293b9a2a77e33b7f4`

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

R1 corrected Opening/T0 source classification:

- `opening_supplement` = atomic P2;
- selected Entry `opening_seed` = atomic P2;
- selected Entry identity remains P0;
- First Opening remains unchanged/full.

Engineering evidence: focused `206/206`, relevant regressions `49/49`, Restore/Regenerate/reopen PASS, Godot 4.7.2 import/export/ValidateExportOnly PASS, real Provider calls `0`.

### Post-MW-033 Structured Output audit｜COMPLETE

The audit compared production machine-schema lanes instead of assuming all JSON calls should share one framework.

Observed terminal semantics differed materially across Recommendations, Public d20 control, Information Curator, World semantic, World Evolution and Agency.

Conclusion:

> **A universal Structured Output Reliability framework was not justified.**

Consumer-before-abstraction selected Information Curator as the next concrete reliability consumer.

### MW-034｜Information Curator Bounded Recovery｜ENGINEERING PASS_WITH_NOTES / REVIEWED INTEGRATION COMPLETE

Architecture:

`architecture/G7_INFORMATION_CURATOR_BOUNDED_RECOVERY_V1_0_DECISION.md@v1.0`

Reviewed lineage:

```text
Formal Code Base          651362305a7d4a2872a2da9293b9a2a77e33b7f4
Task Packet / Start       8abed26d60fea2c5cc8bfbad1c60f08bbb707675
Implementation            e5a8464700592ed5c423ca48a8b95c9a53ea3757
Codex Candidate           2f94fd2a843173370e3d9de01fd804f063067014
Independent Review IR1    f0cd73d39c13f39e91d6582a8432a5a9637d2876
Integration Merge         85f57cb861d2e186c0436a7a7349414d1596c3f0
Integration Verification  0066b587f1d756b55ee18abfa5f473e78a3aeea2
```

Current reviewed implementation main:

`my-world/main@0066b587f1d756b55ee18abfa5f473e78a3aeea2`

Integrated outcome:

```text
Information Curator logical opportunity
→ attempt 1
→ only if still-current malformed / Curator timeout / allowlisted transient Provider failure
→ request-isolated max one recovery
→ attempt 2 rebuilt from current owners with fresh request-scoped refs
→ exactly one durable success commit OR final fail-soft
```

Frozen guarantees now integrated:

- both initial Character baseline and lived Character / Experiences / People / Threads curation are covered;
- maximum automatic Provider starts per logical opportunity = `2`;
- strict Curator parser/schema remains unchanged;
- no raw malformed response is echoed into recovery;
- late attempt-1 callbacks are rejected by request serial/currentness isolation;
- timeout disconnects/invalidates old request before deferred recovery;
- People/Thread/actor refs are regenerated per recovery request and old refs have no authority;
- permanent config/credential failures, unknown Provider statuses, deterministic size failures, stale/Restore/cancel/shutdown and persistence failures do not automatically retry;
- `retry_pending()` remains a distinct explicit later repair seam;
- Narrative/free-form gameplay never waits for Curator success.

Engineering evidence:

- MW-034 focused `441/441`, real Provider calls `0`;
- relevant existing regressions `49/49`;
- MW-033 focused `206/206`;
- MW-027 final `157/157`;
- real-window `484/484`;
- Godot 4.7.2 import/export/ValidateExportOnly PASS;
- no retained failing suite.

Independent Review notes remain nonblocking:

1. live Provider curation quality is not proven because deterministic tests used zero real Provider calls;
2. Provider `malformed_stream` is deliberately fail-closed rather than guessed transient;
3. late-callback safety must be revalidated if a future Provider adapter changes the current synchronous cancel contract;
4. existing bounded ObjectDB/resource-at-exit warnings remain unchanged.

No Product PASS is claimed.

### Post-MW-034 Package-8 gate｜GPT EVIDENCE / ARCHITECTURE AUDIT CURRENT

Three real recovery consumers now exist, but this does **not** automatically authorize extraction of a shared framework.

GPT must now compare:

```text
Recommendations recovery
Public d20 control recovery
Information Curator recovery
```

and separate genuinely policy-free lifecycle mechanics from lane-specific semantics:

- opportunity identity/currentness;
- callback/transport isolation;
- recovery eligibility;
- second-failure terminal policy;
- request rebuilding/ref authority;
- diagnostics;
- foreground/background gating.

Only if the common part is both repeated and semantically neutral may a tiny shared primitive be proposed. Do not create a generic retry base class merely because three consumers exist.

The same evidence gate must also reassess whether Package 8 now needs another consumer-specific reliability correction, latency/long-session reality hardening, or live-product evidence before further architecture. No next Codex implementation task is authorized yet.

Owner UAT remains deferred; the next concentrated Product test should cover the accumulated G6 corrections plus sufficient G7 long-session behavior rather than MW-034 alone.

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