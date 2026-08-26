---
title: G9-05E 使用我的资产库创建游戏 Independent Review correction-02
status: correction-required
version: 1.0
date: 2026-08-22
stage: G9-05
slice: G9-05E
reviewed_implementation: abab72d184a65d9b26e8ffdc7ef363f5ad141a9f
reviewed_branch: agent/g9-05e-use-my-assets-game-creation
---

# G9-05E｜Independent Review｜correction-02 v1.0

## 1. Verdict

```text
Reviewed Implementation SHA
= abab72d184a65d9b26e8ffdc7ef363f5ad141a9f

P0 = 0
P1 = 2

G9-05E = CORRECTION-02 REQUIRED
G9-05F Expansion Creator = NOT AUTHORIZED
```

correction-01 的两个原始 P1 已关闭：

1. Final Create 现在使用 Program-owned `createFingerprint`；同 `creationRef + same fingerprint` 可在 `creating/created` 状态下以原 pre-response `basisRevision` 重放/恢复同一 game；mismatched fingerprint fail closed。
2. 玩家点击“使用已发布的玩家角色卡”只打开 chooser，不再执行 `eligible[0]`；只有点击具体 exact Character snapshot 才写入 `player_character`。

GitHub 未对 reviewed SHA 返回 external CI status / workflow run。本轮判断基于 exact-SHA diff、canonical spec、focused tests 与产品投影逐项复核，不声称 external CI green。

## 2. P1-01｜Expansion 多版本 UI 仍按 stable assetRef 投影选择

当前 `ExpansionStep` 对每个 inventory entry 使用：

```text
intent.expansionSelections.find(entry.snapshot.assetRef === expansion.assetRef)
```

作为 `selected`。

因此若 Source Library 同时存在：

```text
tavern.expansion.weather@1.0.0
tavern.expansion.weather@2.0.0
```

用户明确选择 v1 后，v2 card 也会因为 stable `assetRef` 相同而被投影为 `checked=true`。

更严重的是，v2 card 下的 feature/module toggle 展开后使用 `...selected` 提交；这里的 `selected.snapshot` 可能仍是 v1，于是用户在 v2 UI 上操作却继续修改 v1 binding。

这违反 G9-05E frozen rule：

```text
assetRef + assetType + version + contentHash
= authoritative exact selection identity
```

以及 AC-E-05 的 exact Expansion resolved binding。

### Required closure

Expansion UI 的 selected identity 必须同时匹配：

```text
assetRef
assetType
version
contentHash
```

行为要求：

- v1 selected → only v1 checked；v2 unchecked；
- clicking v2 explicitly swaps authoritative selection to v2 through existing Core upsert；
- after server workspace returns, v1 unchecked / v2 checked；
- feature/module controls only bind to the exact selected version；
- unselected version must not expose/mutate another version's binding；
- no latest/first-version guessing；
- compatibility review displays exact Expansion version + enabled features + enabled modules。

## 3. P1-02｜Other Character 多版本 UI 仍按 stable assetRef 投影 role

当前 `OthersStep` 对每个 Character inventory entry 使用：

```text
intent.characterSelections.find(entry.snapshot.assetRef === character.assetRef)
```

作为 `selected`。

若同一 Character 有 v1/v2，明确选择 v1 后，两张卡都会显示同一 role 状态。

同时，未真正被选中的 v2 card 上“`不加入`”仍执行 `removeCharacter(assetRef)`；这会删除实际选中的 v1，因为 Core 以 logical assetRef 去重/移除。

这会让 UI 的 exact-version 投影与 authoritative Intent 不一致。

### Required closure

Other Character UI selected identity 必须匹配完整 exact snapshot。

行为要求：

- v1 selected as `bound_only/opening_character` → only v1 card shows that role；
- v2 card stays unselected；
- clicking v2 role explicitly swaps selection to v2；
- clicking “不加入” on an already-unselected exact version is no-op/disabled and must not remove selected sibling version；
- clicking “不加入” on the actually selected exact version removes that logical Character selection；
- compatibility review displays exact Character version + role。

## 4. Already PASS / Do Not Rework

以下保持 PASS，correction-02 不得重做：

- published Source inventory only；
- Draft excluded；
- exact World version selection；
- Source-player explicit chooser + exact version selection；
- Program-owned createFingerprint + response-loss original request replay；
- hard dependency selected-set closure；
- no latest guessing in Core；
- player eligibility；
- zero-character local player；
- bound_only no materialization；
- opening/player explicit materialization；
- deterministic Source→Game-local compiler；
- public/private boundary；
- exact Manifest + existing G9-03 validators；
- existing G9-04 compileAssetBindingPlan reuse；
- exactly-once Runtime bootstrap；
- My Games / Session；
- Save / Continue / Restore lineage；
- Source v2 does not mutate existing v1 game；
- Provider calls = 0。

## 5. Gate

correction-02 必须新增至少以下 UI proofs：

```text
Expansion A v1 + A v2
→ select v1
→ only v1 selected
→ v2 controls cannot mutate v1
→ explicitly select v2
→ authoritative binding becomes exact v2

Character A v1 + A v2
→ select v1 bound_only
→ only v1 role selected
→ v2 "不加入" does not remove v1
→ explicitly select v2 opening_character
→ authoritative selection becomes exact v2

Review
→ selected Character version + role visible
→ selected Expansion version + enabled feature/module visible
```

P0=0 / P1=0 后才允许：

```text
G9-05E PASS / CLOSED
→ fast-forward main exactly to reviewed SHA
→ G9-05F Expansion Creator AUTHORIZED / NEXT
```
