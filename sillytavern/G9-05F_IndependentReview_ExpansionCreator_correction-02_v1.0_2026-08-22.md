---
title: G9-05F Expansion Creator Independent Review correction-02
status: current-review-rework
version: 1.0
date: 2026-08-22
stage: G9-05
slice: G9-05F
reviewed_implementation: c7a809f170baa17f99823d885f681dcbb7da5556
main_baseline: f1ec971b09dc9ed6dc59474f2c8ad1137e0f2e26
executor: Kimi
---

# G9-05F｜Expansion Creator Independent Review｜correction-02

## 0. Verdict

```text
Reviewed Implementation SHA
c7a809f170baa17f99823d885f681dcbb7da5556

P0 = 0
P1 = 1

G9-05F correction-01 six blockers = CLOSED
G9-05F overall = CORRECTION-02 REQUIRED
Primary Asset End-to-End Closure = NOT AUTHORIZED
```

`main` 在本轮审核时仍精确保持 `f1ec971b09dc9ed6dc59474f2c8ad1137e0f2e26`。

GitHub 对 reviewed SHA 没有 external CI status / workflow run；本 review 不声称 CI green。

---

## 1. correction-01 六项复审

Kimi correction-01 已完成并通过静态/行为测试代码审计的六项：

1. production Program Host / Capability composition seam；
2. AI Program-owned ref gate + section/dependency/feature/module/UI exact scope；
3. Expansion Import formal locator；
4. stale scalar + structured node dirty overlay；
5. Expansion Source Detail completeness；
6. publishing recovery 在 Source append 前重新执行当前 Host Gate。

这些项不需要 correction-02 重做。

---

## 2. P1-01｜AI 可以改写既有 semantic identity

### 2.1 Frozen authority

G9-05F current spec 明确：

```text
existing semantic refs 只读身份

sectionRef
dependencyRef
featureRef
moduleRef
surfaceId
contributionRef
```

AI authoring 只是被授权修改当前字段/节点内容，不取得 semantic identity 重命名权。

### 2.2 Current code gap

当前 authoring gate：

```text
section      → 主要按 sectionRef scope
upsert dependency → 只检查 nodeRef 是否在 scope.nodeRefs
upsert typed node → 只检查 nodeKind + nodeRef + allowed operation
Program ref gate → 只检查 runtimeModuleRef/config.schemaRef/projectionRef/hostCapability
```

之后 `applyDependency()` / `applyTypedNode()` 按 `nodeRef` 直接 replace，并未比较 existing semantic identity。

因此以下 AI 输出在 scope 内且 Program-owned refs 合法时仍可能被接受：

```text
existing nodeRef = node:module:1
existing moduleRef = module:politics.core
AI upsert same nodeRef but moduleRef = module:politics.renamed
→ identity 被静默改写
```

同类风险存在于：

- dependencyRef；
- featureRef；
- moduleRef；
- surfaceId；
- contributionRef；
- section 在多个 section 同时授权时，也必须以 nodeRef→existing sectionRef 防止 cross-identity replacement。

### 2.3 Required behavior

在 AI output 写入 Draft 前增加 current-draft identity gate：

```text
same existing nodeRef
+
incoming semantic identity != existing semantic identity
→ operation ignored
→ reason = CREATOR_DRAFT_PROTECTED_TARGET（或现有等价稳定 protected error）
→ legal sibling operations still apply
→ Draft existing identity unchanged
```

不得依赖 prompt；不得等到 Publish 才发现。

优先在共享 authoring policy / `CreatorDraftFlow.author()` 的 Program-owned application gate 附近完成，避免为 Expansion UI 再造一套规则。

不得改变 new-node identity creation：本轮只保护“已有 nodeRef 对应的既有身份”。

---

## 3. Required regression proof

至少证明：

1. Existing Expansion Module：同 nodeRef 改 moduleRef → ignored；同任务合法 label 修改仍 apply；
2. Existing Feature：同 nodeRef 改 featureRef → ignored；
3. Existing Dependency：同 nodeRef 改 dependencyRef → ignored；
4. Existing UI Surface / Contribution semantic identity → ignored；
5. Existing Section 在多 section scope 下用 A.nodeRef + B.sectionRef 交叉替换 → ignored；
6. legal content edits 保持可用；
7. `ignoredOperations` 可审计；
8. World / Character Creator 与 G9-05E/G9-05F regressions 不回退。

---

## 4. Non-goals

correction-02 不得：

- 重做 production Host composition；
- 重做 Program-owned ref gate；
- 重做 Import locator / Source detail / stale overlay；
- 修改 G9-03 wire/schemaVersion；
- 重写 G9-04 / G9-05E；
- 进入 Primary Asset End-to-End Closure；
- 新建 correction branch；
- 修改 main。

---

## 5. Exit

```text
P1-01 identity protection PASS
+
full regression no P0/P1
→ G9-05F P0=0 / P1=0
→ exact-SHA final review
→ PASS only then fast-forward main
```
