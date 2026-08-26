---
title: G9-05F Expansion Creator Independent Review correction-01
status: current-review-rework
version: 1.0
date: 2026-08-22
stage: G9-05
slice: G9-05F
formal_code_base: f1ec971b09dc9ed6dc59474f2c8ad1137e0f2e26
reviewed_implementation: f0e809424b78f3c9ea0a736fb758289289b731d2
reviewed_branch: agent/g9-05f-expansion-creator
p0: 0
p1: 6
---

# G9-05F｜Expansion Creator｜Independent Review correction-01

## 1. Verdict

```text
Reviewed Implementation SHA
f0e809424b78f3c9ea0a736fb758289289b731d2

P0 = 0
P1 = 6

G9-05F0 / G9-05F = CORRECTION-01 REQUIRED
Primary Asset End-to-End Closure Gate = NOT AUTHORIZED
```

Formal Code Base：

```text
f1ec971b09dc9ed6dc59474f2c8ad1137e0f2e26
```

Task Packet：

```text
agent tasks/G9-05F_Grok_ExpansionCreator产品纵向执行包_v1.0_2026-08-22.md
packet commit: 1de5c3c8dbf16c4a2a27fac13c894117e698a235
```

任务分支相对 Formal Code Base 为纯向前后代，仅包含 Task Packet + 一次实现提交。审核锚点 PR #3 仅用于读取 private task branch exact `head_sha`，随后关闭；`merged=false`，未改变 main。

GitHub 对 reviewed SHA 未返回 external combined status，也无 workflow run；因此本审核不声称 CI green。Independent Review 基于 exact diff、冻结规格与提交测试代码进行。

---

## 2. 已通过的主体

以下实现方向成立，不要求重做：

- Expansion Draft 继续复用 G9-05B Core，没有第二套 Draft / Store / Source / Protocol；
- Import union 已支持 `expansion.packageRef / ownerNamespace / dependency / feature / module / UI surface / UI contribution`；
- import 保持 evidence、blank-only、semantic identity collision → unresolved/conflict、Program-minted nodeRef；
- `assertCreatorPublicationHostGate()` 复用 `validateAssetCatalog()`、default enablement、`resolveActiveExpansionModuleBindings()` 与真实 `RuntimeDomainModuleHost.validateBindings()`；
- 首次 editable Publish 的顺序是 compile → Host Gate → preparePublishing → append；
- 手工 Module / UI Contribution 编辑会检查 Program Catalog；
- feature/module 被引用时删除 fail closed；
- `feature_conditional` 只在 Expansion 产品中开放，非法 scope 最终由 G9-03 fail closed；
- Expansion-specific Provider 的 `author()` / `organize()` 共用 exact Expansion type+revision preflight；
- Source revision 映射保留 Expansion features/modules/UI/dependencies/sections；
- No-Provider 手工路径、Source list/detail DTO、G9-05E exact Expansion selection interoperability 已有正向测试；
- Publish 不主动激活 Runtime 的设计成立。

---

## 3. P1-01｜正式组合根没有接入 Program Host / Capability Truth

测试 harness 显式把同一套：

```text
AssetProtocolHostRegistry
RuntimeDomainModuleHost
CreatorProgramCapabilityCatalog
```

传给 `CreatorDraftFlow`、`CreatorPublicationService` 与 `ExpansionCreatorProductApi`。

但正式 `startG2LocalServer()` 仍然：

```ts
new CreatorDraftFlow(...)
new CreatorPublicationService(creatorFlow, sourceAssets)
new ExpansionCreatorProductApi(..., { aiAvailable })
```

没有 production capability catalog / protocol host / runtime module host 注入 seam；Runtime bootstrap 同样没有传 `domainModuleHost`。

因此正式产品默认为空 Catalog/Host：合法的 Program-owned `runtimeModuleRef` 在真实 Product Publish 也会变成 unknown，且 Creator 无法读取已注册能力。测试证明了 Gate 函数，但没有证明正式产品接线。

### Required closure

建立一个 Program-owned capability truth 的组合入口，使 Creator Import / Product Catalog / Publication Host Gate / Runtime Host 使用同源注册事实；不得把 `tests/g9/统一资产协议语料.ts` 的 `PROTOCOL_HOST` 硬编码进 production。

---

## 4. P1-02｜AI Authoring 的 Catalog Gate 与 exact-scope 产品能力不完整

手工 Product API 和 Import apply 都会检查 Program-owned refs，但 `CreatorDraftFlow.author()` 当前只执行 `authoringScopeDenial()`；被授权编辑某个 Module / UI Contribution 后，AI 可以把任意 `runtimeModuleRef / config.schemaRef / projectionRef / hostCapability` 写入 Draft，直到 Publish 才失败。

