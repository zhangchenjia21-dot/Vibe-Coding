---
title: my world｜G6 Package 2 Core Interaction Control v1.0 Decision
status: FROZEN / CURRENT
version: 1.0
created: 2026-09-08
updated: 2026-09-08
phase: G6 Package 2
owner: Owner + GPT
basis:
  - MY_WORLD_总体规划路线图_CURRENT.md@v4.4
  - experience/SILLYTAVERN_REFERENCE_IMPROVEMENT_DISCUSSION_2026-09-06.md P-13/P-17
---

# G6 Package 2｜Core Interaction Control

## 1. Product outcome

Package 2 adds two complementary controls without weakening free-form roleplay:

```text
角色行动
→ protagonist acts in the world
→ may invoke mechanics/world consequences
→ final accepted action may become Character evidence

OOC / GM 指导
→ player speaks to the GM out of character
→ influences current/recent play direction
→ is not protagonist action or World mutation

Recommended Actions
→ remain optional inspirations for 角色行动
→ reflect current player-safe Character as a soft tendency
→ never become a legal-action list
```

Package 2 is one Product gate but executes as two bounded implementation work items:

1. MW-024 — OOC / GM Guidance + typed accepted input mode;
2. MW-025 — Character-guided Recommendations + accepted-action Character evidence.

Owner UAT occurs once after both are reviewed/integrated unless a hard blocker makes the first task unusable.

## 2. Program-owned input mode

The input mode is explicit Program-owned structure, never inferred from prose.

v1.0 supports exactly:

- `action`
- `ooc`

GM-only opening remains its existing system-owned special case. Future `reality_correction` is not implemented now.

Backward compatibility:

- historical accepted entries with no mode and non-empty `player_text` normalize as `action`;
- historical GM-only opening with empty `player_text` remains opening semantics;
- no migration of old SQLite rows/tables is required merely to add the accepted-entry field.

Mode is part of accepted-version identity/currentness. Any accepted-prefix/hash/binding function that determines whether World/identity/curation/diagnostic material is current must distinguish `action` from `ooc`; switching mode with identical prose must not reuse displaced semantic/curation artifacts.

## 3. OOC / GM Guidance semantics

### 3.1 UI

Near the existing composer expose a simple explicit two-mode control:

`角色行动 | OOC / GM 指导`

Default on each normal gameplay interaction is `角色行动`.

The Player may type arbitrary natural-language OOC guidance. No keyword syntax such as `/ooc` is required.

Recommended-action click always means a role action: clicking a recommendation switches/ensures `角色行动` and prefills the paired draft; it never silently sends.

### 3.2 Accepted Conversation

OOC is still a durable accepted Conversation turn because the Player and GM should be able to reopen/Restore and see what guidance was given and how the GM responded.

An accepted OOC turn stores its exact mode alongside Player/GM text. Save/Restore/reopen/regenerate/correction preserve that mode/currentness.

Narrative UI renders OOC distinctly from role action, e.g. Player `OOC / GM 指导` and `GM · OOC`, while preserving raw accepted text bytes.

### 3.3 Provider context

Context Assembly uses the structural mode to mark OOC material explicitly as out-of-character guidance. It must not infer OOC from text.

For an active OOC request, the GM should answer out of character: acknowledge/clarify/adapt guidance without narrating new authoritative in-world events, forcing mechanics results, or pretending the protagonist acted.

Accepted recent OOC remains part of bounded recent Conversation context so the GM can respect the guidance for the current/recent stretch of play. No separate persistent preference owner is added; long-term Narrative Preference remains deferred.

### 3.4 Gameplay isolation

An OOC accepted turn must not itself trigger:

- Public d20/adjudication;
- World semantic mutation/materialization;
- actor identity materialization;
- Agency/Evolution progression;
- Character/Important Experiences/People curation as lived protagonist evidence.

These exclusions are structural by `input_mode`, not keyword/regex filtering.

OOC cannot directly change World Truth, NPC truth or mechanics. If the Player wants a different creative direction, the GM can apply that direction in later actual Narrative/action play; OOC itself is not a mutation API.

Action Recommendations may refresh after an accepted OOC/GM response using the ordinary one-call recommendation opportunity, because OOC may change what kinds of next role actions the Player wants. This is not an extra click-time call or a second recommendation lane.

## 4. Character ↔ Player Action ↔ Recommendations

### 4.1 Character-guided recommendations

The Action Recommender may consume:

- the same bounded recent player-visible accepted Conversation, with structural input modes preserved;
- current player-safe Character projection;
- the latest final accepted role action already present in that Conversation.

