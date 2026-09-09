---
title: my world｜当前状态
status: current-project-status
version: 17.25
created: 2026-08-26
updated: 2026-09-09
phase: G6 RPG Core Closure + UAT Observability + Internal Dynamic UI
current_task: MW-030 Internal Dynamic UI Host v0.1 + Model-curated Visibility Preference
current_owner: Codex
current_dispatch_state: AUTHORIZED / TASK SHAPED / NO IMPLEMENTATION RETURN YET
parent_task: G6 Package 6 Internal Dynamic UI Host v0.1
semantic_owner: GPT
owner_uat_required: deferred / Package 7 concentrated Reality Gate
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
current_roadmap: MY_WORLD_总体规划路线图_CURRENT.md@v4.8
active_uat_record: my world/docs/uat/G6_PACKAGE2_OWNER_UAT_U1.md@v1.6
active_architecture: my world/architecture/ui/G6_INTERNAL_DYNAMIC_UI_HOST_V0_1_DECISION.md@v1.0
active_task_packet: my-world/docs/tasks/MW-030_INTERNAL_DYNAMIC_UI_HOST_TASK.md
active_task_branch: mw-030-internal-dynamic-ui-host
formal_code_base: 396bfcc0c91cdff6e6816795826b95fa0c0d358c
governance_base_at_task_shape: 88c8581cbd02823e53e72e7fb2d28f089b4da5fb
task_packet_commit: bd4f2a49f3a44df7aa148d5437e51e90dc18f483
reviewed_implementation_main: 396bfcc0c91cdff6e6816795826b95fa0c0d358c
mw029_review: my-world/docs/mw029/MW-029_INDEPENDENT_REVIEW_IR1.md
mw029_integration: my-world/docs/mw029/MW-029_INTEGRATION_VERIFICATION.md
---

# my world｜CURRENT STATUS

## 1. Stage state

```text
G1 Foundation                               PASS / CLOSED
G2 AI Conversation Spine                    PASS / CLOSED
G3 Persistence / Save / Timeline            PASS / CLOSED
G4 Primary Source Assets & Local Game       PASS / CLOSED
G5 World Semantics & GM Runtime             PRODUCT PASS / CLOSED

G6 RPG Core Closure + UAT Observability + Internal Dynamic UI ACTIVE
```

Current G6 flow:

```text
Package 0  Correction Train + Owner UAT          PRODUCT PASS / CLOSED
Package 1  UAT Observability / Debug Mode v0.1   PRODUCT PASS / CLOSED
MW-023     Gameplay Typography Readability        PRODUCT PASS / CLOSED
Package 2  Core Interaction Control              ENGINEERING COMPLETE / CORE OUTCOME ACCEPTED / PRODUCT CONFIRMATION DEFERRED
  MW-024   OOC / GM Guidance                     ENGINEERING PASS_WITH_NOTES / INTEGRATED
  MW-025   Character-guided Recommendations      ENGINEERING PASS_WITH_NOTES / INTEGRATED
  MW-026   Package-2 UAT Cleanup                  ENGINEERING PASS_WITH_NOTES / INTEGRATED
Package 3  Open Threads / 事务                    ENGINEERING PASS_WITH_NOTES / INTEGRATED / PRODUCT CONFIRMATION DEFERRED
  MW-027   Open Threads / 事务                    ENGINEERING PASS_WITH_NOTES / INTEGRATED
Package 4  System / Public d20                   ENGINEERING PASS_WITH_NOTES / INTEGRATED / PRODUCT CONFIRMATION DEFERRED
  MW-028   System / Public d20 Surface            ENGINEERING PASS_WITH_NOTES / INTEGRATED
Package 5  factual Inventory / 行囊               ENGINEERING PASS_WITH_NOTES / INTEGRATED / PRODUCT CONFIRMATION DEFERRED
  MW-029   Factual Inventory Vertical             ENGINEERING PASS_WITH_NOTES / INTEGRATED
Package 6  Internal Dynamic UI Host v0.1         CURRENT / MW-030 TASK SHAPED + AUTHORIZED / CORE REQUIRED
Package 7  V0 Core Closure Reality Gate          QUEUED
```

## 2. Owner route instruction — concentrated UAT remains active

Owner explicitly instructed:

> **“UAT就以后再UAT吧，这次先跳过了”**

Formal interpretation:

