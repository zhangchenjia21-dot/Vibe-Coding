---
title: my world｜当前状态
status: current-project-status
version: 18.1
created: 2026-08-26
updated: 2026-09-13
supersedes: 18.0
phase: G7 Long-session Context & Knowledge Hardening — Package 8 Narrative Working Set
current_task: MW-033 G7 Narrative Working-Set Orchestrator v0.1
current_owner: Codex
current_dispatch_state: AUTHORIZED / TASK SHAPED / NO IMPLEMENTATION RETURN YET
parent_task: G7 Package 8 Long-session Core
semantic_owner: GPT
owner_uat_required: deferred until concentrated MW-032 + G7 Product test
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
current_roadmap: MY_WORLD_总体规划路线图_CURRENT.md@v5.1
reviewed_implementation_main: e876e217f0220fdc6a577cd0b52143dc8d5b6b5c
active_architecture: my world/architecture/G7_NARRATIVE_WORKING_SET_ORCHESTRATOR_V0_1_DECISION.md@v1.0
active_task_packet: my-world/docs/tasks/MW-033_G7_NARRATIVE_WORKING_SET_ORCHESTRATOR_V0_1_TASK.md
active_task_branch: mw-033-g7-narrative-working-set
formal_code_base: e876e217f0220fdc6a577cd0b52143dc8d5b6b5c
task_packet_commit: dc6c46d952ba0b63a8f713e9388896969cd71f7d
closed_work_item: MW-032 G6 Reality Gate U1 Correction Train
mw032_integration_verification: e876e217f0220fdc6a577cd0b52143dc8d5b6b5c
active_uat_record: my world/docs/uat/G6_V0_CORE_CLOSURE_REALITY_GATE_U1.md@v1.2
owner_build_pck_sha256_u1: 16a3aeb252eba841fedbf8a4f38d7643912764316e561d5b417b88e89085f8c4
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
G7 Long-session Context & Knowledge         MW-033 CURRENT
G8 Product Expansion / Authoring            QUEUED
G9 Standalone Alpha                         QUEUED
```

## 2. G6 handoff state

MW-032 is reviewed and integrated. The reviewed implementation baseline entering G7 is:

`my-world/main@e876e217f0220fdc6a577cd0b52143dc8d5b6b5c`

G6 remains:

> **ENGINEERING CORE COMPLETE / PRODUCT CONFIRMATION DEFERRED**

Per Owner instruction there is no immediate MW-032 build/re-UAT. The next concentrated Product test will validate the MW-032 corrections together with sufficient G7 long-session work.

## 3. Package-8 evidence audit result

The pre-dispatch G7 audit is complete for the first bounded slice.

Current production Narrative continuation effectively combines:

```text
full frozen Game/T0 World + Character + Guaranteed NPC setup
+ current World materialization / Knowledge / Agency / Evolution
+ factual Inventory
+ Public mechanics
+ literary style reference
+ latest 12 accepted Conversation turns
+ current Player attempt
```

These pieces have local protections, but there is no one request-level owner for cross-domain budget, whole-block selection, inclusion/omission diagnostics, or long-session working-set composition.

At the same time, G6 already maintains durable/current model-curated Character / Important Experiences / People / Open Threads, yet these current summaries are not first-class Narrative continuation material. This creates both context-starvation and context-flooding risk as a Game grows.

The two retained G3 Context failures were also audited. Their original safety goal remains valid—no raw/opaque/stale persisted World/Provider blobs in Context—but their literal assertion now incorrectly forbids any `Current Game Context`, including later reviewed bounded owner-projected Context. MW-033 must repair these tests to the actual authority/currentness boundary, not remove legitimate Game Context.

## 4. Frozen first G7 slice

Architecture:

`architecture/G7_NARRATIVE_WORKING_SET_ORCHESTRATOR_V0_1_DECISION.md@v1.0`

Executable work:

**MW-033｜G7 Narrative Working-Set Orchestrator v0.1**

Product outcome:

> As a Game gets longer, the GM should continue from current protagonist/world/matters/people rather than depending mainly on whatever fits in the last 12 Turns, while avoiding an ever-growing dump of starting and historical data.

The existing `src/context` request-assembly ownership evolves into one Narrative continuation Working-Set Orchestrator. It owns request composition/budget/whole-block selection/diagnostics only; canonical domains still own truth, currentness and disclosure.

## 5. MW-033 authorized architecture

### Consumer scope

Narrative continuation only for v0.1.

First Opening keeps its exact current frozen Game-local setup path. Other model lanes do not migrate into a universal Context platform in this task.

### Required current contributions

Narrative working set may consume only domain-authorized/public contributions:

- durable Game/World/GM source inertia;
- current Information Curation: Character / Important Experiences / People / Open Threads;
- current World/Knowledge/Agency/Evolution projection;
- factual Inventory;
- Public mechanics;
- current accepted Conversation + current Player attempt;
- literary style reference as explicitly non-factual style material.

Context does not read raw `world_state` and then reinterpret semantic truth.

### Budget

Current model settings already expose the authoritative `context_token_ceiling` for supported 256k/1m profiles.

Frozen v0.1 rule:

```text
Narrative safe input byte budget = floor(context_token_ceiling * 0.80)
```

Final UTF-8 Provider-message payload is budgeted conservatively. No Narrative output `max_tokens` cap is introduced. Whole contribution blocks / complete accepted Turns are selected atomically; required P0 overflow fails before Provider start.

### Structural selection

```text
P0 REQUIRED
→ system/protocol + current Player attempt + minimum Game/World instructions