同时前端 Scope Picker 只暴露 scalar / feature / module；没有 section / dependency / UI surface / UI contribution。Provider 的 bounded context 也未包含授权的 UI nodes。

这不突破最终 Source Host Gate，但违反：

```text
AI 不得发明 Program-owned ref
invalid sibling op must be ignored at edit time
exact field/node scope must cover Expansion workspace
```

### Required closure

- AI result 在写 Draft 前对 Program-owned refs 使用同一 Catalog policy；合法 sibling apply、非法 sibling ignored；
- Scope UI 覆盖 scalar / section / dependency / feature / module / UI surface / UI contribution；
- Provider bounded context 只包含本次被授权节点，并包括 UI nodes；
- 不新增 arbitrary patch/path。

---

## 5. P1-03｜Expansion Import Review continuation locator 不完整

F0 已产生新的 formal targets：

```text
dependency:<dependencyRef>
expansion_feature:<featureRef>
expansion_module:<moduleRef>
expansion_ui_surface:<surfaceId>
expansion_ui_contribution:<contributionRef>
```

但 UI 仍只复用 scalar/section locator。Conflict 的上述 target 无法定位；unresolved 更直接把 `item.itemRef` 传给 locator，而不是 formal target。

因此 Import Review 无法按规格“回到相关 dependency / feature / module / UI node 继续创作”。

### Required closure

新增 Program-owned exact Expansion locator：只按当前 workspace 的 semantic identity 解析 formal target，不按 label/title 猜测。必须覆盖 dependency/feature/module/UI surface/UI contribution；missing target 显示不可定位且不得写入。Unresolved 必须携带或确定性关联合法 formal target，而不是把 arbitrary itemRef 当 locator。

---

## 6. P1-04｜stale CAS 失败输入保留只覆盖 title

父层 stale 会 reload latest workspace。当前只有 `metadata.title` 通过 `staleTitle` overlay 保留失败输入；`summary / language / targetVersion / packageRef / ownerNamespace` 会被 `useEffect` 用服务端值覆盖。已有 Feature/Module 等 node editor 同样会在 latest view 到达后覆盖本地未成功保存的数据。

现有 UI test 只证明 title。

### Required closure

使用 Draft-local dirty overlay 或等价机制：

```text
server latest revision refresh
+
failed local field/node input preserved
+
explicit retry uses latest revision
```

成功保存后清 overlay；切换到另一 Draft 时清理。至少测试一个非 title scalar + 一个 existing structured node 的真实 stale refresh/retry。

---

## 7. P1-05｜Expansion Source Detail UI 没有展示规格要求的 Source truth

Product DTO 已提供完整 Source detail，但正式 UI 当前只显示 title、assetRef、packageRef、ownerNamespace、version 与版本历史。

缺少：

- exact contentHash / snapshot identity；
- Features + defaultEnabled；
- Modules + runtimeModuleRef / defaultEnabled（及必要配置摘要）；
- dependencies；
- UI surfaces / contributions；
- semantic sections（至少应可检查 Source 声明）。

这使玩家无法在“拓展包”正式详情页检查自己实际发布的声明。

### Required closure

补齐只读 Source detail；明确这些是 **Source defaults/declarations**，不是当前某局 enablement 或 Runtime state。不得把当前游戏状态混入 Source 页面。

---

## 8. P1-06｜`publishing` recovery 会绕过当前 Host Gate

首次 publish 会先 Host Gate，再进入 `publishing`。但如果在 `preparePublishing` 成功后、Source append 前崩溃，重启后 `publish()` 对 `publishing` 直接调用 `resume()`；`resume()` 重新编译后直接 `commit()`，没有再次执行 Host Gate。

因此存在：

```text
Host A PASS
→ publishing 持久化
→ crash before append
→ restart under Host B（module/schema/projection 已不可用）
→ resume
→ Source append without current Host Gate
```

这违反 F0 的“Source append 前必须通过 Program-owned Host Gate”。

### Required closure

Expansion `resume()` 在 append 前必须再次调用与首次 Publish 相同的 Host Gate。测试：Host A 进入 publishing 后模拟中断；Host B 缺能力时恢复 fail closed、Source unchanged；恢复 Host A 后同一 intended snapshot 可继续完成。World/Character recovery 不得回退。

---

## 9. Correction Gate

```text
P0 = 0
P1 = 6

Only same-branch correction is authorized.
main remains f1ec971b09dc9ed6dc59474f2c8ad1137e0f2e26.
```

correction-01 关闭全部 P1 后，重新执行 exact-SHA Independent Review。只有 `P0=0 / P1=0` 才允许 G9-05F PASS/CLOSED、main fast-forward 和 Primary Asset End-to-End Closure Gate。
