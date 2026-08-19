---
title: 酒馆游戏项目开发核心总纲
status: current-integrated
updated: 2026-08-19
current_path: sillytavern/酒馆游戏项目开发核心总纲_CURRENT.md
---

# 酒馆游戏项目开发核心总纲｜CURRENT

## 0. 当前状态

```text
G1–G8                         PASS / CLOSED
G8-UAT-02 final SHA            cdbd9cd7ff0b5b9a5672156066478b57f732307c
Project Owner Stage UAT        PASS WITH NON-BLOCKING UX FINDINGS
P0                             0
P1                             0
G9                             ACTIVE / G9-02A
G9-01 Compatibility Audit      PASS / CLOSED
G9-02A                         ACTIVE / NEXT
G9-02B                         BLOCKED BY G9-02A
G9-02C                         BLOCKED BY G9-02B
G9-03                          NOT AUTHORIZED
Current Next                   G9-02A Source Binding + Game-local Revision Foundation
```

G9-01 已完成对现有 Semantic Assets、G8 Runtime、#15 Context Orchestration 与 #16 Game-local Asset 模型的兼容性审计。结论不是“资产需要推倒重做”，而是：**现有资产语义总体兼容；当前 Runtime 的资产承接层不足以直接冻结 external asset-spec。**

新增 #17 裁定进一步冻结：**Current Scene Visible Characters != Player-known Character Directory**。People Surface 必须是长期玩家已知人物目录，而不是当前场景 presence 列表；该裁定不打断正在执行的 G9-02A，传播到 G9-02B/C 与后续 Product maturity。

---

## 1. 当前 active 正式来源

- `G9-01_资产兼容性审计与G9-02基础门禁_v1.0_2026-08-18.md`
- `G9阶段启动与G9-02实施切片裁定_v1.0_2026-08-18.md`
- `G9-02A_SourceBinding与GameLocalRevisionFoundation规格_v1.0_2026-08-18.md`
- `17_酒馆游戏_PlayerKnownEntityDirectory与长期资料表面信息架构裁定_v1.0_2026-08-18.md`
- `G9及后续阶段_Agent资源分配与GrokBuild代码协作裁定_v1.0_2026-08-19.md`
- `G8_StageUAT_最终收口与体验Backlog_v1.0_2026-08-18.md`
- `15_酒馆游戏_RuntimeContextOrchestration与模块化复杂度控制裁定_v1.2_2026-08-18.md`
- `16_酒馆游戏_RuntimeWorldMaterialization与当局游戏资产演化裁定_v1.1_2026-08-18.md`
- `酒馆游戏新版主体重建总路线 v2.0.md`

Discussion-only, not implementation authority:
- `G9_世界包OpeningScenario与Creator首轮创作验证_DRAFT_v0.4_2026-08-19.md`

---

## 2. G9-01 最终结论

```text
Semantic Asset Architecture       COMPATIBLE
Existing Assets Rewrite           NOT REQUIRED
Current Runtime Asset Foundation  PARTIAL
G9-02 Foundation                  REQUIRED
G9-03 Schema Freeze               BLOCKED BY G9-02
```

已确认可以保留：

- World Pack / Character Card / Expansion 分权；
- Source Definition != Game-local Instance != Runtime State；
- Program-owned RNG / Judge / Formal Outcome / Atomic Commit / Save；
- Open Attempt；
- Canonical Owner / typed dependency / handoff / contribution；
- Package / Feature / Module activation semantics；
- Runtime Context Contract 18 项语义；
- UI Surface Owner / Contributor 语义。

因此 G9 不先批量改资产 Markdown，也不根据现有 frontmatter 猜 final machine schema。

---

## 3. G9-02 Mandatory Runtime Gaps

G9-01 + #17 当前确认 G9-03 前必须补齐：

1. **Source lineage / binding**：source stable identity / type / version / hash / bind ancestry；
2. **Game-local definition revision / typed mutation**：已有 Character / Item / Place/Scene 等定义字段可按 owner-policy 演化；
3. **Full Game-local canonical layer**：未来 Relationship / Knowledge / Objective / Organization / Politics / Economy / War / Magic 等 owner-typed records 不被五类硬编码堵死；
4. **Extensible Runtime State ownership**：不能永远只靠 `character | item | relationship` Core enum；
5. **Package / Feature / Module Binding Host**；
6. **Runtime Domain Extension seam**：typed candidate / event / handoff / projection，资产不上传任意代码；
7. **Generic Context Orchestrator**；
8. **Save / Restore / Branch** 覆盖 source bindings、definition revisions、extension records/domain state；
9. **Objective architecture guard**：不实现完整 Objective Engine，但不能把未来 Objective owner 封死；
10. **Player-known Entity Directory**：长期已知人物目录与 current-scene visible/interactable set 分离，只投影 player-known / last-known safe facts。

---

## 4. G9-02 内部实施顺序

```text
G9-02A Source Binding + Game-local Revision Foundation
↓
G9-02B Runtime Domain / Expansion Binding Host
↓
G9-02C Context Orchestration Foundation
↓
G9-02 Integrated Closure
↓
G9-03 asset-spec vNext Machine Contract
```

