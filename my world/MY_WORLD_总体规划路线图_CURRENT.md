---
title: my world｜总体规划路线图
status: current-canonical-roadmap
version: 5.5
created: 2026-08-25
updated: 2026-09-14
current_phase: G7 Long-session Context & Knowledge Hardening
current_status_source: MY_WORLD_CURRENT_STATUS.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
supersedes: v5.4
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
G7 Long-session Context & Knowledge Hardening         PACKAGE 8 / MW-035 CURRENT
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

### MW-034｜Information Curator Bounded Recovery｜ENGINEERING PASS_WITH_NOTES / REVIEWED INTEGRATION COMPLETE

Architecture:

`architecture/G7_INFORMATION_CURATOR_BOUNDED_RECOVERY_V1_0_DECISION.md@v1.0`

Current reviewed implementation baseline entering MW-035:

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

Engineering evidence:

- focused `441/441`, real Provider calls `0`;
- relevant existing regressions `49/49`;
- MW-033 `206/206`;
- MW-027 final `157/157`;
- real-window `484/484`;
- Godot 4.7.2 import/export/ValidateExportOnly PASS.

No Product PASS is claimed.

### Post-MW-034 Structured Output Recovery abstraction gate｜FROZEN / NO EXTRACTION

Decision:

`architecture/G7_STRUCTURED_OUTPUT_RECOVERY_ABSTRACTION_GATE_V1_0_DECISION.md@v1.0`

Three real recovery consumers were compared directly:

```text
Recommendations
Public d20 control
Information Curator
```

They do **not** share one policy-free lifecycle:

- Recommendations is a background optional UI lane keyed to accepted prefix; foreground invalidates it; final failure = unavailable.
- Public d20 control is one foreground action-pipeline stage; only parse failure gets one recovery; second parse failure degrades to ordinary Narrative; Provider failure is terminal.
- Information Curator is a background durable-state lane keyed to Game + binding/prefix/parent + Restore epoch; malformed/timeout/allowlisted transient failures may recover; final failure may later be explicitly repaired.

Conclusion:

> **Do not extract a generic Structured Output Reliability framework or cross-lane retry base class.**

A shared framework would need policy hooks for opportunity identity, foreground gating, retry classification, request-scoped authority, persistence, degradation and terminal publication. That is not a small semantically neutral primitive.

Package 8 now moves to remaining long-session currentness/latency defects instead of spending another Work Item on retry abstraction.

### MW-035｜Public d20 Control Working-Set Currentness v0.1｜CURRENT / AUTHORIZED

Architecture:

`architecture/G7_PUBLIC_D20_CONTROL_WORKING_SET_V0_1_DECISION.md@v0.1`

Product problem:

Public d20 Narrative stages already use MW-033 working-set assembly, but `control` / `control_recovery` still use the full Opening-era Game-local projector plus the old fixed-window Conversation assembler.

That old control path repeatedly sends broad T0 material while failing to make current long-session Character and current World/Knowledge/Agency/Evolution first-class control inputs.

Product promise:

> In a long-running Game, Public d20 should judge the current action from the current character, current world, current inventory and current mechanics, while treating large New-Game source material as optional background rather than repeatedly making it the dominant control context.

Frozen mechanics-control tiers:

```text
P0 REQUIRED
- control schema / protocol
- active Player action exactly once
- exact materialized Expansion mechanics rules
- minimum Game/World identity + World/GM instructions
- selected Entry identity
- recovery cue only on control_recovery

P1 CURRENT CONTINUITY
- latest accepted Conversation
- current Character
- current World / Knowledge / Agency / Evolution
- factual Inventory
- current Public mechanics
- remaining complete accepted Conversation under budget

P2 DURABLE STARTING BACKGROUND
- Opening supplement
- selected Entry opening seed
- T0 World source sections
- Player Character source sections
- Guaranteed NPC source sections
```

`literary_style_reference` is excluded from mechanics control.

Budget:

```text
safe control input bytes = floor(context_token_ceiling × 0.80)
```

Expected current capacities:

- 256k → `209715` bytes;
- 1m → `838860` bytes.

Whole-block / whole-Turn selection only; P0 overflow fails before Provider start. No hardcoded capacity fallback.

Current Character is a derived continuity snapshot, not a second mechanics truth. Durable d20, factual Inventory and accepted/current World owners remain authoritative.

Existing control recovery remains unchanged:

```text
control parse failure
→ one control_recovery
→ second parse failure
→ degraded ordinary Narrative
```

MW-035 does not add Provider-failure or timeout retry.

Explicitly not authorized:

- generic retry/Structured Output framework;
- all-agent Context platform;
- embeddings/vector DB/semantic retrieval;
- new stat system;
- semantic NPC ranking;
- Provider/model fallback;
- Narrative output caps;
- UI redesign;
- persistence schema changes;
- Package-9 work.

Post-MW-035 gate:

```text
GPT Independent Review
→ reviewed integration if PASS
→ reassess whether Package 8 has enough Engineering coverage for concentrated Owner Product evidence
→ only mint another G7 Work Item if a concrete remaining long-session blocker is proven
```

Owner UAT remains deferred until that gate.

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
