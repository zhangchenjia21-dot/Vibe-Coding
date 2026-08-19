# G9-02C Core｜Pre-Review 阻塞发现 v1.0

状态：`CORRECTION-01 REQUIRED / BEFORE EXACT-SHA REVIEW`
日期：2026-08-19

## 1. Review Identity

```text
Implementation repo
zhangchenjia21-dot/sillytavern

Base
0ee847e1173ae8d17e643d5b838d238cf889031e

Task branch
agent/g9-02c-router-core

Topology
base → branch = ahead_by 1 / behind_by 0
merge base = exact Base

origin/main
0ee847e1173ae8d17e643d5b838d238cf889031e
```

GitHub connector 当前能证明 branch 是 Base 的纯 fast-forward descendant，但没有在 branch-ref compare 返回顶点 40 位 SHA。因此本文件只记录在 exact-SHA Gate 前已经足以阻塞合并的静态架构发现；**不得把本文件当成最终 Independent Review PASS/FAIL authority。**

当前：

```text
P0 0
P1 3
main unchanged
G9-02C Core NOT PASS
G9-02C Breadth BLOCKED
G9-03 NOT AUTHORIZED
```

#19 Creator / #18 Library 等后续文档更新明确不回开 G9-02C，本次阻塞全部来自当前 02C Core 实现自身。

---

## 2. P1-01｜Bounded fan-out 会把合法 Core Attempt 变成 ACTION_REJECTED

当前 `领域路由工作集构建器.ts` 在任一 hierarchy level 的 profiles 超过 `profilesPerWorkingSet=16` 或 serialized-size bound 时抛出：

```text
DOMAIN_ROUTING_WORKING_SET_BOUNDS
```

当前 focused test 还把 `17 packages → fail closed` 固定成预期行为。

但 `正式回合提交流程.ts` 对 Domain Orchestrator 的任意异常统一：

```text
failExecution(..., DOMAIN_CHANGE_REJECTED)
→ ACTION_REJECTED
```

因此存在：

```text
合法 Core Attempt
+
17+ package / feature / module fan-out
→ Domain routing capacity failure
→ ACTION_REJECTED
```

这违反 current Spec：

```text
Router = Context Selection
Router miss / Domain fail != Attempt illegal
```

也不能把“adaptive bounded routing/refinement rails”实现成“超一个 working-set 就让玩法失效”。

Correction-01 必须建立 bounded paging / cursor / equivalent fan-out continuation，或其它不会遗漏合法 profile、不会一次全量 prompt、也不会因纯 capacity 将合法 Core Attempt 判 illegal 的机制。

---

## 3. P1-02｜Package / Feature summary 可能语义饥饿

当前 group summary 使用：

```text
distinct(descendant routingTags, 4)
distinct(descendant capabilityRefs, 4)
```

即按目录顺序只保留前 4 个 distinct child semantics。

如果真正相关 Feature / Module 的语义位于第 5 个以后：

```text
child exists + enabled
→ parent routing profile 不包含其语义
→ Model 无依据选择 refine
→ false router miss
```

这违反：

```text
Bounded != Starved
```

也没有满足 Routing Profile 的 `label + one-line scope + small typical semantics` 应表达 group 真实职责的产品语义。

当前 100+ fixture 通过，是因为 fixture router 直接按 `packageRef / featureRef` 找目标；不是模型根据 parent semantic profile 判断。当前真实 DeepSeek smoke 也手工拼 human-friendly leaf profile，没有证明 production `RuntimeDomainRoutingCatalog` 产生的 group profile 可被真实模型稳定使用。

Correction-01 必须建立 Program-owned bounded package/feature group descriptor / summary seam，不得继续 arbitrary first-N child truncation；并让 real smoke 至少覆盖 `production catalog/group profile → DeepSeek router → refinement/selection`。

---

## 4. P1-03｜`PlayerSafeRelationshipSubgraph` 实际没有 Player-known eligibility boundary

当前 `相关关系子图选择器.ts` 从 exact anchor 对完整 Runtime `relationships` 做 BFS，只过滤 relationship runtime slot 的 `visibility=public`。

它没有验证 traversal 另一端 Character 是否：

- Player-known；
- current-visible；
- 或存在 explicit knowledge evidence。

因此：

```text
known A
→ Runtime relationship
→ unseen public B
```

可能把 B 的 stable ref 与 relationship existence 带入名为 `PlayerSafeRelationshipSubgraph` 的结果。

该 helper 还从 Runtime L3 public interface 导出，因此不能按“test-only helper”处理。

这与 #17：

```text
all public Game-local Characters
!= Player-known
```

以及 02C：

```text
Model only sees owner-safe / player-safe bounded projection
```

冲突。

Correction-01 必须引入明确的 player-safe eligibility source，例如 `allowed player-known/current-visible refs + optional allowed relationship evidence`，并新增“未认识关系邻居不得泄漏；获得合法知识后才进入”的 negative/positive test。

---

## 5. 已确认正确、Correction 不得回滚

当前实现已有以下健康轨道：

- `RuntimeAuthorizedTurnAnchors`；
- Domain selection 不等于新玩家授权；
- Candidate 回显 `authorizedTurnRef` + bounded referenced refs；
- module-owned `validateCandidateAuthorization`；
- `model_immediate / state_mandatory / authoritative_continuation` provenance；
- `program_activated` 不进 immediate directory；
- selected-only JIT projection；
- 1,000 Player-known unrelated dossier no-load；
- deterministic background zero-router / zero-candidate-model；
- semantic-ready recovery 不重复 Router / Candidate Provider；
- one-domain Formal Change cardinality 未扩张；
- 无 G9-03 / Creator / Library scope drift。

---

## 6. Correction Gate

继续使用同一：

```text
branch
agent/g9-02c-router-core

worktree
D:\AI\Projects\.worktrees\sillytavern-agent
```

不得新开 fix branch。

Correction 完成后 Final Report 必须给出：

```text
Correction start HEAD
New Final SHA (40 chars)
origin/main
branch
full validation results
```

GPT 将对：

```text
0ee847e1173ae8d17e643d5b838d238cf889031e
→ New Final SHA
```

重新执行 exact-SHA Independent Review。

只有 PASS 后才允许 fast-forward main。