- do not stop the Core-first train for standalone Package-2/3/4/5 Product confirmation;
- do not fake Product PASS where experiential evidence is deferred;
- Engineering Review/integration evidence remains authoritative;
- deferred Product evidence moves into the Package-7 concentrated Reality Gate unless Owner asks sooner;
- Package 6 is implemented before returning to Owner for the full V0 Core Reality Gate.

Product confirmation is **deferred, not waived**.

## 3. MW-029 — REVIEWED / INTEGRATED

Lineage:

- Formal Base: `f6aae06f6be3be4b7fd24762a10e524b6eb9b683`
- Task Packet / Starting: `574f6ecff87c401d35a8d9e5b95f9edbfecc4fd6`
- Production Implementation: `5ea0ed9e8826aa51f4900e8e96b10e8cf0c67f0e`
- Submitted Final Candidate: `4c2529ab845db056ef291843a31314e77813ac91`
- Independent Review: `1ba4b2a80e72114ba2ace3917bf1679bfa0b0b48`
- reviewed/integrated current implementation main: `396bfcc0c91cdff6e6816795826b95fa0c0d358c`

Verdict:

**ENGINEERING PASS_WITH_NOTES / INTEGRATED / PRODUCT CONFIRMATION DEFERRED**.

Independent Review verified:

```text
accepted Narrative factual possession
→ existing World semantic call
→ bounded ADD / UPDATE / REMOVE
→ Program-owned item identity + request-only refs
→ version-bound Inventory events in existing World/Timeline owner
→ current factual Inventory fold
→ 行囊 Surface
→ current Inventory grounding in ordinary/OOC/Public-d20 foreground
→ Debug inventory lane
→ Save / reopen / Restore / Regenerate currentness
```

Evidence:

- focused: 174 checks / 0 failures;
- real-window: 174 checks / 0 failures at 960×540 / 1280×720 / 1920×1080;
- 41 directly relevant regression suites pass;
- exact-baseline G3-03 / G3-05 Context failures reproduced and remain retained debt;
- Godot 4.7.2 import + fresh Windows export + `ValidateExportOnly` pass;
- real Provider calls: 0.

Remaining Product evidence: real-model possession extraction judgment and natural GM use during long play.

## 4. Package 6 architecture — FROZEN / CURRENT

Current decision:

`architecture/ui/G6_INTERNAL_DYNAMIC_UI_HOST_V0_1_DECISION.md@v1.0`

This **supersedes** the pre-consumer:

`G6_INTERNAL_DECLARATIVE_UI_HOST_V0_1_DECISION.md@0.1`.

The old `MW-013` Task Packet remains historical / held and **must not be executed**.

Product outcome:

```text
Character / Important Experiences / People / Open Threads / Inventory / System
→ domain-owned player-safe DTOs
→ first-party bounded presentation adapters
→ one shared Internal Dynamic UI Host
→ Godot Controls
```

Current internal vocabulary is intentionally bounded to proven needs such as:

```text
section
text
fact_list
field_list
card
existing collapsible presentation
```

Definitions are disposable internal presentation material. They are not Gameplay/Timeline truth, a Runtime query language, or an external Mod UI contract.

Protected boundaries:

- renderer never receives omniscient `world_state` to locally decide disclosure;
- no arbitrary GDScript callback / method name / NodePath / expression / SQL / OS command / Provider call / arbitrary resource path;
- no new semantic domain or Provider call;
- Character / Experiences / People / Threads / Inventory / System semantics remain owned by existing domains;
- Narrative/composer, Save, Debug and inline d20 remain imperative;
- external Source/Expansion/Mod UI declaration remains G8;
- generic Action Intent remains Deferred.

## 5. Owner-approved visibility preference — ACTIVE IN MW-030

Broader authority:

`architecture/ui/G6_MODEL_CURATED_SURFACE_VISIBILITY_PREFERENCE_DEFERRED_DECISION.md@v1.1`

People specialization:

`architecture/ui/G6_PEOPLE_CARD_VISIBILITY_PREFERENCE_DEFERRED_DECISION.md@v1.1`

Frozen principle:

> **Model decides what is semantically worth retaining; Player has final control over which retained model-curated units are visible in their own interface.**

MW-030 v0.1 implements this only where legitimate stable identity already exists:

```text
People
Important Experiences
→ opaque presentation key
→ 隐藏 / 已隐藏(N) / 恢复显示
```

Hide semantics:

- presentation only;
- never deletes semantic information;
- never becomes model importance feedback;
- hidden information may continue updating;
- model update does not auto-unhide;
- survives reopen;
- Restore does not rewind visibility preference;
- preference storage is Game-local but outside Timeline gameplay truth;
- no Provider calls or gameplay mutation from hide/recover.

Deliberately non-hideable in v0.1:

- Open Threads: no stable Thread item identity yet; no title/text/index identity hacks;
- Character: no stable group/item presentation identity yet;
- System/Public d20: authoritative mechanics;
- Inventory: factual gameplay state;
- Debug/errors/Recommendations.

## 6. CURRENT — MW-030

Formal Code Base:

`my-world/main@396bfcc0c91cdff6e6816795826b95fa0c0d358c`

Governance Base at Task Shape:

`Vibe-Coding/main@88c8581cbd02823e53e72e7fb2d28f089b4da5fb`

Task branch:

`mw-030-internal-dynamic-ui-host`

Required worktree:

`D:/AI/Projects/.worktrees/my-world/mw-030-internal-dynamic-ui-host`

Task packet:

`docs/tasks/MW-030_INTERNAL_DYNAMIC_UI_HOST_TASK.md`

Task packet commit:

`bd4f2a49f3a44df7aa148d5437e51e90dc18f483`

Current dispatch meaning:

> **AUTHORIZED / TASK SHAPED / NO IMPLEMENTATION RETURN YET**

`current_owner: Codex` is authorization/responsibility, not evidence that a Codex process is currently running.

Implementer return ceiling:

**READY FOR INDEPENDENT REVIEW**.

## 7. MW-030 critical review gates

Independent Review must verify:

- one actual shared production Host, not merely duplicated dictionary conventions;
- real production migration of multiple existing surfaces, with semantic/display equivalence;
- safe L3 DTO → adapter → definition → Host boundary; no raw Runtime/world in leaf renderer;
- closed vocabulary validation/fail-soft behavior;
- People default-collapse and current right-nav behavior preserved;
- System exact d20 facts, Inventory factual possessions and Threads model semantics preserved;
- People/Experience stable **opaque** presentation identity, never display-name/text/index identity;
- hide/recover affects presentation only and underlying safe projection remains intact;
- hidden semantic updates do not auto-unhide;
- visibility survives reopen and does not rewind on Restore;
- recovery shows current content;
- System/Inventory/Threads/Character do not accidentally gain generic hide controls;
- hide/recover causes zero Provider calls and zero gameplay durable mutations;
- 960×540 / 1280×720 / 1920×1080 >=20px and operable;
- no external UI schema, Action Intent or new gameplay persistence owner;
- Package 0–5 adjacent regressions remain intact except exact-baseline retained debt.

## 8. Next route after MW-030

After reviewed MW-030 integration:

```text
Package 7  V0 Core Closure Reality Gate
→ Owner concentrated 20–30 turn real play
→ close deferred Package 2–6 Product evidence together
→ G6 Exit only on explicit Owner V0 Core Game Loop PRODUCT PASS
```

Do not insert G3 debt cleanup, Visual Runtime, external UI protocol, Creator, Context Orchestrator or discretionary polish unless a real Package-6 blocker emerges or Owner explicitly changes route.

## 9. Retained debt / deferred notes

- G3-03 / G3-05 Context assertions remain exact-baseline retained debt for relevant later Context work;
- existing teardown/resource warnings remain non-blocking baseline evidence;
- Application Shell decomposition remains evolutionary; Package 6 may extract presentation components but must not become a Shell rewrite;
- long-session Context Orchestrator / general Structured Output Reliability remain G7;
- Source-authored initial Inventory remains future explicit Source-contract work;
- Thread hide waits for legitimate stable Thread identity;
- Character per-item hide waits for legitimate stable presentation identity;
- Visual Runtime waits for real first-party authored visual demand.

## 10. Protected invariants

- Model Freedom First;
- Reversibility over prevention;
- free-form natural-language role action remains primary;
- World Truth != actor Knowledge != human-player disclosure;
- UI/Debug is projection, never second truth;
- Save / Restore / Regenerate currentness remains authoritative;
- OOC is guidance, not mutation;
- Public d20 Program RNG/no-reroll truth remains authoritative;
- factual Inventory is explicit possession truth, not Program prose inference;
- visibility preference never becomes semantic curation feedback;
- no fake state is introduced merely to fill UI.
