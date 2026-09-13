---
title: my world｜当前状态
status: current-project-status
version: 18.2
created: 2026-08-26
updated: 2026-09-13
supersedes: 18.1
phase: G7 Long-session Context & Knowledge Hardening — Package 8 Narrative Working Set R1
current_task: MW-033 R1 P0 Source Tier Correction
current_owner: Codex
current_dispatch_state: IR1 ENGINEERING CORRECTION REQUIRED / R1 AUTHORIZED
parent_task: G7 Package 8 Long-session Core
semantic_owner: GPT
owner_uat_required: deferred until concentrated MW-032 + G7 Product test
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
current_roadmap: MY_WORLD_总体规划路线图_CURRENT.md@v5.1
reviewed_implementation_main: e876e217f0220fdc6a577cd0b52143dc8d5b6b5c
active_architecture: my world/architecture/G7_NARRATIVE_WORKING_SET_ORCHESTRATOR_V0_1_DECISION.md@v1.0
active_task_branch: mw-033-g7-narrative-working-set
formal_code_base: e876e217f0220fdc6a577cd0b52143dc8d5b6b5c
mw033_original_task_packet: my-world/docs/tasks/MW-033_G7_NARRATIVE_WORKING_SET_ORCHESTRATOR_V0_1_TASK.md
mw033_starting_head: dc6c46d952ba0b63a8f713e9388896969cd71f7d
mw033_implementation: d6ebe8ca2ac23ec589ca266c104470888e6daa4f
mw033_submitted_candidate: 32cb5325b251c81ac8d883d5921edababe0d4cbf
mw033_ir1_review: 73253db312c429a62bf610eed10f3a570b20b2d6
active_task_packet: my-world/docs/tasks/MW-033_R1_P0_SOURCE_TIER_CORRECTION_TASK.md
r1_dispatch_branch_tip_at_status: 9697d6398ebbf059a4067205ce935d705ff292c4
closed_work_item: MW-032 G6 Reality Gate U1 Correction Train
mw032_integration_verification: e876e217f0220fdc6a577cd0b52143dc8d5b6b5c
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
G7 Long-session Context & Knowledge         MW-033 R1 CURRENT
G8 Product Expansion / Authoring            QUEUED
G9 Standalone Alpha                         QUEUED
```

## 2. G6 handoff remains unchanged

MW-032 is reviewed and integrated at:

`my-world/main@e876e217f0220fdc6a577cd0b52143dc8d5b6b5c`

G6 remains:

> **ENGINEERING CORE COMPLETE / PRODUCT CONFIRMATION DEFERRED**

Per Owner instruction there is no immediate MW-032 build/re-UAT. The next concentrated Product test will validate MW-032 together with sufficient G7 long-session work.

## 3. MW-033 submitted candidate / IR1 result

Codex submitted:

```text
Starting HEAD       dc6c46d952ba0b63a8f713e9388896969cd71f7d
Implementation      d6ebe8ca2ac23ec589ca266c104470888e6daa4f
Final Candidate     32cb5325b251c81ac8d883d5921edababe0d4cbf
```

Independent Review IR1:

`my-world/docs/mw033/MW-033_INDEPENDENT_REVIEW_IR1.md`

Verdict:

> **ENGINEERING CORRECTION REQUIRED**

The candidate is **not integrated**. `my-world/main` remains exactly `e876e217...`.

IR1 found one blocking architecture mismatch, not a general failure of MW-033.

## 4. IR1 blocking finding｜P0 source tier

The submitted working-set Orchestrator successfully implements the main Package-8 v0.1 design, but its continuation source projector builds required P0 by reusing full Opening helpers.

That accidentally keeps these Opening/T0 bodies inside required P0:

- `game.opening_supplement`;
- selected Entry `opening_seed`.

Frozen architecture requires:

```text
P0 REQUIRED
→ system/protocol
→ current Player attempt/input mode
→ minimum current Game/World identity + World/GM instructions

