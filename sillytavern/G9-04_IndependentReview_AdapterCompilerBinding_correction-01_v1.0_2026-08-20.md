---
title: G9-04 Adapter / Compiler / Binding Independent Review correction-01
status: current-review-correction-required
version: 1.0
date: 2026-08-20
stage: G9-04
---

# G9-04｜Adapter / Compiler / Binding｜Independent Review correction-01

## 1. Review Identity

```text
Formal Code Base
5da2294a9d21585665167e69307d9c693427582d

Codex Task Packet
8b4d1c1fa4848f55b0a82520255a9d735529f018

Reviewed / Tested Implementation
892867e72f44cb97557b944f7149d650d89a0abe

origin/main during review
5da2294a9d21585665167e69307d9c693427582d
```

Review range：`5da2294a... → 892867e7...`，strict fast-forward ancestry，main 未被 Agent 修改。

## 2. Verdict

```text
P0 = 0
P1 = 3

G9-04 implementation = FAIL / CORRECTION REQUIRED
G9-04 CLOSED = NO
G9-05 AUTHORIZED = NO
main = unchanged at 5da2294a9d21585665167e69307d9c693427582d
```

Parser、真实资产读取、G9-03 validate/hash、real SQLite bootstrap/no-phantom/Save-Restore 方向基本成立；阻断集中在 Library 与 primary binding compiler 的 frozen boundary。

---

## 3. P1-01｜Library 被错误写入 Game-local binding lineage

Frozen G9-04：

```text
primary binding anchors
= selected World + Character + Expansion only

Library G9-04 scope
= parse / validate / canonical round-trip / cross-reference proof
!= Game-local canonical truth
```

当前 `compileAssetBindingPlan()`：

```text
snapshots
= world + characters + expansions + libraries
↓
sourceDescriptors = all unique selected
↓
bindingAnchors = sourceDescriptors.map(...runtime.asset-binding...)
```

因此只要 Manifest `libraries` 非空，Library 就获得 `AdditionalGameLocalAssetDefinition` hidden anchor，并进入 G9-02 `sourceLineage`。

Portable positive test还明确期待：

```text
sourceDescriptors = world + character + expansion + library
```

这与 current spec DEC-12 / Binding Compiler Foundation 第 3、5 条冲突。

Correction：

- Manifest Library snapshot 可以被 exact validate；
- 但不得进入 primary `sourceDescriptors / bindingAnchors / runtimeDefinition.additionalGameLocalAssets`；
- portable test 必须在 `libraries != []` 时证明 runtime binding 仍只有 World / Character / Expansion 三个 primary anchors；
- Real Gate 继续证明三个 primary lineage exact。

---

## 4. P1-02｜Library relatedRefs 没有 cross-reference validation

Frozen Library Exit 明确要求：

```text
stable entry + provenance + audience + cross-reference validation
```

当前 portable fixture 只断言：

```text
relatedRefs == ['character:fixture-explicit']
```

但 Adapter/Compiler 没有验证该 ref 是否 exact resolve 到 catalog assetRef /允许的同 Library entryRef；把它改成不存在的 ref 仍会通过 G9-03 structure validator。

Correction 必须新增 AI-independent exact cross-reference validator：

- Library `relatedRefs` 只做 exact identity lookup；
- 至少允许 resolve 到当前 validated catalog assetRef，及同一 Library 中 stable entryRef（若实现需要）；
- unknown target → `LEGACY_REFERENCE_UNRESOLVED` 或等价稳定细分错误；
- 禁止 fuzzy / title / filename / alias resolution；
- positive + unresolved negative tests。

Library validator 不得因此获得 Runtime mutation/retrieval authority。

---

## 5. P1-03｜Duplicate primary 被静默 dedupe

当前 compiler：

```text
selected = Manifest snapshots
uniqueSelected = selected.filter(findIndex(...))
```

相同 primary snapshot 重复出现时被静默吞掉，而不是 `BINDING_DUPLICATE_PRIMARY`。

同一 logical primary `assetType + assetRef` 若同时 pin 两个 version/hash，还会生成相同：

```text
asset-binding:<assetType>:<assetRef>
```

但 compiler 在新增 anchors 内部没有先 fail closed。

Correction：

- World singleton保持 exact；
- Character / Expansion selected primary logical identities 必须唯一；
- exact duplicate snapshot → `BINDING_DUPLICATE_PRIMARY`；
- same logical primary + different version/hash → `BINDING_DUPLICATE_PRIMARY`；
- 禁止用 silent dedupe 修正 Manifest；
- Library 不属于 primary anchor cardinality。

---

## 6. Non-blocking findings

以下本轮没有形成额外 P1：

- YAML 使用成熟 `yaml` parser，不是手写 happy-path parser；
- assetRef 来自 explicit profile；
- full legacy body private-by-default；
- public selector exact heading/level；
- critical legacy relation exact mapping；
- Adapter 输出复用 G9-03 validator / digest；
- real asset gate锁定 assets HEAD 与 blobs；
- Character no-phantom vertical成立；
- Expansion Host proof明确 proof-only；
- Provider calls = 0；
- G9-02/G9-03/G8 production contracts未修改。

Correction 后必须重新 exact-SHA review 全 diff。

---

## 7. Next

```text
same branch
agent/g9-04-adapter-compiler-binding
↓
Codex correction-01
↓
focused + real asset + full regression
↓
new exact SHA
↓
GPT Independent Review
```

禁止 rebase / amend / force push / new correction branch / push main / G9-05 scope。