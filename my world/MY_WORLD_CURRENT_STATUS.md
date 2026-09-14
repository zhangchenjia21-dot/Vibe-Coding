---
title: my world｜当前状态
status: current-project-status
version: 18.6
created: 2026-08-26
updated: 2026-09-14
supersedes: 18.5
phase: G7 Long-session Context & Knowledge Hardening — Package 8 Public d20 Control Working Set
current_task: MW-035 G7 Public d20 Control Working-Set Currentness v0.1
current_owner: Codex
current_dispatch_state: AUTHORIZED / TASK SHAPED / NO IMPLEMENTATION RETURN YET
parent_task: G7 Package 8 Long-session Core
semantic_owner: GPT
owner_uat_required: deferred until concentrated MW-032 + sufficient G7 Product test
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
current_roadmap: MY_WORLD_总体规划路线图_CURRENT.md@v5.5
reviewed_implementation_main: 0066b587f1d756b55ee18abfa5f473e78a3aeea2
closed_work_item: MW-034 G7 Information Curator Bounded Recovery
structured_output_gate: my world/architecture/G7_STRUCTURED_OUTPUT_RECOVERY_ABSTRACTION_GATE_V1_0_DECISION.md@v1.0
active_architecture: my world/architecture/G7_PUBLIC_D20_CONTROL_WORKING_SET_V0_1_DECISION.md@v0.1
active_task_packet: my-world/docs/tasks/MW-035_G7_PUBLIC_D20_CONTROL_WORKING_SET_V0_1_TASK.md
active_task_branch: mw-035-g7-d20-control-working-set
formal_code_base: 0066b587f1d756b55ee18abfa5f473e78a3aeea2
task_packet_commit: 120062661ad419d52c36fc339cee6f226b09a78d
---

# my world｜CURRENT STATUS

## 1. Stage state

```text
G1 Foundation                               PASS / CLOSED
G2 AI Conversation Spine                    PASS / CLOSED
G3 Persistence / Save / Timeline            PASS / CLOSED
G4 Primary Source Assets & Local Game       PASS / CLOSED
G5 World Semantics & GM Runtime             PRODUCT PASS / CLOSED
G6 RPG Core Closure                         ENGINEERING CORE COMPLETE / PRODUCT CONFIRMATION DEFERRED
G7 Long-session Context & Knowledge         PACKAGE 8 / MW-035 CURRENT
G8 Product Expansion / Authoring            QUEUED
G9 Standalone Alpha                         QUEUED
```

## 2. Current reviewed implementation baseline

`my-world/main@0066b587f1d756b55ee18abfa5f473e78a3aeea2`

This contains reviewed/integrated:

- MW-032 V0 Reality Gate corrections;
- MW-033 Narrative Working-Set Orchestrator v0.1 + R1 source-tier correction;
- MW-034 Information Curator Bounded Recovery.

G6 remains:

> **ENGINEERING CORE COMPLETE / PRODUCT CONFIRMATION DEFERRED**

There is still no immediate MW-032/MW-033/MW-034-only Owner UAT.

## 3. Post-MW-034 recovery abstraction audit — CLOSED

Frozen decision:

`architecture/G7_STRUCTURED_OUTPUT_RECOVERY_ABSTRACTION_GATE_V1_0_DECISION.md@v1.0`

GPT compared the three proven recovery consumers:

```text
Recommendations
Public d20 control
Information Curator
```

Conclusion:

> **Do not extract a generic Structured Output / retry lifecycle framework.**

The visible similarity is superficial.

### Recommendations

- background optional UI lane;
- accepted-prefix opportunity identity;
- foreground invalidates work;
- malformed/timeout/provider failure may recover except known config failures;
- final failure = unavailable;
- no durable semantic write.

### Public d20 control

- foreground action pipeline stage;
- only invalid control parse gets one recovery;
- Provider failure is terminal;
- second parse failure deliberately degrades to ordinary Narrative;
- durable CHECK/NO_CHECK semantics belong to the action pipeline.

### Information Curator

- background durable-state lane;
- Game + initial binding or accepted prefix/parent + Restore epoch identity;
- request serial distinct from logical opportunity;
- malformed/timeout/allowlisted transient Provider failure may recover;
- final failure can later be explicitly repaired;
- successful result durably updates Character/Experiences/People/Threads.

A shared class would therefore require lane policy hooks for currentness, foreground gating, retry classification, request-scoped authority, persistence, degraded behavior and terminal publication. That is not a small policy-free primitive.

Package 8 moves on from retry abstraction.

## 4. Next proven long-session defect

Public d20 Narrative stages already use MW-033 working-set Context.

Public d20 **control/control_recovery** still use:

```text
full Game-local Opening-era projector
+ materialized Expansion rules
+ Inventory
+ historical fixed recent Conversation assembly
```

The full projector retains broad T0 material including:

- Opening supplement;
- selected Entry opening seed;
- World source sections;
- Player Character starting source sections;
- every Guaranteed NPC starting section.

But control does not make current long-session Character or current World/Knowledge/Agency/Evolution first-class inputs.

This creates a concrete long-session risk:

> after a capability or world condition changed many turns ago and its original prose falls outside the old transcript window, mechanics control can still be anchored more strongly to New-Game state than current durable state.

