---
title: G9-05G｜设计前审计结论｜真实 Expansion 行为缺口
status: current-design-audit
version: 1.0
updated: 2026-08-22
---

# G9-05G｜设计前审计结论｜真实 Expansion 行为缺口 v1.0

## 结论

在 `main@26d23d47c5f5ac42d3e1029654a64eda831c4db1` 上进行 Primary Asset End-to-End Closure 设计前审计后，确认：

1. G9-04 已有真实 World + Character + Expansion Markdown Adapter / exact Manifest / binding / Save-Restore 证据；
2. G9-05E 已有 published Source exact selection → deterministic materialization → G9-04 binding → Runtime bootstrap；
3. G9-05F 已有真实 Expansion Creator + Program Host Gate；
4. 但 G9-04 real EP-CHAR-CORE binding 使用 `builtin:g9-04.ep-char-core-proof.v1`，明确为 proof-only，不具备可声称为 production gameplay/runtime behavior 的语义；
5. 当前已有真正有状态行为的 `builtin:player-knowledge.character-directory.v1` 属于 Runtime Core 的玩家已知人物目录语义，与 EP-CHAR-CORE 不同，不能为了通过 E2E 强行映射；
6. `EP-CHAR-CORE Runtime Context Contract` 已明确存在 production-ready 的最小语义切片：bounded character capability evidence projection；
7. 刘备当前 real Character Card 的「能力与局限」已明确声明为 `T0 Capability Bootstrap` 证据输入。

因此正式决定：

```text
先 G9-05G0：真实 EP-CHAR-CORE production Runtime binding / evidence projection
再 G9-05G：三类主资产 Primary E2E
```

不接受以下假闭环：

```text
proof-only module binding exists
→ 声称 Expansion gameplay 已完整接通
```

也不接受：

```text
unrelated Program module
→ 伪装成 Source Expansion implementation
```

本审计记录用于解释 G9-05G0 的必要性；正式技术规则以 G9-05G0 / G9-05G canonical spec 为准。
