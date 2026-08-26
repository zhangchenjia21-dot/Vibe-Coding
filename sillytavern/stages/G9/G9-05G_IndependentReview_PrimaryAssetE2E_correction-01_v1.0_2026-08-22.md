---
title: G9-05G｜Independent Review｜Primary Asset E2E｜correction-01
status: current-review-correction-required
version: 1.0
updated: 2026-08-22
reviewed_repository: zhangchenjia21-dot/sillytavern
reviewed_branch: agent/g9-05g-primary-asset-e2e
reviewed_exact_sha: a9a3bb0a534efaab6db00f2c3a62efcc19818946
formal_code_base: 26d23d47c5f5ac42d3e1029654a64eda831c4db1
---

# G9-05G｜Independent Review｜Primary Asset E2E｜correction-01

## 1. Verdict

```text
P0 = 0
P1 = 2

G9-05G0 = CORRECTION-01 REQUIRED
G9-05G  = CORRECTION-01 REQUIRED

sillytavern/main = KEEP UNCHANGED
```

本轮不得 fast-forward `main`。

Reviewed exact implementation SHA：

```text
a9a3bb0a534efaab6db00f2c3a62efcc19818946
```

其实现历史为：

```text
db1533fc3b2cea0769e8f8c1a6fd466d3444bbae  Task Packet only
↓
e86fefd3f965b75ee9ac1501418f438ca1124446  G9-05G0 implementation
↓
a9a3bb0a534efaab6db00f2c3a62efcc19818946  G9-05G E2E evidence
```

`main` 在审查前后保持：

```text
26d23d47c5f5ac42d3e1029654a64eda831c4db1
```

GitHub combined status 对 reviewed SHA 返回空 `statuses: []`；因此本审查不声明 CI green。

当前审查环境无法独立联网 checkout / 执行仓库命令；GPT 独立本地命令状态记为 `NOT RUN`。本 verdict 来自 exact-SHA diff、当前 canonical specs、Task Packet、production path 与测试代码的独立静态审计。Kimi 的本地执行结果若未提交为可复核 artifact，不替代本审查。

---

# 2. Passed / keep unchanged

以下方向与冻结架构一致，不要求重写：

- production module identity 已收口为 `builtin:character-capability.evidence.v1`；
- Owner 为 `runtime.character-capability`；
- v1 保持 read-only：不接受 mutation candidate，不拥有成长、技能数值、Dice 或 Formal Outcome；
- `composeProductionDomainModuleHost()` 将 player-knowledge 与 capability evidence 组合为 Program built-ins；
- `SQLiteRuntimeStore` 构造时组合 production host，默认 Runtime Store 不会缺失 capability module；
- `compileCreatingRuntimeDefinition()` 让直建与 crash/retry 共用同一 manifest → binding → Program foundation 编译链；
- capability record 仅来自 materialized Character 的 public `capability-evidence`，`bound_only` / private-only 不物化；
- Primary E2E 使用 real World / Liu Bei / EP-CHAR corpus，按 Git blob SHA-1 exact verify，缺 root 不 synthetic fallback；
- Liu Bei `0.1.2 → 0.1.3` 与 EP-CHAR `0.1.5 → 0.1.6` 均通过 shared Creator source-revision flow 构造，而不是 hand-build final `TavernAssetV1`；
- Runtime routing/projection 主证据通过 `RuntimeDomainTurnOrchestrator`，没有把直接 `projectContext()` 调用冒充主 E2E；
- enabled/disabled negative control、Save/Restore、turn crash/recovery、Source version isolation、Product/HTTP create→session path 均已有对应 E2E 结构；
- external Provider 在自动 Primary E2E 中保持禁用/fixture boundary。

---

# 3. P1-01｜Capability foundation activation 未锁定 EP-CHAR package ownership

## 3.1 Frozen requirement

G9-05G0 canonical spec 冻结的触发条件必须**同时**满足：

```text
EP-CHAR-CORE package selected
+ feature:character-capability enabled
+ module:character-capability.evidence enabled
+ builtin:character-capability.evidence.v1 registered
```

Production Source package identity 是：

