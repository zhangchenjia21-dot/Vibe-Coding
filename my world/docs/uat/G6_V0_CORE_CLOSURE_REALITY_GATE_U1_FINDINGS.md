---
title: my world｜G6 V0 Core Closure Reality Gate U1 Findings
status: OWNER UAT COMPLETE / CORRECTION SYNTHESIS AUTHORIZED
version: 1.1
created: 2026-09-10
updated: 2026-09-10
owner: Owner
reviewer: GPT
parent_uat: G6_V0_CORE_CLOSURE_REALITY_GATE_U1.md@v1.2
reviewed_implementation_main: 69ac2030b90f4165deb2ecb5302e3743422af585
owner_build_pck_sha256: 16a3aeb252eba841fedbf8a4f38d7643912764316e561d5b417b88e89085f8c4
correction_architecture: architecture/G6_REALITY_GATE_U1_CORRECTION_TRAIN_V1_0_DECISION.md@v1.0
next_work_item: MW-032
---

# Package 7 U1｜Final Findings

Owner ended this Reality Gate session on 2026-09-10 and instructed:

> **“我不想继续测试了，你修吧，修完了直接继续主线等下一次测试”**

Therefore U1 no longer accumulates findings. The four findings below are the complete authorized correction set for MW-032. No immediate re-UAT is requested after the correction train.

## U1-F01｜Opening bootstrap gap for Open Threads / Inventory

Owner observation:

- `事务` and `行囊` are empty at opening;
- information already established by the accepted opening only appears after a later player-authored accepted turn;
- opening should itself trigger a bounded check/refresh.

Implementation diagnosis:

- World semantic materialization currently returns `opening_skipped` when accepted `player_text` is empty;
- Information Curator Initial lane is Character-only and explicitly excludes current Threads/possessions;
- this is a semantic sequencing gap, not merely a stale UI refresh.

Classification:

**bounded Product defect / correction required.**

Correction direction:

- accepted GM Opening becomes a legitimate bounded semantic/bootstrap opportunity;
- only facts actually established by accepted player-visible opening material may enter Inventory/Threads/People support paths;
- no fake default Inventory/Threads and no synthetic Player action;
- preserve non-blocking, idempotency, Restore/Regenerate/reopen currentness.

## U1-F02｜People eligibility over-constrained by pre-existing stable actor identity

Owner observation:

Opening/current player-visible material mentioned multiple historically important people such as 张角、皇甫嵩、卢植、朱儁、刘备、曹操, while the People surface showed only 刘备 and 曹操. Owner observed that pre-authored Character Cards appear to correlate with eligibility and stated that historically important persons learned through player-visible information should normally be represented even when information is sparse.

Implementation diagnosis:

- current People updates require identity-receipt evidence bound to exact stable actor IDs;
- compatible pre-authored Character Cards are snapshotted into the Game stable actor registry during Final Create;
- opening semantic processing is skipped, so opening-only known persons without pre-existing actor identity cannot become People candidates;
- the architecture therefore still makes machine actor identity too narrow a gate for legitimate player-known People information.

Classification:

**architecture/Product correction required.**

Correction direction:

- Character Card / stable World actor is not a prerequisite for a People card;
- distinguish stable **player-known People subject/referent identity** from authoritative World actor identity;
- a person known by reputation/history/memory may exist as a People subject before World existence is verified;
- later exact actor linking uses model semantic interpretation plus validated request refs, never display-name matching;
- historically/socially prominent people are a strong model semantic default candidate, not a Program fame table/allowlist/score;
- sparse and explicitly uncertain People cards are valid when that is all the protagonist currently knows;
- a player-known referent alone never becomes World truth, Stable Actor Registry membership, Agency eligibility or NPC knowledge target.

## U1-F03｜Recommended Actions occasionally become unavailable for the whole turn

Owner observation:

> **“有时候【推荐选项】会失效，偶尔产生不生成推荐选项的回合情况。”**

Implementation diagnosis:

- recommendation `_attempted_prefix` is set before the Provider request;
- malformed exact-five response, Provider failure or timeout publishes `unavailable`;
- the unchanged accepted prefix is then considered already attempted, so there is no same-turn recovery;
- the strict exactly-five `{label,draft}` contract is correct, but transient/schema failure is too brittle.

Classification:

**bounded reliability defect / correction required.**

Correction direction:

- preserve strict exactly five paired recommendations;
- permit at most one automatic recovery attempt for the same still-current accepted prefix: maximum two requests total;
- recoverable malformed/transient Provider/timeout failures may retry;
- stale/foreground interruption/cancellation/new accepted history must not retry obsolete work;
- no infinite retry, no hard-coded fallback actions, no semantic ranking/classification;
- free-form input remains immediately available and primary.

## U1-F04｜Open Threads accumulation + missing Player visibility control

Owner observation:

> **“右边的【事务】栏既没有给玩家隐藏按键，又没让模型做完成或结束检查，会导致越堆越多。我建议双管齐下，既要让模型经常检查哪些事务已经过时、完成、结束以更新【事务】栏，也要让玩家有自行判断、隐藏的能力。”**

Implementation diagnosis:

- current Curator prompt technically permits removal, but `open_threads=null` allows passive carry-forward and UAT shows excessive inertia;
- current Thread items are only `{title,summary,details}` and have no stable identity;
- Package 6 correctly refused title/text/index identity hacks, so Threads could not yet use the approved hide/recover mechanism.

Classification:

**bounded semantic-maintenance + identity/presentation correction required.**

Correction direction:

1. every legitimate opening/lived curation opportunity actively reviews current Threads and model-semantically decides keep/update/remove/new;
2. resolved/completed/superseded/stale/no-longer-worth-attention Threads should be removed by the model rather than passively retained;
3. Program adds stable Thread identity and request-scoped refs; no title/text/array-position authoritative identity;
4. Player receives `隐藏 / 已隐藏(N) / 恢复显示` for stable Threads;
5. hide is presentation-only and is not completion/deletion/model feedback;
6. hidden Threads continue semantic updates, do not auto-unhide, survive reopen, and Restore does not rewind the visibility preference;
7. presentation-preference schema evolves backward-compatibly so existing People/Experience hide choices remain intact.

## Authorized synthesis

Canonical correction architecture:

`architecture/G6_REALITY_GATE_U1_CORRECTION_TRAIN_V1_0_DECISION.md@v1.0`

Executable work item:

`MW-032｜G6 Reality Gate U1 Correction Train`

Formal implementation base:

`my-world/main@69ac2030b90f4165deb2ecb5302e3743422af585`

After Engineering Review/integration, Owner explicitly does **not** want an immediate re-UAT/build cycle. Route advances to G7 Package 8 and these corrected outcomes are re-confirmed at a later concentrated test.
