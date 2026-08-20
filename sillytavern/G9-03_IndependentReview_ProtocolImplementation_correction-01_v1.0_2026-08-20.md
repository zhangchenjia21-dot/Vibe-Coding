---
title: G9-03 Protocol Implementation Independent Review correction-01
status: current-review-correction-required
version: 1.0
date: 2026-08-20
stage: G9-03
---

# G9-03｜Protocol Implementation Independent Review｜correction-01

## Review Identity

```text
Formal Code Base
ab09c7ce6960a99b062d22fd49c143f9ae876f4e

Task Packet
51db59aa48b860e36959436c847da6c6bd65ce89

Reviewed implementation
3f86c6fe0a45ef3c8412dc9a38455ef32098f298

Final evidence
3129befca0d79d2ed90db840897e25401408e324
```

`3129bef...` 为 evidence-only commit；本次 implementation review 以 `3f86c6fe...` 为主要代码对象。

## Verdict

```text
P0 = 0
P1 = 3

G9-03 implementation = FAIL / CORRECTION REQUIRED
main = unchanged at ab09c7ce6960a99b062d22fd49c143f9ae876f4e
G9-03 CLOSED = NO
G9-04 AUTHORIZED = NO
```

Offline / regression evidence 本身可信；阻断来自 protocol → existing Runtime rails 的语义接线缺口，而不是测试命令失败。

---

## P1-01｜Source moduleRef 被错误写入 Runtime binding moduleRef

Source fixture 明确区分：

```text
moduleRef
module:politics.core

runtimeModuleRef
builtin:protocol.politics.v1
```

但 `resolveExpansionModuleBindings()` 当前生成：

```text
RuntimeDomainModuleBinding.moduleRef
= module.moduleRef
```

而 G9-02 `RuntimeDomainModuleHost.validateBindings()` 使用 `binding.moduleRef` 直接查 Program module registry；不存在则：

```text
DOMAIN_BINDING_UNKNOWN_MODULE
```

因此当前 AC-05 所谓 mapping proof 只比较 DTO shape，没有证明 binding 能进入真实 G9-02 Host。

Current authority 由：

`G9-03A_RuntimeModuleBinding与TypedConfig增量裁定_v1.0_2026-08-20.md`

补充为：

```text
Source ExpansionModuleV1.moduleRef
= Source declaration identity

ExpansionModuleV1.runtimeModuleRef
= Program RuntimeDomainModuleDescriptor.moduleRef

RuntimeDomainModuleBinding.moduleRef
= runtimeModuleRef
```

并要求真实 Host vertical proof。

---

## P1-02｜TypedConfig schema known 但 validator missing 会被接受

Frozen Task / Spec 要求：

```text
schemaRef known
+ Program-owned validator PASS
→ config allowed
```

当前实现中 `configValidators` optional；module config 若 schemaRef 位于 `configSchemaRefs`，但没有对应 validator，会直接通过。

这会把 typed config 降级成“已知名字 + arbitrary JSON”。

Correction 必须要求 exact Program validator 存在并 PASS。

---

## P1-03｜feature_conditional sourceScope typo 可静默关闭 dependency

当前结构校验只要求 conditional dependency 带非空 `sourceScope.featureRef` 或 `sourceScope.moduleRef`。

Catalog 阶段没有验证 scope ref 是否真的存在于源 Expansion；unknown ref 在 enablement set 中不存在时会被解释为 disabled，于是 dependency target 不再强制。

因此拼写错误可能从：

```text
required conditional dependency
```

静默退化为：

```text
not active
```

Correction 必须验证 source asset / feature / module / parent identity，合法后才计算 enablement。

---

## Non-blocking findings

以下目前没有形成额外 P1：

- canonical JSON / SHA-256 material 排除 own digest 的实现与 v1 spec 一致；
- strict top-level / nested protocol object unknown-field gate 基本成立；
- Library 4-audience 与 snapshot pinning 方向正确；
- Bundle embedded / catalog_reference 职责边界正确；
- 本轮没有修改 G9-02 / G8 production contracts。

Correction 后必须重新 exact-SHA review 全 diff，不因本次其余部分已通过而跳过。

---

## Next

```text
same branch
agent/g9-03-unified-asset-protocol
↓
correction-01 commits
↓
full focused + regression
↓
Grok Final Report + new exact SHA
↓
GPT Independent Review
```

禁止：rebase / amend / force push / new correction branch / push main。
