---
title: G9-03 Protocol Implementation Independent Review correction-01
status: historical-correction-record
version: 1.0
date: 2026-08-20
stage: G9-03
superseded_by: G9-03_IndependentReview_最终收口_v1.0_2026-08-20.md
---

# G9-03｜Protocol Implementation Independent Review｜correction-01

> 本文件保留首次 G9-03 implementation review 的 correction 证据。其 `CORRECTION REQUIRED` 状态已由 `G9-03_IndependentReview_最终收口_v1.0_2026-08-20.md` supersede；不得再作为当前 Stage 状态读取。

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

`3129bef...` 为 evidence-only commit；本次 initial implementation review 以 `3f86c6fe...` 为主要代码对象。

## Historical Verdict

```text
P0 = 0
P1 = 3

G9-03 implementation = FAIL / CORRECTION REQUIRED AT THIS REVIEW POINT
main = unchanged at ab09c7ce6960a99b062d22fd49c143f9ae876f4e
```

这三个 P1 后续已由 correction implementation `5da2294a9d21585665167e69307d9c693427582d` 关闭，并通过最终 Independent Review；当前 Stage 状态请读取 superseding final review。

---

## P1-01｜Source moduleRef 被错误写入 Runtime binding moduleRef

Source fixture 明确区分：

```text
moduleRef
module:politics.core

runtimeModuleRef
builtin:protocol.politics.v1
```

Initial implementation 的 `resolveExpansionModuleBindings()` 生成：

```text
RuntimeDomainModuleBinding.moduleRef
= module.moduleRef
```

而 G9-02 `RuntimeDomainModuleHost.validateBindings()` 使用 `binding.moduleRef` 直接查 Program module registry；不存在则：

```text
DOMAIN_BINDING_UNKNOWN_MODULE
```

因此 initial AC-05 mapping proof 只比较 DTO shape，没有证明 binding 能进入真实 G9-02 Host。

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

Initial implementation 中 `configValidators` optional；module config 若 schemaRef 位于 `configSchemaRefs`，但没有对应 validator，会直接通过。

这会把 typed config 降级成“已知名字 + arbitrary JSON”。

Correction 要求 exact Program validator 存在并 PASS。

---

## P1-03｜feature_conditional sourceScope typo 可静默关闭 dependency

Initial structure validation 只要求 conditional dependency 带非空 `sourceScope.featureRef` 或 `sourceScope.moduleRef`。

Catalog 阶段没有验证 scope ref 是否真的存在于源 Expansion；unknown ref 在 enablement set 中不存在时会被解释为 disabled，于是 dependency target 不再强制。

因此拼写错误可能从 required conditional dependency 静默退化为 not active。

Correction 要求验证 source asset / feature / module / parent identity，合法后才计算 enablement。

---

## Historical Non-blocking Findings

- canonical JSON / SHA-256 material 排除 own digest 的实现与 v1 spec 一致；
- strict top-level / nested protocol object unknown-field gate 基本成立；
- Library 4-audience 与 snapshot pinning 方向正确；
- Bundle embedded / catalog_reference 职责边界正确；
- initial implementation 没有修改 G9-02 / G8 production contracts。

---

## Resolution

```text
same branch
agent/g9-03-unified-asset-protocol
↓
correction packet
b3f6f4efeaa3faf9d346c88d4939263f0fc359fd
↓
correction implementation
5da2294a9d21585665167e69307d9c693427582d
↓
final exact-SHA Independent Review PASS
↓
G9-03 PASS / CLOSED
```

Current result is governed by `G9-03_IndependentReview_最终收口_v1.0_2026-08-20.md`.
