---
title: my world｜G6 Visual Runtime Re-entry Audit
status: CURRENT / DEFER IMPLEMENTATION
version: 0.1
created: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
semantic_owner: GPT
trigger: MW-011 R3 Owner UI UAT PASS
---

# G6 Visual Runtime Re-entry Audit

## 1. Why this audit exists

The canonical G6 route says:

```text
real Runtime projection / ViewModel / consumer
→ re-audit Runtime Asset Resolution for actual visual consumers
→ portrait / scene / authored-map presentation
```

MW-011 has now produced real Player Host and World Surface consumers and passed Owner UAT, so the historical G4-10 visual deferral reaches its formal G6 re-entry gate.

Historical authority:

- `architecture/source/G4_RUNTIME_ASSET_RESOLUTION_V0_1_DECISION.md`
- `architecture/source/G4_VISUAL_ASSET_DEFERRAL_TO_G6_DECISION.md`

This audit decides timing only. It does not reactivate the superseded G4-10M1 task.

## 2. Actual consumer audit

Current product surfaces could eventually consume visuals as follows:

```text
Player Host     → optional Player Character portrait
Narrative Host  → optional scene/context illustration
World Surface   → optional authored-map/reference image
```

However, the current first-party product flow still does not contain a materially authored visual set whose absence is blocking or degrading the accepted core experience. The freshly accepted Zhang Chen `0.1.1` Character is intentionally portrait-absent, and the Owner's current UAT concern is information architecture rather than missing art.

Therefore there is a real placement slot, but there is not yet a sufficiently real authored visual product demand to justify building the resolver now.

## 3. Re-entry questions

### Q1 — Which surfaces genuinely consume portrait / scene / map?

Potential consumers are now concrete: Player Host / Narrative Host / World Surface. But no current first-party visual asset is required for the accepted play flow. Result: **consumer placement exists; implementation demand is not yet mature**.

### Q2 — May old Games use later presentation-only assets?

Still defer. No current user-facing visual update flow requires this behavior. Existing semantic Source ancestry remains immutable. Do not invent a presentation-override layer until a real old-Game/new-art case is requested.

### Q3 — What identity/fallback rules are currently justified?

Retain only semantic guidance, not an implementation contract:

- declared authored visual bytes belong to their immutable Source generation;
- canonical absence is valid;
- missing/broken presentation should fail soft and must not block Narrative play;
- fallback/placeholder must never impersonate authored Source truth;
- UI Texture/cache is presentation projection only.

### Q4 — Has any visual become mechanic authority?

No.

```text
portrait / scene / authored map
!= gameplay/world/location/knowledge authority
```

An authored map image remains reference/presentation data, not topology, current location, travel graph, distance, pathfinding, fog-of-war or GIS truth.

## 4. Decision

```text
G6 Runtime Asset Resolution implementation = DEFERRED
portrait / scene / authored-map implementation = DEFERRED
reason = no materially authored first-party visual demand yet
```

Do not spend Codex/KimiCode on a media resolver merely to satisfy roadmap ordering mechanically.

The next re-entry trigger is one of:

- Owner supplies/approves a real first-party portrait, scene or map asset intended for product display;
- a G6 surface cannot reach its product outcome without a visual;
- a real old-Game/new-presentation use case requires a presentation-only override decision.

## 5. Consequence for G6 route

Because the visual re-entry gate has been evaluated and remains intentionally deferred, G6 may continue with the next grounded UI capability rather than manufacture visual infrastructure.

MW-011 also provided two real reusable UI consumers (Player Host profile material and World Overview/Save). That is sufficient evidence to begin a narrowly bounded **Internal Declarative UI Host v0.1** vertical, provided it remains internal, consumer-derived and non-executable.
