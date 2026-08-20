---
title: G9-05B Creator Core Independent Review correction-02
status: current-review-correction-required
version: 1.0
date: 2026-08-20
stage: G9-05
slice: G9-05B
---

# G9-05B｜Creator Core｜Independent Review correction-02

## 1. 审核对象

```text
Formal Code Base
c492ac4a0eb33ec055f582a2a023066853e2c323

Previous Reviewed Final
0757f4674da23bcc2588b6265cc5c3d663e3667b

correction-01 Packet
21bf495bbbb1d1dcfec7714ac8c76e059740a431

Reviewed correction-01 Final
f789150d584f8f2e538558c0129e0b25e5bbb73e

origin/main during review
c492ac4a0eb33ec055f582a2a023066853e2c323
```

`c492ac4a... → f789150...` 为严格前向后代；correction-01 只修改 `src/资产创作/` 与 G9-05B focused tests，没有修改 Runtime、G9-03 资产协议生产实现、G9-04 Legacy Adapter 或最终 Creator UI。

GitHub 对 `f789150...` 没有 CI status / workflow run 证据；但本轮已发现代码级 P1，因此无需等待测试证据即可判定不得合并。

## 2. 最终结论

```text
P0 = 0
P1 = 2

P1-01 complex typed-node task authorization = FAIL / residual blocker
P1-02 import section identity               = PASS
P1-03 complex AI runtime type/value gate    = FAIL / residual blocker
P1-04 version conflict recovery             = PASS

G9-05B implementation = FAIL / CORRECTION-02 REQUIRED
G9-05B CLOSED = NO
G9-05C AUTHORIZED = NO
main = unchanged at c492ac4a0eb33ec055f582a2a023066853e2c323
```

correction-01 已完整关闭导入 section identity 和版本冲突恢复，并显著改善任务级 AI scope 与局部合法应用；当前仅剩复杂 typed node / dependency 场景的两个同源边界。

---

## 3. 已关闭｜P1-02 导入 section identity

当前实现已经把导入 section 的正式语义目标固定为 `sectionRef`：

- blank-only 按现有 `sectionRef` 判断；
- Provider 提交的 `nodeRef` 不能指定覆盖目标；
- 新导入 section 的 Creator `nodeRef` 由 Program 重新生成；
- `existing sectionRef + different nodeRef` 失败关闭；
- `new sectionRef + existing nodeRef` 失败关闭；
- 状态器进一步阻止同一 `nodeRef` 改换 `sectionRef`，或同一 `sectionRef` 被另一 `nodeRef` 占用。

对应正向/负向测试已加入。

结论：**PASS**。

---

## 4. 已关闭｜P1-04 Source 版本冲突恢复

当前 `CreatorPublicationService` 能区分 `SOURCE_ASSET_VERSION_CONFLICT`：

```text
editable
→ publishing
→ appendValidated() version conflict
→ CAS restore editable
→ revision + 1
→ preserve content + targetVersion
→ rethrow original Source conflict
```

测试证明：Source 旧版本不变，Draft 恢复可编辑，用户修改新版本后可再次发布成功。

暂时性失败和“Source 已写入 / Draft 尚未 final”仍保留原 exact retry / recovery 语义。

结论：**PASS**。

---

## 5. 残余 P1-01｜复杂 typed node 的授权只绑定 nodeRef，没有绑定 node kind

correction-01 新增：

```text
CreatorAuthoringScope
= operationKinds
+ scalarTargets
+ sectionRefs
+ nodeRefs
```

这已经能正确限制标量和 section。

但对于 `upsert_typed_node`，Program 当前只检查：

1. `operationKinds` 是否包含 `upsert_typed_node`；
2. operation 的 `nodeRef` 是否存在于 `scope.nodeRefs`。

没有把授权绑定到具体 typed node kind。

因此可能出现：

```text
当前 Draft：
node:feature:1 = expansion_feature

本次用户授权：
编辑 node:feature:1 这个 Feature

模型返回：
upsert_typed_node
kind = expansion_module
nodeRef = node:feature:1

当前 scope gate：
nodeRef 命中 → 允许
```

而底层 typed node 列表按 `node.kind` 分开查找，模型可以借一个已授权 nodeRef 在另一类节点列表中新增不同语义对象。

这违反 #19 的任务级授权：

```text
授权一个具体 Feature
!= 授权任意 typed node 使用同一个 nodeRef
```

### Correction 要求

Program-owned authoring scope 必须把复杂节点授权绑定到**精确节点种类 + 节点身份 + 允许操作族**。

