---
title: my world｜当前状态
status: current-project-status
version: 13.2
created: 2026-08-26
updated: 2026-09-04
phase: G5 World Semantics & GM Runtime
current_task: MW-002 Selective World Evolution Evaluator — Revision 2 correction
current_owner: KIMI
parent_task: G5-04 Event / Priority-driven World Evolution
semantic_owner: GPT
owner_uat_required: true
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
G5-04 Event / Priority Evolution            ACTIVE
MW-002 Selective World Evolution Evaluator CORRECTION REQUIRED — REVISION 2 / KIMI
G5-GATE                                     NOT YET
```

Current G5-04 canonical decision:

`architecture/world/G5_SELECTIVE_WORLD_EVOLUTION_V0_1_DECISION.md`

Current task packet:

`my-world/docs/tasks/MW-002_SELECTIVE_WORLD_EVOLUTION_EVALUATOR_TASK.md`

Current Independent Review:

`my-world/docs/g5_04/MW-002_INDEPENDENT_REVIEW_IR1.md`

## 2. MW-002 identity / lineage

Revision 1 actual-code review completed on 2026-09-04.

```text
Work Item: MW-002
Name: Selective World Evolution Evaluator
Capability-Anchor: G5-04
Reviewed Revision: 1
Review-Round: IR#1
Verdict: CORRECTION REQUIRED

