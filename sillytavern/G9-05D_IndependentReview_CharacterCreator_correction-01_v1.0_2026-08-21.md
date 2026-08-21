---
title: G9-05D Character Creator Independent Review correction-01
status: current-review-correction-required
version: 1.0
date: 2026-08-21
stage: G9-05
slice: G9-05D
reviewed_implementation: 077f191a25416f1e27a9a1a528427c7ac9529bd6
---

# G9-05D｜Independent Review｜Character Creator correction-01

## 1. Verdict

```text
Reviewed Implementation SHA
= 077f191a25416f1e27a9a1a528427c7ac9529bd6

P0 = 0
P1 = 2

G9-05D Character Creator = CORRECTION-01 REQUIRED
G9-05E Use My Assets Game Creation = BLOCKED / NEXT AFTER D PASS
G9-05F Expansion Creator = DEFERRED / NOT AUTHORIZED
```

`agent/g9-05d-character-creator` 在审核时与 reviewed SHA 完全一致；`sillytavern/main` 仍精确为 `1b79323bb53b5fb243465294a50c9d0b3f63dac8`，未提前集成。

GitHub 对 reviewed SHA 未返回外部 CI status，因此本审核不声称 CI green。审核依据为 exact diff、冻结规格、当前实现、测试代码与现有协议/运行时边界交叉检查。

## 2. 已通过部分

本轮主体方向成立，不要求重做：

- G9-05D0 exact typed operations：`set/clear_metadata_aliases`、`set/clear_character_player_supported`；
- aliases / player-supported 的 runtime parse、apply、inverse、fingerprint、ChangeSet / Undo；
- Character import：displayName / metadata.aliases / playerCharacterSupported / section，继续 evidence-backed + blank-only + partial apply；
- Character Creator blank / `.md/.txt` import / exact Source revision；
- Character Workspace 基础档案、三态 PC support、sections、referenceSources、dependencies；
- Character Product dependency 只允许 `hard | optional | reference`，拒绝 `feature_conditional` / `sourceScope`；
- Character exact AI scope 与 ignored operation 可见性；
- explicit Publish / Source list-detail-history-revision；
- published Character + synthetic hard target 的 `validateAssetCatalog()` 正向证明；
- SQLite reopen；
- No-Provider manual path；
- HTTP No-Phantom 正向证明：Creator 流程前后 Runtime session revision / turn 不变，Character Source identity 不进入当前 session；
- Character UI 是独立人物档案工作区，不是 World composition 复制。

## 3. P1-01｜Character Import Organizer 缺少 Character-only Program Gate

### 3.1 事实

`DeepSeekCharacterCreatorProviderAdapter.author()` 在调用 Provider 前读取 Draft 并检查：

```text
draft exists
+ exact basisRevision
+ assetType === character
```

但同一个 Character-specific adapter 的 `organize()` 没有执行对应 Draft type / revision gate，直接调用 Character import organization tool。

冻结规格明确要求：

> Character adapter 只对 `assetType = character` 工作。

当前生产启动通过 `TypedAssetCreatorProvider` 按 Draft type 分发，正常 Product 路径多数情况下被外层保护；但 Character adapter 本身是公开导出的专用边界，不能依赖某个具体 bootstrap wrapper 才成立。

### 3.2 风险

若 Character adapter 被直接作为 `CreatorImportOrganizer` 注入或未来另一装配路径绕过 `TypedAssetCreatorProvider`：

```text
World Draft
→ Character organizer
→ metadata / section 等跨类型合法 assignment
→ Shared Import Core 可能接受
```

这违反 type-specific Provider contract，并会让“入口明确选择资产类型”在 adapter 边界失效。

### 3.3 Required Fix

- 将 Character draft existence / exact revision / assetType gate 抽成共用 helper；
- `author()` 与 `organize()` 都在 Provider call 前调用；
- wrong type / missing / stale 必须 fail closed；
- wrong-type `organize()` 必须证明真实 Provider call = 0、Draft 不变；
- 不修改 Shared Creator Core，不修改 G9-03。

## 4. P1-02｜stale revision 刷新会覆盖基础字段未保存输入

### 4.1 事实

Character UI 的顶层 `run()` 遇到：

```text
CREATOR_DRAFT_STALE_REVISION
CREATOR_AI_STALE_RESULT
```

会主动重新 `loadCharacterDraft()` 并 `setView(latest)`。

Character Workspace 的 title / displayName / aliases / summary / language / version 使用本地受控 state；其 `useEffect` 在 `view` 更新后会把这些本地 state 全部同步回服务端值。

因此基础字段出现 stale CAS 时：

```text
玩家输入新值
→ blur 保存
→ server 返回 STALE
→ UI reload latest Draft
→ useEffect 用 server value 覆盖玩家刚刚失败的本地输入
```

这违反 G9-05D §6 / Task Packet UI expectation：mutation 失败或 stale revision 时，不得清空玩家未成功提交的表单内容。

### 4.2 当前测试缺口

现有 UI 测试“mutation 失败不清空未提交输入”只覆盖：

```text
新增章节表单
+ generic Error
```

该路径不会触发 stale reload，也没有覆盖基础字段本地 state 被 `useEffect` 覆盖的真实风险。

### 4.3 Required Fix

- 保留 CAS，不允许 last-write-wins；
- stale 时可以刷新 server freshness / revision，但必须保留本次失败的本地输入；
- 用户应看见冲突/刷新提示，并能基于最新 revision 明确重试或编辑；
- 不允许静默自动提交失败值；
- 至少对 displayName/title/aliases 中一个基础字段增加真实 stale UI test：
  - 输入本地新值；
  - mutation 返回 `ProductSessionClientError(CREATOR_DRAFT_STALE_REVISION)`；
  - `loadCharacterDraft()` 返回更新后的 server Draft；
  - 页面仍保留玩家刚才失败的输入；
  - 最新 revision 被用于后续显式重试；
  - 不削弱已有 section/new-node generic failure 输入保留。

## 5. Non-Issues / No Rework

以下不得借 correction-01 重构：

- G9-05D0 operation model；
- Source Store / publication transaction；
- Character dependency policy；
- referenceSources model；
- `playerCharacterSupported` 三态语义；
- No-Phantom 架构；
- World Creator；
- G9-03 protocol；
- Runtime；
- G9-05E / G9-05F。

## 6. Exit Gate

```text
P1-01 Character organizer type/revision gate closed
+
P1-02 stale UI preserves failed local input without weakening CAS
+
focused regression tests PASS
+
main remains unchanged
↓
return new exact implementation SHA for Independent Review
```

只有新 exact SHA 达到 `P0=0 / P1=0`，G9-05D 才可 PASS/CLOSED，并按已冻结阶段重排授权 G9-05E【使用我的资产库】创建游戏。