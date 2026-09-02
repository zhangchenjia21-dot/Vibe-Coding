---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-09-02
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G4-11 Two Primary Asset Families Reality Test
current_execution_task: G4-11P1 Two-Family Reality Prep / Engineering Proof
semantic_owner: GPT
current_execution_owner: CODEX
---

# my world｜GPT CONTEXT HANDOFF CURRENT

> 接管导航 / 最小充分摘要。新 GPT 必须先刷新两个 GitHub `main` HEAD。

## 1. Current state

```text
G4-07 First Playable A                PASS / CLOSED
G4-08 Expansion Pack v0.1             PASS / CLOSED
G4-09 First Playable B                PASS / CLOSED
G4-09UATB Owner Product UAT           PASS / CLOSED
G4-10 Runtime Asset Resolution        DEFERRED / MOVED TO G6
G4-10M1 Mechanism                     SUPERSEDED / DO NOT EXECUTE
G4-11 Two Primary Asset Families      ACTIVE
G4-11P1 Engineering Reality Prep      ACTIVE — CODEX
G4-11UAT Owner Reality Test           NOT YET
G4-GATE                               NOT YET
```

Do not start G5 before G4-11P1 Independent Review + G4-11UAT Owner PASS + G4-GATE.

## 2. Owner visual deferral decision

Owner explicitly decided on 2026-09-02 that portrait / scene / authored-map resources are not currently mature and are not part of the core experience.

Canonical decision:

`my world/architecture/source/G4_VISUAL_ASSET_DEFERRAL_TO_G6_DECISION.md`

Effect:

```text
G4-10 Runtime Asset Resolution   DEFERRED / MOVED TO G6
G4-10M1                          SUPERSEDED / DO NOT EXECUTE
```

Runtime visual asset resolution and exact visual generation are removed from G4-GATE.

This does not block G5 because visual presentation is not World/Character/Event/Knowledge/Location authority.

Protected distinction:

```text
portrait / scene / map image
!= gameplay semantic truth

map image
!= topology / travel / current location / GIS
```

Future G6 must re-audit the actual visual consumer before implementing a resolver or deciding old-Game presentation overrides.

## 3. G4-11 canonical product test

Decision:

`my world/architecture/source/G4_TWO_FAMILY_REALITY_TEST_V0_1_DECISION.md`

Fixed families:

```text
A — historical / low-magic
World:      汉末三国：天下未定
Entry:      208｜赤壁前夕
Player:     刘备
Expansion:  none

B — high-magic / fantasy
World:      埃瑟维亚：诸界余辉
Entry:      t0-1287-ovista
Player:     莉维娅·塞兰
Expansion:  none
```

Engineering real-provider comparison uses the same current selected runtime model profile through the shared adapter. Do not change Owner persisted model preference solely for the test.

No portrait / scene / authored-map requirement exists.

## 4. G4-11P1 current task

Packet:

`my-world/docs/tasks/G4-11P1_TWO_FAMILY_REALITY_PREP_TASK.md`

Owner: **Codex**.

Type: validation / UAT-support. No production behavior change is expected.

Required real vertical for each family:

```text
exact full-fidelity Source
→ Composition
→ Atomic Final Create
→ independent Game / SQLite
→ real Provider Opening
→ >=2 accepted continuation turns
→ Save
→ close/switch
→ reopen / Continue
→ another accepted continuation
```

Also prove:

- A/B Game and Conversation isolation;
- no cross-family Source/Context leakage;
- T0 quarantine remains intact;
- exact semantic Source ancestry / bounded Source-update isolation;
- no visual dependency;
- no G5/G6 scope creep;
- Owner production Source/Games/settings/credentials untouched.

If a blocker requires product runtime changes, Codex must stop and return the blocker instead of fixing it inside P1.

Return ceiling: **READY FOR INDEPENDENT REVIEW**.

## 5. Independent Review focus

After Codex returns, GPT must inspect actual evidence/code and especially verify:

1. both verticals use production seams, not a fake parallel harness;
2. same effective selected model profile is used for both real Provider paths;
3. A/B have distinct Game IDs + SQLite and switch A→B→A correctly;
4. no opposite-family Source identity/content appears in model-visible Context;
5. Save/reopen/Continue restores each Game's own accepted history;
6. Source current update cannot mutate existing Game semantic ancestry;
7. no visual resolver, output-format gate, keyword classifier or premature G5 semantics were added;
8. Owner production surfaces remain unchanged.

Engineering PASS does not prove the two worlds feel different.

## 6. Owner UAT after P1

If P1 passes GPT Independent Review, activate a short `G4-11UAT`.

Owner product question:

> Do the actual Han / 刘备 and Afterglow / 莉维娅 play experiences clearly feel like two different RPG worlds rather than one generic AI chat with swapped names?

Owner is not asked to judge visuals.

If Owner PASS:

```text
G4-11 PASS / CLOSED
→ G4-GATE PASS
→ G4 CLOSED
→ inspect/shape G5 route
```

If Owner FAIL, correct only the concrete Source/Context/Game seam exposed. Do not reintroduce visual work as a substitute for world differentiation.

## 7. Protected runtime principle

```text
Model Freedom First
+
Visible Narrative First
+
Canonical Commit Behind a Turn Finalize Barrier
```

Do not add model-format gates, genre keyword gates, mandatory prose structures or scripted beats merely to force family differentiation.
