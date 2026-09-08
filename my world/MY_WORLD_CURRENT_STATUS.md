---
title: my world｜当前状态
status: current-project-status
version: 17.13
created: 2026-08-26
updated: 2026-09-08
phase: G6 RPG Core Closure + UAT Observability + Internal Dynamic UI
current_task: MW-022 UAT Observability / Debug Mode v0.1
current_owner: Codex
parent_task: G6 Package 1 UAT Observability
semantic_owner: GPT
owner_uat_required: true
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
current_roadmap: MY_WORLD_总体规划路线图_CURRENT.md@v4.4
package0_closure_record: my world/docs/uat/G6_PACKAGE0_OWNER_REUAT_U2.md@v1.2
active_architecture: architecture/observability/G6_UAT_OBSERVABILITY_DEBUG_MODE_V0_1_DECISION.md
active_task_packet: my-world/docs/tasks/MW-022_UAT_OBSERVABILITY_DEBUG_MODE_TASK.md
active_task_branch: mw-022-uat-observability-debug-mode
formal_code_base: d81f5f215360780cc50038ccd3bce7cb4163b866
---

# my world｜CURRENT STATUS

## 1. Stage state

```text
G1 Foundation                               PASS / CLOSED
G2 AI Conversation Spine                    PASS / CLOSED
G3 Persistence / Save / Timeline            PASS / CLOSED
G4 Primary Source Assets & Local Game       PASS / CLOSED
G5 World Semantics & GM Runtime             PRODUCT PASS / CLOSED

G6 RPG Core Closure + UAT Observability + Internal Dynamic UI ACTIVE
```

Current G6 Package state:

```text
Package 0  Correction Train + Owner UAT          PRODUCT PASS / CLOSED
Package 1  UAT Observability / Debug Mode v0.1   CURRENT / CODEX
Package 2  Core Interaction Control              QUEUED
Package 3  Open Threads                          QUEUED
Package 4  System / Public d20                   QUEUED
Package 5  factual Inventory                     QUEUED
Package 6  Internal Dynamic UI Host v0.1         QUEUED / CORE REQUIRED
Package 7  V0 Core Closure Reality Gate          QUEUED
```

## 2. Package 0 — CLOSED

Owner final bounded confirmation:

> **PASS，继续。**

Formal closure evidence:

`my world/docs/uat/G6_PACKAGE0_OWNER_REUAT_U2.md@v1.2`

Final Package-0 verdicts:

```text
MW-018 R1 People                  PRODUCT PASS
MW-015 R1 Important Experiences  PRODUCT PASS
MW-019 R1 Recommendations        PRODUCT PASS
MW-020 Context Budget            ENGINEERING PASS_WITH_NOTES / INTEGRATED
MW-021 Narrative Scroll          PRODUCT PASS
```

Final reviewed/integrated implementation artifact at closure:

`d81f5f215360780cc50038ccd3bce7cb4163b866`

These closed outcomes are not replayed merely because later Packages touch adjacent UI. Reopen only on concrete new regression evidence.

## 3. CURRENT — MW-022 UAT Observability / Debug Mode v0.1

Frozen architecture:

`architecture/observability/G6_UAT_OBSERVABILITY_DEBUG_MODE_V0_1_DECISION.md`

Task Packet:

`my-world/docs/tasks/MW-022_UAT_OBSERVABILITY_DEBUG_MODE_TASK.md`

Task branch:

`mw-022-uat-observability-debug-mode`

Formal Code Base:

`d81f5f215360780cc50038ccd3bce7cb4163b866`

Product target:

```text
Debug OFF
→ normal Game has no diagnostic panel and behaves normally

Debug ON
→ bounded read-only current-session UAT panel
→ per accepted Turn accumulate real terminal evidence
→ Narrative / World / Identity / Character / Experiences / People / Recommendations
→ changed / no-change / failure / stale / cancelled distinctions
→ Save / Restore / currentness key outcomes
→ understandable sanitized failure reasons
```

Initial owner-facing purpose:

> After a real turn, Owner should not need to inspect logs or SQLite to know whether backend systems actually changed or failed.

## 4. Protected Debug boundaries

- Debug UI is not part of `概览 / 角色 / 重要经历 / 人物 / 事务 / 行囊 / 系统 / 地图 / 存档` taxonomy;
- OFF by default each Game activation; no persisted preference required;
- diagnostic owner is bounded in-memory only; no SQLite/World/Conversation/Save diagnostic storage;
- no extra Provider calls caused by observability or toggling;
- no gameplay/model-input/mechanics/currentness changes;
- no hidden GM/NPC-private semantic text, raw world-change prose, raw Provider payload, credential, model reasoning or unrelated local privacy;
- Character/Experiences/People change evidence comes from player-safe projection;
- World/identity exposes only safe structural counts/terminal evidence;
- Recommendations gains diagnostic terminal distinctions only; strict five `{label,draft}` product contract remains unchanged;
- Restore invalidates/clears old current-session diagnostic trace so displaced-future evidence is not presented as current;
- no giant EventBus / universal telemetry framework / full Consequence Diff / token-cost dashboard.

## 5. Current implementation evidence informing Task Shape

Existing production already provides usable seams:

- Conversation/Narrative lifecycle signals;
- WorldTurn `finished` + `opportunity_terminal` with current turn/status/count evidence;
- InformationCurator `finished` and existing player-safe Character/Experiences/People projections;
- ActionRecommender `changed + snapshot`, with generic `unavailable` diagnostic collapse to be corrected in this task;
- Runtime `restore_completed` and Save/Restore APIs;
- Application Shell owns composition of these current Game-session nodes.

MW-022 must aggregate these existing seams rather than replace them.

## 6. MW-022 gate

Codex highest return state:

`READY FOR INDEPENDENT REVIEW`

Required order:

```text
Codex implementation
→ GPT Independent Review
→ reviewed integration
→ fresh Owner build
→ Owner focused Debug Mode UAT
```

Engineering does not grant Product PASS.

Owner Product PASS direction:

> 一回合之后，我能一眼看出哪些后台域变了、没变或失败了，失败原因能看懂；关闭调试后，游戏本身完全不受影响。

## 7. Next after Package 1

After explicit Owner Product PASS:

```text
Package 2  OOC / GM Guidance + Character-guided Recommendations + accepted-action Character evidence
↓
Package 3  事务 / Open Threads
↓
Package 4  System / Public d20
↓
Package 5  factual Inventory
↓
Package 6  Internal Dynamic UI Host v0.1
↓
Package 7  V0 Core Closure Reality Gate
```

## 8. Retained audit/debt notes

- Development-audit layer-boundary findings remain architecture debt, not a reason to interrupt Core-first flow;
- Application Shell decomposition remains evolutionary as real consumers arrive;
- pre-existing G3-03 Context assertion / known resource-exit diagnostics remain non-blocking baseline evidence unless a new task changes them;
- long-session Context Orchestrator / Structured Output Reliability remain G7, not MW-022.

## 9. Protected project invariants

- Model Freedom First;
- free-form Player natural-language action remains primary;
- `World Truth != actor Knowledge != human-player disclosure`;
- UI/Debug is projection, never second truth;
- Save / Restore / Regenerate currentness remains authoritative;
- tests cover legitimate change and legitimate no-change/hold;
- no generic framework pulled forward solely for convenience.

## 10. Agent routing

```text
GPT
→ semantics / architecture / Task Shape / Independent Review / UAT interpretation

Codex
→ MW-022 implementer

Owner
→ focused Product UAT after reviewed integration and fresh build
```
