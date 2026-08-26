# G9-02C Core｜Correction-01 Independent Review｜剩余阻塞 v1.0

状态：`REVIEW FAIL / CORRECTION-02 REQUIRED`
日期：2026-08-19

## 1. Review Identity

```text
Implementation repo
zhangchenjia21-dot/sillytavern

Base
0ee847e1173ae8d17e643d5b838d238cf889031e

Reviewed Final SHA
8941076666345ac816312a55bf054b919d6a0919

Task branch
agent/g9-02c-router-core

Topology
base → final = ahead_by 4 / behind_by 0
merge base = exact Base

origin/main
0ee847e1173ae8d17e643d5b838d238cf889031e
```

结论：

```text
P0 = 0
P1 = 2
P1-03 = CLOSED
main = unchanged
G9-02C Core = NOT PASS
G9-02C Breadth = BLOCKED
G9-03 = NOT AUTHORIZED
```

---

## 2. 已关闭｜P1-03 Player-safe Relationship Subgraph

Correction-01 已增加显式 `PlayerSafeRelationshipEligibility`：

```text
allowedCharacterRefs
+ optional allowedRelationshipRefs
```

Traversal 两端都必须已经在 player-safe character eligibility 内；未认识 / 当前不可见角色不会因 Runtime relationship edge 首次泄漏。Hidden runtime slots 继续过滤，canonical directed edge 不生成反向 mirror。

本项 PASS。

---

## 3. P1-01 仍阻塞｜Global routing call bound 会静默漏掉未访问 profile

Correction-01 新增 `rootWorkingSets()` / `refineWorkingSets()`，单层 17+ fan-out 已能分页；这关闭了“超过 16 就立即抛错并 ACTION_REJECTED”的原始缺陷。

但当前 Orchestrator 在：

```text
routingModelCallCount >= modelCallsPerTurn(8)
```

时直接：

```text
break
```

之后仍按当前已收集 selection 返回 `none/change`。

因此当 hierarchy/page 总量需要 >8 个 Router call 时：

```text
尚未访问的合法 Package / Feature / Module profiles
→ silent omission
→ router miss without explicit exhaustion evidence
```

例如 root 有 8 页，目标 package 在第 8 页且仍需要 feature/module refinement；第 8 次 call 之后 queue 仍有 refinement page，但流程直接结束。玩家 Core Attempt 不再被判 illegal，这是进步；但相关 Domain 仍可能被静默漏掉。

这不满足 Correction-01 已冻结要求：

```text
不静默漏掉尚未被模型看见的合法 routing profile
hard global bound reached
→ explicit routing exhaustion / safe failure evidence
```

也不满足长期“Enabled Modules ↑↑↑，ordinary working set bounded，但 relevant domain 不能被任意目录位置饿死”的目标。

### Correction-02 要求

必须让 routing exhaustion 成为显式 orchestration result / evidence，而不是 `break` 后伪装成普通 router miss。

至少区分：

```text
normal miss
semantic clarification
malformed/unsafe model decision
routing capacity exhausted
```

Core legal action 仍不能因此自动变 illegal；但 capacity exhausted 也不能被记录为普通 `selection=[] / candidateProposed=false`。

并新增目标位于“超过 8-call 后才可完成 refinement”的测试。

---

## 4. P1-02 仍阻塞｜Group descriptor 仍随 child 数量增长，且 hash truncation 不等于 semantic sufficiency

Correction-01 去掉了原来的 arbitrary first-4 child truncation，这是正确方向；但当前 `RuntimeDomainRoutingGroupDescriptor` 仍是运行时从所有 child metadata 合成：

```text
Package typicalSemantics
= one token per feature

Feature typicalSemantics
= one token per capability
```

且单 token 超长时采用：

```text
prefix + hash
```

两个问题仍存在：

### A. 单个 group profile size 仍随 child count 增长

例如一个 package 含大量 feature，`typicalSemantics[]` 会线性增长。即使 Working Set 只有这 1 个 package profile，也可能超过 8,000 chars，最终 `paginateWorkingSets()` 连单 profile 都无法承载并触发 capacity limitation。

这说明：

```text
model-visible group profile size
仍可能随 descendants 数量增长
```

没有真正实现 bounded semantic group descriptor。

### B. prefix + hash 只保留完整性指纹，不保证模型可理解语义

当真实区分语义位于长 tag/capability 的后半段时，hash 对模型没有语义意义；“没有丢 child item identity”不等于“保留了模型判断 refine 所需的语义”。

Correction-01 的 5-semantic fixture 证明了“第五项不再被数组截断”，但没有证明“大量 child 时 parent profile 仍 bounded 且语义充分”。

### Correction-02 要求

Package / Feature group 必须拥有**独立、静态、bounded、模型可理解的 Program-owned semantic descriptor**，而不是把所有 child semantics 全量拼接后再压缩。

推荐内部语义：

```text
package/feature stable routing ref
label
one-line scope
small typical semantics
```

这些 descriptor 由 Program registry 显式提供 / 注册；child directory 用于 structural expansion，不用于动态堆出 parent prompt。

本轮仍只冻结 internal seam，不冻结 G9-03 external schema。

至少证明：

1. 100+ descendants 时单个 group profile size 仍近似常量；
2. target semantic 位于任意 child 位置都不依赖 first-N / prefix truncation；
3. production catalog → model router 可以依据 group descriptor 做 refinement；
4. child count 增长不会让一个 group profile 自身超过 working-set bound。

---

## 5. 保持通过的核心轨道

Correction-02 不得回滚：

- Authorized Turn Anchors；
- Domain selection != new player authorization；
- candidate `authorizedTurnRef` + referenced refs；
- module-owned authorization validation；
- model/state/continuation provenance；
- state-mandatory deterministic participation；
- program_activated recipient no eager routing；
- selected-only JIT projection；
- 1,000 Player-known unrelated dossier no-load；
- Player-safe relationship eligibility；
- deterministic background zero Router / zero Candidate Provider；
- semantic-ready recovery non-duplication；
- one-domain Formal Change cardinality；
- no G9-03 / Creator / Library scope drift。

---

## 6. Next Gate

继续使用：

```text
agent/g9-02c-router-core
D:\AI\Projects\.worktrees\sillytavern-agent
```

不得新开 fix branch。

Correction-02 完成后必须返回新的 40-char Final SHA，GPT 再对：

```text
0ee847e1173ae8d17e643d5b838d238cf889031e
→ New Final SHA
```

执行 exact-SHA Independent Review。

只有 P0=0 / P1=0 后才允许 fast-forward main。