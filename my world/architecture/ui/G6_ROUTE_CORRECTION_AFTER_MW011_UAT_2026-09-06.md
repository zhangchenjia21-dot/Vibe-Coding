---
title: my world｜G6 Route Correction after MW-011 UAT
status: CURRENT / ROUTE AUTHORITY
version: 1.0
created: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
owner: OWNER + GPT
---

# G6 Route Correction after MW-011 UAT

## 1. Owner challenge

After MW-011 R3 Product PASS, GPT prematurely shaped `MW-013 Internal Declarative UI Host v0.1` as the immediate next implementation task.

The Owner correctly challenged that ordering.

The canonical roadmap already defines the G6 sequence as:

```text
Runtime projection → ViewModel → real UI consumer
→ re-audit + implement Runtime Asset Resolution only for actual visual consumers
→ portrait / scene / authored-map presentation
→ Character / Relationship / Inventory / Faction / Map / Save real Surfaces
→ Expansion mechanic-state consumer
→ Internal Declarative UI Host v0.1
→ bounded Action Intent
→ responsive / Theme / navigation
→ Owner UAT / visual polish
```

Therefore Declarative UI Host is **not** the next default step merely because MW-011 produced two renderer-shaped consumers.

## 2. What is complete

```text
Runtime projection / ViewModel / first real UI consumer  DONE — MW-011 PRODUCT PASS
Visual Runtime re-entry audit                            DONE — implementation intentionally DEFERRED
```

The visual audit correctly found no mature first-party authored visual demand. That deferral removes visual implementation from the immediate path; it does not authorize skipping every later grounded consumer step.

## 3. Correct next route

The next G6 work is to ground **real RPG Surfaces** from existing domain owners and the Owner's newly observed information-architecture need.

Before implementation, GPT must answer:

- what belongs persistently in Player Host vs World Surface;
- which right-side Surface has a real current data owner;
- which candidate surfaces would currently be fake/empty because the domain does not yet exist;
- which first Surface creates meaningful player value without inventing new Runtime truth;
- whether that Surface is frontend-only or requires a new backend/domain projection.

Candidate families remain:

```text
Character
Relationship
Inventory
Faction
Map
Save / Timeline
```

but this list is not permission to create empty tabs. Only grounded surfaces proceed.

After enough real surfaces and the Expansion mechanic-state consumer exist, Internal Declarative UI Host v0.1 may be re-authorized from actual repeated component patterns.

## 4. MW-013 disposition

`MW-013 Internal Declarative UI Host v0.1` remains a plausible future Work Item, but its immediate execution is withdrawn.

```text
MW-013 = HOLD / NOT AUTHORIZED YET
```

If an Agent has already started, preserve the isolated worktree/branch, do not merge, and report state. Do not continue until explicit re-authorization.

## 5. Immediate next step

GPT performs a bounded **G6 Surface / Information Architecture Audit** before shaping the next code task.

This audit is product/architecture work, not an implementation task and does not require an Agent yet.
