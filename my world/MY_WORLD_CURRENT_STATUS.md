---
title: my world｜当前状态
status: current-project-status
version: 17.7
created: 2026-08-26
updated: 2026-09-08
phase: G6 RPG Core Closure + UAT Observability + Internal Dynamic UI
current_task: MW-019 R1 Recommendation UX Correction
current_owner: Codex
parent_task: G6 Core Closure Package 0
semantic_owner: GPT
owner_uat_required: true
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
current_roadmap: MY_WORLD_总体规划路线图_CURRENT.md@v4.3
owner_uat_record: my-world/docs/uat/G6_PACKAGE0_OWNER_UAT_U1.md
active_task_packet: my-world/docs/tasks/MW-019_R1_RECOMMENDATION_UX_TASK.md
active_task_branch: mw-019-r1-recommendation-ux
audit_triage: architecture/reviews/DEVELOPMENT_AUDIT_2026-09-08_ADOPTION_DECISION.md
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

Current Package 0 correction train：

```text
MW-018 R1 Known / Off-screen People          ENGINEERING PASS_WITH_NOTES / INTEGRATED / PRODUCT RE-UAT PENDING
MW-015 R1 Sparse Important Experiences       ENGINEERING PASS_WITH_NOTES / INTEGRATED / PRODUCT RE-UAT PENDING
MW-019 R1 Recommendation UX                  CURRENT / CODEX
Core Context Budget Accounting Correction    QUEUED AFTER MW-019 / BEFORE RE-UAT
↓
focused Owner re-UAT
↓
Package 0 close
```

UAT Observability / Debug Mode v0.1 remains the first package after Package 0 closes.

---

## 2. Owner UAT U1 — COMPLETE

Formal evidence：

`my-world/docs/uat/G6_PACKAGE0_OWNER_UAT_U1.md`

Confirmed product findings：

- MW-018: meaningful player-known/off-screen person could be ineligible because the identity evidence seam was too narrow；
- MW-015: Important Experiences over-generated into a per-turn recap；
- MW-019: recommendation controls need concise directions, detailed click-to-composer drafts, five standalone alternatives and more readable sizing/spacing。

---

## 3. MW-018 R1 — INTEGRATED / ENGINEERING PASS_WITH_NOTES

Reviewed result：

- implementation HEAD: `318a0a2a79bb5f45f582605ac6b98d891a368b4c`
- submitted candidate: `8f48f3cc96a01b1c132c2ccb583193df9c93511a`
- Independent Review: `docs/mw018/MW-018_R1_INDEPENDENT_REVIEW_IR1.md`
- review commit: `97c1862f3e0c9809e8b8879130b0aade2a848681`
- integration verification: `docs/mw018/MW-018_R1_INTEGRATION_VERIFICATION.md`

Product re-UAT note：if natural, clearly player-known people are still systematically suppressed by identity guardrails, reopen the architecture instead of stacking more rules.

No Product PASS yet.

---

## 4. MW-015 R1 — INTEGRATED / ENGINEERING PASS_WITH_NOTES

Reviewed result：

- formal base: `2680b2616db69987a451c9d1bf53b24339a9c7cb`
- implementation HEAD: `aca189b35f5539a190e17a32590c5ca867459df3`
- submitted candidate: `a9cb2ba7c253e8a9c2d07cbf058a6b8d55d16338`
- Independent Review: `docs/mw015/MW-015_R1_INDEPENDENT_REVIEW_IR1.md`
- review commit: `865f45c59abb8945f6dceb0dbceadc8f22e83335`
- integration verification: `docs/mw015/MW-015_R1_INTEGRATION_VERIFICATION.md`
- integrated main tip after verification: `5168893109ef7d22ad3ef6b392988d602304e54c`

Result：the lived Information Curator prompt now treats Important Experiences as selected life milestones rather than rolling recap. Ordinary turns should often return `experiences=[]`; model remains semantic owner; no Program keyword/category/score/turn-threshold classifier was added. Character and milestone mutation remain independent. MW-018 R1 People semantics remain intact.

Product re-UAT must verify both sparsity and freedom：ordinary play should not create recap-like entries, while quiet but genuinely life-shaping events must still be recordable.

No Product PASS yet.

---

## 5. MW-019 R1 — CURRENT

Frozen correction authority：

`architecture/ui/G6_FIVE_RECOMMENDED_ACTIONS_UAT_CORRECTION_V1_0_DECISION.md`

Task Packet：

`my-world/docs/tasks/MW-019_R1_RECOMMENDATION_UX_TASK.md`

