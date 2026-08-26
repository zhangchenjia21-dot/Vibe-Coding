---
title: G9-05F0 Expansion Import 与 Host Publish Gate 增量裁定
status: current-decision-frozen
version: 1.0
date: 2026-08-22
stage: G9-05
slice: G9-05F0
prerequisite: G9-05E PASS / CLOSED
next_gate: G9-05F Expansion Creator Vertical
---

# G9-05F0｜Expansion Import 与 Host Publish Gate 增量裁定 v1.0

## 0. 结论

G9-05E 已 PASS / CLOSED。进入 Expansion Creator 前完成底层能力审计后，确认共享 Creator Core 已经具备完整 Expansion Draft 主体：

```text
packageRef / ownerNamespace
features
modules
ui.ownsExtensionSurfaces
ui.contributions
sections
dependencies
```

`CreatorExpansionModuleNode` 已覆盖 `runtimeModuleRef / runtimeModuleVersion / config / routingMode / routingProfile / projectionRef / capabilityRefs / definitionRegistryRefs / acceptedHandoffKinds / emittedHandoffKinds`；`upsert_typed_node / remove_typed_node`、ChangeSet / Undo 与 Draft→G9-03 publish compiler 均已存在。

因此 G9-05F0 **不建立第二套 Expansion Draft / Store / Publish / Protocol**，只补两个共享前置 seam：

1. Expansion-aware Import Assignment；
2. Expansion Publish 的 Program Host Compatibility Gate。

---

## 1. Authority

1. `G9-03_UnifiedAssetReferenceProtocol规格_v1.0_2026-08-20.md`；
2. `G9-03A_RuntimeModuleBinding与TypedConfig增量裁定_v1.0_2026-08-20.md`；
3. `G9-05B_CreatorCore草稿导入发布内部合同规格_v1.0_2026-08-20.md`；
4. `G9-05E_IndependentReview_最终收口_v1.0_2026-08-22.md`；
5. `sillytavern/main@f1ec971b09dc9ed6dc59474f2c8ad1137e0f2e26`。

---

## 2. 永久边界

```text
Expansion Source declaration
!= executable plugin
!= Program Runtime module implementation
!= active Runtime binding
!= current game enablement
```

```text
ExpansionModuleV1.moduleRef
= Source declaration identity

ExpansionModuleV1.runtimeModuleRef
= Program-owned Runtime module identity
```

Source `ownerNamespace` 只是 Source semantic namespace，不取得 Runtime Domain Owner 权限。

Creator Publish 只产生正式 Source Asset；**不得因为发布而自动启用 Feature / Module、修改当前游戏或生成 Runtime State。**

---

## 3. DEC-F0-01｜Expansion Import Assignment

现有 Import Core 的 evidence / blank-only / conflict / unresolved / single-CAS 规则保持不变，但 `CreatorImportAssignment` 必须能够表达 Expansion 明确结构。

最低增量：

```text
scalar:
- expansion.packageRef
- expansion.ownerNamespace

dependency:
- exact CreatorDraftDependency

typed node:
- expansion_feature
- expansion_module
- expansion_ui_surface
- expansion_ui_contribution
```

可采用 discriminated union 扩展，不允许 arbitrary path / JSON Pointer / patch。

### 3.1 blank-only / identity rule

- scalar 只填空白字段；
- typed node 只在对应 semantic identity 尚不存在时自动应用；
- dependency 只在对应 `dependencyRef` 尚不存在时自动应用；
- 已有正式节点不得被导入静默覆盖；
- 冲突进入 `conflicts`，证据不足进入 `unresolvedItems`。

### 3.2 Program-owned ref evidence

以下值不是可以由模型从自然语言“猜出来”的文案：

```text
runtimeModuleRef
config.schemaRef
projectionRef
hostCapability
```

导入只有在原文提供明确、唯一的正式 ref，且 Program Catalog 证明该 ref 可用时才能形成 certain assignment；否则 unresolved。

自然语言“使用政治模块”“做一个状态栏”不得自动映射成某个 Program ref。

---

## 4. DEC-F0-02｜Expansion Creator Host Catalog

