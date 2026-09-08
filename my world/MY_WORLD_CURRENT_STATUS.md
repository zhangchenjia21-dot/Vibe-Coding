---
title: my world｜当前状态
status: current-project-status
version: 17.8
created: 2026-08-26
updated: 2026-09-08
phase: G6 RPG Core Closure + UAT Observability + Internal Dynamic UI
current_task: MW-020 Core Context Budget Accounting Correction
current_owner: Codex
parent_task: G6 Core Closure Package 0
semantic_owner: GPT
owner_uat_required: true
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
current_roadmap: MY_WORLD_总体规划路线图_CURRENT.md@v4.3
owner_uat_record: my-world/docs/uat/G6_PACKAGE0_OWNER_UAT_U1.md
audit_triage: architecture/reviews/DEVELOPMENT_AUDIT_2026-09-08_ADOPTION_DECISION.md
active_correction_decision: architecture/world/G6_CORE_CONTEXT_BUDGET_ACCOUNTING_CORRECTION_V1_0_DECISION.md
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
MW-019 R1 Recommendation UX                  ENGINEERING PASS_WITH_NOTES / INTEGRATED / PRODUCT RE-UAT PENDING
MW-020 Core Context Budget Accounting        CURRENT / CODEX
↓
fresh Owner build
↓
focused Owner re-UAT
↓
Package 0 close only if product verdicts pass
```

Package 1 UAT Observability / Debug Mode v0.1 remains first after Package 0 closes.

---

## 2. Package 0 Owner UAT U1 — COMPLETE

Formal evidence：

`my-world/docs/uat/G6_PACKAGE0_OWNER_UAT_U1.md`

Confirmed product findings and correction state：

- MW-018 — player-known/off-screen People eligibility seam was too narrow；R1 integrated, re-UAT pending；
- MW-015 — Important Experiences over-generated into per-turn recap；R1 integrated, re-UAT pending；
- MW-019 — recommendation controls needed short labels, detailed click-to-composer drafts, independent alternatives and larger/readable controls；R1 integrated, re-UAT pending。

Do not grant Product PASS before focused Owner re-UAT.

---

## 3. MW-018 R1 — INTEGRATED / ENGINEERING PASS_WITH_NOTES

Reviewed implementation：

- implementation HEAD: `318a0a2a79bb5f45f582605ac6b98d891a368b4c`
- submitted candidate: `8f48f3cc96a01b1c132c2ccb583193df9c93511a`
- Independent Review: `docs/mw018/MW-018_R1_INDEPENDENT_REVIEW_IR1.md`
- review commit: `97c1862f3e0c9809e8b8879130b0aade2a848681`
- integration verification: `docs/mw018/MW-018_R1_INTEGRATION_VERIFICATION.md`

Re-UAT note：if natural, clearly player-known people are still systematically suppressed by identity guardrails, reopen the seam rather than stacking more Program rules.

---

## 4. MW-015 R1 — INTEGRATED / ENGINEERING PASS_WITH_NOTES

Reviewed implementation：

- implementation HEAD: `aca189b35f5539a190e17a32590c5ca867459df3`
- submitted candidate: `a9cb2ba7c253e8a9c2d07cbf058a6b8d55d16338`
- Independent Review: `docs/mw015/MW-015_R1_INDEPENDENT_REVIEW_IR1.md`
- review commit: `865f45c59abb8945f6dceb0dbceadc8f22e83335`
- integration verification: `docs/mw015/MW-015_R1_INTEGRATION_VERIFICATION.md`

Re-UAT must test both sparsity and freedom：ordinary turns should often produce no milestone, while quiet but genuinely life-shaping events remain recordable.

---

## 5. MW-019 R1 — INTEGRATED / ENGINEERING PASS_WITH_NOTES

Reviewed implementation：

- formal base: `5168893109ef7d22ad3ef6b392988d602304e54c`
- implementation HEAD: `23ba847169f901497a36fabe0514704969452d9e`
- submitted candidate: `5fbb946e4841fc24fcff463e36603443db1664a6`
- Independent Review: `docs/mw019/MW-019_R1_INDEPENDENT_REVIEW_IR1.md`
- review commit: `0967aceb43ff685dd08114b9c9bda83870d3ef62`
- integration verification: `docs/mw019/MW-019_R1_INTEGRATION_VERIFICATION.md`
- integrated main tip after verification: `f5dea508be2db5904c2d5ebc726b6f130d8c57fe`

Integrated result：one Action Recommender call produces exactly five `{label,draft}` pairs; UI shows short labels; click fills the paired detailed editable draft and never auto-sends or triggers a second recommendation call. Five-independent-next-action semantics remain model-owned; Program adds no semantic diversity/ranking/category classifier. Recommendation/composer typography and controls are materially larger within bounded local scope.

Re-UAT notes：watch grounding embellishment, 10–25s-class recommendation latency, five-alternative usefulness and small-window scrolling comfort. Do not respond by adding Program semantic filters unless normal play proves a real blocker.

---

## 6. MW-020 — CURRENT｜Core Context Budget Accounting Correction

Frozen authority：

`architecture/world/G6_CORE_CONTEXT_BUDGET_ACCOUNTING_CORRECTION_V1_0_DECISION.md`

Trigger：2026-09-08 development audit + independent source confirmation.

Proven defect：`WorldTurnContextProjector` does not account for the 16,000-character GM world-context budget from the actual assembled output exactly once. Existing code can both omit headings/separators from some checks and double-count already assembled world-change material before Agency/Evolution. A durable NPC action can therefore be excluded even though it physically fits.

Required result：

```text
used budget
= actual already-assembled context_text length
+ separator that will really be inserted

