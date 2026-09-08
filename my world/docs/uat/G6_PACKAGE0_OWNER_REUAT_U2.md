---
title: my world｜G6 Package 0 Focused Owner Re-UAT U2
status: PRODUCT PASS / PACKAGE 0 CLOSED
version: 1.2
created: 2026-09-08
updated: 2026-09-08
owner: Owner
semantic_owner: GPT
final_product_code_artifact: d81f5f215360780cc50038ccd3bce7cb4163b866
supersedes_for_current_uat:
  - my-world/docs/uat/G6_PACKAGE0_OWNER_UAT_U1.md
---

# G6 Package 0｜Focused Owner Re-UAT U2

## 1. Final Package-0 verdict

Owner completed the Package-0 correction UAT and the bounded MW-021 follow-up confirmation.

Final verdicts:

```text
MW-018 R1 People                  = PRODUCT PASS
MW-015 R1 Important Experiences  = PRODUCT PASS
MW-019 R1 Recommendations        = PRODUCT PASS
MW-020 Context Budget            = ENGINEERING PASS_WITH_NOTES / INTEGRATED / no standalone Product gate
MW-021 Narrative Scroll          = PRODUCT PASS

Package 0                        = PRODUCT PASS / CLOSED
```

Final reviewed/integrated product-code artifact at closure:

`d81f5f215360780cc50038ccd3bce7cb4163b866`

## 2. U2 original correction results

Owner's ordinary-play U2 confirmed the three original corrections were broadly successful:

- People now supports meaningful player-known/off-screen persons without reopening the earlier current-scene-only product failure;
- Important Experiences no longer behaved like a mandatory per-turn recap during the focused run;
- Recommendations were accepted as the corrected short-label → detailed editable draft interaction with five independent next-step directions.

These outcomes remain closed. Do not replay them merely because later Packages touch adjacent UI unless a new concrete regression appears.

## 3. MW-021 follow-up finding and closure

U2 found two additional Narrative Host usability defects:

1. long main Narrative history lacked a practically visible/draggable vertical scrollbar;
2. Continue/reopen restored history at the top instead of latest progress.

They were isolated as:

`MW-021｜Narrative Scroll Navigation & Reopen Position`

Engineering review/integration proved:

- a local 18px main Narrative scrollbar with real mouse drag;
- Continue/reopen/full history reconstruction settles at latest/current bottom;
- deliberate manual upward reading disables follow-latest for ordinary incremental updates;
- returning near bottom restores follow-latest;
- no persistent scroll position, Conversation/World/Timeline/Save mutation, Provider call or global Theme redesign.

Owner bounded confirmation verdict:

> **PASS，继续。**

Therefore MW-021 = **PRODUCT PASS**.

## 4. Package-0 closure meaning

Package 0 is now closed. It established the current usable G6 baseline for:

- player-known People curation;
- sparse Important Experiences;
- five recommended actions UX;
- corrected world-context budget accounting;
- practical long-Narrative scrolling and reopen positioning.

Known non-blocking evidence retained:

- G3-03 has one pre-existing Context assertion already reproduced on the MW-021 starting baseline;
- several older suites retain known resource-exit diagnostics;
- these were not introduced by MW-021 and do not block Package-0 Product closure.

## 5. Next authorized package

Proceed immediately to:

**Package 1｜UAT Observability / Debug Mode v0.1**

Purpose:

> reduce Owner UAT cost by making it obvious, per accepted Turn, which backend domains changed, remained unchanged, failed, became stale or were cancelled.

Debug Mode remains read-only and player-safe; it must not become a second truth source, alter model input/gameplay/currentness, expose hidden GM/NPC-private semantic values, or grow into a giant telemetry framework.
