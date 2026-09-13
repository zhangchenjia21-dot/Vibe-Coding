---
title: my world｜当前状态
status: current-project-status
version: 18.3
created: 2026-08-26
updated: 2026-09-13
supersedes: 18.2
phase: G7 Long-session Context & Knowledge Hardening — Package 8 Next-slice Evidence Audit
current_task: G7 Package 8 — Post-MW-033 Evidence / Architecture Audit
current_owner: GPT
current_dispatch_state: MW-033 REVIEWED INTEGRATED / NEXT IMPLEMENTATION NOT YET DISPATCHED
parent_task: G7 Package 8 Long-session Core
semantic_owner: GPT
owner_uat_required: deferred until concentrated MW-032 + sufficient G7 Product test
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
current_roadmap: MY_WORLD_总体规划路线图_CURRENT.md@v5.2
reviewed_implementation_main: 651362305a7d4a2872a2da9293b9a2a77e33b7f4
mw033_architecture: my world/architecture/G7_NARRATIVE_WORKING_SET_ORCHESTRATOR_V0_1_DECISION.md@v1.0
mw033_task_branch: mw-033-g7-narrative-working-set
mw033_formal_base: e876e217f0220fdc6a577cd0b52143dc8d5b6b5c
mw033_original_implementation: d6ebe8ca2ac23ec589ca266c104470888e6daa4f
mw033_original_candidate: 32cb5325b251c81ac8d883d5921edababe0d4cbf
mw033_ir1: 73253db312c429a62bf610eed10f3a570b20b2d6
mw033_r1_implementation: 03a226396e14baf1ee780d9b145a72e148e6187e
mw033_r1_candidate: 4bd29c13be6da8e3e8c3b299c76122a1de97ba68
mw033_ir2_review: 80fcd17c26758f0e97ba9f9b6580f878aac13ae1
mw033_integration_verification: 651362305a7d4a2872a2da9293b9a2a77e33b7f4
closed_work_item: MW-033 G7 Narrative Working-Set Orchestrator v0.1
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
G7 Long-session Context & Knowledge         PACKAGE 8 NEXT-SLICE EVIDENCE AUDIT CURRENT
G8 Product Expansion / Authoring            QUEUED
G9 Standalone Alpha                         QUEUED
```

## 2. G6 Product confirmation remains deferred

MW-032 is reviewed/integrated and G6 remains:

> **ENGINEERING CORE COMPLETE / PRODUCT CONFIRMATION DEFERRED**

Per Owner instruction there is still no immediate MW-032-only UAT. The next concentrated Product test will validate the deferred G6 corrections together with sufficient G7 long-session behavior.

## 3. MW-033 final reviewed state

MW-033 established **Narrative Working-Set Orchestrator v0.1** for ordinary post-Opening continuation.

Reviewed lineage:

```text
Formal Base                e876e217f0220fdc6a577cd0b52143dc8d5b6b5c
Original Task Start        dc6c46d952ba0b63a8f713e9388896969cd71f7d
Original Implementation    d6ebe8ca2ac23ec589ca266c104470888e6daa4f
Original Candidate         32cb5325b251c81ac8d883d5921edababe0d4cbf
IR1                        73253db312c429a62bf610eed10f3a570b20b2d6
R1 Implementation          03a226396e14baf1ee780d9b145a72e148e6187e
R1 Candidate               4bd29c13be6da8e3e8c3b299c76122a1de97ba68
Independent Re-review IR2  80fcd17c26758f0e97ba9f9b6580f878aac13ae1
Integration Verification   651362305a7d4a2872a2da9293b9a2a77e33b7f4
```

Final Engineering verdict:

> **ENGINEERING PASS_WITH_NOTES / REVIEWED INTEGRATION COMPLETE**

`my-world/main` now contains the full reviewed MW-033 + R1 lineage.

## 4. Integrated Narrative working-set behavior

Ordinary continuation now derives one request-local working set from current canonical owners instead of blindly concatenating full T0 material plus a fixed recent-12 transcript.

Frozen structural tiers:

```text
P0 REQUIRED
→ system/protocol
→ current Player attempt/input mode
→ minimum Game/World identity + World/GM instructions

P1 CURRENT CONTINUITY
→ latest accepted Conversation
→ current Character / Open Threads
→ current World/Knowledge/Agency/Evolution
→ factual Inventory / Public mechanics
→ remaining recent accepted Conversation while budget remains