candidate section fits
iff final assembled context <= 16000
```

Protected boundaries：

- keep `MAX_PROJECTED_CHARS = 16000`；
- include headings/newlines/separators exactly once；
- preserve section order and existing all-or-nothing section policy；
- preserve current accepted-hash / stale-history filtering；
- no new semantic ranking/retrieval/summarization；
- no larger budget；
- no G7 Context Orchestrator；
- no new Provider call / SQLite / storage redesign。

Required deterministic boundaries：fits exactly, one-character overflow, audit-equivalent no-double-count case, heading/separator accounting, final `context_text.length() <= 16000`, currentness/quiet-state regressions.

MW-020 is a correctness repair. Engineering PASS is sufficient for integration; no separate Owner Product UAT is required, but it must be integrated before preparing the next Owner build.

---

## 7. Next gate after MW-020

```text
MW-020 Independent Review / integrate
↓
safely sync Owner checkout without overwriting unknown local files
↓
fresh Windows export verification
↓
Owner Launch Ready
↓
focused Package-0 re-UAT
```

Focused re-UAT targets：

- People：natural known/off-screen people can become cards when meaningful; incidental people may remain absent; watch over-restriction and no-change collapse UX；
- Important Experiences：ordinary play usually remains sparse; true milestones still appear；
- Recommendations：short useful labels, detailed editable drafts, five standalone alternatives, free-form-first, reasonable arrival timing/readability；
- Living World：no known context-budget omission contaminates observations。

---

## 8. Package 1 — UAT Observability / Debug Mode v0.1

Owner explicitly promoted this immediately after Package 0 because it lowers all later UAT cost.

Target：

```text
Debug OFF → normal play unchanged
Debug ON  → per accepted Turn compact UAT trace
Narrative / World semantic / actor identity / Character / Experiences / People / Recommendations / Save-Restore
→ changed / no-change / failed / stale / cancelled
→ human-readable sanitized failure reason
```

Audit finding about collapsed recommendation/curation failure reasons belongs here. No giant EventBus, credentials or hidden GM/NPC-private semantic values.

---

## 9. Owner-approved core route after Package 0

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

## 10. Audit triage retained

Canonical：`architecture/reviews/DEVELOPMENT_AUDIT_2026-09-08_ADOPTION_DECISION.md`

- context-budget bug：MW-020 CURRENT；
- failure diagnostics：Package 1；
- People no-change collapse UX：observe in re-UAT；
- 8 layer-boundary findings：architecture debt, opportunistic/bounded hygiene only；
- Application Shell concentration：evolutionary extraction as real consumers arrive；
- documentation drift：Current Status/Roadmap are authoritative; reduce duplicate fast-moving stage tables over time。

---

## 11. Protected project invariants

- Model Freedom First；
- free-form Player natural-language action remains primary；
- `World Truth != actor Knowledge != human-player disclosure`；
- Program owns structure/identity/currentness/persistence, not open semantic meaning；
- Save / Restore / Regenerate currentness remains authoritative；
- semantic tests cover both legitimate update and legitimate no-op/hold；
- no generic framework pulled forward solely for a correction。

---

## 12. Agent routing

```text
GPT
→ product semantics / Task Shaping / Independent Review / integration / UAT interpretation

Codex
→ current MW-020 implementer

Owner
→ focused Product re-UAT only after MW-020 is reviewed/integrated and a fresh build is prepared
```
