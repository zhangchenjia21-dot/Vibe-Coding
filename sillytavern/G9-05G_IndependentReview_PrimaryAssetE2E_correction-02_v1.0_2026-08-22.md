---
title: G9-05G｜Independent Review｜Primary Asset E2E｜correction-02
status: current-review-correction-required
version: 1.0
updated: 2026-08-22
reviewed_repository: zhangchenjia21-dot/sillytavern
reviewed_branch: agent/g9-05g-primary-asset-e2e
reviewed_exact_sha: d560b6213676d292ca64c49d20f77d442b339b9b
formal_code_base: 26d23d47c5f5ac42d3e1029654a64eda831c4db1
supersedes_review: G9-05G_IndependentReview_PrimaryAssetE2E_correction-01_v1.0_2026-08-22.md
---

# G9-05G｜Independent Review｜Primary Asset E2E｜correction-02

## 1. Verdict

```text
P0 = 0
P1 = 1

P1-01 = FIXED
P1-02 = FIXED
P1-03 = CORRECTION REQUIRED

G9-05G0 = CORRECTION-02 REQUIRED
G9-05G  = CORRECTION-02 REQUIRED

sillytavern/main = KEEP UNCHANGED
```

Reviewed exact implementation SHA：

```text
d560b6213676d292ca64c49d20f77d442b339b9b
```

Correction history：

```text
a9a3bb0a534efaab6db00f2c3a62efcc19818946  reviewed implementation before correction-01
↓
b9697da6af865f8752a038881a349e0a74a91274  correction-01 packet only
↓
d560b6213676d292ca64c49d20f77d442b339b9b  correction-01 implementation
```

`main` 在本轮审查时仍为：

```text
26d23d47c5f5ac42d3e1029654a64eda831c4db1
```

GitHub combined status 对 reviewed SHA 返回空 `statuses: []`，因此本审查不声明 CI green。当前 GPT 环境无该仓库 checkout，独立本地命令记为 `NOT RUN`；本 verdict 来自 exact-SHA diff、current specs、production code path 与测试代码的独立静态审计。

---

## 2. P1-01｜FIXED｜Capability foundation exact ownership gate

Correction 已将 activation gate 收口为同时匹配：

```text
packageRef = package:EP-CHAR-CORE
featureRef = feature:character-capability
moduleRef = builtin:character-capability.evidence.v1
packageIncluded = true
featureEnabled = true
moduleEnabled = true
```

并增加 canonical constant：

```text
CHARACTER_CAPABILITY_PACKAGE_REF = package:EP-CHAR-CORE
```

`tests/g9/人物能力证据RuntimeBinding测试.test.ts` 的正向 fixture 已改为 canonical package；新增 foreign-package negative 证明：相同 runtime module、相同 feature、三 flags 全 true，但 `packageRef=package:foreign-capability` 时 binding 可以合法进入 RuntimeDefinition，而 capability foundation record / metadata 均为 0。

该 P1 判定 FIXED。

---

## 3. P1-02｜FIXED｜Historical proof Source revision under default production Host

Correction 没有把 G9-04 proof module 重新注册进 production Host。

Primary E2E 已改为：

```text
E2E protocolHost = createG2CreatorProgramHost().protocolHost
```

且 production Host 仍只暴露正式 built-ins，不含：

```text
builtin:g9-04.ep-char-core-proof.v1
```

Creator Publication Gate 新增 candidate-current / historical-source-catalog 分层：

- candidate Expansion：current Program Host strict；
- immutable historical Source：继续验证 structure / integrity / identity / dependency / cycle，但 stale Program refs 不阻断合法新版 candidate；
- current candidate unknown runtime ref 仍 fail closed。

Primary real E2E 已证明：historical `EP-CHAR-CORE@0.1.5` 留库且仍含 proof ref 时，默认 production stack 可通过 shared source_revision 发布 `0.1.6`；`0.1.5` exact snapshot 保持 immutable；另有 unknown candidate runtime ref 的 Publish FAIL negative。

该 P1 判定 FIXED。

---

# 4. P1-03｜Expansion Publish 在 Source append 后 / Draft finalize 前 crash 时无法 exactly-once resume

## 4.1 Frozen invariant

Creator Core 的跨阶段冻结不变量仍包括：

```text
Draft → explicit Publish → append-only Source
Save / Crash / Resume / Recovery exactly-once
```

Expansion Creator 不能因为增加 Host Gate 而失去 already-appended exact candidate 的恢复能力。

目标恢复语义：

