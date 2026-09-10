---
title: my world｜G6 V0 Core Closure Reality Gate U1 Findings
status: OWNER UAT ACTIVE / FINDINGS ACCUMULATING
version: 1.0
created: 2026-09-10
updated: 2026-09-10
owner: Owner
reviewer: GPT
parent_uat: G6_V0_CORE_CLOSURE_REALITY_GATE_U1.md@v1.1
reviewed_implementation_main: 69ac2030b90f4165deb2ecb5302e3743422af585
owner_build_pck_sha256: 16a3aeb252eba841fedbf8a4f38d7643912764316e561d5b417b88e89085f8c4
---

# Package 7 U1｜Incremental Findings

This file accumulates Owner findings during the active Reality Gate. No implementation dispatch is authorized by this file while UAT is active unless a hard blocker prevents meaningful continuation.

## U1-F01｜Opening bootstrap gap for Open Threads / Inventory

Owner observation:

- `事务` and `行囊` are empty at opening;
- information that is already established by the accepted opening does not enter these surfaces until a later player-authored accepted turn gives the background semantic lanes an opportunity to run;
- Owner expects the game to perform an initial check/refresh after the opening rather than requiring an artificial first action merely to populate already-known current information.

Implementation diagnosis:

- World semantic materialization explicitly returns `opening_skipped` when accepted `player_text` is empty, so the opening cannot currently create factual Inventory events or runtime actor identity/evidence through that lane;
- the Information Curator initial lane is intentionally Character-only and explicitly excludes current unresolved threads and possessions;
- therefore this is not merely a stale UI render: the required structured truth has never been materialized at opening time.

Classification:

**bounded defect / product-sequencing gap worth correcting before final G6 exit, pending end-of-session synthesis.**

Direction to preserve:

- accepted opening should have a bounded, non-blocking post-opening semantic/bootstrap opportunity for domains that can legitimately be established by the opening;
- do not invent default Inventory or Threads merely to fill empty surfaces;
- opening extraction must remain based on accepted player-visible opening facts and must preserve Timeline/currentness/retry/reopen guarantees;
- exact implementation should be shaped after UAT, not patched during active play.

## U1-F02｜People eligibility is still over-constrained by pre-existing stable actor identity

Owner observation:

The opening/current player-visible information mentions multiple historically important people (examples observed by Owner include 张角、皇甫嵩、卢植、朱儁、刘备、曹操), yet the People surface shows only 刘备 and 曹操. Owner suspects this correlates with those figures having pre-authored Character Cards and prefers historically important/public figures learned about by the protagonist to normally become People cards even when the available information is sparse.

Implementation diagnosis:

- Information Curator People updates only operate on `people_evidence` bound to an exact Program-owned stable actor identity;
- identity receipts only bind IDs from the current stable NPC registry;
- compatible pre-authored Character Cards are snapshotted into `stable_npcs` during Final Create, so a pre-authored card gives that person an immediate legitimate stable actor identity;
- runtime narrative can also mint a stable actor, but the World semantic lane currently skips GM-only opening turns;
- therefore a person mentioned/known only through the opening can be semantically important to the player yet remain ineligible for People curation unless a stable identity already exists by another route.

This is the exact class of architecture risk already called out by the People known-person eligibility correction: legitimate player-known/off-screen importance must not be blocked merely by an overly narrow machine identity seam.

Classification:

**architecture/product correction candidate before final G6 exit; not a hard blocker to continued UAT.**

Product direction captured from Owner:

- pre-authored Character Card must not be a prerequisite for a People card;
- People should be able to represent important people known by reputation/history/accepted player-visible information, even before physical meeting;
- historically/socially prominent figures are strong default semantic candidates for persistent memory, and a sparse card is acceptable when little is currently known;
- keep model semantic judgment: do not introduce Program fame tables, hard-coded historical-name allowlists, encounter thresholds, importance scores or `named person always card` rules;
- do not solve identity by display-name matching;
- because player-known referent identity may exist before verified World-actor identity, the post-UAT architecture should examine whether People needs a safe stable player-known referent identity/linking seam rather than requiring authoritative actor identity for every card.

## Current UAT handling

U1 remains **OWNER UAT ACTIVE**.

These findings are accumulated for one end-of-session correction synthesis. Do not dispatch implementation now unless Owner reports a hard blocker or explicitly ends the session and asks to correct findings.
