---
title: my world｜当前状态
status: current-project-status
version: 18.4
created: 2026-08-26
updated: 2026-09-13
supersedes: 18.3
phase: G7 Long-session Context & Knowledge Hardening — Package 8 Information Curator Recovery
current_task: MW-034 G7 Information Curator Bounded Recovery
current_owner: Codex
current_dispatch_state: AUTHORIZED / TASK SHAPED / NO IMPLEMENTATION RETURN YET
parent_task: G7 Package 8 Long-session Core
semantic_owner: GPT
owner_uat_required: deferred until concentrated MW-032 + sufficient G7 Product test
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
current_roadmap: MY_WORLD_总体规划路线图_CURRENT.md@v5.3
reviewed_implementation_main: 651362305a7d4a2872a2da9293b9a2a77e33b7f4
active_architecture: my world/architecture/G7_INFORMATION_CURATOR_BOUNDED_RECOVERY_V1_0_DECISION.md@v1.0
active_task_packet: my-world/docs/tasks/MW-034_G7_INFORMATION_CURATOR_BOUNDED_RECOVERY_TASK.md
active_task_branch: mw-034-g7-information-curator-recovery
formal_code_base: 651362305a7d4a2872a2da9293b9a2a77e33b7f4
task_packet_commit: 8abed26d60fea2c5cc8bfbad1c60f08bbb707675
closed_work_item: MW-033 G7 Narrative Working-Set Orchestrator v0.1
mw033_integration_verification: 651362305a7d4a2872a2da9293b9a2a77e33b7f4
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
G7 Long-session Context & Knowledge         MW-034 CURRENT
G8 Product Expansion / Authoring            QUEUED
G9 Standalone Alpha                         QUEUED
```

## 2. Reviewed baseline entering MW-034

MW-033 Narrative Working-Set Orchestrator v0.1, including R1 P0 source-tier correction, is reviewed and integrated.

Current implementation baseline:

`my-world/main@651362305a7d4a2872a2da9293b9a2a77e33b7f4`

MW-033 final Engineering state:

> **ENGINEERING PASS_WITH_NOTES / REVIEWED INTEGRATION COMPLETE**

G6 Product confirmation remains deferred. Per Owner route there is no immediate MW-032/MW-033-only UAT.

## 3. Post-MW-033 Package-8 audit result

GPT completed the second Package-8 evidence audit across current machine-schema lanes.

The audit did **not** find one uniform retry policy suitable for a universal Structured Output framework.

Current lane semantics:

### Recommendations

- strict five-action schema;
- initial + max one recovery for recoverable malformed/timeout/transient failure;
- permanent configuration failure does not retry;
- final terminal is unavailable.

### Public d20 control

- strict CHECK/NO_CHECK schema;
- one malformed-control recovery;
- second parse failure degrades to ordinary Narrative without inventing mechanics.

### Information Curator

- strict Character/Experiences/People/Threads schema;
- bounded response + 120s timeout + Restore epoch/current prefix/parent checks;
- explicit manual `retry_pending()` exists;
- **no automatic recovery today**;
- malformed/timeout/provider failure can leave the current curation opportunity uncommitted for the runtime.

### World semantic materialization

- factual durable World/Knowledge/Actor/Inventory lane;
- strict core `changes` schema but several optional fields intentionally fail-soft independently;
- separate receipt/currentness semantics;
- no automatic retry currently;
- existing historical structural compatibility behavior differs from Curator/Recommendations.

### World Evolution

- intentionally best-effort;
- source comments explicitly freeze `hold / no event / no automatic retry` on malformed/provider failure.

### Agency

- intentionally best-effort per actor;
- failed actor simply does not commit that cycle;
- no generalized retry contract.

Conclusion:

> **Do not extract a generic Structured Output Reliability layer yet.**

Consumer-before-abstraction remains controlling. The next slice hardens one proven high-value consumer locally, then Package 8 will reassess commonality from three real recovery consumers.

## 4. Why MW-034 is next

Information Curator owns the current player-information views that matter to long-session continuity:

```text
Character
Important Experiences
People
Open Threads
```

Its current strict machine-schema request can fail transiently without blocking Narrative, but a single malformed response/timeout/transient Provider failure can leave these surfaces stale or leave the initial Character baseline unavailable until an explicit repair seam is invoked.

This is a concrete reliability gap, not a reason to build universal middleware.

## 5. MW-034 frozen outcome

Architecture:

`architecture/G7_INFORMATION_CURATOR_BOUNDED_RECOVERY_V1_0_DECISION.md@v1.0`

For one still-current logical Curator opportunity:

```text
initial Provider start
+
max one automatic recovery start
=
maximum 2 Provider starts
```

Applies to:

1. initial Character baseline curation;
2. lived Opening/action Character / Experiences / People / Threads curation.

Recovery-eligible first-attempt failures:

- strict parser malformed/unusable response;
- Curator timeout;
- recognized transient Provider/transport failure.

No automatic retry for:

- permanent credential/settings/profile/config failure;
- input/response oversize;
- invalid profile/storage prerequisites;
- stale history/parent;
- Restore/cancel/shutdown;
- persistence failure after a valid semantic candidate reaches storage.

## 6. Critical implementation boundary

MW-034 must establish request-attempt isolation before retrying.

Required:

- request serial / attempt token or equivalent;
- old attempt callbacks cannot terminate/mutate attempt 2;
- timeout cancellation cannot race into recovery;
- Restore/shutdown invalidates all old attempt callbacks;
- lived recovery uses fresh request-scoped People/Thread refs;
- malformed attempt raw response is not fed into the recovery prompt;
- max two starts, no backoff loop/provider fallback.

Curator parser/schema remains strict. No Markdown fence stripping, regex repair, JSON extraction, fallback cards or schema relaxation is authorized.

## 7. Durable / Product boundaries

Only one successfully parsed/current candidate may reach the existing durable curation commit.

MW-034 must not create:

- duplicate initial/lived records;
- duplicate Experiences;
- duplicate People/Thread identities;
- partial attempt-1 durable state;
- new persistence tables/schema.

Narrative remains authoritative and never waits for Curator success/recovery.

`retry_pending()` remains a distinct explicit later repair seam after automatic recovery is exhausted.

## 8. Task facts

```text
Formal Code Base
  651362305a7d4a2872a2da9293b9a2a77e33b7f4

