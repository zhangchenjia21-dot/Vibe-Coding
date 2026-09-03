# G5 Multi-Actor Agency Cycle v0.2 Decision

Status: **SUPERSEDED / HISTORICAL**  
Superseded by: `G5_AGENCY_SCHEDULER_V0_3_DECISION.md`

This document remains as historical design context for the multi-actor transition away from one-NPC-per-turn round-robin.

The following v0.2 ideas were accepted and are inherited by v0.3 unless the newer decision says otherwise:

- one world window may produce several independent NPC actions;
- max-4 first-version fan-out safety ceiling;
- separate actor-scoped execution requests;
- actor-private Source / Knowledge / history;
- concurrent actor execution;
- serialized durable sibling commits;
- foreground wins;
- committed actor actions may enter bounded omniscient GM Context without automatically becoming Player/other-actor knowledge.

The following v0.2 rule is **not current** and must not be implemented:

```text
existing G5-01/G5-02 semantic-analysis request
→ also returns agency_candidates
→ Agency Selection piggybacks on semantic materialization
```

That optimization caused repeated currentness/timeline coupling defects. Current architecture deliberately spends a separate lightweight selector request and treats Agency as a best-effort evaluation of the latest current world snapshot.

For current authority read:

`G5_AGENCY_SCHEDULER_V0_3_DECISION.md`
