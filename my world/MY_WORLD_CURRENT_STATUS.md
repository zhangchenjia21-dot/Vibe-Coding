---
title: my world｜当前状态
status: current-project-status
version: 12.6
created: 2026-08-26
updated: 2026-09-04
phase: G5 World Semantics & GM Runtime
current_task: G5-03M2 Stable NPC Materialization / Registry Expansion
current_owner: KIMI
parent_task: G5-03 NPC / Faction Agency
semantic_owner: GPT
owner_uat_required: false
context_handoff: handoff/GPT_CONTEXT_HANDOFF_CURRENT.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜CURRENT STATUS

## 1. Stage state

```text
G1 Foundation                               PASS / CLOSED
G2 AI Conversation Spine                    PASS / CLOSED
G3 Persistence / Save / Timeline            PASS / CLOSED
G4 Primary Source Assets & Local Game       PASS / CLOSED
G4-10 Runtime Asset Resolution              DEFERRED / MOVED TO G6

G5 World Semantics & GM Runtime             ACTIVE
G5-01 World Turn / Semantic Materialization PASS / CLOSED
G5-02 Knowledge Provenance                  PASS / CLOSED
G5-03 NPC / Faction Agency                  ACTIVE
G5-03M1 Multi-Actor Agency v0.3             ENGINEERING PASS / CLOSED
G5-03M1R01C02 Dirty Opportunity Consumption PASS / CLOSED
G5-03M1R02 Semantic-Terminal Wake Ownership PASS / CLOSED
G5-03M2 Stable NPC Materialization          ACTIVE — KIMI
G5-04 Event / Priority Evolution            NOT YET
G5-GATE                                     NOT YET
```

## 2. M1 closeout

R02 reviewed implementation:

`d56ff094885c334a791c17429d76a1e21b7fd92d`

R02 PASS restores the frozen v0.3 wake ownership:

```text
ordinary generation_completed
→ mark Agency dirty only
→ semantic worker settles
→ semantic finished wake
→ selector evaluates post-semantic current world
```

M1 now has deterministic/integration Engineering proof for standalone selection, multi-actor concurrent execution, actor-private knowledge, sibling durable commits, foreground/Restore cancellation, replay protection, current-hash filtering, dirty opportunity consumption, and post-semantic wake order.

No extra Owner UAT is inserted at M1 closure. M2 is the first more informative product slice because it expands the actor pool beyond explicit Guaranteed NPCs.

Parent real G5-03 feature proof remains honestly:

`PENDING / EXTERNAL PROVIDER UNAVAILABLE`

Do not rewrite that historical/provider-reality gap as PASS.

## 3. Current M2 decision

Canonical:

`architecture/world/G5_STABLE_NPC_MATERIALIZATION_V0_1_DECISION.md`

Core:

```text
first new-Game creation intent
→ inspect validated Character Source inventory
→ exact_profile for selected exact World+Entry only
→ exclude Player + explicit Guaranteed asset IDs
→ deterministic Game-local stable_npcs snapshot
→ Program-owned local IDs
→ exact provenance + frozen T0 projection
→ creation intent owns the exact initial_setup
```

Important boundaries:

- automatic stable NPCs remain distinct from `guaranteed_npcs`;
- same `creation_id` retry/resume reuses frozen intent and does not rescan Source current;
- old Games missing `stable_npcs` remain valid and are never retrofitted from mutable Source;
- G5-02 actor roster expands to Player + all stable NPCs;
- Agency eligible pool expands to Guaranteed + automatic stable NPCs, Player excluded;
- registry existence grants no knowledge and no automatic action;
- no display-name identity, model-minted identity, SQLite migration, Faction agency, or G5-04.

## 4. Current execution packet

`my-world/docs/tasks/G5-03M2_STABLE_NPC_CREATION_SNAPSHOT_AND_REGISTRY_EXPANSION_TASK.md`

Validation uses the slim policy: M2 focused first, then one affected pass over Final Create, G5-03, G5-02, one relevant G3 Save/Restore suite, and `git diff --check`.

M2 requires **zero real Provider calls**.

## 5. Next

After M2 Independent Review, GPT decides whether a remaining Faction-agency slice is necessary before closing G5-03. Do not start G5-04 early.

## 6. Routing

Through 2026-09-06 23:59 (+08:00): GPT owns architecture/review; Kimi owns code-changing implementation. Correct in-flight Kimi work may finish after expiry.