这是同一 G9-02 阶段内的 implementation slicing，不新增生命周期阶段。

### G9-02B 新增 #17 Gate

必须使：

```text
Current Scene Visible Characters
!=
Player-known Character Directory
```

People Surface 只消费长期 player-known character projection；未认识 NPC 不因 Game-local existence 被泄露；角色离场不从目录删除。

### G9-02C 新增 #17 Stress Gate

```text
Known Character Directory size ↑↑↑
ordinary unrelated Turn context ≈ bounded
```

current-scene characters 与 referenced/relevant known characters 分开选取，People Directory 不直接等于 Prompt inclusion。

---

## 5. 当前 G9-02A

必须证明：

```text
Source Asset Descriptor / Snapshot Identity
↓ bind
Game-local Canonical Identity + Lineage
↓ typed definition mutation
Game-local Definition Revision
↓ Runtime / Product projection
↓ Save / Restore / Branch / Recovery
```

关键 Reference Case：

```text
已有“重要信件”
→ 玩家检查并发现应长期保留的新公开细节
→ AI typed Item definition patch
→ Program validation / atomic local revision
→ Product 读取更新后的 canonical description
→ Save / Restore 保留
→ Restore 到发现前恢复旧描述
→ Source Asset 不变
```

G9-02A 只使用 handwritten normalized fixtures，不解析 external Markdown，不冻结 asset-spec。

#17 对当前已经发出的 G9-02A Codex 任务：`NO CHANGE / DO NOT INTERRUPT`。

---

## 6. Agent 执行策略｜Sol 稀缺、Grok Build 常态化

当前正式治理：`G9及后续阶段_Agent资源分配与GrokBuild代码协作裁定_v1.0_2026-08-19.md`。

```text
GPT
= Product / Architecture Lead
+ Freshness / Decision Propagation
+ Canonical Spec / Task Packet
+ exact-SHA Independent Review

Sol
= scarce high-risk shared-foundation executor

Grok Build
= default bounded implementation executor after contract freeze
```

当前建议：G9-02A 完成并 review 后，若 Sol 只剩约一次深任务，将其保留给：

`G9-02BC Shared Runtime Foundation Convergence`

目标是冻结并 production-proof：

```text
Runtime Domain Module Host
↔ owner-typed canonical/state extension seam
↔ typed candidate/event/handoff/projection seam
↔ Package/Feature/Module activation boundary
↔ Context Projection Provider / Orchestrator boundary
```

Sol 不负责扩完所有 Domain / UI / Creator / Adapter。该 shared foundation 通过后，G9 后续默认由 Grok Build 主导 implementation，GPT 继续独立审核。

Grok Build 已正式允许在冻结合同下承担后端代码、测试、Adapter/Compiler 与 Creator，不再只限 UI；但不得自行重定义 canonical owner、Save/Restore/Recovery、Context authority 或 external schema。

同一 repo 默认 serialized writer；Codex 与 Grok 不同时直接修改重叠 main 工作区。

---

## 7. 不可回滚的 G1–G8 Authority

继续保持：

- Semantic AI judges open semantics；Program 不重复 NLP；
- Program Final Outcome authority；
- No Phantom Interactable；
- Player Agency / Open Attempt；
- World Materializer need-gated；
- Source 不被 Runtime 反写；
- Save / Restore / Branch；
- Crash / Recovery / Idempotency；
- private / public disclosure boundary；
- Runtime Relevant != Model Visible；
- Dependency Graph != Context Inclusion Graph；
- Bounded != Starved。

新增：

```text
Canonical Character Truth
!= Player-known Character Directory
!= Current Scene Visible Character Set
```

---

## 8. Owner UAT 非阻塞 Backlog 路由

```text
Item durable known-description evolution
→ G9-02A reference case

People persistent known-character directory
→ G9-02B Runtime/Product semantic correction
→ G9-02C bounded-context proof

DeepSeek model selector
→ G10 Provider Expansion

Game delete lifecycle
→ G11 Alpha

carried Item description display polish
→ G11 Product polish

People / Information / Objective classification/search/filter/history organization
→ G11 Product information architecture maturity

full Objective / Task vertical
→ dedicated later vertical，最迟 G11 前
```

Opening Scenario 当前保持 Discussion Draft：玩家端入口与 Prologue Runtime 暂不开放；先在未来 Creator 阶段做最小 authoring prototype + 首轮真实创作复盘，再决定正式 Runtime / asset-spec。

---

## 9. 长期资料 Surface 信息架构原则

会长期累积的资料 Surface 不得通过“只保留当前可见项”规避规模问题。

```text
People
Information / Knowledge
Objective / Task
future long-lived dossier surfaces
```

G9 保留 stable identity、status/source/player-safe facet metadata 与正确 owner；G11 再实现具体 grouping / filter / sort / search / active-history separation / pagination 等交互。

---

## 10. 当前 Next

> **G9-02A｜Source Binding + Game-local Revision Foundation implementation。**

G9-03 当前 `NOT AUTHORIZED`。