```text
package:EP-CHAR-CORE
```

## 3.2 Current implementation

`src/游戏创建/L1_器件层/Program源基础编译器.ts`

当前只判断：

```text
binding.moduleRef == builtin:character-capability.evidence.v1
&& packageIncluded
&& featureEnabled
&& moduleEnabled
```

没有同时验证：

```text
binding.packageRef == package:EP-CHAR-CORE
binding.featureRef == feature:character-capability
```

因此 foundation activation 的 Source ownership 被放宽成“任何 package 只要绑定同一个 Program module 就触发”。这违反 `Source Expansion declaration → exact Program capability ownership` 的冻结边界，并可能让未来另一份 Expansion 复用同一 runtime module 时错误生成 EP-CHAR capability canonical records。

## 3.3 Test evidence

`tests/g9/人物能力证据RuntimeBinding测试.test.ts` 的 `capabilityExpansion()` 甚至使用：

```text
packageRef = package:character-capability
```

而不是 canonical：

```text
package:EP-CHAR-CORE
```

现有 A5-8 只覆盖 flags false，没有覆盖 foreign package + all flags true，因此测试将过宽行为固化成了绿路径。

## 3.4 Required correction

必须：

1. 把 canonical package identity 作为 Program contract 固定，推荐：

```text
CHARACTER_CAPABILITY_PACKAGE_REF = package:EP-CHAR-CORE
```

2. foundation activation exact 匹配：

```text
packageRef
featureRef
runtime moduleRef
packageIncluded
featureEnabled
moduleEnabled
```

3. Phase A 正向 fixture 改用 canonical packageRef；
4. 新增 foreign-package negative：相同 feature/runtimeModuleRef、全部 flags=true，但 packageRef 非 `package:EP-CHAR-CORE` → **no capability foundation record**；
5. 不修改 G9-03 schema，不引入 fuzzy owner 推断。

---

# 4. P1-02｜默认 production Host 无法完成历史 proof Source → production Source revision

## 4.1 Frozen requirement

历史：

```text
EP-CHAR-CORE@0.1.5
runtimeModuleRef = builtin:g9-04.ep-char-core-proof.v1
```

必须保持 immutable / reproducible。

新版本：

```text
EP-CHAR-CORE@0.1.6
runtimeModuleRef = builtin:character-capability.evidence.v1
```

必须通过**正常 production Creator source_revision + explicit Publish** 形成。

同时 production Program Host/Catalog 必须保持：

```text
不注册 G9-04 proof/test fixture
```

## 4.2 Current production gate conflict

`CreatorPublicationService.createSourceRevisionDraft()` 本身只 exact-read base Source，能正确打开 `0.1.5`。

但 `publish()` 会调用：

```text
assertCreatorPublicationHostGate({
  asset: newAsset,
  catalog: currentSourceCatalog(),
  protocolHost: currentProductionHost,
  ...
})
```

而 `assertCreatorPublicationHostGate()` 对 Expansion 调用：

```text
validateAssetCatalog([newAsset, ...entireCurrentCatalog], protocolHost)
```

`validateAssetCatalog()` 会对 catalog 中**每一份 Expansion**执行 `assertExpansionHost()`，并要求其每个 `runtimeModuleRef` 都存在于当前 registry。

因此只要 immutable historical `EP-CHAR-CORE@0.1.5` 仍在 Source Library，而 production Host 正确地不注册：

```text
builtin:g9-04.ep-char-core-proof.v1
```

则发布新的 `0.1.6` 会被旧历史 snapshot 的 unknown runtimeModuleRef 阻断。

这使 canonical migration 在默认 production stack 中不可达。

## 4.3 E2E currently masks the production defect

`tests/g9/G9-05G主资产E2E测试.test.ts` 自建：

```text
E2E_PROTOCOL_HOST.runtimeModuleRefs =
- builtin:player-knowledge.character-directory.v1
- builtin:character-capability.evidence.v1
- builtin:g9-04.ep-char-core-proof.v1
```

注释把 proof ref 描述成“historical read-only identity”，但 `AssetProtocolHostRegistry` 当前没有这种 read-only historical identity 类型；把 ref 放进 `runtimeModuleRefs` 就是在测试 Host 中重新承认它。

