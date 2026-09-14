---
title: my world｜当前状态
status: current-project-status
version: 18.5
created: 2026-08-26
updated: 2026-09-14
supersedes: 18.4
phase: G7 Long-session Context & Knowledge Hardening — Package 8 Post-MW-034 Evidence Audit
current_task: G7 Package 8 — Post-MW-034 Evidence / Architecture Audit
current_owner: GPT
current_dispatch_state: MW-034 REVIEWED INTEGRATED / NEXT IMPLEMENTATION NOT YET DISPATCHED
parent_task: G7 Package 8 Long-session Core
semantic_owner: GPT
owner_uat_required: deferred until concentrated MW-032 + sufficient G7 Product test
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
current_roadmap: MY_WORLD_总体规划路线图_CURRENT.md@v5.4
reviewed_implementation_main: 0066b587f1d756b55ee18abfa5f473e78a3aeea2
closed_work_item: MW-034 G7 Information Curator Bounded Recovery
mw034_architecture: my world/architecture/G7_INFORMATION_CURATOR_BOUNDED_RECOVERY_V1_0_DECISION.md@v1.0
mw034_task_branch: mw-034-g7-information-curator-recovery
mw034_formal_base: 651362305a7d4a2872a2da9293b9a2a77e33b7f4
mw034_task_start: 8abed26d60fea2c5cc8bfbad1c60f08bbb707675
mw034_implementation: e5a8464700592ed5c423ca48a8b95c9a53ea3757
mw034_candidate: 2f94fd2a843173370e3d9de01fd804f063067014
mw034_ir1: f0cd73d39c13f39e91d6582a8432a5a9637d2876
mw034_integration_merge: 85f57cb861d2e186c0436a7a7349414d1596c3f0
mw034_integration_verification: 0066b587f1d756b55ee18abfa5f473e78a3aeea2
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
G7 Long-session Context & Knowledge         PACKAGE 8 POST-MW-034 EVIDENCE AUDIT CURRENT
G8 Product Expansion / Authoring            QUEUED
G9 Standalone Alpha                         QUEUED
```

## 2. Current reviewed implementation

Current reviewed implementation main:

`my-world/main@0066b587f1d756b55ee18abfa5f473e78a3aeea2`

This contains the reviewed MW-033 Narrative Working-Set Orchestrator and reviewed MW-034 Information Curator Bounded Recovery.

G6 remains:

> **ENGINEERING CORE COMPLETE / PRODUCT CONFIRMATION DEFERRED**

Per Owner route, there is still no immediate MW-032/MW-033/MW-034-only UAT.

## 3. MW-034 final reviewed state

MW-034 established one bounded automatic recovery for the Information Curator without creating a shared retry framework.

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

Final Engineering verdict:

> **ENGINEERING PASS_WITH_NOTES / REVIEWED INTEGRATION COMPLETE**

No Product PASS is claimed.

## 4. Integrated Curator recovery behavior

For one still-current logical Curator opportunity:

```text
attempt 1
→ if malformed_response / Curator timeout / allowlisted transient Provider failure
→ invalidate serial + detach old callbacks/timer + terminal/cancel old transport
→ deferred currentness check
→ attempt 2 rebuilt from current owners
→ one durable success commit OR final fail-soft
```

The same lifecycle covers:

- initial Character baseline curation;
- lived Opening/action Character;
- Important Experiences;
- People;
- Open Threads.

Maximum automatic Provider starts per logical opportunity = `2`.

`retry_pending()` remains a separate explicit later repair seam and cannot reset the automatic budget while work is still active/pending.

## 5. Currentness / authority guarantees

Integrated guarantees:

- request callbacks and timers are bound to a monotonic request serial;
- attempt-1 late delta/completed/failed/cancelled cannot mutate or terminate attempt 2;
- Restore/shutdown invalidate prior epoch/request work;
- initial recovery remains bound to exact Game + frozen-profile binding + epoch;
- lived recovery remains bound to Game + accepted index + exact prefix + current curation parent + epoch;
- attempt 2 rebuilds bounded current material rather than replaying attempt-1 request objects;
- People / Thread / actor refs are fresh request-local authorities on attempt 2;
- old refs and display-name matching cannot regain write authority;
- malformed attempt-1 raw output is never echoed into the recovery prompt;
- strict Curator parser/schema remains unchanged;
- only a valid, current attempt can reach the existing single durable curation commit;
- Narrative/free-form gameplay never waits for Curator recovery.

## 6. Failure policy now integrated

Automatic recovery is allowed only on attempt 1 for:

- `malformed_response`;
- Curator-owned `timeout`;
- `transport`;
- HTTP `408`, `429`, `500`, `502`, `503`, `504`.

No automatic retry for:

- missing credential/key;
- invalid/unknown/incompatible profile/settings/context/reasoning configuration;
- HTTP `401` / `403`;
- unknown Provider statuses;
- deterministic input/response oversize;
- invalid profile/storage prerequisites;
- stale history / stale parent / changed initial binding;
- Restore / explicit cancel / shutdown;
- persistence failure.

Unknown Provider failures fail closed rather than being guessed transient.

## 7. Engineering evidence

MW-034 focused:

- `441 / 441` checks pass;
- real Provider calls: `0`.

Relevant regressions:

- `49 / 49` existing suites pass;
- MW-033 focused `206 / 206`;
- MW-027 final `157 / 157`;
- three-size real-window smoke `484 / 484`.

Build:

- Godot 4.7.2 final import PASS;
- fresh Windows export PASS;
- `ValidateExportOnly` PASS.

Five existing ObjectDB/resource-at-exit warning suites remain the same bounded warning family with exit 0. No retained failing suite.

## 8. Independent Review notes

Nonblocking notes retained:

1. live Provider curation quality remains unproven because MW-034 deterministic acceptance used zero real Provider calls;
2. Provider stream status `malformed_stream` is not guessed transient and therefore does not auto-retry; this may be revisited only if live evidence warrants it;
3. late-callback safety depends on the current synchronous Provider `cancel()` contract plus request-serial isolation; a future async adapter must revalidate the lifecycle;
4. existing bounded teardown warnings remain unchanged.

## 9. Integration verification

MW-034 was integrated without force/history rewriting.

The integration merge tree was taken from the reviewed task integration-record tip. Comparing that reviewed tip with merge commit `85f57cb861d2e186c0436a7a7349414d1596c3f0` reported zero changed files.

A short documentation-only preparation lineage on `main` remains visible in Git history, but its temporary content is absent from the current tree and introduced no production-code mutation. The post-merge `0066b587…` commit only refines the integration record.

## 10. Current Package-8 gate

Ownership returns to GPT for **post-MW-034 evidence / architecture audit**.

Three real recovery consumers now exist:

```text
Recommendations
Public d20 control
Information Curator
```

Before authorizing another Codex task, GPT must determine whether their shared mechanics are genuinely policy-free or only superficially similar.

Audit dimensions:

- logical opportunity identity/currentness;
- callback and transport isolation;
- retry eligibility;
- second-failure terminal policy;
- request rebuilding / request-scoped authority;
- diagnostics;
- foreground/background gating.

A tiny shared lifecycle primitive may be proposed only if it removes repeated mechanics without flattening lane semantics. There is no automatic authorization for a generic retry base class or Structured Output platform.

The same gate should reassess whether Package 8 next needs another bounded reliability consumer, long-session latency/reality hardening, or live Product evidence.

## 11. Immediate route

```text
MW-034 reviewed integration COMPLETE
→ GPT post-MW-034 Package-8 evidence audit
→ choose next bounded problem only from actual evidence
→ freeze architecture if needed
→ next flat MW-xxx Task Packet only after that gate
→ Codex implementation
→ GPT Independent Review
→ concentrated Owner Product test at the planned risk boundary
```

No next Codex implementation task is currently authorized.