Branch：

`mw-019-r1-recommendation-ux`

Formal Code Base：

`5168893109ef7d22ad3ef6b392988d602304e54c`

Product target：

```text
one Action Recommender call
→ exactly five {short label + detailed draft} pairs
→ UI shows short labels only
→ click fills exact detailed editable draft
→ click never sends
→ five items are model-generated standalone alternative next actions
→ free-form input remains primary
```

Readability correction：recommendation/composer controls must be materially larger and more comfortable than the current compact 12/13px, ~28px-button treatment, without becoming a global UI redesign.

Protected boundaries：

- no click-time second Provider call；
- no Character/personality-guided recommendation yet；
- no semantic diversity/ranking/category Program classifier；
- current player-safe Conversation input/currentness seam remains；
- no Structured Output general framework / fence stripping / hidden retry / Provider fallback；
- no Debug Mode / Context Budget / Dynamic UI work in this revision；
- no persistence/new SQLite table。

Codex highest return state：`READY FOR INDEPENDENT REVIEW`.

---

## 6. Proven context-budget correctness defect — QUEUED BEFORE RE-UAT

2026-09-08 audit independently confirmed `WorldTurnContextProjector` double-counts already assembled semantic-change text when calculating later Agency/Evolution remaining budget. A stored NPC action can therefore be omitted from subsequent GM context even when it physically fits under `MAX_PROJECTED_CHARS`.

Adoption decision：

- do not interrupt MW-019 R1；
- after MW-019 review/integration, Task Shape one bounded Context Budget Accounting Correction；
- fix actual assembled-text accounting and add exact-fit / true-overflow boundary regression；
- do not pull G7 Context Orchestrator forward；
- integrate before the next Owner build/re-UAT。

---

## 7. Package 1 after Package 0 — UAT Observability / Debug Mode v0.1

Owner explicitly promoted Debug Mode immediately after Package 0 because it lowers all later UAT cost.

Target：

```text
Debug OFF → normal play unchanged
Debug ON  → per accepted Turn compact UAT trace
Narrative / World semantic / actor identity / Character / Experiences / People / Recommendations / Save-Restore
→ changed / no-change / failed / stale / cancelled
→ human-readable safe failure reason
```

The 2026-09-08 audit's finding that recommendation/curation failure causes are currently collapsed into generic unavailable/provider_failure is assigned to this Package 1 outcome. Do not build a giant EventBus or leak credentials/private GM/NPC state.

---

## 8. Owner-approved core route after Package 0

```text
Package 1  UAT Observability / Debug Mode v0.1
↓
Package 2  OOC + Character-guided Recommendations + accepted-action Character evidence
↓
Package 3  事务 / Open Threads
↓
Package 4  System / Public d20 real consumer
↓
Package 5  factual Inventory vertical
↓
Package 6  Internal Dynamic UI Host v0.1
↓
Package 7  V0 Core Closure Reality Gate
```

Dynamic UI remains Core-required before V0 closure. Old MW-013 packet remains stale and must not be executed as-is.

---

## 9. Audit triage retained

Canonical adoption record：

`architecture/reviews/DEVELOPMENT_AUDIT_2026-09-08_ADOPTION_DECISION.md`

- context-budget bug：confirmed correctness fix before re-UAT；
- failure diagnostics：fold into Package 1；
- People no-change collapse UX：observe in re-UAT; fix only if materially disruptive；
- 8 layer-boundary findings：real technical debt but not a reason to interrupt Core closure; repair opportunistically / bounded hygiene pass, no whole-repo rewrite；
- Application Shell responsibility concentration：extract proven real components as later consumers arrive, then use evidence for Dynamic UI；
- documentation drift：keep current Status/Roadmap authoritative; reduce duplicated stage tables over time。

---

## 10. Protected project invariants

- Model Freedom First；
- free-form Player natural-language action remains primary；
- `World Truth != actor Knowledge != human-player disclosure`；
- Program owns structure/identity/currentness/persistence, not open semantic meaning；
- Save / Restore / Regenerate currentness remains authoritative；
- tests must cover both legitimate update and legitimate no-op/hold；
- no generic framework pulled forward solely for a correction。

---

## 11. Agent routing

```text
GPT
→ product semantics / correction architecture / Task Shaping / Independent Review / integration / UAT interpretation

Codex
→ current MW-019 R1 implementer

Owner
→ one focused Product re-UAT after MW-019 R1 + Context Budget correction are integrated and a fresh build is prepared
```
