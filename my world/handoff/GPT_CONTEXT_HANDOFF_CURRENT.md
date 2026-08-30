---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-08-30
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G4-02R1
current_execution_task: G4-02R1M1-IR01
semantic_owner: GPT
current_execution_owner: Codex
---

# my world｜GPT CONTEXT HANDOFF CURRENT

> 本文件是接管导航 / 最小充分摘要，不是新的 Product / Architecture / Status 权威。
>
> 新 GPT 必须先刷新两个 GitHub `main` HEAD，并以 current Product / Principles / Architecture / Roadmap / Status / implementation facts 为准。

---

## 1. 接管第一动作

刷新：

```text
Implementation: zhangchenjia21-dot/my-world
Governance:     zhangchenjia21-dot/Vibe-Coding
```

然后优先读取：

### Governance

1. `AGENTS.md`
2. `my world/MY_WORLD_项目启动总纲_CURRENT.md`
3. `my world/MY_WORLD_核心设计原则_CURRENT.md`
4. `my world/MY_WORLD_架构_CURRENT.md`
5. `my world/MY_WORLD_总体规划路线图_CURRENT.md`
6. `my world/MY_WORLD_CURRENT_STATUS.md`
7. `my world/architecture/source/G4_SOURCE_SEMANTIC_OWNERSHIP_AND_REAUDIT_DECISION.md`
8. `my world/architecture/source/G4_GAME_LOCAL_EVOLVABLE_SEMANTICS_DECISION.md`
9. `my world/architecture/source/G4_T0_SCOPED_SOURCE_AND_POST_T0_CANON_QUARANTINE_DECISION.md`
10. `my world/architecture/source/G4_OPTIONAL_TEMPORAL_SOURCE_SCOPE_DECISION.md`

### Implementation

11. `AGENTS.md`
12. `docs/tasks/G4-02R1M1-IR01_OPTIONAL_TEMPORAL_SCOPE_EVIDENCE_CORRECTION_TASK.md`
13. `docs/tasks/G4-02R1M1_SOURCE_V0_2_R2_MECHANISM_CORRECTION_TASK.md`
14. `docs/source/G4-02R1_R2_MECHANISM_IMPLEMENTATION_EVIDENCE.md`
15. `docs/source/World Pack与Character Card合同v0.2_SEMANTIC_FREEZE.md`
16. `docs/source/G4-02R1_T0_SCOPED_SOURCE_CONTRACT_ADDENDUM.md`
17. `docs/source/G4-02R1_OPTIONAL_TEMPORAL_SCOPE_CAPABILITY_CLARIFICATION.md`
18. `docs/source/G4-02R1_T0_CHARACTER_INDIVIDUALITY_ADDENDUM.md`
19. `docs/source/G4-02R1_FIXED_T0_MULTI_ENTRY_CHARACTER_BINDING_CLARIFICATION.md`
20. `docs/source/G4-02R1_CROSS_FAMILY_PACKAGE_SHAPE_STABILITY_DECISION.md`

不要默认重读全部历史仓库。

---

## 2. 产品核心

`my world` 是 local-first、single-player-first 的长期 AI RPG。

核心原则：

> **Model freedom first. Reversibility over prevention.**
>
> **Narrative richness over artificial brevity.**
>
> **Model authors the world; Runtime makes it durable; Player owns the timeline.**
>
> **玩法拉出 Schema，不让玩法迁就万能 Schema。**

---

## 3. Source v0.2-r2 当前语义

World / Character 使用 rich `semantic_sections` + package-local UTF-8 Markdown/TXT；`section_type` 是开放 semantic hint，不是 closed ontology。

Disclosure：

```text
gm_reference
gm_private
```

但：

```text
Disclosure != Character Knowledge != Player Knowledge
```

Exact generation fingerprint 覆盖所有声明 bytes，包括当前未选择或 private 的文件。

> **Fingerprint coverage != Runtime visibility.**

---

## 4. Temporal scope｜2026-08-30 Owner clarification

T0-scoped / post-T0 canon quarantine **不是所有 Source 的强制模式**。

正式决策：

`architecture/source/G4_OPTIONAL_TEMPORAL_SOURCE_SCOPE_DECISION.md`

正式实现语义澄清：

`my-world/docs/source/G4-02R1_OPTIONAL_TEMPORAL_SCOPE_CAPABILITY_CLARIFICATION.md`

游戏必须同时支持：

```text
Temporal-scoped Source
→ 多个真实 authored T0 cut
→ exact selected projection
→ future/canon quarantine

Non-temporal Source
→ rich top-level starting semantics
→ 不强迫建立 T0 profile matrix
```

典型需要 quarantine：三国等有多个历史开局、后期史实会污染早期开局的资产。

典型不需要：原创/幻想人物只有一个稳定起始版本，没有多个时间切片需求。

不要增加：

```text
historical=true/false
temporal_mode
requires_quarantine
family mode
```

等全局硬分类。

`character_card.v0.2` 的 `t0_profiles` 保持 optional。没有 temporal variation 的 Character 可以只用 top-level `semantic_sections`。

World Entry 可以只是 scenario/opening/location choice；存在 Entry 不等于必须做历史未来隔离。

