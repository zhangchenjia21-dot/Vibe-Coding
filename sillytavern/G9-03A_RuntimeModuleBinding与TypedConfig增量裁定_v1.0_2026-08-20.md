---
title: G9-03A Runtime Module Binding 与 Typed Config 增量裁定
status: current-decision-addendum
version: 1.0
date: 2026-08-20
stage: G9-03
supplements:
  - G9-03_UnifiedAssetReferenceProtocol规格_v1.0_2026-08-20.md
---

# G9-03A｜Runtime Module Binding 与 Typed Config 增量裁定 v1.0

## 0. 触发原因

G9-03 implementation exact-SHA Independent Review 在 `3f86c6fe0a45ef3c8412dc9a38455ef32098f298` 发现：Source Expansion 的 `moduleRef`、`runtimeModuleRef` 与 G9-02 `RuntimeDomainModuleBinding.moduleRef` 的语义没有被 v1.0 规格写得足够精确，导致实现虽然 DTO 形状通过测试，却无法直接通过真实 `RuntimeDomainModuleHost.validateBindings()`。

本增量不回开 G9-02；它只补齐 G9-03 Source → Runtime Binding 的 identity translation，并强化 frozen typed-config / conditional-dependency gate。

---

## 1. 正式 Identity 分层

永久区分：

```text
ExpansionModuleV1.moduleRef
= Source-side module declaration identity
= 在该 Expansion Source 内稳定、唯一

ExpansionModuleV1.runtimeModuleRef
= Program-owned built-in Runtime module identity
= 必须解析到 G9-02 RuntimeDomainModuleDescriptor.moduleRef

RuntimeDomainModuleBinding.moduleRef
= Program Runtime module identity
= runtimeModuleRef
```

因此：

```text
Source moduleRef
!= Runtime binding moduleRef
```

除非某个具体资产恰好选择了相同字符串；不得依赖字符串偶然相等。

### 1.1 G9-02 映射

v1 mapping 必须是：

```text
Expansion packageRef
→ RuntimeDomainModuleBinding.packageRef

Expansion module.featureRef
→ RuntimeDomainModuleBinding.featureRef

Expansion module.runtimeModuleRef
→ RuntimeDomainModuleBinding.moduleRef
```

`ExpansionModuleV1.moduleRef` 继续用于：

- Source declaration identity；
- Game Asset Manifest `enabledModuleRefs`；
- Source config / UI / dependency sourceScope 引用；
- compiler diagnostics / provenance。

它不直接冒充 Program module identity。

### 1.2 Program Owner 不可被 Source 覆盖

`ExpansionAssetPayloadV1.ownerNamespace` 是 Source semantic namespace / package ownership declaration；它不得覆盖 `RuntimeDomainModuleDescriptor.ownerNamespace`。

真实 Runtime owner 继续由 Program-registered module descriptor 决定。

```text
Source ownerNamespace
!= permission to rewrite Runtime ownerNamespace
```

### 1.3 同一 Program module 的 active cardinality

G9-02 Host 对 `RuntimeDomainModuleBinding.moduleRef` 要求唯一。因此同一个 Game resolved active set 中：

```text
两个 enabled Source modules
→ same runtimeModuleRef
= invalid active binding set
```

Validator / resolver 必须 fail closed，而不是生成重复 Runtime binding。

同一 Expansion 可以声明多个 Source module 指向同一 `runtimeModuleRef`，但它们不得同时 active；这种形态只适合互斥 / alternative configuration。

---

## 2. Real Host Vertical Proof

G9-03 不允许只证明“对象长得像 RuntimeDomainModuleBinding”。

Independent Review correction 必须新增真实 G9-02 Host 纵向证明：

```text
validated TavernAssetV1 expansion
+
Program-registered RuntimeDomainModule descriptor / fixture module
↓
resolve RuntimeDomainModuleBinding
↓
RuntimeDomainModuleHost.validateBindings()
↓
PASS
```

至少证明：

1. `binding.moduleRef` 命中真实 Program module identity；
2. Feature 必须在 descriptor.supportedFeatureRefs 中；
3. unknown runtimeModuleRef fail closed；
4. duplicate active runtimeModuleRef fail closed；
5. Source `ownerNamespace` 不能改写 Program descriptor owner。

不得修改 G9-02 Host 来迁就错误 Source mapping。

---

## 3. Typed Config Gate 强化

G9-03 v1.0 已冻结：

```text
schemaRef known
+ Program-owned validator PASS
→ config allowed
```

正式补充：

```text
schemaRef merely listed as known
+ no validator
!= valid config
```

任何使用 `config` 的 module 都必须找到 exact `schemaRef` 的 Program-owned validator，并且 validator 返回 `true`。

否则 fail closed：

- validator missing → `UNKNOWN_CONFIG_SCHEMA`（或更细稳定错误）；
- validator returns false → `INVALID_CONFIG_VALUE`。

`AssetProtocolHostRegistry.configValidators` 对被使用的 schema 不得是可选语义。

---

## 4. feature_conditional sourceScope 必须可验证

`feature_conditional` 只对 Expansion Source 有意义。

Validator 必须证明：

1. source asset 是 Expansion；
2. `sourceScope.featureRef` 若存在，必须命中该 Expansion 的真实 Feature；
3. `sourceScope.moduleRef` 若存在，必须命中该 Expansion 的真实 Source Module；
4. 若 featureRef 与 moduleRef 同时存在，module 的 parent feature 必须等于 featureRef；
5. typo / unknown source scope 必须 fail closed，不能被解释为“当前未启用”；
6. scope 合法后，才根据 resolved enablement 判断 conditional dependency 是否需要 target。

推荐错误沿用现有：

```text
INVALID_FEATURE_REF
INVALID_MODULE_REF
INVALID_MODULE_FEATURE_PARENT
CONDITIONAL_DEPENDENCY_SCOPE_MISSING
```

---

## 5. 对 frozen Source Wire 的影响

本增量**不改变** `TavernAssetV1` v1 Source wire 字段：

```text
moduleRef
runtimeModuleRef
ownerNamespace
sourceScope
config
```

改变的是这些字段到既有 G9-02 Runtime rails 的规范解释与 validator obligation。

因此不需要新 schemaVersion；`tavern.asset.v1` 保持有效。

---

## 6. Correction Acceptance

G9-03 correction 至少新增自动测试：

- Source moduleRef != runtimeModuleRef 时，resolved binding 使用 runtimeModuleRef；
- resolved binding 能通过真实 `RuntimeDomainModuleHost.validateBindings()`；
- Program module unsupported feature fail；
- 两个 active Source modules 指向同 runtimeModuleRef fail；
- known schemaRef 但 validator missing fail；
- config validator false fail；
- conditional unknown feature fail；
- conditional unknown module fail；
- conditional wrong module-parent fail；
- conditional on non-Expansion source fail；
- 原 G9-03 positive/negative corpus 与 G5–G9 regression 全 PASS。

---

## 7. Decision Propagation

```text
G9-03 semantic protocol v1.0
= remains valid
+
G9-03A identity/config/scope clarification
= current mandatory addendum

G9-03 implementation
= CORRECTION REQUIRED

G9-03 CLOSED
= NOT YET

G9-04
= NOT AUTHORIZED
```

本增量不授权新的 Runtime factory platform、plugin execution、Creator、Markdown adapter 或 Library retrieval。

> **External Source module identity 与 Program Runtime module identity 必须显式分层；协议可以配置 Program capability，但不能借一个同名字段偷渡成 Runtime authority。**
