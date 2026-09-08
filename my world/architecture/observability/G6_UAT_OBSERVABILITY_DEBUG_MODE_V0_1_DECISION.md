---
title: my world｜G6 UAT Observability / Debug Mode v0.1 Decision
status: FROZEN / CURRENT
version: 1.0
created: 2026-09-08
updated: 2026-09-08
phase: G6 Package 1
owner: Owner + GPT
parent:
  - MY_WORLD_总体规划路线图_CURRENT.md
  - MY_WORLD_核心设计原则_CURRENT.md
---

# G6 Package 1｜UAT Observability / Debug Mode v0.1

## 1. Product purpose

Package 1 is a UAT leverage capability, not a new gameplay system.

It answers one practical Owner question after each accepted Turn:

> **“这一回合后台到底哪些东西真的变了，哪些没变，哪里失败了？”**

The goal is to reduce repeated manual log/database inspection while preserving normal play.

Frozen principle:

> **Debug Mode observes existing truth and terminal evidence; it never becomes truth, changes truth, or changes model behavior.**

## 2. User-facing shape

Debug Mode is independent of the player-information mother taxonomy. It is not a new `角色/人物/事务/...` world-information tab.

v0.1 uses a first-party UAT control in the Game shell:

```text
TopBar: [调试]

Debug OFF
→ no visible diagnostic panel
→ normal player experience unchanged

Debug ON
→ bounded collapsible/scrollable Debug panel
→ recent per-turn diagnostic trace
```

The toggle is **OFF by default** for each Game-session activation. No persistent Debug preference is required in v0.1.

The diagnostic panel should remain compact and must not permanently take over the Narrative Host. A local bounded scroll region is appropriate.

## 3. Trace contract

A diagnostic entry is Program-owned metadata only. It may contain:

- accepted Turn index / current trace epoch;
- lane/domain name;
- terminal state;
- change state where meaningful;
- sanitized failure/status code;
- short human-readable reason;
- elapsed time when Program can measure it without invasive instrumentation;
- configured Provider / Model identifiers where relevant;
- bounded player-safe counts/summary where explicitly allowed.

Do **not** expose raw hashes/IDs as the primary Owner-facing UI. Internal opaque currentness keys may exist in the diagnostic owner but are not leaf-display content.

### Terminal vocabulary

Do not force all heterogeneous domains into one fake semantic state machine. Normalize only the common UAT dimensions:

```text
terminal:
accepted | committed | ready | unavailable | failed | stale | cancelled | restored | saved

change:
changed | no-change | unknown
```

UI may collapse these into natural labels such as:

```text
Narrative      accepted
World          changed
Identity       no-change
Character      changed
Experiences    no-change
People         changed
Recommendations ready
```

Abnormal terminal states take precedence over change labels.

## 4. Initial v0.1 consumers

### A. Narrative

Use existing Conversation / Narrative terminal evidence.

Show at minimum:

- accepted;
- failed + sanitized reason;
- cancelled.

Do not log raw Narrative payload as diagnostic evidence; the Narrative is already visible in the normal game surface.

### B. World semantic

Use the existing `WorldTurn` terminal seam, especially `opportunity_terminal` / bounded terminal result.

Safe observable result includes structural counts already owned by Program, for example:

- durable world change count;
- actor-knowledge event count;
- new stable actor count;
- success/failure/stale/cancelled terminal.

The Debug panel must **not display the hidden world-change prose, NPC-private knowledge text, actor private profiles or raw semantic response**.

`change_count > 0` may support `World changed`; zero may support `World no-change` when the terminal itself succeeded.

### C. Actor materialization / identity bridge

This is a separate diagnostic row even though it currently shares the World semantic call/commit.

Display only bounded structural evidence such as:

- new stable actor count;
- identity-receipt/binding count or equivalent safe structural terminal;
- success/no-change/failure/stale.

Do not display internal actor IDs, opaque actor refs, hidden profiles or private identity material.

If current World terminal lacks one small structural count needed for this row, it may be extended with that count. Do not create a second identity resolver or extra model call.

### D. Character / Important Experiences / People

These three rows remain semantically distinct even though the current Information Curator produces them in one model call.

Determine `changed / no-change` from **player-safe current projections**, not from raw curation storage or hidden model reasoning.

Allowed compact evidence:

- Character changed / no-change;
- Important Experiences added/changed count;
- People card add/update/remove count.

Do not display private actor truth. Any optional text detail must already be player-visible through the normal safe projection; counts alone are sufficient for v0.1.

Information Curator failure/stale/cancelled must remain one real shared-lane terminal and may cause all three semantic rows to show the same abnormal terminal rather than inventing independent successes.

### E. Recommendations

The current recommendation UI collapses several failure paths into `unavailable`. Package 1 must preserve a **bounded diagnostic terminal seam** for the existing Action Recommender.

At minimum distinguish where applicable:

- ready;
- input unavailable / no valid opportunity;
- malformed/invalid structured response;
- response oversized;
- Provider/start failure;
- timeout;
- cancelled;
- stale/currentness discard.

