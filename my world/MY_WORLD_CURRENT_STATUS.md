---
title: my world｜当前状态
status: current-project-status
version: 6.4
created: 2026-08-26
updated: 2026-08-29
phase: G4 Primary Source Assets & Local Game Creation
current_task: G4-02R1 World / Character Source Semantic Re-audit — T0-SCOPED CONTRACT REVISION
current_owner: GPT
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜CURRENT STATUS

## 1. Current

```text
G1 Foundation                         PASS / CLOSED
G2 AI Conversation Spine              PASS / CLOSED
G2-GATE                               PASS
G3 Persistent Game / Save / Timeline PASS / CLOSED
G3-GATE                               PASS
G4-01 Application Shell / Lifecycle  PASS / CLOSED
G4-02 Engineering implementation      HISTORICAL PASS for v0.1
G4-02 Semantic adequacy               REOPENED as G4-02R1
G4-03 Managed Local Source Library   PASS / CLOSED
G4-04 Multi-Game / Game Library      PASS / CLOSED
G4-05 Asset-only New Game Wizard     REWORK / HOLD

Current Task                          G4-02R1
Semantic audit                        COMPLETE
v0.2 base semantic contract           FROZEN / IMPLEMENTATION PENDING
T0-scoped correction                  FROZEN as v0.2 revision 2
Current subtask                       GPT temporal pressure remigration: 孙权 + 汉末三国 World first
Current Owner                         GPT
Codex                                 NO ACTIVE TASK
G4-06+                                HOLD
G4-GATE                               NOT YET
```

---

## 2. Why G4-02 semantics were reopened

G4-02 v0.1 correctly proved filesystem loading, validation, safe paths, Source/Game live-state boundary and content-sensitive exact generation fingerprint for its original contract.

G4-05 later exposed a stronger product fact: real `汉末三国` / `诸界余辉` assets were compressed into compact `summary/traits/background/drives` and small World summaries. Load/install PASS therefore proved only that a compact derivative fixture fit v0.1, not that the Source contract naturally carried real authored complexity.

Owner decision:

> **GPT owns Meaning; Codex owns Mechanism.**

No destructive rollback is allowed. Preserve G4-03/G4-04 and accepted G4-05 Wizard engineering; correct Source semantics/content forward on `main`.

Canonical ownership decision:

`architecture/source/G4_SOURCE_SEMANTIC_OWNERSHIP_AND_REAUDIT_DECISION.md`

---

## 3. G4-02R1 semantic audit result｜2026-08-29

Primary evidence was re-read from exact historical snapshot:

```text
zhangchenjia21-dot/sillytavern-assets
@ 4a5364a042e41f4c8a69621fc4467956a78703c0
```

Audited at minimum:

```text
World
- 汉末三国_天下未定_World_Pack_v0.2.3.md
- 埃瑟维亚_诸界余辉_World_Pack_v0.1.3.md

Character
- 刘备
- 曹操
- 孙权
- 莉维娅·塞兰
- 阿德里安·维尔克
- 杜恩·石痕
```

Formal audit:

`my-world/docs/source/G4-02R1_SOURCE_SEMANTIC_AUDIT.md`

Verdict:

> **Current v0.1 is NOT semantically adequate. Contract correction required.**

### Character v0.1 failure

`public_profile.summary/traits + gm_private_profile.background/drives` is too thin for real stable authored semantics such as personality contradictions, abilities/limitations, behavior logic, relationship/autonomy, expression, knowledge boundaries, T0/history/player-takeover guidance and rich tables.

Required portrait is also semantically wrong for real text-only historical cards because it encourages fake Source placeholders.

### World v0.1 failure

`source_lore` has raw text capacity, but no section-level disclosure/semantic retrieval boundary. Arbitrary mandatory `source_material` is a catch-all without a real consumer and already enabled provenance-only / over-summary misuse.

### Rejected correction

Do **not** replace the thin profile with a giant Character field forest. Real characters share broad semantic categories, but their internal shapes differ materially.

---

## 4. World / Character Source v0.2｜BASE SEMANTIC FREEZE

Base contract:

`my-world/docs/source/World Pack与Character Card合同v0.2_SEMANTIC_FREEZE.md`

Core shape:

```text
thin Source identity
+ catalog_summary
+ compact always-on instructions where appropriate
+ ordered rich semantic_sections
+ explicit gm_reference / gm_private disclosure
+ package-local UTF-8 Markdown/TXT content files
+ exact fingerprint over all declared content bytes
```

World keeps `world_instructions`, `gm_instructions`, `entries[]`, `authored_assets[]`; replaces compact `source_lore` with rich sections and removes generic mandatory `source_material`.

Character replaces compressed public/private profile objects with rich sections; broad types remain open vocabulary; section internals stay authored text/tables until a real mechanic consumer proves machine structure is needed; portrait is optional; `player_character_supported` stays explicit.

Important:

> `catalog_summary` and `gm_reference` do **not** mean player-known or Character-known.

---

## 5. T0-scoped Source / Post-T0 Canon Quarantine｜FROZEN 2026-08-29

New canonical decision:

`architecture/source/G4_T0_SCOPED_SOURCE_AND_POST_T0_CANON_QUARANTINE_DECISION.md`

Implementation-repo contract addendum:

`my-world/docs/source/G4-02R1_T0_SCOPED_SOURCE_CONTRACT_ADDENDUM.md`

Together with the base semantic freeze, this is current **v0.2 revision 2**.

Reason:

DSH long-play evidence showed that prose rules such as “history is not a script” are insufficient if ordinary GM Context still exposes complete post-T0 canon / later character biography. The model may use that lower-cost future template and converge the Game back toward source history.

Formal invariant:

> **Do not show the model a post-T0 answer and then ask it to forget that answer.**

Therefore:

```text
Immutable Source Package Total Content
!= Selected T0 Source Projection
!= Game-local Canonical Reality
!= Runtime Relevant Set
!= Model-visible Working Set
```

### World

- top-level sections must be cross-T0/always-safe;
- selected Entry/T0 may own Entry-scoped rich sections;
- ordinary Runtime gets top-level always-safe + selected Entry sections only;
- later Entry snapshots remain in exact generation but are quarantined from current Game Context.

### Character

- top-level sections must be always-safe;
- optional `t0_profiles[]` may bind explicit `world_asset_id + entry_id` to rich Character sections;
- ordinary Runtime gets always-safe + exact matching profile only;
- no matching profile must never fallback to latest/nearest/later/complete-life biography;
- same Character `asset_id` remains stable across T0 profiles.

### Model freedom self-check

PASS. This is an information authority/Context boundary, not a Narrative restriction.

Do not add:

- anti-history state machine;
- divergence score;
- fixed output format/length;
- “must differ from canon” rule;
- canon/slight-change/radical-change probability quota。

T0 projection must retain rich present depth: personality inertia, pre-T0 experiences, capabilities/limits, relationships, knowledge provenance, institutions/resources/geography and open goals.

> **Quarantine future answers; preserve present depth.**

### No convergence force / no divergence force

If current causality naturally recreates an original historical outcome, it is valid. If premises changed, canon has no special convergence privilege. Randomness applies to current uncertainty, not to a lottery over “follow history or not”.

### Provider pretraining knowledge

Post-T0 pretrained/external canon has no authority as Game fact, character motive or future prediction. Keep this as a short authority invariant; do not enumerate future events in Prompt.

---

## 6. Game-local evolvable semantics｜FROZEN

Canonical decision:

`architecture/source/G4_GAME_LOCAL_EVOLVABLE_SEMANTICS_DECISION.md`

Formal invariant:

> **Source schema is not the possibility ceiling of the Living World. Game-local semantic structure is evolvable.**

After Final Create:

- exact Source generation remains immutable;
- Game-local canonical object has stable Program-owned identity/provenance/lifecycle kernel;
- model/runtime may add/update/remove previously unanticipated local semantic facets;
- model does not rewrite Source, global Source contract, or physical SQLite schema;
- if an existing canonical Domain owns the concept (Location/Relationship/Knowledge/Injury/Inventory/Faction/etc.), that Domain wins and generic duplicate truth is prohibited;
- local semantic evolution must be durable and Timeline/Save/Restore reversible;
- repeated facets are promoted to a formal Domain only after a real mechanic/UI/query consumer appears.

T0 quarantine and local evolvability are complementary:

```text
T0 quarantine removes leaked future answers
+
Game-local evolvability provides post-T0 creative capacity
```

---

## 7. Reference Audit guardrails retained

G4-02R1 explicitly checked against prior SillyTavern / The World / DSH failures:

- no compact-summary self-proof;
- no giant schema forest;
- no universal asset protocol before consumers;
- no `public == player-known` leak;
- no Source future-script authority;
- no post-T0 canon exposed to ordinary Runtime with prompt-only “ignore it” protection;
- no paper-personality-equals-agency assumption;
- no total-state prompt dump;
- no generic `source_material` second schema;
- no forced fake historical portrait;
- no machine skill/spell ontology before mechanic consumer;
- no local evolution outside durability/timeline;
- no generic facet duplicating formal Domain truth;
- no binding-only evidence substituting for real materialization / playable proof;
- no forced anti-history divergence mechanism.

---

## 8. Current subtask｜Temporal pressure remigration

Migration authority remains:

`my-world/docs/source/G4-02R1_REAL_ASSET_V0_2_MIGRATION_SPEC.md`

but temporal/T0 rules are superseded where conflicting by:

`my-world/docs/source/G4-02R1_T0_SCOPED_SOURCE_CONTRACT_ADDENDUM.md`

Current pressure order:

```text
1. 孙权
   prove early/late personality does not leak backward

2. 汉末三国：天下未定
   prove selected Entry only exposes facts valid at that T0

3. re-audit 刘备 / 曹操 historical temporal packages

4. continue fixed-1287 诸界余辉 characters/world
```

Already-created pre-r2 fixtures are design evidence only and are not final packages until revalidated against T0 quarantine.

Required real packages remain 2 World + 6 Character.

Codex still has **no active task**. Loader/validator implementation is intentionally not started until GPT proves the r2 package shape against real content.

---

## 9. G4-05 disposition

Implementation candidate:

`145c3e1192b443f6284da7f36aee74619adad5bf`

Independent Review: **REWORK**, while the following Wizard/Composition engineering remains provisionally accepted:

- explicit click is authoritative selection;
- no implicit first-row selection;
- exact generation identity / X→Y drift protection;
- World change clears Entry;
- Player eligibility / Player-NPC overlap;
- exact Review lookup / tamper fail-loud;
- Wizard→Review creates no Game DB, no Game Library mutation, no Provider call;
- Cancel returns Main Menu / no Session.

Old packet:

`my-world/docs/tasks/G4-05R1_REAL_ASSET_FIDELITY_CORRECTION_TASK.md`

Status:

> **SUPERSEDED / DO NOT EXECUTE**

G4-05 cannot close until v0.2-r2 mechanism support + faithful T0-scoped real asset packages + regressions + GPT Independent Review are complete.

---

## 10. Next execution order

```text
DONE
G4-02R1 real-source semantic audit
↓
DONE
World / Character v0.2 base semantic freeze
↓
DONE
T0-scoped Source / Post-T0 Canon Quarantine decision + v0.2-r2 addendum
↓
NOW
GPT temporal pressure remigration: 孙权 + 汉末三国 World
↓
GPT faithful remaining 2 World + 6 Character v0.2-r2 packages
↓
THEN
narrow Codex v0.2-r2 loader/validator/fingerprint implementation correction
↓
Codex automated/filesystem/Windows regression evidence
↓
GPT engineering IR + source fidelity + T0 leakage review
↓
G4-05 PASS/CLOSE if evidence holds
↓
G4-06 Atomic Final Create
↓
G4-07 First Playable A — Owner UAT + anti-convergence pressure test
```

G4-03 remains PASS unless corrected v0.2-r2 exposes a concrete Library regression; repair forward if needed. G4-04 remains PASS/CLOSED.

---

## 11. Current blockers / waiting

```text
Blocking: v0.2-r2 T0-scoped real packages are not yet proven
Current: 孙权 + 汉末三国 World temporal pressure remigration
Owner: GPT
Semantic audit: COMPLETE
v0.2 base semantic contract: FROZEN
T0-scoped addendum: FROZEN
Game-local evolvable semantics: FROZEN
Codex active task: NONE
G4-05R1: SUPERSEDED / DO NOT EXECUTE
G4-05: REWORK / HOLD
G4-06+: HOLD
```