所以 E2E 通过的路径不是默认 production Host path，而是专门为历史 proof snapshot 扩大的测试 Host。

## 4.4 Required correction

目标不是把 proof module 注册回 production Host。

必须收口 Publication Host Gate 的**active candidate vs historical catalog**语义，使：

```text
new/republished Expansion candidate
→ 当前 Program Host 严格校验

immutable historical Source siblings / unrelated historical expansions
→ 仍做结构、integrity、dependency/reference catalog 校验
→ 但不得因为其旧 runtimeModuleRef 已退出 current Program Host 而阻塞新版本发布
```

要求：

1. `createG2CreatorProgramHost()` 继续不含 proof/test runtime refs；
2. 不在 production `runtimeModuleHost` 注册 proof module；
3. 不通过永久扩大 `AssetProtocolHostRegistry.runtimeModuleRefs` 来兼容历史版本；
4. Publication Gate 对**本次 candidate Expansion**仍必须 current Host strict；unknown current runtimeModuleRef fail closed；
5. catalog 依赖图、exact identity、integrity、hard cycle/reference 等校验不得被绕过；
6. 新增 production-host migration regression：
   - Library 同时含 historical `EP-CHAR-CORE@0.1.5` proof snapshot；
   - 使用默认 `composeG2CreatorStack()` / `createG2CreatorProgramHost()`；
   - source_revision → 删除 proof nodes → 加 production module → `0.1.6`；
   - Publish PASS；
   - `0.1.5` 保持 exact immutable；
   - production protocolHost 断言**不含** G9-04 proof ref；
7. G9-05G real E2E 删除 `E2E_PROTOCOL_HOST` 对 proof ref 的特殊放行，改用真实 production Host truth。

如果需要给 G9-03 catalog validator 增加“host validation mode”，必须保持 public contract 最小化，并保证正常 Game Manifest / active catalog validation 仍严格 fail closed；优先在 Creator Publication Gate 层区分 candidate 与 historical source，避免改变全局资产协议语义。

---

# 5. Correction-01 Acceptance

```text
AC-C01 canonical package/feature/module/flags 全匹配才产生 capability foundation
AC-C02 foreign package + same runtime module + flags true → no foundation
AC-C03 package/feature/module disabled 旧负例继续 PASS
AC-C04 default production Program Host 不含 proof runtime ref
AC-C05 historical EP-CHAR@0.1.5 留在 library 时，默认 production stack 可 source-revision/publish 0.1.6
AC-C06 0.1.5 exact snapshot 不被改写；0.1.6 为新 digest
AC-C07 current candidate 使用 unknown runtimeModuleRef 仍 fail closed
AC-C08 real Primary E2E 不再通过 custom proof-allowing protocolHost 才能通过
AC-C09 G9-05G enabled/disabled、Save/Restore、Crash/Recovery、Source isolation 不回退
AC-C10 external Provider calls = 0 for automated correction/E2E gates
AC-C11 main remains unchanged until independent review PASS
```

---

# 6. Required regression

Correction 完成后至少重新执行并逐条报告：

```text
npm run g9:05g0:test
npm run g9:05g:test
npm run g9:05g:real-assets -- <exact assets root>
npm run g9:05f:test
npm run g9:05f:product-e2e
npm run g9:05e:test
npm run g9:05e:product-e2e
npm run g9:04:test
npm run g9:03:test
npm run g8:test
npm run typecheck
npm run lint
npm run product:build
npm run launcher:smoke
npm run g2:disclosure
git diff --check
```

缺失环境/资产必须报告 `NOT RUN`；不得把 skip / unavailable 记成 PASS。

---

# 7. Integration Gate

Correction 必须继续使用：

```text
agent/g9-05g-primary-asset-e2e
```

禁止创建 correction 分支。

GPT 将只审查 correction 后的 exact branch SHA。

只有：

```text
P0 = 0
P1 = 0
```

才允许：

```text
fast-forward sillytavern/main exactly to reviewed SHA
force = false
no merge/squash/rebase/integration-only commit
```
