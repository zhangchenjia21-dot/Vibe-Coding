---
title: my world｜People Card Visibility Preference
status: OWNER-APPROVED / ACTIVE VIA PACKAGE 6
version: 1.1
created: 2026-09-09
updated: 2026-09-09
phase: G6 Package 6 Internal Dynamic UI convergence
owner: Owner + GPT
parent:
  - architecture/ui/G6_PEOPLE_SURFACE_V1_0_DECISION.md
  - architecture/ui/G6_MODEL_DRIVEN_INFORMATION_CURATION_AUTHORITY_DECISION.md
  - architecture/ui/G6_MODEL_CURATED_SURFACE_VISIBILITY_PREFERENCE_DEFERRED_DECISION.md
implementation_authorization: MW-030 bounded v0.1 implementation
---

# People Card Visibility Preference｜玩家隐藏权

## 1. Product decision

Owner wants every People card to expose a **隐藏** action once Package 6's shared presentation Host is active.

Frozen product principle:

> **Model decides which people are semantically worth remembering; Player decides which remembered People cards are visible in their own interface.**

This is a presentation preference, not a semantic deletion mechanism.

## 2. Hide semantics

When the Player hides a People card:

- the People card's durable player-known snapshot remains intact;
- the stable Game-local actor identity remains intact;
- Information Curator may continue updating that hidden card when later accepted player-visible information changes it;
- the hidden card continues to exist as part of the current player-known People information model;
- only the ordinary People UI suppresses that card from the visible list.

Hide MUST NOT:

- delete or tombstone the People snapshot;
- remove the stable actor;
- alter World Truth, actor Knowledge, Conversation, Timeline or Information Curation currentness;
- change whether the model considers that person important;
- be interpreted as the Player forgetting the person in-world;
- become negative evidence for later model curation.

## 3. Stable presentation identity

People hide state must derive from the existing exact stable Game-local actor identity but does not need to expose that raw identity to the UI.

Package 6 may derive an opaque presentation key for the safe presentation DTO/preference owner.

Display name is never authoritative hide identity. Same-name people must remain independently hideable.

## 4. Restore visibility

Package 6 must give the Player a clear bounded recovery path, for example a surface-level `已隐藏 (N)` drawer and `恢复显示` action.

Hidden cards must never become permanently unreachable through the interface.

Restoring visibility shows the current People snapshot at that moment, not a stale copy stored when hidden.

## 5. Persistence semantics

Visibility preference is Game-local presentation preference and survives ordinary reopen/Continue.

It is **not Timeline truth** and must not rewind because the Player Restores an older world/history Save. Restore controls Game/Conversation/World currentness; Player-chosen UI visibility is a separate presentation preference.

A hidden card remains hidden when the model later updates its player-known snapshot, until the Player explicitly restores visibility. The system must not automatically unhide a card because the model later considers the person more important.

## 6. Model / Program / Player authority

This decision preserves the existing authority split:

- Model owns: who merits a People card and what the latest player-known snapshot means;
- Program owns: normalized storage/currentness and presentation plumbing;
- Player owns: optional visibility of cards in their personal interface.

Program must not use hide state as a semantic classifier, importance score, curation hint, prompt instruction or actor deletion trigger.

## 7. Active implementation placement

This feature is now authorized only inside **Package 6 / MW-030 Internal Dynamic UI Host v0.1**, so People visibility is implemented once against the shared card Host rather than as a bespoke People-only patch.

Current Package 6 authority:

`architecture/ui/G6_INTERNAL_DYNAMIC_UI_HOST_V0_1_DECISION.md@v1.0`

## 8. Product acceptance direction

Future Owner acceptance should prove:

- every visible People card can be hidden;
- hiding immediately removes only its UI card;
- underlying player-known People information remains available to the system/model;
- hidden card updates can continue without auto-unhiding;
- reopen preserves hidden state;
- Restore does not incorrectly treat hide state as Timeline truth;
- Player can find hidden cards and restore them to visible state;
- no actor/People snapshot deletion occurs merely from hiding;
- no display-name identity is used.