P1 CURRENT CONTINUITY
→ recent accepted Conversation + current Character + current Threads
→ current World/Knowledge/Agency/Evolution + Inventory/mechanics

P2 DURABLE BACKGROUND
→ Important Experiences + current People
→ broader T0/source/NPC background + literary style reference
```

This is structural request policy, not Program semantic scoring. No keywords, importance scores, fame rules, embeddings or similarity search are authorized.

### Currentness

Working set is rebuilt from current owners for every request. It is not durable truth. Restore/Regenerate/reopen must not carry displaced-future Context. UI hide/recover preferences do not change model Context eligibility.

## 6. MW-033 task facts

```text
Formal Code Base
  e876e217f0220fdc6a577cd0b52143dc8d5b6b5c

Branch
  mw-033-g7-narrative-working-set

Required worktree
  D:/AI/Projects/.worktrees/my-world/mw-033-g7-narrative-working-set

Task Packet
  docs/tasks/MW-033_G7_NARRATIVE_WORKING_SET_ORCHESTRATOR_V0_1_TASK.md

Task Packet Commit / Starting HEAD
  dc6c46d952ba0b63a8f713e9388896969cd71f7d
```

`current_owner: Codex` means authorized implementation responsibility only; it is not evidence that a Codex process is currently running.

## 7. Required engineering outcome

Before return MW-033 must prove at minimum:

- one Narrative continuation working-set owner;
- first Opening unchanged;
- 256k/1m model capacity changes working-set budget correctly;
- final request payload remains under safe derived budget;
- required overflow fails loud with zero Provider start;
- lower-tier pressure omits whole blocks/Turns rather than truncating bytes;
- current Character/People/Threads/Experiences can survive beyond recent transcript roll-off;
- People remains player-known information rather than World actor truth;
- UI hide preferences do not alter model Context;
- stale World/Knowledge/Agency/Evolution remains excluded;
- Restore/Regenerate/reopen currentness remains exact;
- the two old G3 Context failures become passing stronger raw/stale-leak tests;
- request-local inclusion/omission/budget diagnostics are available;
- directly affected G2–G6 regressions remain safe;
- Godot 4.7.2 import + fresh Windows export + `ValidateExportOnly` pass.

Codex return ceiling:

> **READY FOR INDEPENDENT REVIEW**

No Product PASS / G7 completion claim is authorized.

## 8. Explicit non-scope

MW-033 must not absorb:

- embeddings / vector DB / semantic similarity retrieval;
- model-generated memory as new truth;
- universal all-agent Context framework;
- Structured Output Reliability middleware;
- generic JSON repair/retry platform;
- Package 9 Provenance/Epistemic/Reality Correction;
- new durable Context table/cache;
- Provider routing/model split redesign;
- Narrative brevity/max-token policy;
- external Source/Expansion/Creator Context contract;
- UI redesign;
- broad World/Knowledge ownership changes.

## 9. Post-MW-033 route

After Codex return:

```text
GPT Independent Review
→ reviewed integration if PASS
→ inspect real production-shaped working-set diagnostics / long-session evidence
→ decide whether the next Context slice truly needs semantic retrieval/source recall
→ separately audit machine-schema failure evidence for Structured Output Reliability
```

Do not pre-commit the next G7 executable Work Item before MW-033 evidence is reviewed.
