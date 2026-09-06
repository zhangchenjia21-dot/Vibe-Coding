---
title: my world｜当前状态
status: current-project-status
version: 15.8
created: 2026-08-26
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
current_task: MW-011 Revision 3 — Integrated / Owner UI UAT
current_owner: Owner UAT / GPT semantic-review lane
parent_task: G6 RPG Experience & Internal Declarative UI Host
semantic_owner: GPT
owner_uat_required: true
context_handoff: handoff/GPT_CONTEXT_HANDOFF_CURRENT.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜CURRENT STATUS

## 1. Stage state

```text
G1 Foundation                               PASS / CLOSED
G2 AI Conversation Spine                    PASS / CLOSED
G3 Persistence / Save / Timeline            PASS / CLOSED
G4 Primary Source Assets & Local Game       PASS / CLOSED
G5 World Semantics & GM Runtime             PRODUCT PASS / CLOSED
G5-GATE                                     PRODUCT PASS

G6 RPG Experience & Internal Declarative UI Host ACTIVE
MW-011 R1 G6 RPG Host ViewModel Baseline    ENGINEERING PASS / INTEGRATED
MW-011 R1 Owner UI UAT                      NOT PASS — Player Host too information-thin
MW-011 R2 Player Profile Surface            IR#2 NOT PASS
MW-011 R3 Player Profile Surface            ENGINEERING PASS / INTEGRATED — OWNER UI UAT
MW-012 Zhang Chen Player Character Card     ENGINEERING PASS / INTEGRATED
```

G5 remains closed. G6 remains active.

## 2. MW-011 R3 authoritative state

Reviewed branch head:

`my-world@78bd5ce26b5ec8a465a9f5d6fbcdb536925d5fc0`

Reviewed production/content test HEAD:

`my-world@16c42d576b28c6119c26ff310b426d0caec202ce`

Formal review:

`my-world/docs/mw011/MW-011_INDEPENDENT_REVIEW_IR3.md`

Integration commit:

`my-world@12eedba6a6da47d351d33fb544efbdaa188c85b8`

Integration verification:

`my-world/docs/mw011/MW-011_R3_INTEGRATION_VERIFICATION.md`

Verdict:

**MW-011 Revision 3 / IR#3 = ENGINEERING PASS — INTEGRATED / OWNER UI UAT.**

The integration merge has the exact reviewed branch head `78bd5ce2...` as a direct parent. Independent comparison from that reviewed head to integrated main shows only review/governance document changes after the reviewed outcome; no production, Source content, profile, ViewModel, projector or UI bytes were rewritten.

No additional Independent Review round is required solely for integration.

## 3. R3 accepted product/mechanism outcome

The UAT-driven Player Character profile path is now on main:

```text
optional bounded Character Card v0.2 player_profile
→ existing selected Character projection
→ Final Create freezes exact profile into Game-local Character source_projection
→ separate fail-closed Player Character Profile Projection
→ MW-011 presentation-only ViewModel
→ rich bounded Player Host
```

Protected properties:

- legacy Character Card v0.2 packages without `player_profile` remain valid;
- old Games frozen without a profile remain profile-empty and do not backfill from Source Library current;
- new Games freeze the exact selected generation's profile;
- `player_profile` is presentation-only and does not become World Truth, mechanics state, GM semantic authority or persistence owner;
- raw `semantic_sections`, `gm_reference`, `gm_private`, `catalog_summary`, internal IDs/hashes/fingerprints and Source-current bytes do not flow through the human-player profile projection;
- MW-009 remains the owner of current Player-known facts;
- Player Host may scroll vertically; Narrative remains the dominant center surface;
- no stat ontology, Inventory mechanics, generic UI DSL, Mod schema, Provider summarization or new SQLite table was added.

## 4. Zhang Chen current profile generation

Committed first-party Source:

```text
asset_id: character.han_end.zhang_chen
schema: character_card.v0.2
version: 0.1.1
headline: 24岁 · 现代穿越者
generation fingerprint:
0b6cb72af535ef6147f71cb7592fe6ba048626dd997acf54c4e6893c848b59e4
```

Authored visible group order:

```text
背景
性格
能力
局限
初始目标
行为原则
随身物品
```