```text
Draft lifecycle = publishing
+
intended exact Source snapshot 已 append 成功
+
process crashes before finalizePublished
↓
retry same publishing lifecycle
↓
revalidate candidate under current Host
↓
recognize exact Source already exists
↓
no second append
↓
finalize same Draft as published
```

## 4.2 Current exact code path

`src/资产创作/L3_外交层/资产创作公开接口.ts`：

```text
publish
→ preparePublishing
→ commit
   → library.readExact(snapshot)
   → appendValidated(asset)      // durable Source write
   → finalizePublished(...)      // separate Draft write
```

因此真实 crash window 存在：Source append 已成功，但 Draft 仍停在 `publishing`。

`resume()` 会在 `commit()` 之前重新调用：

```text
assertCreatorPublicationHostGate({
  asset,
  catalog: currentSourceCatalog(),
  ...
})
```

此时 `currentSourceCatalog()` 已包含本次 exact candidate。

Correction-01 的 Gate 当前执行：

```text
historicalSourceSnapshots = Set(input.catalog.map(snapshotKey))
validateAssetCatalog([input.asset, ...input.catalog], ...)
```

于是同一 exact candidate 同时出现为：

```text
input.asset
+
input.catalog 中 already-appended exact Source
```

`validateAssetCatalog()` 在 Host validation 前先执行 `assertIdentityUniqueness()`；相同 `assetRef + version + digest` 的 duplicate 会抛 `DUPLICATE_ASSET_REF`。

结果：

```text
append succeeded
→ crash
→ Draft = publishing
→ retry
→ Publication Gate duplicate fail
→ commit(existing exact → finalize) 永远不可达
```

这违反 Creator Publish exactly-once recovery。

## 4.3 Existing tests do not close this window

`tests/g9/ExpansionCreator导入发布基础测试.test.ts` 当前 recovery proof 只模拟：

```text
preparePublishing persisted
→ crash BEFORE append
```

随后 Host B fail-closed / Host A recover。

该测试没有覆盖：

```text
append durable success
→ crash BEFORE finalizePublished
```

所以当前绿路径不能证明 P1-03。

---

# 5. Required correction

只修 Publication Gate / recovery 的 exact-candidate duplicate seam，不重写 correction-01 已通过内容。

必须满足：

1. candidate 永远不是 historical；即使 exact candidate 已存在于 Source Library，retry 时仍必须按 current Program Host strict validate；
2. historical catalog validation 输入不得把“本次已 append 的 exact candidate 副本”再次加入 `[candidate, ...historical]`；
3. 只能排除与 candidate **完整 exact snapshot** 相同的 catalog entry：

```text
assetRef
assetType
version
contentHash
```

4. 同 `assetRef + version` 但不同 `contentHash` 的冲突 Source 不得被过滤，必须继续 fail closed；
5. current candidate unknown runtime ref 在 already-appended recovery 情况下仍必须 fail closed；
6. retry 成功时 Source 仍只有一份 exact snapshot，Draft lifecycle 变为 `published`，`replayed=true`；
7. 不注册 historical proof module，不删除/改写 historical 0.1.5，不改变 Game Manifest active validation。

推荐最小实现方向：在 Creator Publication Gate 内先从 historical catalog 中剔除与 `input.asset` 完整 exact snapshot 相同的 entry，再构造 historical snapshot set；不要把 candidate 本身标成 historical。

---

# 6. Required regression

至少新增 Expansion Publish recovery regression：

```text
1. legal Expansion candidate under Host A
2. preparePublishing
3. appendValidated(candidate) durable success
4. inject crash before finalizePublished
5. assert Draft lifecycle = publishing
6. assert Source exact candidate already exists exactly once
7. retry under Host B where candidate current runtime module is unavailable
   → PUBLISH_INVALID_DRAFT
   → proves already-appended candidate is still current-host strict
8. retry under original Host A
   → replayed=true
   → Draft published
   → Source still exactly one exact candidate
```

可通过包装 `appendValidated()`：先调用真实 append，再抛一次模拟 crash，制造“append 后 / finalize 前”窗口；不要发明第二套 recovery mechanism。

还应保留已有 crash-before-append regression。

---

# 7. Scope / Gate

不得重做：

- P1-01 exact package ownership；
- P1-02 historical proof migration split；
- Primary real corpus / exact blob Gate；
- routing / projection；
- Save/Restore；
- Game/Turn crash recovery；
- Source 0.1.7 isolation；
- Product/HTTP create→session。

本轮 correction 完成后必须重新跑 correction packet 指定的 focused/full commands；无法执行的命令必须显式 `NOT RUN`。

只有 P0=0、P1=0 后才允许把 `main` exact fast-forward 到 reviewed SHA。