Branch
  mw-034-g7-information-curator-recovery

Required worktree
  D:/AI/Projects/.worktrees/my-world/mw-034-g7-information-curator-recovery

Task Packet
  docs/tasks/MW-034_G7_INFORMATION_CURATOR_BOUNDED_RECOVERY_TASK.md

Task Packet / Starting HEAD
  8abed26d60fea2c5cc8bfbad1c60f08bbb707675
```

`current_owner: Codex` indicates authorized implementation responsibility only; it does not prove a Codex process is running.

## 9. Required Engineering proof

MW-034 must prove at minimum:

- initial malformed → one recovery → one valid baseline commit;
- lived malformed → one recovery → one valid curation commit;
- timeout → recovery, with late attempt-1 callbacks isolated;
- transient Provider failure → recovery;
- second recoverable failure → final fail-soft / no third start;
- permanent configuration failure → no retry;
- deterministic size failure → no retry;
- explicit cancel/shutdown → no retry;
- Restore invalidates pending/in-flight recovery;
- stale prefix/parent cannot recover/commit;
- recovery request uses fresh request refs and does not reuse malformed raw output;
- later lived opportunity continues after final failed recovery;
- explicit `retry_pending()` remains functional;
- successful recovery is deduped on reopen;
- Information Curation / People / Threads / Initial Character regressions pass;
- Recommendations, d20, World semantic, Agency, Evolution and MW-033 behavior remain unchanged;
- Godot 4.7.2 import + fresh Windows export + `ValidateExportOnly` pass.

Codex return ceiling:

> **READY FOR INDEPENDENT REVIEW**

No Product PASS / Package-8 completion / generic reliability-framework claim is authorized.

## 10. Explicit non-scope

MW-034 must not implement:

- generic Structured Output middleware/base class;
- JSON repair/fence stripping/schema relaxation;
- more than one auto recovery;
- Provider/model fallback;
- World semantic recovery;
- Agency/Evolution recovery;
- Context/source retrieval changes;
- new UI;
- new persistence schema;
- Package-9 provenance/epistemic work.

## 11. Post-MW-034 gate

After Codex returns:

```text
GPT Independent Review
→ reviewed integration if PASS
→ compare Recommendations + d20 control + Information Curator recovery evidence
→ decide whether a tiny shared lifecycle primitive is genuinely policy-free
→ otherwise keep lane-local implementations
```

No Owner UAT is requested for MW-034 alone. Live curation quality remains part of the later concentrated G7 Product test.
