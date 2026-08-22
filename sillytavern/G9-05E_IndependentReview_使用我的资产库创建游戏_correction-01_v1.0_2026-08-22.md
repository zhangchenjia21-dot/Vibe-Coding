---
title: G9-05E 使用我的资产库创建游戏 Independent Review correction-01
status: current-review-rework
version: 1.0
date: 2026-08-22
stage: G9-05
slice: G9-05E
formal_code_base: dd67bd9c02f42717dab139e9c87b4fe7e25f0fc6
reviewed_implementation: d0071141b54877853a2b0abadb110b7dcbc29d45
---

# G9-05E｜Independent Review｜correction-01 v1.0

## 1. Review Target

```text
Repository
zhangchenjia21-dot/sillytavern

Formal Code Base
dd67bd9c02f42717dab139e9c87b4fe7e25f0fc6

Task Branch
agent/g9-05e-use-my-assets-game-creation

Task Packet Commit
540fdf5d72208a932d8f0656ff43ffea8e89d89c

Reviewed Implementation SHA
d0071141b54877853a2b0abadb110b7dcbc29d45
```

Branch ancestry at review time:

```text
main = dd67bd9c02f42717dab139e9c87b4fe7e25f0fc6

agent/g9-05e-use-my-assets-game-creation
= main + Task Packet + one implementation commit
```

`main` remains unchanged.

GitHub returned no external CI status and no workflow run for the reviewed SHA. This review therefore does not claim external CI green.

## 2. Verdict

```text
P0 = 0
P1 = 2

G9-05E Semantics          NOT CLOSED
G9-05E Implementation     REWORK REQUIRED
G9-05F Expansion Creator  NOT AUTHORIZED
```

The implementation direction is substantially correct and does not require architectural restart. Rework is limited to exactly-once response-loss recovery and explicit player Source selection.

## 3. Confirmed Correct / No New P0-P1

The reviewed SHA correctly establishes the following important G9-05E rails:

- `use_my_assets` is wired as a real product path while `ai_assisted` remains available;
- inventory is sourced from `SourceAssetLibraryStore` and carries exact `assetRef + assetType + version + contentHash`;
- World selection is exact and does not silently rebind to Source v2;
- Character roles are typed as `bound_only | opening_character | player_character`;
- `player_character` requires `playerCharacterSupported === true`;
- zero-Character Source path works with explicit local player identity;
- `bound_only` Character enters Manifest/binding but does not materialize a Runtime Character;
- `opening_character` materializes only at Final Create into the explicit opening scene;
- hard dependency closure is checked against the actually selected set, not merely Source Library existence;
- hard Library dependency fails closed in v1;
- `validateAssetCatalog()` and `validateGameAssetManifest()` are reused;
- Source→Game-local materialization is Program-owned and deterministic;
- topology remains minimal: one region / one opening place / one opening scene; no prose ontology guessing;
- public Source sections and private Source sections are separated into player-safe setup fields vs server-side private storage;
- `compileAssetBindingPlan()` is reused; G9-04 hidden binding anchors are not reimplemented;
- Runtime bootstrap is only started after the intent enters `creating`;
- asset-created games enter existing Runtime / My Games / Session / Save / Restore rails;
- Source v2 append does not mutate an already-created v1 lineage;
- no new G9-03 wire, G9-04 binding compiler, Creator Draft/Source Store, Runtime Store, Provider, Expansion Creator, or Library product implementation was introduced.

## 4. P1-01｜Response-loss replay is not exactly-once from the original create request

### 4.1 Required semantics

Canonical G9-05E requires:

```text
bootstrap completed but response interrupted
→ same creationRef retry
→ return the same game
```

Task Packet A-10 also requires:

```text
editing
→ persist creating(gameId + fingerprint + exact intent)
→ bootstrap
→ created

retry:
- already created → same game replay
- bootstrap exists / finalize interrupted → recover same game
- same creationRef + different fingerprint → fail closed
- double click → never creates a second game
```

### 4.2 Current implementation

`AssetGameCreationFlow.createGame()` begins with:

```text
requireAtRevision(creationRef, basisRevision)
```

A successful create transitions:

```text
editing revision N
→ creating revision N+1
→ created revision N+2
```

If the final HTTP success response is lost, the browser still owns the original create request basis `N`.

A retry of that same request reaches:

```text
requireAtRevision(..., N)
```

while durable state is already `created @ N+2`, therefore it throws `ASSET_GAME_CREATION_STALE_REVISION` before `replayCreated()` can run.

The existing positive replay test does not prove the required failure mode: it calls the second create with `first.intent.revision`, i.e. the already-returned **new created revision**, not the original pre-response revision.

### 4.3 Product-level trap

