---
title: my world｜2026-09-08 Development Audit Adoption Decision
status: CURRENT AUDIT TRIAGE
version: 1.0
created: 2026-09-08
updated: 2026-09-08
owner: Owner + GPT
source: Owner-provided Codex read-only development audit
implementation_repo: zhangchenjia21-dot/my-world
---

# 2026-09-08 Development Audit｜Adoption Decision

## 1. Decision summary

The audit is accepted as useful technical evidence, but it does **not** supersede the Core-first route and does not authorize broad refactoring.

Route principle remains:

> Finish the playable V0 core loop first. Fix proven correctness blockers before Owner UAT; fold observability into Package 1; defer architecture hygiene and shell decomposition unless they become direct blockers or are naturally touched by later consumers.

Current MW-015 R1 stays uninterrupted.

## 2. Finding triage

### A-01｜World context budget double-counting — ACCEPT / PRE-RE-UAT CORRECTION

Independent source inspection confirms the current WorldTurnContextProjector can double-count already assembled world-change text when budgeting later Agency / Evolution material.

Product risk:

```text
NPC/world action is already durable
→ context projection incorrectly omits it despite available budget
→ later GM may not receive a real world consequence/action
```

This directly affects the living-world core promise, so it must be corrected **after the current Package-0 MW-015/MW-019 correction train and before the next focused Owner re-UAT**.

Correction scope must stay minimal:

- compute consumed budget from actual assembled context exactly once;
- include separators/headings consistently;
- add fits-exactly / truly-over-budget boundary tests;
- do not enlarge the total budget merely to hide the accounting bug;
- do not pull forward the G7 Context Orchestrator.

Executable Work ID / lineage is assigned only when shaped.

### A-02｜Eight layer-boundary violations — ACCEPT AS ARCHITECTURE DEBT / DO NOT BLOCK CURRENT LOOP

The reported cross-module internal imports and one L2→L3 dependency are accepted as bounded architecture debt.

They are not evidence of data corruption or product failure by themselves, therefore they do not preempt Package 0 / Package 1.

Policy:

- repair a violation when a current task already touches that seam and the repair is narrow;
- otherwise collect remaining violations for a bounded architecture-hygiene pass before/alongside Dynamic UI abstraction, when stable public seams are clearer;
- add a lightweight dependency check only after the allowed dependency direction is encoded without false positives;
- do not create a universal broker/service layer or duplicate shadow contracts merely to make imports look clean.

### A-03｜Backend failure reasons lost — ACCEPT / MERGE INTO PACKAGE 1 DEBUG MODE

This is not a separate production task.

Package 1 must preserve bounded terminal evidence for real lanes, including where available:

```text
Turn / accepted version
lane / stage
changed | no-change | failed | stale | cancelled
sanitized failure code / reason
elapsed time
Provider / Model
```

Normal mode stays simple. Debug Mode may expose player-safe explanations but never raw private model payloads, hidden world/NPC truth, credentials, or unrelated local data.

### A-04｜People cards collapse after no-change refresh — ACCEPT AS REAL UX DEFECT / DEFER UNLESS RE-UAT BLOCKS

The call-chain evidence is plausible and consistent with current rebuild behavior. It does not corrupt People semantics/currentness.

Do not expand the current MW-015/MW-019 train for it. During focused re-UAT, explicitly observe whether background no-change refresh disrupts reading. If materially disruptive, keep it in People/Shell UX revision lineage; otherwise resolve during real Surface extraction / Dynamic UI migration.

No display-name key may be introduced to preserve expansion state.

### A-05｜Application shell responsibility concentration — ACCEPT / EVOLUTIONARY EXTRACTION

Do not perform a whole-shell refactor now.

As Open Threads, System, Inventory and Debug Mode become real consumers, extract concrete presentation/components one at a time. Package 6 Internal Dynamic UI then abstracts only repeated proven patterns. `应用壳.gd` remains composition root rather than a growing universal owner.

### A-06｜Documentation propagation lag — ACCEPT / GOVERNANCE HYGIENE

Current Status and Roadmap remain authoritative. README/AGENTS/architecture summaries should avoid duplicating fast-changing stage tables where possible and link to current sources.

Do not move implementation main during an active task solely for documentation cleanup. Propagate at the next safe integration boundary.

## 3. Structured output note

The audit observation that model requests do not currently set a formal `response_format` is retained as evidence for the approved Structured Output Reliability direction.

Do not broaden current Package 0 solely for this unless real MW-019 re-UAT shows recommendation availability/format reliability is materially failing. Otherwise keep the systematic Provider capability work in the later reliability package.

## 4. Updated near-term sequence

```text
MW-015 R1                                  ← CURRENT
→ Independent Review / integrate
MW-019 R1
→ Independent Review / integrate
Core Context Budget Accounting Correction  ← new proven correctness fix
→ Independent Review / integrate
focused Owner Package-0 re-UAT
→ close Package 0 when product verdicts pass
Package 1 Debug Mode / per-turn UAT observability
→ continue OOC / Open Threads / System / Inventory / Dynamic UI / Core Closure
```

The context-budget correction is inserted before re-UAT because testing a known context-omission defect would contaminate conclusions about living-world quality.

## 5. Acceptance philosophy retained

Every semantic capability should test both positive and legitimate no-op behavior:

- meaningful person can be remembered; incidental person may remain uncarded;
- life-shaping event can become milestone; ordinary turn may produce none;
- NPC/world may act; world may legitimately hold;
- state change must survive/revert with current Timeline; no-change must remain stable.

Model owns open meaning. Program tests structure, identity, disclosure, durability, currentness and recovery.