可采用等价合同，例如：

```text
typedNodeTargets[]
- nodeKind
- nodeRef
- allowedOperations
```

或在 Program gate 中从当前 Draft 精确解析 `nodeRef → nodeKind` 并拒绝 kind mismatch。

对于“新增节点”，也必须显式授权具体 node kind，并使用 Program-owned / Program-approved node identity；不能让模型把已有其它 kind 的 nodeRef 重新解释。

### 必须测试

1. 已有 `expansion_feature node:feature:1`；只授权该 Feature；模型提交同 nodeRef 的 `expansion_module` → ignored / protected，Module 不得插入；
2. 同 kind Feature 修改 → PASS；
3. 新节点只授权具体 kind；另一 kind 复用同 nodeRef → fail closed。

---

## 6. 残余 P1-03｜复杂 AI operation 缺少完整运行时类型 / 取值校验

correction-01 正确地把 Provider 输出改成：

```text
operations: unknown[]
```

并新增逐项 partial apply。这一方向正确。

但 `parseCreatorOperation()` 对复杂操作只校验外层 envelope，例如：

```text
upsert_dependency
→ dependency 是 object 即可

upsert_typed_node
→ node 是 object
+ node.kind 是已知 typed node kind
```

随后现有 `validateDependency()` / `validateTypedNode()` 仍主要只检查若干字符串字段，不足以把 unknown 收敛成真实、合法的 `CreatorDraftOperation`。

因此畸形模型输出可能在发布前进入 Draft，例如：

```text
expansion_feature.defaultEnabled = "yes"
expansion_module.defaultEnabled = "true"
expansion_module.routingMode = "invented-mode"
world_composition.disposition = "whatever"
dependency.kind = "invented"
target.assetType = "invented"
```

TypeScript 类型声明对此没有运行时保护；而 correction-01 已明确把 Provider 边界定义成 `unknown[]`，所以 Program 必须自己做完整解析。

即便最终 G9-03 发布 Validator 会拒绝这些值，也不能把“发布时才发现”替代 Creator Core 的 task-level typed tool gate。

### Correction 要求

对所有 AI 可暴露的 operation payload 建立**深度、精确、运行时**解析/验证，至少覆盖：

- exact/allowed keys；
- required string / boolean；
- enum；
- nested object shape；
- string array 成员类型；
- `target.assetType`；
- dependency kind / sourceScope；
- World composition disposition；
- Expansion Feature / Module booleans；
- routingMode / routingProfile；
- typed config shape；
- UI capability / secondaryViews / staticData；
- 其它当前 `CreatorTypedListNode` 暴露的正式字段。

最佳形态：

```text
unknown Provider operation
→ exact runtime parser
→ bona-fide CreatorDraftOperation
→ task scope authorization
→ domain mutation validation
→ partial apply
```

坏复杂 operation 必须进入 `ignoredOperations`，不能污染 Draft；合法 sibling 仍保留为同一个 ChangeSet。

### 必须测试

至少增加：

```text
valid scalar
+ malformed feature(defaultEnabled = string)
+ valid scalar
→ 2 valid apply
→ malformed ignored
→ one ChangeSet
```

以及：

- malformed dependency kind / target.assetType；
- malformed module `routingMode` / boolean；
- valid complex node 正向用例。

---

## 7. 保持不变

以下已经通过或方向成立，correction-02 不得重写：

- `Creator Draft != Source Asset != Game-local != Runtime`；
- Source Store validated-only + append-only；
- Draft revision/CAS；
- stale AI result 丢弃；
- `.md/.txt` exact raw artifact + SHA-256 + segment refs；
- import `sectionRef` blank-only + Program node identity；
- task-level ChangeSet / inverse Undo；
- scalar/section task authorization；
- partial valid apply + one final CAS；
- Draft → `computeAssetDigest()` → `validateAndVerifyAsset()`；
- Source-written / Draft-not-finalized exact recovery；
- Source version conflict → editable recovery；
- Provider calls = 0；
- 不修改 G9-03 wire、G9-04 Adapter、Runtime production contracts、最终 UI。

---

## 8. 下一 Gate

```text
same branch correction-02
↓
new exact Tested Implementation SHA
↓
GPT independent review
↓
P0 = 0 / P1 = 0 only
↓
fast-forward sillytavern/main
↓
G9-05B PASS / CLOSED
↓
G9-05C AUTHORIZED / NEXT
```

在此之前：

```text
main remains c492ac4a0eb33ec055f582a2a023066853e2c323
G9-05C = NOT AUTHORIZED
```
