---
title: my world｜G4 Two Primary Asset Families Reality Test v0.1
status: current-canonical-product-test-decision
created: 2026-09-02
updated: 2026-09-02
phase: G4 Primary Source Assets & Local Game Creation
parent_task: G4-11 Two Primary Asset Families Reality Test
semantic_owner: GPT
owner_value_gate: OWNER
---

# G4 Two Primary Asset Families Reality Test v0.1｜Canonical Decision

## 1. Purpose

G4-11 is the final core-product reality pressure before G4-GATE.

It answers one product question:

> **Can the same Host create and sustain two genuinely different RPG worlds from two full-fidelity Primary Source families, without cross-family leakage or collapsing them into the same generic AI-chat texture?**

Visual asset runtime is explicitly excluded by the Owner's 2026-09-02 deferral decision.

## 2. Test families

Use the two already-proven full-fidelity Source families.

### Family A — historical / low-magic

```text
World:      汉末三国：天下未定
Entry:      208｜赤壁前夕
Player:     刘备
Expansion:  none
```

### Family B — high-magic / fantasy

```text
World:      埃瑟维亚：诸界余辉
Entry:      t0-1287-ovista
Player:     莉维娅·塞兰
Expansion:  none
```

`莉维娅` 的 fixed-1287 profile authoredly covers all three 1287 entries, including `t0-1287-ovista`.

Do not add Guaranteed NPCs merely to make the test look richer. The purpose is to isolate World + Player Character family identity.

## 3. Controlled variables

For engineering/provider evidence, use the **same current selected runtime model profile** for both Games through the canonical shared runtime adapter.

Do not modify the Owner's persisted Runtime Model Settings solely for this test.

Both Games use `Expansion = none` because Public d20 value/semantics have already passed Owner UAT. Reintroducing it here would add a confounding variable rather than prove family distinction.

No portrait / scene / authored-map requirement exists.

## 4. Required vertical for each family

Each family must traverse the real product/runtime seams:

```text
exact full-fidelity Source generations
→ explicit composition
→ atomic Final Create
→ independent Game / One Game = One SQLite
→ real Provider Opening
→ at least 2 accepted continuation turns
→ durable Conversation / Game progression
→ Save
→ Main Menu / switch Game
→ reopen / Continue
→ another accepted continuation
```

The task may use automation/harnesses for engineering proof, but it must exercise production seams rather than a fake parallel RPG implementation.

## 5. Engineering invariants

### INV-11-01 — Independent Game truth

The two Games must have different Game identities and independent SQLite files. Switching A→B→A must not carry current Session/Conversation/Source identity across Games.

### INV-11-02 — Exact Source ancestry

Each Game must retain exact Source generation provenance for its selected World and Character.

A Source current update in a task-owned library must not silently change the already-created Game's semantic ancestry/materialized starting truth.

### INV-11-03 — No cross-family Context leakage

Han Provider Context must not contain Afterglow Source bytes/identity; Afterglow Context must not contain Han Source bytes/identity except where the test harness itself labels evidence outside model-visible Context.

### INV-11-04 — T0 quarantine remains intact

Do not expose unselected/later Entry/T0 answers merely to strengthen the test. Existing T0-scoped projection rules remain authoritative.

### INV-11-05 — Model Freedom remains intact

Do not add output-format validators, genre keyword gates, required vocabulary, or scripted-beat checklists to force the two worlds to look different.

The distinction must arise from real Source + current Game reality, not from a post-generation classifier gate.

### INV-11-06 — No visual dependency

Portraits, scene images and authored maps are absent from acceptance. Their absence is not a failure and must not block play.

### INV-11-07 — No premature G5

Do not implement NPC autonomous world turns, faction simulation, event engine, relationship state machine, knowledge graph or other G5 semantics just to increase world differentiation.

G4-11 tests the G4 product we actually built.

## 6. Product Value Acceptance

Engineering can prove isolation, provenance, real Provider use, durability and reopen/switch correctness.

Engineering **cannot** declare the product-value question passed.

Owner UAT must judge whether the two actual play experiences are materially distinct.

Positive signals include, without becoming hard format requirements:

- the Han game naturally reflects late-Han institutions, political pressures, historical social texture and 刘备-specific identity;
- the Afterglow game naturally reflects 1287 magic/material life, Ovista/international-fantasy pressures and 莉维娅-specific professional/personality texture;
- choices and descriptions that make sense in one world do not casually appear in the other;
- neither game feels like the same generic fantasy/adventure assistant with names swapped.

Automatic rejection signals:

- cross-family Source/identity leakage;
- one Game's Conversation appearing in the other;
- Source current silently mutating an existing Game;
- both Openings/continuations are materially generic because required Source/context never reaches the Provider;
- Save/reopen/switch loses or contaminates current reality.

Owner is not asked to judge visual polish.

## 7. Execution route

```text
G4-11P1 Two-Family Reality Prep / Engineering Proof — CODEX
↓
GPT Independent Review
↓
G4-11UAT Owner Two-Family Reality Test — OWNER
↓
GPT closeout
↓
G4-GATE
```

If G4-11UAT passes, GPT may close G4 and begin G5 route shaping. If it fails, correct only the concrete Source/Context/Game seam exposed; do not reintroduce deferred visual work as a substitute for world differentiation.