It must not consume raw `world_state`, private NPC material, Curator storage IDs, hidden model reasoning or unaccepted recommendation drafts.

Character is a soft behavioral/tendency input, not a whitelist. The model may recommend:

- actions consistent with established tendencies;
- reasonable experiments/deviations;
- growth/change when the scene supports it.

Program adds no personality score, trait classifier, keyword routing, quota or semantic diversity engine.

### 4.2 Same-turn timing

Do not introduce a Curator→Recommender blocking barrier or an extra recommendation call solely to wait for same-turn Character curation.

At recommendation request time use the current player-safe Character plus the latest accepted role action in the recent Conversation. Durable Character changes produced asynchronously by Curator influence subsequent recommendation requests once they become current.

This preserves foreground responsiveness and one recommendation call per ordinary opportunity.

### 4.3 Accepted Player action as Character evidence

Only a final accepted `action` turn may be treated as protagonist behavioral evidence by Information Curator.

The accepted Player action is evidence, not a deterministic personality mutation:

- Curator decides whether it materially changes current Character;
- ordinary acts may produce `character=null`;
- one unusual act does not mechanically overwrite established personality;
- repeated/meaningful accepted choices may legitimately shift personality/direction.

The following never become Character evidence merely by existing:

- recommendations shown but not sent;
- a recommendation draft clicked into the composer but not accepted;
- OOC guidance;
- cancelled/failed/unaccepted attempts;
- Program heuristic classifications.

## 5. Currentness / persistence

Accepted `input_mode` participates in the canonical accepted Conversation projection and version identity.

Required backward-compatible audit includes all current accepted-prefix/currentness owners such as:

- Conversation durable validation/projection;
- Context Assembly;
- World/identity accepted-prefix receipt functions;
- Information Curation prefix chain;
- Recommendation full-prefix currentness;
- Debug observability accepted token/currentness.

Do not duplicate multiple slightly different mode-hash rules. Prefer one stable accepted-entry normalization/version-material seam if the current architecture permits it without building a generic framework.

No new SQLite table is required. Existing Conversation persistence should carry the normalized accepted-entry dictionary if its current generic JSON owner supports that; if exact persistence code proves otherwise, STOP rather than silently redesigning storage.

Restore/regenerate/correction must not leave action-derived World/Character/People records current after the accepted turn changes from `action` to `ooc` or vice versa.

## 6. Debug Mode

Debug remains read-only.

For OOC turns it may show Narrative accepted/failed/cancelled and Recommendation terminal. Absence of World/Curator rows is legitimate because those lanes are structurally not scheduled.

Package 2 does not expand Debug into a new taxonomy or raw transcript inspector.

## 7. Explicit non-scope

Do not implement:

- Narrative Preference;
- Reality Correction;
- slash-command parser;
- generic Action Intent framework;
- OOC directly mutating World/People/Character;
- OOC bypassing mechanics on a role action;
- personality scores/traits as Program numbers;
- extra recommendation model call after Character curation;
- persistent OOC preference/settings;
- Open Threads/System/Inventory/Dynamic UI;
- Context Orchestrator or general Structured Output framework.

## 8. Package implementation train

### MW-024

Freeze and implement typed accepted input mode + OOC vertical:

- action/ooc UI;
- durable accepted mode/backcompat;
- context marking + OOC GM response;
- action-only world/mechanics/curation gating;
- Save/Restore/reopen/regenerate/currentness;
- recommendation opportunity remains available after OOC;
- Debug/currentness compatible.

### MW-025

Then implement:

- Recommender consumes current player-safe Character;
- mode-aware recent Conversation input;
- latest accepted role action as soft immediate evidence;
- Curator explicitly treats accepted `action` as behavioral evidence and skips `ooc`;
- no extra calls/barriers/scores.

## 9. Package Product gate

After both work items integrate, Owner verifies in one real run:

1. normal `角色行动` behaves exactly as before;
2. OOC can tell GM how to play the current/recent segment and receives an OOC response;
3. OOC creates no d20/world/Character/People mutation by itself;
4. subsequent normal play reflects the guidance naturally;
5. recommendations feel more like things the current protagonist might consider, while still allowing meaningful deviation/growth;
6. a genuinely repeated/meaningful accepted action can influence later Character and thus future recommendations, while one-off/unaccepted/OOC text does not mechanically rewrite personality;
7. Save/reopen/Restore preserve mode and currentness.

Package 2 closes only on explicit Owner Product PASS.
