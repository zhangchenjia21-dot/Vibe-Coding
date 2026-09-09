---
title: my world｜G6 Package 2 Owner UAT U1
status: OWNER UAT COMPLETE / CORE OUTCOME ACCEPTED / BOUNDED CLEANUP REQUIRED
version: 1.4
created: 2026-09-08
updated: 2026-09-09
package: G6 Package 2 Core Interaction Control
reviewed_product_code: 716d8dbfadaad07d992baef531912b6ce1078e2d
owner_build_pck_sha256: 62ee95f113bbbc905fd29762149bb2cf4641d008d9fe0a18cc84c2ec5547d0bc
owner_build_pck_utc: 2026-09-08T13:06:13Z
owner_verdict: core outcome accepted; bounded cleanup requested before formal Package 2 closure
---

# G6 Package 2｜Owner UAT U1

## 1. Final UAT state

**OWNER UAT COMPLETE.**

Owner ended the UAT and judged the remaining findings to be small issues that should be solved once, then the project should return to the main core route as quickly as possible.

Interpretation:

- Package 2 core product direction is accepted;
- do not reopen broad OOC / Character-guided Recommendation product design;
- do not run another full Package-2 exploratory UAT;
- perform one bounded cleanup containing only the confirmed small findings below;
- after Engineering Review/integration, require at most a very small spot confirmation of the corrected behaviors, then close Package 2 and proceed immediately to Package 3 Open Threads.

Engineering train already integrated:

- MW-024 OOC / GM Guidance = ENGINEERING PASS_WITH_NOTES / INTEGRATED;
- MW-025 Character-guided Recommendations + Accepted-Action Character Evidence = ENGINEERING PASS_WITH_NOTES / INTEGRATED.

Tested Owner build product code:

`716d8dbfadaad07d992baef531912b6ce1078e2d`

Fresh Windows PCK used in UAT:

- Built UTC: `2026-09-08T13:06:13Z`
- SHA256: `62ee95f113bbbc905fd29762149bb2cf4641d008d9fe0a18cc84c2ec5547d0bc`

## 2. Accepted Package-2 core outcome

Owner's play confirms the Package-2 direction is useful enough to keep:

- `角色行动` remains the ordinary protagonist-action path;
- `OOC / GM 指导` works as a direct channel to the GM and returns an OOC response;
- Owner wants more long-run experience with guidance strength, but does not treat this as a redesign blocker;
- Character-guided recommendations remain part of the accepted direction;
- free-form input remains primary;
- remaining findings are treated by Owner as small cleanup items rather than reasons to reopen the package architecture.

Formal Product PASS remains pending until the bounded cleanup is integrated; however, no second full Package-2 UAT is required.

## 3. Bounded cleanup findings

### U1-F02｜Public d20 truth / consequence continuity

Observed:

- UI showed a real Program-owned Public d20 check with DC, roll, total, failure outcome and failure stakes;
- a later OOC question received a GM answer claiming that no dice-check node had been triggered;
- Owner also felt the failure had little visible downstream effect.

Confirmed product problem:

> A Program-owned mechanics result already disclosed to the Player must remain available as player-safe GM context, so later Narrative/OOC cannot contradict it and declared stakes can remain causally legible.

Correction direction:

- expose a bounded player-safe projection of current accepted Public d20 / NO_CHECK truth to the Narrative/OOC continuation context;
- use only Program-owned facts already disclosed to the Player;
- preserve accepted/current Timeline matching and Restore behavior;
- do not expose hidden mechanics control payload, model control reasoning, actor-private material or raw world state;
- do not add a new Provider call;
- do not build the future full `系统 / System Surface` or generic mechanics platform here;
- a failure stake does not need to make recovery impossible, but subsequent Narrative must treat the accepted failure/outcome/stakes as real prior context rather than silently forgetting them.

### U1-F03｜OOC request-only marker leakage

Observed player-visible text included:

`[GM OOC response | input_mode=ooc]`

This is an internal/request-only structural wrapper and should not appear as ordinary GM prose.

Correction direction:

- keep Program-owned typed `action/ooc` semantics;
- keep durable accepted Player/GM raw prose unchanged;
- replace internal-looking request wrappers with a presentation-safe request representation and/or bounded instruction that does not leak implementation syntax into visible prose;
- do not remove structural mode information;
- do not add output regex cleanup that could accidentally rewrite legitimate GM prose.

### U1-F06｜Recommendation compact layout did not realize short-label space savings

Observed:

- recommendation labels are now short as intended;
- controls still stretch across large grid cells and reserve the old long-copy layout height;
- recommendation area + composer consumes roughly one third of the Narrative host in the observed desktop layout.

Current implementation evidence:

- recommendation buttons use horizontal expand/fill;
- each button has 48px minimum height;
- ordinary layout is a two-column full-width grid;
- recommendation scroll may reserve up to 168px height;
- composer independently has a 132px minimum height.

Correction direction:

- short model-authored labels should produce genuinely compact content-width controls;
- prefer compact horizontal/wrapping choice chips/buttons (or equivalent) instead of long full-cell bars;
- five ordinary short labels should normally consume about one or two compact rows on desktop widths;
- recommendation region height follows actual compact rows rather than reserving old long-copy space;
- preserve >=20px readability and practical click targets;
- clicking still prefills the paired detailed draft and never sends;
- do not further shorten model labels merely to compensate for layout waste;
- do not shrink Narrative text;
- composer sizing is not part of this cleanup unless implementation evidence shows the recommendation-only correction cannot produce a usable layout.

## 4. Deferred observations — do not pull into cleanup

### U1-F04 / U1-F05｜Player hide rights for model-curated information

Owner approved a People-specific hide right and then generalized it across eligible model-curated persistent information surfaces.

Canonical deferred decisions:

- `architecture/ui/G6_PEOPLE_CARD_VISIBILITY_PREFERENCE_DEFERRED_DECISION.md`
- `architecture/ui/G6_MODEL_CURATED_SURFACE_VISIBILITY_PREFERENCE_DEFERRED_DECISION.md`

These remain **deferred to Package 6 Internal Dynamic UI / surface convergence** unless information clutter becomes a true core-play blocker.

Do not implement them in the Package-2 cleanup.

## 5. Cleanup / closure rule

Create exactly one bounded cleanup work item for F02 + F03 + F06.

After implementation:

1. GPT Independent Review;
2. integrate reviewed main;
3. fresh Owner build;
4. only a minimal spot confirmation if needed:
   - OOC knows an already-visible d20 result instead of denying it;
   - internal OOC wrapper is not visible;
   - recommendation choices are genuinely compact and return Narrative space;
5. no repeat full Package-2 UAT;
6. close Package 2 on Owner confirmation and proceed directly to Package 3 Open Threads.

Do not insert further discretionary polish or architecture work between this cleanup and Package 3.
