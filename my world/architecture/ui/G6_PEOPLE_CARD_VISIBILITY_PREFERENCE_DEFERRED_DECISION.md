---
title: my world｜People Card Visibility Preference
status: OWNER-APPROVED / DEFERRED
version: 1.0
created: 2026-09-09
updated: 2026-09-09
phase: G6 future People / Dynamic UI convergence
owner: Owner + GPT
parent:
  - architecture/ui/G6_PEOPLE_SURFACE_V1_0_DECISION.md
  - architecture/ui/G6_MODEL_DRIVEN_INFORMATION_CURATION_AUTHORITY_DECISION.md
implementation_authorization: deferred
---

# People Card Visibility Preference｜玩家隐藏权

## 1. Product decision

Owner wants every People card to eventually expose a **隐藏** action.

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

## 3. Restore visibility

A future UI must give the Player a clear way to recover hidden cards, for example a bounded `已隐藏` / `显示已隐藏人物` entry.

The exact visual treatment is deferred, but hidden cards must never become permanently unreachable through the interface.

## 4. Persistence semantics

Visibility preference should be Game-local presentation preference and survive ordinary reopen/Continue.

It is **not Timeline truth** and should not normally rewind simply because the Player Restores an older world/history Save. Restore controls Game/Conversation/World currentness; Player-chosen UI visibility is a separate presentation preference.

A hidden card remains hidden when the model later updates its player-known snapshot, until the Player explicitly restores visibility. The system must not automatically unhide a card because the model later considers the person more important.

## 5. Model / Program authority

This decision preserves the existing authority split:

- Model owns: who merits a People card and what the latest player-known snapshot means;
- Program owns: normalized storage/currentness and presentation;
- Player owns: optional visibility of cards in their personal interface.

Program must not use hide state as a semantic classifier, importance score, curation hint, prompt instruction or actor deletion trigger.

## 6. Deferred implementation placement

Do not interrupt active Package 2 Owner UAT or the core closure route for this feature.

Preferred implementation timing: when People presentation is next touched for the later Internal Dynamic UI / People Surface convergence, so visibility preference is implemented once against the mature card host instead of duplicated in the current bespoke renderer and then rebuilt.

A separate bounded task may be pulled earlier only if real play proves visible-card clutter is itself blocking meaningful UAT or core gameplay.

## 7. Product acceptance direction

Future Owner acceptance should prove:

- every visible People card can be hidden;
- hiding immediately removes only its UI card;
- underlying player-known People information remains available to the system/model;
- hidden card updates can continue without auto-unhiding;
- reopen preserves hidden state;
- Restore does not incorrectly treat hide state as Timeline truth;
- Player can find hidden cards and restore them to visible state;
- no actor/People snapshot deletion occurs merely from hiding.
