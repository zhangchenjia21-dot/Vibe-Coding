---
title: my world｜G4 Source Semantic Ownership & Re-audit Decision
status: current-canonical-architecture-decision
created: 2026-08-29
updated: 2026-08-29
phase: G4 Primary Source Assets & Local Game Creation
owner: GPT
implementation_repo: zhangchenjia21-dot/my-world
---

# G4 Source Semantic Ownership & Re-audit Decision

## Decision

Owner 明确调整 G4 Source 工作分工：

> **GPT owns Meaning; Codex owns Mechanism.**

具体：

- World Pack / Character Card / Expansion Pack 的产品语义、信息归属、GM-facing authored content、Source→Game-local 语义边界，由 GPT 主导设计与审核；
- 历史真实资产的语义迁移与 fidelity review，由 GPT 主导；
- Codex 负责已冻结语义后的 GDScript、validator/loader、filesystem/library、SQLite、UI wiring、tests、Windows/build/runtime engineering；
- Codex 不再独立决定复杂叙事资产应如何压缩、拆分或映射。

## Why

G4-05 Independent Review 发现：虽然 Wizard / exact Composition / Review 工程路径成立，但 historical real-asset conversion 把大型 World / Character 内容高度摘要化。工程测试只能证明 compact conversion package 能通过 parser/library，不能证明 current contract 真能承载真实资产复杂性。

这暴露了一个 ownership 问题：复杂 Source semantics 与工程实现不应默认由同一 implementation Agent 一并定义、自建 fixture、自证通过。

当前核心原则本已要求：

- `Narrative richness over artificial brevity.`
- `玩法应该拉出 Schema；不要让玩法迁就先写好的万能 Schema。`
- 未发布 v0.1 若语义模型错误，应直接修正，而不是建立兼容层森林。

## No Git rollback

**不回滚到 G4-02 之前，也不撤销 G4-03/G4-04/G4-05 已完成的工程提交。**

原因：

- G4-03 的核心价值是 immutable Source generation / install / exact lookup / tamper detection；它主要依赖 Source loader 的公开 contract，而不是某个角色究竟有几段人格文字；
- G4-04 Multi-Game / per-Game SQLite 与 Source 文本语义独立；
- G4-05 Wizard 已通过审查的 selection/composition 语义主要依赖 exact identity、World entries、Character eligibility 与 display projection；这些能力应保留，后续只需在 Source contract 调整后做回归复验。

因此采取：

```text
preserve verified engineering history
+ reopen semantic decision
+ correct contract/content forward on main
+ rerun affected regressions
```

而不是 destructive rollback。

## Reopened Gate｜G4-02R1

G4-02 历史 Engineering PASS 保留，但其 **Source semantic adequacy** 因真实资产证据不足而重新打开：

> **G4-02R1 — World / Character Source Semantic Re-audit**

Owner: GPT。

G4-02R1 必须先用真实 `汉末三国` 与 `诸界余辉` 资产反向审计：

1. 哪些内容真正属于 reusable World Source；
2. 哪些内容真正属于 reusable Character Source；
3. 哪些内容属于 Entry/T0；
4. 哪些属于 Expansion；
5. 哪些属于某一局 Game-local / Runtime live state，必须排除；
6. current v0.1 是否能自然、可读、无过度压缩地承载这些内容；
7. 若 contract 过薄，最小正确修正是什么。

不允许先假定 current schema 正确，也不允许为了迁移方便把历史资产摘要成 compact fixture。

## G4-05R1 disposition

原：

`my-world/docs/tasks/G4-05R1_REAL_ASSET_FIDELITY_CORRECTION_TASK.md`

现状态：**SUPERSEDED / DO NOT EXECUTE**。

原因不是 fidelity finding 消失，而是 correction ownership 和顺序被改正：先由 GPT 完成 G4-02R1 semantic re-audit，再决定 actual asset migration 与是否需要 code correction。

## Execution order from now

```text
G4-02R1 GPT semantic re-audit
→ freeze corrected World/Character semantic contract
→ GPT performs / owns real-asset semantic migration specification and content
→ if code contract changes are required: issue narrow Codex implementation correction
→ Codex runs engineering regression / Windows evidence as needed
→ GPT Independent Review of engineering + content fidelity
→ resume G4-05 closure
→ G4-06
```

Codex has **no active implementation task** during G4-02R1 semantic design.

## Preservation status

- G4-03: remains PASS/CLOSED unless the corrected contract exposes a concrete regression; then repair forward, do not erase history.
- G4-04: remains PASS/CLOSED.
- G4-05 implementation candidate `145c3e1`: preserved as provisional accepted Wizard/Composition engineering, but G4-05 remains REWORK/HOLD until real Source semantics and assets are corrected and revalidated.
- G4-06+: HOLD.