It also repeats large T0 material without the runtime-capacity budget already proven by MW-033.

## 5. MW-035 frozen product outcome

Architecture:

`architecture/G7_PUBLIC_D20_CONTROL_WORKING_SET_V0_1_DECISION.md@v0.1`

Product promise:

> In a long-running Game, Public d20 should judge the current action from the current character, current world, current inventory and current mechanics, while treating large New-Game source material as optional background rather than repeatedly making it the dominant control context.

No new player-facing screen/button is added.

## 6. MW-035 mechanics working-set tiers

### P0 REQUIRED

- control schema/protocol;
- active Player action exactly once;
- exact materialized Expansion mechanics rules;
- minimum Game/World identity + World/GM instructions;
- selected Entry identity;
- control-recovery cue only for attempt 2.

P0 overflow must fail before Provider start.

### P1 CURRENT CONTINUITY

- latest complete accepted Conversation Turn;
- current Character;
- current World / Knowledge / Agency / Evolution;
- factual Inventory;
- current Public mechanics;
- remaining complete accepted Conversation while budget remains.

Current Character is a derived current continuity snapshot, not a second mechanics truth.

### P2 DURABLE STARTING BACKGROUND

- Opening supplement;
- selected Entry opening seed;
- T0 World source sections;
- Player Character source sections;
- Guaranteed NPC source sections.

These are starting reference/inertia, not current lived truth.

`literary_style_reference` is excluded entirely from mechanics control.

## 7. Capacity rule

Use current validated runtime model capacity:

```text
safe control input bytes = floor(context_token_ceiling × 0.80)
```

Expected current values:

- 256k → `209715` bytes;
- 1m → `838860` bytes.

Selection is whole-block / whole-Turn only.

No partial source/NPC section, no partial accepted Turn, no silent Expansion-rule truncation, no hardcoded capacity fallback.

## 8. Existing d20 semantics protected

MW-035 must not change:

- CHECK_REQUIRED / NO_CHECK parser/schema;
- DC/modifier/stance rules;
- RNG timing;
- durable check/NO_CHECK identity;
- same-action replay/dedup;
- narrative accepted-marker recovery;
- System/Public mechanics projection;
- World semantic grounding from durable Mechanical Resolution.

Existing recovery remains:

```text
control parse failure
→ one control_recovery
→ second parse failure
→ degraded ordinary Narrative
```

No Provider-failure retry or timeout retry is added.

All d20 Narrative phases continue to use the existing MW-033 Narrative working-set path.

## 9. Task facts

```text
Formal Code Base
  0066b587f1d756b55ee18abfa5f473e78a3aeea2

Branch
  mw-035-g7-d20-control-working-set

Required worktree
  D:/AI/Projects/.worktrees/my-world/mw-035-g7-d20-control-working-set

Task Packet
  docs/tasks/MW-035_G7_PUBLIC_D20_CONTROL_WORKING_SET_V0_1_TASK.md

Task Packet / Starting HEAD
  120062661ad419d52c36fc339cee6f226b09a78d
```

`current_owner: Codex` means implementation is authorized; it does not prove a Codex process is currently running.

## 10. Required Engineering proof

MW-035 must prove at minimum:

- >12-turn Character currentness survives old transcript eviction;
- current accepted-hash World/Knowledge/Agency/Evolution enters control independently of old prose;
- current Inventory and Public mechanics enter through their owners;
- large T0 source/NPC background cannot crowd current P1 at 256k;
- 1m may admit more whole P2 material;
- final serialized control/control_recovery messages stay within runtime budget;
- P0 overflow produces zero Provider start and zero RNG use;
- no partial Turn/source/NPC/supplement/seed;
- literary-style canary never enters control;
- active Player action appears exactly once;
- Restore/Regenerate displaced material is excluded;
- one malformed control still gets one recovery;
- second malformed still degrades to ordinary Narrative without fake mechanics;
- CHECK and NO_CHECK durable/replay semantics remain unchanged;
- Narrative stages remain MW-033 consumers;
- safe context diagnostics contain no raw/private material;
- Godot 4.7.2 import + fresh Windows export + ValidateExportOnly PASS.

Codex return ceiling:

> **READY FOR INDEPENDENT REVIEW**

No Product PASS / Package-8 completion / generic Context-platform claim is authorized.

## 11. Explicit non-scope

MW-035 does not implement:

- shared Structured Output/retry framework;
- generic all-agent Context platform;
- JSON repair;
- embeddings/vector DB/semantic retrieval;
- NPC/person semantic ranking;
- new stats/attribute/skill-number system;
- Expansion rule DSL;
- Provider/model fallback or routing redesign;
- Narrative output cap;
- UI redesign;
- new persistence schema;
- World/Knowledge ownership rewrite;
- Package-9 provenance/epistemic correction.

## 12. Product validation route

No Owner UAT for MW-035 alone.

After Codex return:

```text
GPT Independent Review
→ reviewed integration if PASS
→ GPT Package-8 sufficiency audit
→ concentrated Owner Product test if G7 evidence is now sufficient
→ only create another G7 engineering task if a concrete blocker remains
```

The later concentrated Product test will validate accumulated MW-032 + MW-033 + MW-034 + sufficient G7 mechanics/currentness behavior together.