P2 DURABLE BACKGROUND
→ Experiences / People
→ selected T0/source background
→ NPC authored background
→ literary style reference
```

Therefore Opening supplement/seed must not be capable of causing continuation `required_context_overflow` merely because they were useful when establishing the first scene.

This matters both for long-session salience and capacity:

- stale first-scene material should not permanently outrank current Character/Threads/World continuity;
- a valid large multi-byte Opening supplement/seed may fit the first-Opening character guard but exceed the conservative 256k continuation P0 byte budget;
- the correct v0.1 behavior is to omit such a P2 block atomically, not block the Narrative request.

The original 107/107 focused suite did not cover this because its pressure fixture used `selected_entry_id = null`, no substantial opening supplement, and placed the large payload in a semantic section that was already correctly P2.

## 5. Parts already independently accepted in principle

IR1 found no blocking defect in the rest of the core MW-033 architecture:

- one `src/context` Narrative continuation owner;
- current model 256k/1m capacity metadata;
- `floor(context_token_ceiling × 0.80)` safe input budget;
- final serialized `messages` UTF-8 byte accounting;
- whole-block / whole-Turn omission, including exact-bound +1 behavior;
- current Character / Threads / Experiences / People contribution;
- People remains player-known, not World actor truth;
- World/Knowledge/Agency/Evolution accepted-hash currentness;
- Inventory/mechanics remain domain-owned;
- Restore / Regenerate / reopen rebuild current Context;
- UI hide preferences do not change model Context;
- G3-03/G3-05 now test the real raw/stale-leak boundary and pass;
- first Opening still uses its full frozen setup path;
- d20 Narrative stages use the same Context owner while mechanics control remains separate;
- no embeddings/vector DB/retrieval/universal Memory/Context DB/Package-9 scope expansion.

Submitted evidence also records:

```text
focused                  107 / 107
relevant regressions      49 / 49
real-window               484 / 484
256k final bytes          198976 / 209715
1m final bytes            714501 / 838860
exact optional boundary   209715 / 209715
real Provider calls       0
final import/export       PASS
ValidateExportOnly        PASS
```

These results are useful evidence but do not waive the uncovered P0 classification defect.

## 6. R1 authorized correction

Active Task Packet:

`my-world/docs/tasks/MW-033_R1_P0_SOURCE_TIER_CORRECTION_TASK.md`

Current task branch remains:

`mw-033-g7-narrative-working-set`

At this status update, the remote branch tip containing IR1 + the latest R1 packet is:

`9697d6398ebbf059a4067205ce935d705ff292c4`

Codex must still fetch the remote branch immediately before editing and record the **actual fetched remote tip containing the R1 packet** as R1 Starting HEAD; this protects against any later review/task-packet update.

Required R1 change is intentionally narrow:

1. keep continuation P0 to minimum Game/World identity + required instructions;
2. move non-empty `opening_supplement` into an atomic P2 source block;
3. keep selected Entry identity in P0 but move its `opening_seed` body into atomic P2;
4. preserve small supplement/seed as useful P2 when budget permits;
5. preserve first Opening exact full payload unchanged;
6. add explicit large multi-byte 256k/1m fixtures that would have failed the submitted candidate;
7. rerun MW-033 + directly affected/broad relevant regression/build evidence.

Do not redesign the Orchestrator or add semantic retrieval/ranking.

## 7. Worktree preservation note

The submitted local worktree was not completely clean because an automatic bulk cleanup was rejected before execution.

Committed evidence records preserved Godot import/UID sidecars and fixture `.import` normalization noise. They are not in the submitted candidate product diff and are not an IR1 blocker.

R1 must **not** bulk-clean, delete, normalize, stage or commit those files merely for cosmetic `git status` cleanliness. Unknown dirty product files remain a STOP condition.

Five legacy regression suites also retain disclosed ObjectDB/resource-at-exit warnings with exit code 0 and no failing checks. This remains non-blocking debt.

## 8. Current gate

No reviewed integration is authorized yet.

No Owner build or UAT is requested for this narrow correction.

Current route:

```text
MW-033 R1 narrow P0 source-tier correction
→ Codex READY FOR INDEPENDENT REVIEW
→ GPT Independent Re-review
→ reviewed non-force integration only if corrected candidate passes
→ inspect working-set evidence before deciding the next G7 slice
```

Do not pre-commit the next Package-8 implementation item before MW-033 R1 evidence is reviewed.