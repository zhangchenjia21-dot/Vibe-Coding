---
title: my world｜G4 Runtime Asset Resolution v0.1 Decision
status: deferred-to-g6-historical-design-note
created: 2026-09-02
updated: 2026-09-02
phase: deferred from G4 to G6
semantic_owner: GPT
superseded_by: G4_VISUAL_ASSET_DEFERRAL_TO_G6_DECISION.md
---

# G4 Runtime Asset Resolution v0.1｜DEFERRED TO G6

## Current disposition

This document previously froze a G4-10 implementation route for exact-generation portrait / scene / authored-map loading.

On 2026-09-02 the Owner explicitly decided that current visual assets are not sufficiently authored and visual integration is not part of the present core experience. The implementation timing in this document is therefore **superseded**.

Canonical route decision:

`G4_VISUAL_ASSET_DEFERRAL_TO_G6_DECISION.md`

```text
G4-10 Runtime Asset Resolution   DEFERRED / MOVED TO G6
G4-10M1                          SUPERSEDED / DO NOT EXECUTE
```

Do not use this file to reactivate G4 visual work.

## Retained future semantic notes

These points remain useful design evidence for a future G6 consumer, but are **not a current implementation contract**:

- declared authored visual bytes belong to immutable Source generations and their fingerprints;
- Character portrait remains optional; canonical absence is valid;
- World authored `portrait | scene | map` remains presentation/reference data;
- a presentation fallback must not impersonate authored Source truth;
- `map` image is not topology, travel, current-location, pathfinding, GIS or simulation authority;
- UI Texture/Image/cache/placeholder is projection, not canonical gameplay truth;
- pure presentation failure should normally fail-soft rather than block Narrative gameplay;
- exact-generation vs later presentation override behavior must be re-audited against the actual G6 product consumer.

## Future re-entry gate

Before G6 implements visual runtime resolution, GPT must re-open semantic design from the actual consumer and answer at least:

1. which UI surfaces genuinely consume portrait / scene / map;
2. whether old Games can opt into later presentation-only assets without changing semantic Source ancestry;
3. what exact identity/fallback rules the real presentation experience requires;
4. whether any visual has become authoritative mechanic input and therefore needs a stronger integrity gate.

Until that re-entry gate, no Agent should build the former G4-10 mechanism.
