---
title: my world｜当前状态
status: current-project-status
version: 13.0
created: 2026-08-26
updated: 2026-09-04
phase: G5 World Semantics & GM Runtime
current_task: G5-04 Event / Priority-driven World Evolution — architecture shaping
current_owner: GPT
parent_task: G5 World Semantics & GM Runtime
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
G5-03 NPC / Faction Agency                  ENGINEERING PASS / CLOSED
G5-03M1 Multi-Actor Agency v0.3             ENGINEERING PASS / CLOSED
G5-03M2 Stable Actor Registry               ENGINEERING PASS / CLOSED
G5-03M2A Registry Foundation                ENGINEERING PASS / CLOSED
MW-001 Runtime Narrative Actor Materialization PASS / CLOSED
  Capability Anchor                         G5-03
  Legacy Planning Ref                       G5-03M2B
G5-04 Event / Priority Evolution            ARCHITECTURE SHAPING — GPT
G5-GATE                                     NOT YET
```

Parent real G5-03 Provider proof remains honestly:

`PENDING / EXTERNAL PROVIDER UNAVAILABLE`

Do not switch Provider merely to manufacture PASS evidence.

## 2. G5-03 closeout

MW-001 passed GPT actual-code Independent Review IR#1.

Implementation repository evidence:

- `docs/g5_03/G5-03M2A_INDEPENDENT_REVIEW_IR2.md`
- `docs/g5_03/MW-001_INDEPENDENT_REVIEW_IR1.md`
- `docs/g5_03/G5-03_CLOSEOUT.md`

The closed stable actor / Agency capability supports:

```text
Guaranteed Source NPC
+ automatic Source-backed NPC
+ creation-authored Game-local NPC without Card
+ runtime Narrative-materialized NPC without Card
→ Program-owned stable actor registry
→ Knowledge / Agency / Save-Restore consumption
```

Agency remains standalone after semantic terminal, bounded to 0..4 current stable NPCs, actor-scoped, background/best-effort, and subordinate to Player foreground.

## 3. MW-001 review result

MW-001 implementation head: `306fe2e9b69b20dc916014583d924e73b16186c0`  
Evidence head: `7464b7f4d0d87b394dfe59aee84c3bcc9c31374a`

IR#1 verdict: **ENGINEERING PASS / CLOSED**.

Confirmed semantics:

- existing semantic lane only; no extra mandatory Provider call;
- optional independently fail-soft `new_actor_candidates`;
- raw bounded candidate material only;
- model-provided identity/provenance stripped;
- deterministic Program-owned runtime actor IDs;
- exact runtime origin turn/hash currentness;
- actors share the same atomic semantic mutation as changes/knowledge;
- actor-only commit works without fake changes;
- actor-only replay has a durable origin-based idempotence signal;
- regenerate/correction makes stale actors inert without deleting history;
- materialization grants no automatic Knowledge;
- semantic-terminal wake exposes the committed actor to same-turn Agency through the existing Scheduler ordering;
- production Save/Restore/reopen preserves the exact runtime actor record and ID;
- no runtime mutable Source lookup;
- no Scheduler/Cycle, UI, SQLite schema, Public d20, Faction, or G5-04 production changes.

Committed evidence reports focused **53 PASS / 0 FAIL**, affected G5-01/G5-02/G5-03 regressions green, G3-04 PASS, `git diff --check` clean, real Provider calls = 0.

Reviewer environment had no Godot executable and the commit has no external CI statuses, so commands were not independently rerun; actual committed code/diff/test source and production integration seams were inspected directly.

Non-blocking advisory: `actors_dropped` over-reports by one when exactly eight valid unique candidates reach the extraction ceiling. No product authority or acceptance semantics depend on this count; fix opportunistically when the parser is next touched.

## 4. Faction decision

**No separate Faction-agency slice is required for G5-03 closeout now.**

This follows the roadmap's consumer-first rule. Faction identity/agency was explicitly not to be built merely for symmetry, and G5-02 already deferred Faction/shared knowledge. There is still no concrete stable Faction identity, private Faction Knowledge, or Faction action consumer that justifies a new platform seam.

Therefore:

```text
G5-03 closes on proven stable-NPC independent agency
Faction platform work remains deferred
until a real later consumer requires it
```

Most likely pull points are G5-04 pressure/priority world evolution or a later product surface.

This deferral does not mean treating Factions as NPCs, granting shared knowledge, or claiming Faction agency is already implemented.

## 5. Protected G5-03 semantics

Do not reopen absent concrete regression / real new consumer:

- Model Freedom First / Visible Narrative First;
- Narrative acceptance independent of semantic/agency extraction;
- standalone Agency Selector after semantic terminal;
- ordinary accepted turn marks dirty; selector start consumes one opportunity;
- Player foreground invalidates remaining uncommitted background work;
- committed actor actions remain durable;
- 0..4 selector fan-out;
- actor-private material / Knowledge / history;
- Program-owned actor identity; display name never authority;
- no-Card stable actor support;
- runtime-origin accepted-hash currentness;
- no automatic Knowledge from registry/materialization;
- no mutable Source lookup during ordinary runtime;
- Save/Restore preserves stable identity/history;
- no semantic `agency_candidates` selection authority.

## 6. Current next capability — G5-04

G5-04 Event / Priority-driven World Evolution is now current, but implementation is **not yet authorized**.

GPT first owns architecture shaping. The goal is to identify the smallest real pressure / priority / event consumer that lets the world evolve selectively outside direct Player causation without building a universal simulation engine.

Do not prebuild:

- minute-by-minute NPC simulation;
- generic Faction platform;
- universal entity/relationship ontology;
- new UI;
- SQLite schema merely for symmetry;
- Public d20/mechanics redesign.

A new flat executable Work Item ID should be minted only after the G5-04 semantics and first implementation seam are frozen.

## 7. Task identity / lineage

Current governance:

`governance/TASK_IDENTITY_AND_LINEAGE_V1_0.md`

```text
Roadmap / Capability Anchor
!= Executable Work Item ID
!= Revision / Review Lineage
```

MW-001 remains closed under its immutable flat identity. G5-03M2B remains only its legacy planning reference.

## 8. Routing

Through 2026-09-06 23:59 (+08:00): GPT owns architecture/task shaping/review; Kimi owns code-changing implementation. No G5-04 code task exists yet.