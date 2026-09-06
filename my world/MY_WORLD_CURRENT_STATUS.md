---
title: my world｜当前状态
status: current-project-status
version: 15.6
created: 2026-08-26
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
current_task: MW-011 Revision 3 — Committed Profile Source + Reproducible Evidence
current_owner: ZCODE weekend implementation / GPT semantic-review lane
parent_task: G6 RPG Experience & Internal Declarative UI Host
semantic_owner: GPT
owner_uat_required: true
context_handoff: handoff/GPT_CONTEXT_HANDOFF_CURRENT.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜CURRENT STATUS

## 1. Stage state

```text
G1 Foundation                               PASS / CLOSED
G2 AI Conversation Spine                    PASS / CLOSED
G3 Persistence / Save / Timeline            PASS / CLOSED
G4 Primary Source Assets & Local Game       PASS / CLOSED
G5 World Semantics & GM Runtime             PRODUCT PASS / CLOSED
G5-GATE                                     PRODUCT PASS

G6 RPG Experience & Internal Declarative UI Host ACTIVE
MW-011 R1 G6 RPG Host ViewModel Baseline    ENGINEERING PASS / INTEGRATED
MW-011 R1 Owner UI UAT                      NOT PASS — Player Host still too thin
MW-011 R2 Player Profile Surface            IR#2 NOT PASS — REVISION 3 REQUIRED
MW-011 R3 Committed Profile Source Fix      READY FOR ZCODE
MW-012 Zhang Chen Player Character Card     ENGINEERING PASS / INTEGRATED
```

G5 remains closed. G6 remains active.

## 2. Current implementation / governance heads at IR#2

Implementation main reviewed before the R2 candidate:

`my-world@6968e137e210430bab37e0a9bdbb74c346ba8bfa`

R2 candidate reviewed:

`my-world@09de33c3ac34485b7a5ec7e807d0e2464d04c7ca`

Governance main reviewed:

`Vibe-Coding@5f53983ef641aa6e95bad1a8cb80668ed5d2459c`

Formal R2 review:

`my-world/docs/mw011/MW-011_INDEPENDENT_REVIEW_IR2.md`

Revision 3 addendum:

`my-world/docs/tasks/MW-011_REVISION3_COMMITTED_PROFILE_SOURCE_AND_REPRODUCIBLE_EVIDENCE_ADDENDUM.md`

## 3. MW-011 lineage

```text
MW-011 Revision 1 / IR#1 = ENGINEERING PASS / INTEGRATED
MW-011 Revision 1 Owner UI UAT = NOT PASS
MW-011 Revision 2 / IR#2 = NOT PASS
MW-011 Revision 3 = same-outcome correction, not a new Work ID
```

R1 remains technically valid for its frozen boundary. The Owner UAT proved the left Player Host still did not communicate rich Character definition at a fresh opening.

R2 architecture remains accepted in principle:

```text
optional bounded Character Card v0.2 player_profile
→ frozen Game-local selected Character projection
→ separate fail-closed Player Character Profile Projection
→ MW-011 presentation-only ViewModel
→ rich bounded Player Host
```

Do not revert to raw Character `semantic_sections` or broaden MW-009.

## 4. IR#2 blocking finding

The R2 candidate evidence claimed that Zhang Chen's product package was updated to v0.1.1 with a seven-group `player_profile`, but the exact pushed candidate does not contain that file change.

Independent GitHub inspection shows:

```text
base 6968e137.../tests/fixtures/mw012/汉末三国/张琛/source.json
candidate 09de33c3.../tests/fixtures/mw012/汉末三国/张琛/source.json
```

are the same blob:

`d403e2ba6eae139257fbb8cfe02acfefd2c988ba`

The committed package remains:

```text
version = 0.1.0
no player_profile field
```

while the candidate publish script expects `VERSION = 0.1.1`.

Consequences:

- a clean candidate cannot display the intended Zhang Chen rich Player Host;
- the production publish script and committed package are version-inconsistent;
- the recorded successful v0.1.1 publication/fingerprint is not reproducible from the candidate;
- the focused test's reported 45/0 result is not reproducible from a clean checkout because that test loads the real Zhang Chen package and requires `player_profile.headline` + seven groups.