P2 DURABLE BACKGROUND
→ Important Experiences / player-known People
→ T0/source/NPC authored background
→ literary style reference
```

Context owns request composition/budget/whole-block selection/diagnostics only. It does not become a second World, Knowledge, People, Threads, Inventory, Mechanics or Timeline authority.

## 5. Budget / currentness guarantees now integrated

Current validated model settings provide context capacity.

Frozen v0.1 rule:

```text
safe Narrative input bytes = floor(context_token_ceiling × 0.80)
```

Integrated evidence proves:

- 256k → `209715` safe input bytes;
- 1m → `838860` safe input bytes;
- exact final serialized `messages` bytes are counted;
- optional whole block at exact budget fits;
- +1 byte omits the optional whole block;
- P0 overflow fails before Narrative Provider start;
- selected accepted Turns remain whole and chronological;
- current Player attempt appears exactly once;
- no Narrative output `max_tokens` cap was added.

Restore / Regenerate / reopen rebuild from current canonical owners; final Provider messages are not durable truth or a persisted Context cache.

## 6. R1 correction now integrated

IR1 found one narrow source-tier defect: `opening_supplement` and selected Entry `opening_seed` were incorrectly inherited into required P0.

R1 corrected this:

- minimum Game / selected Entry identity / World identity / World instructions / GM instructions remain P0;
- `opening_supplement` is an atomic P2 source block;
- selected Entry `opening_seed` is an atomic P2 source block;
- 256k can omit a ~240 KB Chinese background body while keeping current Character/Thread/World and continuing normally;
- 1m can admit the same body whole;
- small supplement/seed remain available when budget permits;
- First Opening remains unchanged and receives the full frozen initial payload.

## 7. Final Engineering evidence

MW-033 R1 focused:

- `206 / 206` checks pass;
- real Provider calls: `0`.

Relevant regression manifest:

- `49 / 49` suites pass;
- G3-03 / G3-05 stronger raw/stale Context leakage checks pass;
- G4 First Opening / continuation pass;
- Public d20 CHECK / NO_CHECK / degraded Narrative pass;
- World/Knowledge/Agency/Evolution currentness pass;
- MW-032 passes.

Final build:

- Godot 4.7.2 import PASS;
- fresh Windows export PASS;
- `ValidateExportOnly` PASS;
- reviewed R1 PCK SHA256 `ac6bf6b7d6625a16b84683031b8605141ad16e95b81e68240c04ffd2ec1363f3`.

Five existing regression suites retain the same bounded ObjectDB/resource-at-exit warning family. Preserved Godot import/UID sidecars remain local worktree noise and were not bulk-cleaned.

## 8. What MW-033 does NOT prove

MW-033 Engineering evidence is deterministic and used **0 real Provider calls**.

It proves:

- ownership;
- budget accounting;
- structural selection;
- currentness;
- Restore/Regenerate/reopen isolation;
- disclosure boundaries;
- build operability.

It does **not** yet prove:

- that a live GM feels coherent after many hours;
- that structural P2 omission always retains the most narratively useful optional background;
- that semantic retrieval/embeddings are needed;
- that every structured-output lane should share one reliability platform.

Those questions require the next evidence gate, not speculative platform work.

## 9. Current Package-8 gate

Current owner returns to GPT for **post-MW-033 evidence / architecture audit**.

Before dispatching the next Codex work item:

1. inspect the actual failure/recovery patterns of machine-schema lanes already present in G4–G6;
2. distinguish Provider formatting failures, transport failures, semantic-invalid JSON, stale-currentness failures and permanent configuration failures;
3. identify whether there is a real minimal shared Structured Output Reliability primitive, or whether lane-specific policies should remain separate;
4. inspect MW-033 diagnostics to decide whether the next Context slice needs source recall/retrieval at all;
5. prefer the smallest high-value next consumer slice; do not create a universal memory or JSON middleware platform by default;
6. mint the next flat Work ID only after architecture/evidence review.

No next Codex implementation task is currently authorized.

## 10. Immediate route

```text
MW-033 reviewed integration COMPLETE
→ GPT Package-8 evidence audit
→ choose next bounded consumer/problem
→ freeze narrow architecture if needed
→ next flat MW-xxx Task Packet
→ Codex implementation
→ GPT Independent Review
→ concentrated Owner Product test only at the planned risk boundary
```
