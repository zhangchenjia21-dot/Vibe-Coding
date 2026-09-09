---
title: my world｜G6 Package 2 Owner UAT U1
status: OWNER UAT ACTIVE
version: 1.3
created: 2026-09-08
updated: 2026-09-09
package: G6 Package 2 Core Interaction Control
reviewed_product_code: 716d8dbfadaad07d992baef531912b6ce1078e2d
owner_build_pck_sha256: 62ee95f113bbbc905fd29762149bb2cf4641d008d9fe0a18cc84c2ec5547d0bc
owner_build_pck_utc: 2026-09-08T13:06:13Z
owner_verdict: pending
---

# G6 Package 2｜Owner UAT U1

## 1. Gate state

**OWNER UAT ACTIVE**.

Engineering train is complete and integrated:

- MW-024 OOC / GM Guidance = ENGINEERING PASS_WITH_NOTES / INTEGRATED;
- MW-025 Character-guided Recommendations + Accepted-Action Character Evidence = ENGINEERING PASS_WITH_NOTES / INTEGRATED.

Owner build preparation returned `OWNER LAUNCH READY` against exact reviewed product code:

`716d8dbfadaad07d992baef531912b6ce1078e2d`

Fresh Windows PCK:

- Built UTC: `2026-09-08T13:06:13Z`
- SHA256: `62ee95f113bbbc905fd29762149bb2cf4641d008d9fe0a18cc84c2ec5547d0bc`

Owner canonical checkout tracked HEAD == origin/main at the reviewed product code. Existing `.gitignore` local modification and ten untracked sidecars were preserved unchanged.

## 2. Product question

Does Package 2 make interaction control feel natural without reducing roleplay freedom?

Specifically:

- role actions remain ordinary protagonist actions;
- OOC / GM Guidance feels like speaking directly to the GM rather than acting in-world;
- OOC itself does not create World/mechanics/Character/People consequences;
- subsequent roleplay naturally respects recent OOC guidance;
- recommendations feel informed by the current protagonist;
- recommendations still permit deviation, experiment and growth rather than personality lock-in;
- final accepted role choices may gradually influence Character when genuinely meaningful;
- unsent recommendation drafts, OOC and failed/cancelled attempts do not mechanically rewrite Character;
- free-form action remains primary;
- Save/reopen/Restore preserve typed mode/currentness.

## 3. UAT method

Owner may play naturally rather than execute a synthetic engineering checklist. Debug Mode may be used when useful to verify that OOC does not schedule World/Curator lanes while normal role action still does.

During this UAT, reviewed product code is frozen. Do not modify production code unless Owner reports a hard blocker that prevents meaningful continuation.

Owner may submit observations incrementally. Findings should be accumulated until Owner explicitly states the UAT is complete or gives a final Product verdict.

## 4. Accumulated Owner observations

### U1-F01｜OOC basic interaction — positive, strength still under observation

Owner tried `OOC / GM 指导` and reported that it appears useful and does provide direct GM feedback. Owner wants more play time before judging how strongly and naturally later play follows the guidance.

Current state: **positive / continue observing**.

### U1-F02｜Public d20 truth vs later OOC contradiction

Owner observed a visible Program-owned Public d20 failure card with DC / roll / total / failure outcome and failure stakes. A later OOC question about that event received an answer claiming that no dice-check node had been triggered.

This is a real consistency finding, not Owner misunderstanding.

Working interpretation to verify after UAT:

- Program/UI knows the durable Public d20 result;
- later OOC/Narrative model context may not receive the relevant player-safe Program-owned mechanics truth;
- therefore GM can contradict a mechanics event already shown to the Player.

Also observe whether the declared failure stakes materially constrain subsequent Narrative, rather than becoming a visually isolated card with little causal effect.

Do not patch during active UAT unless this becomes a hard blocker.

### U1-F03｜OOC request-only marker leakage

Owner screenshot showed `[GM OOC response | input_mode=ooc]` in player-visible GM output. This is an internal/request structural marker and should not become ordinary presentation text.