Most likely, testing/publication used dirty or otherwise uncommitted local Source bytes. The reviewed candidate itself is therefore not acceptable.

## 5. MW-011 Revision 3 required outcome

R3 is bounded to candidate integrity, not architecture redesign:

```text
commit the missing Zhang Chen v0.1.1 + player_profile Source bytes
+ make package / publish-script version identity consistent
+ rerun focused + regressions + export from the exact clean candidate HEAD
+ run bounded production publication from those exact committed bytes
+ record the exact resulting fingerprint and correct changed-file list
```

Final pre-evidence state must include:

```text
git rev-parse HEAD
git status --short   # empty
```

The production fingerprint must be recomputed from final committed bytes; do not force preservation of the previously reported `0b6cb72a...` value.

## 6. R2 mechanism pieces to preserve

The following reviewed direction is accepted pending clean proof:

- Character Card v0.2 optional `player_profile`, backward compatible when omitted;
- bounded `headline / summary / groups` validation;
- selected Character projection carries the validated profile through normal Final Create ancestry;
- old Games do not live-fetch/backfill new Source generations;
- separate fail-closed Player Character Profile Projection;
- no `semantic_sections`, `catalog_summary`, GM/private or Source-current fallback;
- MW-009 remains the current Player-known-facts owner;
- ViewModel remains presentation-only;
- Player Host renders profile before World/recent-action/session material;
- Player Host may scroll vertically; Narrative remains desktop primary at the established 60% stretch;
- no new stat system, Inventory mechanics, generic UI DSL, Mod schema, Provider summarization or SQLite table.

## 7. Zhang Chen R3 consumer requirement

The committed Source must faithfully present the already accepted MW-012 semantics only.

Required profile concepts/order:

```text
24岁 · 现代穿越者
退役武警义务兵 / 985高校出身 / 历史与军事爱好者

背景
性格
能力
局限
初始目标
行为原则
随身物品
```

No new powers, equipment, local relationships, guaranteed future history, automatic famous-person recognition or preselected allegiance/self-rule outcome.

## 8. G6 protected order

G6 remains consumer-first:

```text
Runtime projection
→ presentation-only ViewModel
→ real UI consumer
→ Runtime Asset Resolution only for actual visual consumers
→ portrait / scene / authored-map presentation
→ Character / Relationship / Inventory / Faction / Map / Save real surfaces
→ Expansion mechanic-state consumer
→ Internal Declarative UI Host v0.1
→ bounded Action Intent
→ responsive / Theme / navigation
→ Owner UAT / visual polish
```

External Mod/Creator UI contracts remain G8 work.

## 9. Protected G5 semantics

- free-form Narrative remains primary and is not gated by semantic/Knowledge/Agency/Evolution extraction;
- World Truth != actor Knowledge != human-player disclosure;
- stable NPCs may act independently;
- World Evolution may hold or selectively advance;
- Public d20 remains program-owned mechanics grounding rather than a second world truth;
- Save/reopen/Restore currentness remains authoritative;
- Literary Style Reference remains expression-only;
- raw accepted Narrative bytes remain authoritative; Markdown-lite remains disposable UI projection.

## 10. Routing

Owner weekend override remains active through **2026-09-06 23:59 (+08:00)**:

```text
Zcode + GLM-5.3-flash → primary implementation owner for new code-changing work
GPT                    → semantics / architecture / task shaping / Independent Review
```

At **2026-09-07 00:00 (+08:00)**, absent a new Owner instruction, routing returns to Codex-backend / Kimi-frontend according to the implementation seam.

Keep the active MW-011 revision worktree through GPT IR#3.

## 11. Immediate route

```text
Zcode executes MW-011 R3 bounded correction
→ push exact clean candidate
→ GPT IR#3 on actual GitHub diff + candidate bytes + test evidence
→ if PASS, integrate
→ publish verified current Zhang Chen generation from reviewed bytes
→ Owner creates a fresh Zhang Chen Game
→ Owner UI UAT on rich Player Host
→ continue next real G6 consumer / visual vertical
```