The content remains derived from the already Owner-approved MW-012 Character semantics. No new powers, equipment, local relationships, guaranteed history, automatic famous-person recognition or preselected allegiance/self-rule path were introduced.

Production publication evidence from the exact clean R3 content HEAD records:

```text
status = already_installed
zhang_chen_present = true
owner_games_modified = false
```

The Owner's older Zhang Chen `0.1.0` Game correctly remains visually profile-empty because Game-local Source ancestry is frozen. Owner UAT must create a **fresh Zhang Chen 0.1.1 Game**.

## 5. Evidence / regression disposition

Implementer evidence records from the exact clean production/content HEAD:

```text
MW-011 R2 focused profile surface     45 assertions / 0 failures
MW-011 R1 baseline                    0 failures
MW-009 safe projection                0 failures
MW-010 living-world matrix            0 failures
MW-012 Zhang Chen integration         0 failures
G4 Source / Composition / FinalCreate 0 failures
G3 Save/Restore UI                    0 failures
Public d20 / narrative critical path  0 failures
git diff --check                      clean
Windows export                        PASS
Provider calls                        0
SQLite schema/table                   unchanged
```

GitHub exposes no CI status for the candidate, so runtime counts remain implementer-run evidence; GPT independently inspected the actual candidate diff, Source bytes, profile projection, ViewModel, UI renderer, focused test assertions and final integration ancestry.

Non-blocking documentation advisory: the human-readable evidence changed-file list omitted two generated `.gd.uid` companions, while GitHub compare includes them. IR#3 independently reconciles the exact file set; this does not require Revision 4.

## 6. G6 canonical order

G6 remains consumer-first:

```text
Runtime projection
→ presentation-only ViewModel
→ real UI consumer
→ Runtime Asset Resolution only for actual visual consumers
→ portrait / scene / authored-map presentation
→ Character / Relationship / Inventory / Faction / Map / Save real surfaces
→ Expansion mechanic-state consumer
→ Internal Declarative UI Host v0.1
→ bounded Action Intent
→ responsive / Theme / navigation
→ Owner UAT / visual polish
```

External World Pack / Mod UI declaration remains G8 work.

## 7. Protected G5 semantics

- accepted free-form Narrative remains primary and is not gated by semantic/Knowledge/Agency/Evolution extraction;
- World Truth != actor Knowledge != human-player disclosure;
- stable NPCs may act independently;
- World Evolution may hold or selectively advance;
- Public d20 remains program-owned mechanics grounding rather than a second world truth;
- Save/reopen/Restore currentness remains authoritative;
- Literary Style Reference remains expression-only;
- raw accepted Narrative bytes remain authoritative; Markdown-lite remains disposable UI projection.

## 8. Agent routing — Owner update 2026-09-06

The Owner has upgraded to GPT Pro and explicitly changed implementation routing for **new tasks**.

MW-011 R3 has completed its Zcode integration/closeout. For subsequent new implementation tasks, GPT performs Task Shaping and assigns between **Codex** and **KimiCode** according to complexity, importance, blast radius and architectural authority:

```text
Codex
→ high-complexity / high-importance / high-blast-radius work
→ Runtime / Source / Persistence / Save / world semantics / authority boundaries
→ cross-module refactors, difficult debugging, critical integration
→ UI work too when it is architecture-critical or tightly coupled to core state

KimiCode
→ bounded, clear, lower-risk implementation
→ frontend/UI/interaction on established seams
→ ordinary real consumers/surfaces, content tooling, test additions, small refactors
→ batch content-production work once contracts are established

GPT
→ product semantics / architecture / task shaping / assignment / Independent Review

Owner
→ Product UAT / explicit product verdict
```

Cleanly separable mixed tasks may be split `Codex mechanism/backend + KimiCode UI/consumer`. If a task cannot be safely split and touches core authority/persistence/runtime, prefer Codex.

Do not default subsequent new work back to Zcode unless the Owner explicitly changes routing again.

## 9. Immediate route

```text
Owner creates a fresh Zhang Chen 0.1.1 Game
→ Owner UI UAT on rich Player Host
→ verify headline/summary + seven authored profile groups are visible and usable
→ verify left Host scrolls while Narrative remains primary
→ record product verdict
→ if PASS, close MW-011 product outcome
→ shape next real G6 consumer / visual vertical
→ assign that new task to Codex or KimiCode under current Owner routing
```