Treat as a low-severity presentation leak to correct after UAT if still reproducible; do not use this as reason to remove structural typed-mode semantics.

### U1-F04｜Future People-card hide right — Owner-approved, deferred

Owner wants every People card to eventually have a `隐藏` action while preserving model curation authority.

Frozen distinction:

> Model decides which people are semantically worth remembering; Player decides which remembered People cards are visible in their own interface.

Hiding is presentation-only. It must not delete/tombstone the People snapshot, stable actor identity, curation state or later updates. Hidden cards remain part of the information model and may continue to update; they stay hidden until the Player restores visibility.

People-specific deferred decision:

`architecture/ui/G6_PEOPLE_CARD_VISIBILITY_PREFERENCE_DEFERRED_DECISION.md`

### U1-F05｜Generalize hide right across model-curated information surfaces — Owner-approved, deferred

Owner explicitly broadened the People-card idea: other current information surfaces, and future surfaces that use the same model-curated retention-and-presentation pattern, should support the same Player-side hide capability where a stable renderable unit exists.

Frozen cross-surface principle:

> **Model decides what information is semantically worth retaining and presenting; Player has final control over which retained information units are actually visible in their own interface.**

This is intended to reduce pressure to over-tune the model merely to match the Player's preferred sidebar density. The Player can suppress presentation without turning that choice into semantic feedback, negative importance evidence, deletion, or a prompt-training signal.

Canonical cross-surface deferred decision:

`architecture/ui/G6_MODEL_CURATED_SURFACE_VISIBILITY_PREFERENCE_DEFERRED_DECISION.md`

The generic capability applies to eligible model-curated persistent units such as People cards, Important Experience entries and future Open Threads / Organization / World Chronicle items. It does not automatically apply to authoritative mechanics, factual Inventory, blocking errors, Debug diagnostics or other state whose visibility requires separate product judgment.

F04/F05 are **non-blocking for Package 2** and should not interrupt the current UAT/core route. Preferred implementation timing is Package 6 Internal Dynamic UI / information-surface convergence, unless real information clutter becomes a core-play blocker.

### U1-F06｜Recommendation labels became shorter but layout still wastes Narrative space

Owner confirms the earlier product reason for short recommendation labels was to reduce the amount of screen space occupied by the recommendation surface. In the current build, the labels are shorter but the recommendation controls still stretch into long full-width bars, so the product-space benefit was not realized. Together with the composer, the bottom interaction region occupies roughly one third of the main Narrative host in the observed desktop layout.

Current implementation evidence matches the symptom:

- recommendation buttons use `SIZE_EXPAND_FILL` and therefore stretch to fill their grid cells even when the model-authored label is short;
- every recommendation button has a fixed `48px` minimum height;
- the recommendation grid is a two-column full-width layout at ordinary Narrative widths;
- the recommendation scroll area may reserve up to `168px` height on normal-height windows;
- the Player composer independently keeps a minimum height of `132px`.

Product interpretation:

> Short labels should produce a genuinely compact recommendation surface, not merely short text inside oversized bars.

Likely correction direction after UAT:

- make recommendation controls content-width / compact rather than full-cell stretch;
- use a horizontal flow / wrapping compact-choice layout (or equivalent) so five short labels usually consume about one or two compact rows instead of three large grid rows;
- recommendation area height should follow the actual compact rows rather than reserve the old long-copy height;
- keep current >=20px readability and direct click target usability;
- clicking still prefills the detailed draft and never sends;
- do not shorten model labels further merely to compensate for layout waste;
- do not reduce Narrative font size to recover space.

The composer itself is not yet declared defective by this finding; first reclaim the recommendation-space waste that the short-label design was specifically intended to solve. If the combined interaction region remains too dominant afterward, reassess composer sizing separately based on Owner UAT.

Treat F06 as a real UX/product finding to correct after the current UAT unless Owner later deems it non-blocking. Do not patch during active UAT.

## 5. Product PASS rule

Package 2 closes only on explicit Owner Product PASS.

Engineering tests, real-provider samples, build readiness or absence of crashes do not substitute for this verdict.
