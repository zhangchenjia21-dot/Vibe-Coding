# G5 Stable NPC Agency v0.1 Decision

Status: **SUPERSEDED / HISTORICAL — DO NOT IMPLEMENT AS CURRENT**  
Superseded by: `G5_MULTI_ACTOR_AGENCY_CYCLE_V0_2_DECISION.md`  
Supersession authority: Owner explicit product correction on 2026-09-03.

This historical decision introduced the first stable-NPC agency concept, actor-scoped cognition, background/fail-soft execution, durable agency records, and foreground/timeline safety. Those principles remain inherited where not contradicted by v0.2.

The following v0.1 rule is specifically rejected and must not be implemented:

```text
at most one Guaranteed NPC is evaluated per accepted player turn
+ round-robin/fair single-actor scheduling
```

Owner clarified that one world window may plausibly contain several independent NPC actions (for example multiple major actors around Red Cliffs). Current canonical architecture is therefore:

```text
Agency Selection
→ 0..N relevant stable actors
→ separate actor-scoped executions
→ multiple independent actions may become durable in one Agency Cycle
```

See the v0.2 decision for the full current contract, including bounded multi-actor fan-out, concurrent actor requests, sibling-safe serialized commits, and the planned later stable-actor-registry expansion beyond Guaranteed NPCs.

The original v0.1 content remains recoverable in Git history and must not be treated as current authority.