Current Revision: 2
Current Review-Round metadata: 1
Triggered-By: MW-002 IR#1
Correction Base: d42477c45d4699c91ec0e40124ced374101135b9
Owner: Kimi
Reviewer: GPT
```

Same Outcome correction remains on the same flat Work Item under:

`governance/TASK_IDENTITY_AND_LINEAGE_V1_0.md`

Do not mint MW-002C01/R02/PATCH-style identities.

## 3. Reviewed implementation range

Fresh heads at IR#1:

- `my-world/main`: `d42477c45d4699c91ec0e40124ced374101135b9`
- `Vibe-Coding/main`: `ed768d0e0ad912ff9e8f1bcdb439ad3b767b08c1`

MW-002 Revision-1 implementation/evidence:

- implementation commit: `97a874377bc7b25a8e9b7b7b1c2439928ba3429e`
- evidence commit: `d42477c45d4699c91ec0e40124ced374101135b9`

Review confirmed the scope remained narrow: evaluator/rules/parser/context, minimal Scheduler/Shell/projector integration, focused tests/evidence. No UI, SQLite migration, generic Faction platform, pressure/priority platform, random-event engine, Public-d20 protocol redesign or G5-05 implementation was introduced.

## 4. Revision-1 behavior accepted in principle

IR#1 found substantial correct implementation that Revision 2 must preserve:

- dedicated World Evolution evaluation after Agency opportunity terminal;
- `hold` is valid and creates no fake mutation;
- one-event-per-opportunity ceiling;
- bounded raw-string evaluator parser;
- deterministic Program-owned world-event / mutation / node identities;
- additive `living_world.v0.1`; no SQLite schema/table/migration;
- committed-event replay idempotence;
- current accepted-hash filtering of stale event history;
- `World Evolution Events` as the first real next-GM Context consumer;
- explicit event disclosure boundary: omniscient GM fact, not automatic Player/actor knowledge;
- no automatic G5-02 Knowledge creation;
- no Actor Knowledge Provenance as world-causal evaluator input;
- no mutable Source Library current lookup;
- no fixed cadence, numeric priority, pressure queue, random-event engine or event ontology.

G5-04 canonical semantics remain frozen; IR#1 does **not** reopen architecture shaping.

## 5. IR#1 blockers

### F01 — Public-d20 foreground authority gap

Current normal invalidation is driven by `Conversation.attempt_started`.

In a Game with Public d20 materialized, Player Send first starts the adjudication `control` Provider request; `Conversation.begin_turn()` is deferred until a later narrative stage. During that interval, an already-active World Evolution request may still pass its currentness checks and commit even though the Player has started the next action.

Revision 2 must use the earliest existing Public-d20 action/request observability in Application composition to invalidate both Agency and World Evolution immediately. Prefer the existing adjudication `request_assembled` signal; do not redesign Public d20 mechanics/protocol/UI.

Required production-focused proof: active evolution → d20 control start → immediate invalidation → late evolution callback cannot commit.

### F02 — Agency `already_committed` immediate terminal is not surfaced

`AgencyCycle.start_cycle()` can immediately return `already_committed` when all selected actor actions already exist durably. In Revision 1 the Scheduler does not recognize this as terminal, leaves `agency_cycle_runtime` attached, emits no `opportunity_finished`, and therefore can strand both the current World Evolution wake and later Scheduler opportunities.

Revision 2 must clean that runtime, emit exactly one frozen-turn/hash terminal signal, wake World Evolution once, and remain re-armed for later new dirty work without changing G5-03 scheduling semantics.

### F03 — World-only baseline contains non-World Game setup

Revision-1 `project_world_only()` reuses the general Game block, which includes protagonist control mode and arbitrary `opening_supplement`.

`opening_supplement` is player-authored free text and is not guaranteed to be world-level causal truth. This violates the frozen World-only evaluator input boundary.

Revision 2 must project only frozen World / selected Entry / world instructions / GM instructions / World semantic sections plus a neutral Game-local authority header. It must exclude opening supplement, control mode, Player/Character material and Actor Knowledge.

### F04 — required production Restore-active proof is missing

Revision 1 proves foreground invalidation and generic head drift, and separately proves normal Save/Restore after an event is already committed. It does not prove:

```text
active evaluator
→ production Restore/progress switch
→ evaluator invalidated
→ late callback
→ restored head/world remain authoritative
→ no event commit
```

Revision 2 must add this focused production proof, including protection against any synchronous Agency-invalidated terminal waking a surviving evaluator during Restore.

## 6. Revision-2 scope ceiling

Preferred production edits:

- `src/应用壳.gd`;
- `src/世界回合/L2_流程层/行动代理调度流程.gd`;
- `src/首次开场/L1_器件层/游戏本地开场上下文投影器.gd`;
- `tests/g5_04/选择性世界演化评估测试.gd`;
- evidence/task docs.

Read-only unless concrete evidence proves necessary:

- `src/ui/叙事对话视图.gd`;
- `src/行动判定/L2_流程层/公开D20行动判定流程.gd`;
- `src/世界回合/L2_流程层/行动代理循环流程.gd`.

Do not reopen evaluator record/identity/parser/context architecture. Do not add UI, SQLite schema, Faction platform, pressure database, priority scoring, random events, offline evolution or G5-05 work.

## 7. Revision-2 validation

Focused-first under `tests/g5_04/`.

After focused green run only:

- one directly relevant G5-03 Scheduler/Cycle regression;
- one minimal G4-08/Public-d20 regression because F01 crosses that real integration seam;
- one G4-07 continuation/context regression because the World-only projector is touched;
- one G3-04 Save/Restore regression;
- `git diff --check`;
- real Provider calls = 0.

No broad project matrix without a concrete new reason.

Kimi maximum return:

`READY FOR INDEPENDENT REVIEW`

GPT then performs MW-002 IR#2 against correction base `d42477c45d4699c91ec0e40124ced374101135b9`.

## 8. Review execution limitation

Kimi's Revision-1 evidence reports G5-04 focused **112 PASS / 0 FAIL**, directly affected regressions green, G3-04 PASS, `git diff --check` clean, real Provider calls = 0.

GPT independently inspected the actual committed code/diff/test source/evidence. The reviewer environment had no Godot executable and the reviewed commit had no external CI statuses, so GPT could not independently rerun the Godot commands. The IR#1 blockers are based on direct production-code path inspection, not on the agent's self-report.

## 9. G5-03 closeout remains protected

G5-03 is still ENGINEERING PASS / CLOSED. No generic Faction-agency slice is reopened.

Current Faction deferral:

`architecture/world/G5_03_AGENCY_CLOSEOUT_AND_FACTION_DEFERRAL_V1_0_DECISION.md`

Parent real G5-03 Provider proof remains honestly:

`PENDING / EXTERNAL PROVIDER UNAVAILABLE`

Do not switch Provider merely to manufacture evidence.

## 10. G5-04 Product Definition remains frozen

> **The world must be able to move without direct Player causation, but must not manufacture an event every turn.**

```text
World Independence + Player Spotlight
Persistent != Fully Simulated
Evaluation opportunity != event obligation
```

`hold` remains a first-class correct result.

Frozen order remains:

```text
ordinary accepted Player Narrative
→ semantic lane
→ existing Agency Selector / optional actor cycle
→ Agency opportunity truly terminal
→ World Evolution Evaluator
→ hold OR at most one causally-ready world event
→ optional durable mutation
→ next GM continuation may consume the event
```

Player turn is scheduling opportunity, not event causation. Opening-only GM generation gives no opportunity. No offline/wall-clock evolution.

## 11. Owner UAT remains blocked

G5-04 directly changes core world pacing. Engineering PASS is necessary but insufficient.

Route now:

```text
MW-002 Revision 2 — Kimi
→ GPT IR#2
→ if Engineering PASS: Owner G5-04 UAT
→ only Owner Product PASS may close G5-04
→ then G5-05
```

Owner UAT must still counterpose:

1. a quiet/Life Loop situation where `hold` is naturally correct and no artificial escalation appears;
2. a genuinely ripe world pressure that advances outside direct Player causation and later enters GM Narrative naturally.

## 12. Routing

Through 2026-09-06 23:59 (+08:00): GPT owns semantics/architecture/task shaping/review; Kimi owns code-changing implementation. Correct in-flight Kimi work may finish after expiry.