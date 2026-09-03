---
title: my world｜Real Provider Validation Standing Authorization
status: current-canonical-decision
created: 2026-09-03
updated: 2026-09-03
scope: implementation / validation / evidence tasks
owner: OWNER
---

# Real Provider Validation Standing Authorization

## 1. Owner decision

Owner has granted standing authorization for **bounded real Provider validation that is explicitly required by an approved my world Task Packet**.

An execution Agent MUST NOT pause mid-task to ask the Owner again merely because the required evidence sends task-owned or normal game-context material to the currently selected model Provider.

This decision exists to remove repeated permission interruptions from routine engineering reality tests.

## 2. Pre-authorized conditions

A real Provider call is pre-authorized when all of the following are true:

1. the current approved Task Packet explicitly requires real Provider / real model evidence;
2. the call uses the current selected Runtime Model Settings profile, unless the Task Packet explicitly freezes another already-approved profile;
3. the test is bounded by the Task Packet, including a concrete call/turn/attempt ceiling or an equivalently narrow scenario;
4. the call uses task-owned roots/fixtures or content that the product would normally send through its existing Provider path;
5. no credential, `.env.local` content, hidden reasoning, secret token or unrelated private local material is intentionally placed in the prompt/evidence;
6. the Agent does not change Provider/model settings merely to make validation pass;
7. the Agent does not add cross-provider fallback, hidden model switching or loop-until-pass behavior.

When these conditions hold:

```text
Task requires real Provider proof
+ bounded scenario already specified
+ current selected Provider/profile
→ AUTHORIZED
→ execute without asking Owner again
```

## 3. Provider outage / timeout does not block code review

Real Provider evidence and reviewability are distinct gates.

If the Agent executes the Task Packet's bounded authorized attempts and the current Provider is unavailable, times out, or otherwise fails **before the feature-specific real vertical can be exercised**, the Agent must not fabricate evidence, switch Provider, add hidden fallback, or exceed the task ceiling merely to obtain a PASS.

However, external Provider unavailability by itself MUST NOT force already-completed implementation and green offline/integration evidence to remain uncommitted or unreviewable.

When all task-required offline/in-process/integration gates are green and the bounded real attempts have been honestly exhausted because of external Provider availability:

```text
implementation + offline/integration evidence complete
+ bounded real Provider attempts exhausted
+ failure is external Provider availability / timeout
→ commit and push reviewable implementation + evidence
→ mark real Provider vertical as PENDING / EXTERNAL PROVIDER UNAVAILABLE
→ return READY FOR INDEPENDENT REVIEW — REAL PROVIDER PROOF PENDING
```

GPT Independent Review may then inspect and accept or reject the engineering implementation on actual code/evidence. It must not falsely claim that the missing real Provider vertical passed.

A later product/reality gate that genuinely depends on real Provider behavior remains pending until a successful real run or Owner UAT supplies that evidence.

This distinction is deliberate:

> **External Provider availability may block reality proof; it does not block code review.**

## 4. What this authorization does NOT allow

Standing authorization is not permission for arbitrary external activity.

A new Owner confirmation is still required before an Agent expands beyond the approved Task Packet into materially different external behavior, including for example:

- open-ended, repeated or high-volume benchmark loops not already approved;
- changing Provider/model/profile/context settings for the validation solely to improve results;
- purchasing/upgrading plans or otherwise changing billing/account state;
- sending credentials, secrets, unrelated personal files/data or hidden chain-of-thought;
- using a new external service/provider that is not part of the approved runtime path;
- any unrelated irreversible external side effect.

If a task merely says real Provider evidence is optional, this authorization does not make the call mandatory. It removes a permission gate; it does not create new work.

## 5. Task Packet requirement going forward

Every future my world Task Packet that needs real Provider evidence should state:

```text
REAL PROVIDER AUTHORIZATION
Owner standing authorization applies under:
Vibe-Coding/my world/architecture/foundation/REAL_PROVIDER_VALIDATION_STANDING_AUTHORIZATION.md
Do not pause for a second Owner confirmation when the bounded required validation remains inside that decision.
```

The packet should also specify:

- the smallest reasonable call/turn/attempt ceiling;
- what counts as external Provider outage/timeout;
- whether missing real evidence blocks only Product/Reality acceptance or also a specific engineering gate;
- that exhausted bounded external failures do not prohibit commit/push/Independent Review unless the task has an explicit, architecture-justified exception.

## 6. Current G5-01M1 application

G5-01M1's task-owned Han / 208 Red Cliffs / Liu Bei / Expansion none real-selected-Provider proof was covered by this authorization.

Codex used the permitted two real Kimi K3 requests. Both timed out during the ordinary Narrative stage after 420 seconds, before any accepted Narrative, semantic-analysis request, or World mutation could occur. No fallback and no third attempt were used.

Therefore the real feature vertical remains **PENDING / EXTERNAL PROVIDER UNAVAILABLE**, but the already-green implementation/offline/integration work must be committed and pushed for GPT Independent Review rather than held indefinitely in a local worktree.