The HTTP server does include `latestRevision` and latest `workspace` on `AssetGameCreationError`, but `HttpProductSessionClient.requestDto()` discards the typed error body and throws only `ProductSessionClientError(message, code)`.

`AssetGameWizard.createGame()` catches the error and leaves the old pre-create workspace unchanged.

Therefore the actual browser loop after response loss is:

```text
POST create @ revision N
→ server creates game, persists created N+2
→ response lost
→ browser still has N
→ retry POST create @ N
→ 409 STALE
→ latest workspace discarded by client
→ browser still has N
→ repeated retry remains stale
```

This is a Gate failure for AC-E-13.

### 4.4 Required correction

Implement a safe replay identity for Final Create. The correction may choose the exact code shape, but it must prove:

1. the original Final Create request can be retried after response loss without knowing the post-create revision;
2. a retry corresponding to the same exact intent/fingerprint returns the same game;
3. a stale request for a different prior intent cannot be silently mapped to a game created from another selection;
4. `creating` with Runtime already present finalizes/replays the same game;
5. no second game is created;
6. browser client consumes the recovery result or latest workspace instead of becoming permanently stale.

Preferred design is an explicit Program-derived create fingerprint/replay token carried by the workspace/create request, or an equivalently strict revision/fingerprint proof. Do not solve this by globally weakening CAS or accepting arbitrary stale revisions.

Required negative/positive tests:

```text
editing N
→ issue Final Create request using N
→ durable created N+2
→ simulate lost response
→ repeat the original create request identity
→ 200 replay same game
→ game count stays 1
```

and:

```text
stale/different exact intent identity
→ cannot replay someone else's/current created selection
→ fail closed
```

A product/client test must cover the response-loss recovery path, not only direct Core replay with the newest revision.

## 5. P1-02｜Source-player mode implicitly selects `eligible[0]`

### 5.1 Required semantics

G9-05E freezes:

```text
用户明确选择本局资产与角色用途
```

and for versions:

```text
assetRef + assetType + version + contentHash
```

must be the exact selected identity. When multiple versions or multiple candidates exist, Program must not choose an asset for the user.

### 5.2 Current implementation

In `AssetGameWizard` → `PlayerStep`, clicking:

```text
使用已发布的玩家角色卡
```

executes:

```text
const first = eligible[0];
if (first !== undefined) onSource(first exact snapshot)
```

This is not merely switching UI mode. It writes a concrete Character Source selection into the authoritative Intent before the player has chosen a specific card/version.

With multiple eligible characters or multiple eligible versions, ordering decides which exact Source becomes `player_character`.

That violates explicit exact selection and can materialize a player Character the user never selected.

### 5.3 Required correction

Separate UI mode selection from authoritative Source selection.

Required behavior:

```text
click "使用已发布的玩家角色卡"
→ only expose/select the Source-player chooser UI
→ authoritative Intent remains without a player_character until a concrete exact card/version is clicked
```

Only clicking a specific candidate may call the Core mutation that writes `role = player_character`.

Local-player mode may continue to be an explicit authority switch, but Source mode itself must not auto-pick identity or version.

Required UI tests must include at least:

```text
2 eligible Character identities
or
1 eligible identity with 2 versions
```

and prove:

- switching to Source mode performs zero authoritative Character selection;
- both exact choices are visible;
- only clicking one exact card/version changes the Intent;
- the non-clicked candidate is never silently selected.

## 6. Correction Scope

Allowed:

- `AssetGameCreationFlow` Final Create/replay semantics;
- asset game product request/DTO needed for strict create replay identity;
- `HttpProductSessionClient` recovery handling for asset game creation;
- `AssetGameWizard` Source-player chooser behavior;
- focused Core / HTTP / UI tests;
- minimal package script adjustment only if required by tests.

Do not reopen:

- G9-03 protocol wire;
- G9-04 `compileAssetBindingPlan()` semantics;
- Creator Core / World Creator / Character Creator;
- Runtime formal-turn semantics;
- Expansion Creator;
- Library product/retrieval;
- autonomous Provider or AI-assisted asset materialization.

## 7. Re-review Gate

Correction is PASS only when:

```text
P0 = 0
P1 = 0
+
original-create-request response-loss replay proven
+
strict different-intent/fingerprint fail-closed proven
+
no duplicate game proven
+
Source-player mode no longer chooses eligible[0]
+
exact Source card/version click is the only authoritative Source-player selection
+
G9-05E focused backend + HTTP + UI tests PASS
+
G8 / G9-03 / G9-04 / G9-05B/C/D regressions remain green
```

Until then:

```text
sillytavern/main remains
= dd67bd9c02f42717dab139e9c87b4fe7e25f0fc6

G9-05F remains NOT AUTHORIZED
```