This diagnostic seam is read-only and must not change strict parsing, retries, Provider selection, recommendation content or gameplay fallback.

### F. Save / Restore / currentness

Display bounded operation results for Save/Restore when those operations occur during the session.

Restore is also a diagnostic epoch boundary:

- pre-Restore traces must not remain presented as if they describe the restored current timeline;
- v0.1 may clear the current in-memory per-turn trace buffer and append one Restore result entry;
- no diagnostic persistence/history database is required.

A successful Save/Restore result may show its human display label/status, but never raw database paths or unrelated local data by default.

## 5. Diagnostic owner and data lifetime

Use one **bounded in-memory UAT observability owner** composed by the Application Shell from existing public seams.

It may keep a small ring buffer such as the latest **64 diagnostic entries**. This is a storage bound, not a semantic rule.

No new SQLite table, Save payload, World field or Conversation field is allowed.

The diagnostic owner may remain active while the panel is hidden so turning Debug ON can show recent terminal evidence from the current session. Hidden collection must remain read-only, bounded and have zero Provider/model/gameplay side effects.

On Game close it is discarded.

## 6. Currentness and replacement

Diagnostics must never present displaced-future evidence as current.

Required rules:

- each turn-scoped entry binds internally to the accepted version/current trace epoch;
- Restore invalidates/clears prior current-session trace and starts a new epoch;
- Regenerate/correction/replacement must not let an older callback publish as the current version;
- when an existing lane already provides `stale`, preserve that terminal instead of silently converting it to failure/no-change.

Do not persist diagnostic currentness metadata into Game truth.

## 7. Safe failure reasons

Owner should not need to interpret opaque generic `unavailable` states.

Use a bounded mapping from existing Program-known status codes to concise explanations, for example:

- 模型服务调用失败；
- 模型响应结构无效；
- 请求超时；
- 当前回合已被替换，旧结果已丢弃；
- 持久化失败；
- 当前没有合法推荐机会。

A status code may be shown alongside the explanation when useful.

Never show:

- API keys / Authorization headers;
- raw Provider payload/request bodies;
- private NPC/world semantic text;
- hidden Source-current material;
- unrelated file paths/local privacy;
- model chain-of-thought/reasoning.

## 8. Provider / Model metadata

Where a lane uses a model, Debug v0.1 may show the current configured Provider/Profile/Model identifier from the existing validated runtime-settings/public adapter seam.

Do not inspect credentials and do not create a second settings owner.

No token/cost dashboard is included; richer model operations remain later Package 12.

## 9. UI behavior

The Debug panel is a read-only UAT surface.

Recommended row shape:

```text
Turn 12
Narrative       accepted
World           changed · 2 durable changes
Identity        changed · 1 binding
Character       no-change
Experiences     no-change
People          changed · 1 card
Recommendations ready · 5 actions · 12.4s
```

On abnormal states, show the abnormal result and concise reason.

Do not require every row to appear instantly. Background lanes may update the same current Turn trace as they finish.

The panel must not block free-form input, Narrative streaming, Save/Restore or background lanes.

## 10. Explicit non-scope

Package 1 v0.1 does **not** implement:

- a giant EventBus / universal telemetry platform;
- full player-facing `本回合变化 / Consequence Diff`;
- hidden-world inspector or NPC-private dossier viewer;
- log-file browser / SQL browser / raw request-response viewer;
- token/cost analytics dashboard;
- persisted diagnostic history;
- remote telemetry;
- semantic diff of hidden World truth;
- Open Threads/System/Inventory diagnostics before those consumers exist;
- any change to model prompts, gameplay semantics, recommendation strictness, mechanics or currentness.

Open Threads, System and Inventory later join this same bounded observability seam when they become real production consumers.

## 11. Acceptance direction

Engineering must prove, with isolated deterministic tests and bounded real Provider tests only where needed:

1. Debug OFF has no visible panel and causes no extra Provider calls or gameplay mutation;
2. one accepted Turn can accumulate Narrative + World + Identity + Character + Experiences + People + Recommendations rows as real lanes finish;
3. legitimate no-change is visibly distinct from failure;
4. at least one controlled failure shows a human-readable sanitized reason;
5. recommendation malformed/provider/timeout-style paths are distinguishable without loosening the strict recommendation contract;
6. Restore clears/invalidates old current-session turn traces and records the Restore result;
7. stale callbacks cannot publish as current after replacement/Restore;
8. safe projections/diagnostics contain no hidden world/NPC/private canaries;
9. Debug ON/OFF toggling itself makes zero Provider calls and zero durable mutations;
10. normal Narrative/free-form/Save/Restore paths continue to work.

## 12. Product gate

Owner Product acceptance asks one simple question:

> **After a real turn, can I quickly tell what actually changed or failed in the backend without reading logs, while the normal game remains untouched when Debug is off?**

Package 1 closes only on explicit Owner Product PASS.
