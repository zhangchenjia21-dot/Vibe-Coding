---
title: my world｜总体规划路线图
status: current-canonical-roadmap
version: 5.3
created: 2026-08-25
updated: 2026-09-13
current_phase: G7 Long-session Context & Knowledge Hardening
current_status_source: MY_WORLD_CURRENT_STATUS.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
supersedes: v5.2
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
G7 Long-session Context & Knowledge Hardening         PACKAGE 8 / MW-034 CURRENT
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

Reviewed G6→G7 baseline:

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

Final reviewed/integrated baseline:

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

Observed terminal semantics:

```text
Recommendations
→ strict schema
→ one bounded recovery for recoverable malformed/timeout/transient failure
→ final unavailable

Public d20 control
→ strict CHECK/NO_CHECK schema
→ one malformed-control recovery
→ second parse failure degrades to ordinary Narrative

Information Curator
→ strict Character/Experiences/People/Threads schema
→ currently no automatic recovery
→ transient failure can leave current player-information state stale

World semantic
→ factual durable lane with different partial-field fail-soft / receipt semantics
→ no automatic recovery today

World Evolution
→ intentionally best-effort hold/no-event on failure
→ explicitly no automatic retry

Agency
→ intentionally best-effort per actor
→ failed actor simply does not commit that cycle
```

Conclusion:

> **A universal Structured Output Reliability framework is not yet justified.**

Failure classification, currentness and terminal policy still differ materially by lane. Consumer-before-abstraction wins.

### MW-034｜Information Curator Bounded Recovery｜CURRENT / AUTHORIZED

Architecture:

`architecture/G7_INFORMATION_CURATOR_BOUNDED_RECOVERY_V1_0_DECISION.md@v1.0`

Outcome:

```text
Information Curator initial/lived opportunity
→ initial Provider start
→ if still-current recoverable malformed/timeout/transient failure: max one recovery start
→ strict parser unchanged
→ at most one durable curation commit
→ final fail-soft if recovery fails
```

Scope:

- both initial Character baseline and lived Opening/action curation;
- max two Provider starts per logical opportunity;
- request-serial/attempt isolation required so late attempt-1 callbacks cannot kill attempt 2;
- permanent config/credential failures, deterministic size failures, Restore/cancel/shutdown, stale parent/history and persistence failure do not retry;
- recovery prompt contains no malformed raw response/private refs;
- People/Thread request refs are fresh/request-local on recovery;
- `retry_pending()` remains a separate explicit later repair seam;
- Narrative/free-form gameplay never waits for Curator success.

Explicitly not authorized:

- generic retry/Structured Output base class;
- JSON repair/fence stripping/schema relaxation;
- World semantic/Agency/Evolution retry changes;
- Provider/model fallback;
- new persistence schema;
- Context/retrieval changes;
- Package-9 provenance work.

Post-MW-034 gate:

```text
Recommendations + Public d20 control + Information Curator
→ compare three proven recovery consumers
→ extract a tiny shared lifecycle primitive only if common code is genuinely policy-free
→ otherwise keep lane-local implementations
```

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