Program 必须向 Creator Product 提供只读、player-safe 的 Expansion Capability Catalog，作为**选择与校验来源**，而不是浏览器自己知道 Runtime internals。

最低信息：

```text
runtime modules:
- runtimeModuleRef
- player-safe label/description if available
- supportedFeatureRefs if current Program contract exposes it safely

config schemas:
- schemaRef
- player-safe label if available

projections:
- projectionRef
- player-safe label if available

UI host capabilities:
- frozen TAVERN_ASSET_UI_CAPABILITIES
```

Catalog 只投影声明能力，不暴露 hidden Runtime State、数据库路径、任意 selector 或 executable callback。

如果某类注册表当前只有 ref 没有 label，首版允许只显示 exact ref；禁止为了 UI 好看伪造别名。

---

## 5. DEC-F0-03｜Publish Host Compatibility Gate

Expansion Draft 的正式 Publish 必须在 Source append 前经过 Program-owned Host Gate。

顺序：

```text
exact Draft revision
→ compileCreatorDraftForPublication()
→ existing G9-03 structural/integrity validation
→ existing validateAssetCatalog(candidate + current Source catalog, Program Host Registry)
→ default enablement binding proof
→ only then existing append-only Source publication transaction
```

不得复制一套 Creator-specific runtime/config/projection validator。

### 5.1 Host Registry rules

必须继续满足 G9-03A：

```text
runtimeModuleRef known
config.schemaRef known
+ exact Program validator exists
+ validator(value) = true
projectionRef known
feature_conditional sourceScope valid
UI host capability known
```

### 5.2 Default enablement proof

对 Candidate Expansion 的：

```text
packageIncluded = true
enabledFeatureRefs = features where defaultEnabled=true
enabledModuleRefs = modules where defaultEnabled=true
```

必须复用 `resolveActiveExpansionModuleBindings()` 并通过真实 `RuntimeDomainModuleHost.validateBindings()` 或等价现有 Program Host Gate。

此 Gate 证明“默认配置可运行”，不禁止同一 Source 声明多个指向相同 `runtimeModuleRef` 的互斥/替代 Source module；但默认启用集合不得形成 duplicate active runtime module。

### 5.3 Fail closed

Host Gate 失败：

- Draft 保持 editable；
- revision/ChangeSet 不被伪造为成功发布；
- Source Store 不产生新 snapshot；
- 当前游戏 / Runtime 不变化；
- 返回稳定、可展示的错误；
- 不自动修正成其它 Program ref。

---

## 6. DEC-F0-04｜No protocol version change

本增量不改变：

```text
tavern.asset.v1
TavernAssetV1
ExpansionAssetPayloadV1
G9-03 enum / wire field
```

只扩 Creator 内部 Import Contract 和 Publish Policy seam。

---

## 7. Acceptance

必须证明：

1. blank Expansion Draft 可通过 typed operations 保存 feature/module/UI nodes，并 Undo；
2. import 能 evidence-backed 填 `packageRef / ownerNamespace / dependency / expansion typed nodes`；
3. 已存在 semantic node 不被 import 覆盖；
4. ambiguous Program ref 进入 unresolved；
5. unknown `runtimeModuleRef` Publish fail；
6. config schema known but validator missing fail；
7. config value validator=false fail；
8. unknown projection fail；
9. invalid `feature_conditional.sourceScope` fail；
10. duplicate active default runtimeModuleRef fail；
11. 合法 Expansion candidate 通过 G9-03 catalog + real Runtime binding proof；
12. Publish 后只新增 Source snapshot，不激活当前 Runtime；
13. World / Character Creator 回归不变。

---

## 8. Decision Propagation

```text
G9-05E PASS / CLOSED
↓
G9-05F0 Expansion shared seam
↓ focused PASS
G9-05F Expansion Creator Vertical
↓ exact-SHA Independent Review
Primary Asset End-to-End Closure Gate
```

G9-05F0 与 G9-05F 可以放在同一正式任务中分成严格有序的 Phase A / Phase B：只有 Phase A focused Gate 通过才允许进入产品纵向。