当前 Afterglow 三个 Entry 共用一个 1287 profile 是合法 authored choice，但不是未来幻想资产必须复制的模板。

---

## 5. Temporal-scoped Source 的严格规则

当资产真实使用 temporal scoping 时：

> **Do not show the model a post-T0 answer and then ask it to forget that answer.**

```text
Source Package Total Content
!= Selected T0 Source Projection
!= Game-local Reality
!= Runtime Relevant Set
!= Model-visible Working Set
```

World：

```text
top-level always-safe sections
+ exact selected Entry sections
```

Character：

```text
top-level always-safe sections
+ exact matching T0 profile sections
```

禁止 latest / nearest-year / later-profile / complete-life biography fallback。

若 Character 对 World W 声明过任意 profile binding：

```text
exact Entry binding exists -> compatible
selected Entry binding missing -> temporal incompatible
```

若对 W 完全无 binding：走 `no_world_coverage` / always-safe-only，不得凭 family 猜测 hard block。

历史可自然重现，但没有特殊收敛权：

> **No convergence force. No divergence force. Causality first.**

---

## 6. Character individuality

未来隔离不能把早期人物削成 generic child / generic youth。

> **Age / developmental stage is a capability boundary, not a personality template.**

T0 profile 保留截至该时点已形成的个体差异、经历、能力、知识来源、关系历史、压力反应和开放成长空间；禁止用成年 stereotype 或未来结果倒推早期人格。

---

## 7. G4-02R1M1 已完成什么

Codex 已完成 production mechanism implementation：

- v0.2-r2 validator / loader；
- exact-generation fingerprint；
- exact selected World Entry projection；
- exact Character T0 profile projection；
- temporal compatibility states；
- G4-03/G4-05 regressions；
- Godot/Windows evidence。

Implementation commit:

`eb11655f8ff592ae096915fab50553708c0b79df`

Evidence commit:

`3b2c9d5fa0a9756ee670eb42179b5ae8359e0b2b`

GPT first Independent Review found **no blocking production defect**.

---

## 8. 当前唯一活动任务｜IR01

Current execution task:

> **G4-02R1M1-IR01 — Optional Temporal Scope Evidence Correction**

Owner: **Codex**

Packet:

`docs/tasks/G4-02R1M1-IR01_OPTIONAL_TEMPORAL_SCOPE_EVIDENCE_CORRECTION_TASK.md`

Formal Code Base:

`74d1fe8d93e527794db33d0280d76d16674ae1d7`

Governance Base:

`14fb4d686c458c2a09a348e54088377f96655d65`

Packet commit:

`f020848f23464c6022358acf2aa9b82bc940fd83`

Expected shape: **test/evidence correction only**.

Required evidence：

```text
刘备: 220 exact; 229/263/280 incompatible
曹操: 214 exact; 220/229/263/280 incompatible
孙权: 249 exact; 263/280 incompatible
```

以及：

- synthetic non-temporal Character，无 `t0_profiles`，rich top-level semantics 可正常 load/project；
- synthetic non-temporal World，有 Entry 但不需要 temporal section matrix；
- 不增加 historical/temporal global switch；
- Han future quarantine 继续严格；
- Afterglow 当前三 Entry shared profile 继续工作；
- G4-03/G4-05 regressions 继续 green。

若新测试暴露 production defect，Codex 必须明确报告，不能静默扩大 scope。

Return ceiling：

> **READY FOR INDEPENDENT REVIEW**

---

## 9. 新 GPT 应如何行动

如果 IR01 **尚未回包**：

- 不重复实现；
- 不再另发同一个任务；
- 保持 frozen Meaning；
- 等 Codex 正式 return。

如果 IR01 **已经回包**：

GPT 立即做 final Independent Review，重点检查：

1. Sun Quan 280 是否真的走 production exact-binding path；
2. non-temporal Character 是否真的没有 `t0_profiles`；
3. non-temporal World 是否真的没有被人为建立历史时间矩阵；
4. 是否偷偷增加 global historical/temporal flag；
5. Han future quarantine 是否仍严格；
6. frozen 2 World + 6 Character 是否未被修改；
7. G4-03/G4-05 regressions 是否有效。

若 PASS：Decision Propagation 后关闭 G4-02R1，并恢复 G4-05 closure/regression。

---

## 10. 后续顺序

```text
NOW
G4-02R1M1-IR01 Codex evidence correction
↓
GPT final Independent Review
↓
if PASS
close G4-02R1
↓
resume G4-05 closure/regression
↓
G4-06 Atomic Final Create
↓
G4-07 First Playable A — Owner UAT
```

G4-07 Owner UAT 必须验证：Narrative richness、Character individuality、anti-convergence、人物随本局经历发展、Context bounded but not starved。

G4-05 当前仍 `REWORK / HOLD`；G4-06+ 当前仍 `HOLD`。

---

## 11. 治理纪律

- current explicit Owner instruction > current Product/Principles/Architecture/Roadmap/Status > implementation facts > generic skill > history/memory；
- Freshness != Decision Propagation；
- GPT owns Meaning; Codex owns Mechanism；
- routine build/test/Git/Windows evidence 属于 Agent；
- Owner 负责产品方向和真实 UAT；
- 不 destructive rollback；repair forward；
- Engineering PASS 不等于 Product PASS。
