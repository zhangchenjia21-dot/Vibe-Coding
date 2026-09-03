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

## 3. What this authorization does NOT allow

Standing authorization is not permission for arbitrary external activity.

A new Owner confirmation is still required before an Agent expands beyond the approved Task Packet into materially different external behavior, including for example:

- open-ended, repeated or high-volume benchmark loops not already approved;
- changing Provider/model/profile/context settings for the validation solely to improve results;
- purchasing/upgrading plans or otherwise changing billing/account state;
- sending credentials, secrets, unrelated personal files/data or hidden chain-of-thought;
- using a new external service/provider that is not part of the approved runtime path;
- any unrelated irreversible external side effect.

If a task merely says real Provider evidence is optional, this authorization does not make the call mandatory. It removes a permission gate; it does not create new work.

## 4. Task Packet requirement going forward

Every future my world Task Packet that needs real Provider evidence should state:

```text
REAL PROVIDER AUTHORIZATION
Owner standing authorization applies under:
Vibe-Coding/my world/architecture/foundation/REAL_PROVIDER_VALIDATION_STANDING_AUTHORIZATION.md
Do not pause for a second Owner confirmation when the bounded required validation remains inside that decision.
```

The packet should also specify the smallest reasonable call/turn/attempt ceiling.

## 5. Current G5-01M1 application

G5-01M1's required task-owned Han / 208 Red Cliffs / Liu Bei / Expansion none real-selected-Provider proof is covered by this authorization, including the packet's existing maximum of one initial natural persistent-consequence turn plus at most one additional turn if the first legitimately yields no durable consequence.

Codex should continue from its already-green offline state and execute the required bounded real Provider validation without requesting Owner confirmation again.
