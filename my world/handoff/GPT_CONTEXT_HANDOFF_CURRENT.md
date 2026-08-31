---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-08-31
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G4-08 Expansion Pack v0.1 + First Real Runtime Vertical
current_execution_task: G4-08M1 Public d20 Expansion Mechanism
semantic_owner: GPT
current_execution_owner: Codex
---

# my world｜GPT CONTEXT HANDOFF CURRENT

> 接管导航 / 最小充分摘要。新 GPT 必须先刷新两个 GitHub `main` HEAD。

## 1. Current state

```text
G4-06 Atomic Final Create             PASS / CLOSED
G4-07 First Playable A                PASS / CLOSED
G4-07A Opening Runtime                PASS / CLOSED
G4-07B Playable UI Integration        PASS / CLOSED
G4-07UAT01 Owner Launch Freshness     PASS / CLOSED
G4-08 Expansion Pack v0.1             ACTIVE
G4-08S0 Expansion Semantic Freeze     PASS / CLOSED
G4-08M1 Public d20 Mechanism          ACTIVE — CODEX
G4-GATE                               NOT YET
```

---

## 2. Read first

Governance:

1. `my world/MY_WORLD_CURRENT_STATUS.md`
2. `my world/architecture/source/G4_EXPANSION_V0_1_PUBLIC_D20_DECISION.md`
3. `my world/MY_WORLD_架构_CURRENT.md`
4. `my world/MY_WORLD_核心设计原则_CURRENT.md`

Implementation:

5. `AGENTS.md`
6. `docs/tasks/G4-08M1_PUBLIC_D20_EXPANSION_MECHANISM_TASK.md`
7. current Source contract/library layers under `src/source/**`
8. current Composition / Final Create implementation
9. G4-07A/B durable continuation implementation/tests

---

## 3. Semantic freeze

First real Expansion:

```text
Display Name  判定与检定：公开 d20
asset_id      exp.check_core.public_d20
asset_type    expansion_pack
schema        expansion_pack.v0.1
capability    action_check.public_d20.v1
slot          action_resolution
```

Key invariants:

- Expansion optional; no Expansion preserves G4-07 behavior;
- selected as exact immutable generation(s);
- duplicate exact Expansion / same exclusive slot collision fail closed;
- no hidden World-family compatibility guessing;
- three no-roll cases: certain success / certain impossibility / no-cost repeat;
- no natural 1/20 auto override;
- Risk Structure / Check Proposal freezes before RNG;
- model never owns die face / total / outcome;
- no-check normal turn stays one Provider call;
- real check conditionally uses second Provider continuation;
- same action retry/restart never rerolls;
- Expansion owns resolution only, not downstream World consequences;
- Source declares capability binding but contains no executable code.

Historical `the-world` Public d20 was semantic reference only; do not port its Node/plugin architecture.

---

## 4. Current mechanism task

Packet:

`my-world/docs/tasks/G4-08M1_PUBLIC_D20_EXPANSION_MECHANISM_TASK.md`

Formal Code Base:

`3b5cd80a26091a17d61c5a055637a422a9edb3aa`

Owner: **Codex**. Reviewer: **GPT**.

Return ceiling: **READY FOR INDEPENDENT REVIEW**.

M1 implements:

```text
Expansion third Source type
→ Managed Source Library install/current/exact
→ Composition backend 0..N exact expansions
→ exclusive capability-slot compatibility
→ Final Create exact materialization/provenance
→ Program capability registry
→ structured adjudication envelope
→ freeze before RNG
→ Program d20 RNG
→ durable check result
→ retry/restart idempotency
→ real Provider resolution continuation
→ UI-neutral mechanic projection
```

M1 does not own visual UI.

Required real evidence includes Han + Afterglow and no-Expansion control, plus Provider-failure-after-roll no-reroll proof.

---

## 5. After Codex returns

GPT must:

1. refresh both repo `main` heads;
2. inspect actual changed code and evidence, not Codex self-report;
3. verify Source exact-generation semantics and no executable Source code;
4. verify Composition/slot conflict and Final Create exact ancestry;
5. verify proposal freezes before RNG;
6. verify Program owns RNG/total/outcome;
7. verify Provider failure/restart never rerolls same action;
8. verify real DeepSeek Han + Afterglow and no-Expansion regression;
9. check any schema migration preserves prior Games;
10. decide M1 PASS or correction.

If M1 PASS, issue a separate **Kimi G4-08B** UI/integration packet for:

- New Game Expansion selection;
- Review display;
- lightweight mechanic card projected from Program-owned resolution.

Do not add click-to-roll/dice animation in first generation.

---

## 6. Product route after M1

```text
G4-08M1 Codex mechanism
→ GPT Independent Review
→ G4-08B Kimi UI integration
→ GPT Independent Review
→ G4-09 First Playable B
→ Owner UAT B
→ remaining G4 gate work
```

Do not declare G4-08 PASS from M1 alone and do not start G5 before G4-GATE.