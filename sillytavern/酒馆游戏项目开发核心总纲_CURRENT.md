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
G9                             ACTIVE / G9-02B breadth
G9-01 Compatibility Audit      PASS / CLOSED
G9-02A                         PASS / CLOSED
G9-02A final SHA               04603e1e4a3270e9f5740b5957cf545a2bd001d0
G9-02BC Shared Foundation      PASS / CLOSED
G9-02BC final SHA              5962e6f5933f245693e090cbdfd2f79791820ef1
G9-02BC Independent Review     PASS / CLOSED
G9-02B breadth                 ACTIVE / NEXT
G9-02C breadth                 BLOCKED BY G9-02B breadth
G9-03                          NOT AUTHORIZED
Current Next                   G9-02B breadth implementation
```

G9-02A 已完成 Source Binding、Game-local Definition Revision、typed existing-asset mutation、Save / Restore / Branch / Recovery。

G9-02BC 已完成两条后续 breadth 共用的运行时领域模块宿主、包/功能/模块启用边界、扩展定义记录与运行状态、正式变化、事件/交接、最小路由目录、按需上下文投影和有界上下文组合，并通过 exact-SHA Independent Review。当前没有 P0/P1 blocker。

#17 继续冻结：**Current Scene Visible Characters != Player-known Character Directory**。People Surface 必须是长期玩家已知人物目录，而不是当前场景 presence 列表。

---

## 1. 当前 active 正式来源

- `G9-01_资产兼容性审计与G9-02基础门禁_v1.0_2026-08-18.md`
- `G9阶段启动与G9-02实施切片裁定_v1.0_2026-08-18.md`
- `G9-02A_SourceBinding与GameLocalRevisionFoundation规格_v1.0_2026-08-18.md`
- `G9-02A_IndependentReview_SourceBinding与GameLocalRevision_v1.0_2026-08-19.md`
- `G9-02BC_SharedRuntimeFoundationConvergence规格_v1.0_2026-08-19.md`
- `G9-02BC_IndependentReview_共享运行时基础_v1.0_2026-08-19.md`
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
G9 Runtime Foundation             PARTIAL → continuing
G9-03 Schema Freeze               BLOCKED BY G9-02
```

继续保留：

- World Pack / Character Card / Expansion 分权；
- Source Definition != Game-local Instance != Runtime State；
- Program-owned Formal Outcome / Atomic Commit / Save；
- Open Attempt；
- Canonical Owner / typed dependency / handoff / contribution；
- Package / Feature / Module activation semantics；
- Runtime Context Contract；
- UI Surface Owner / Contributor 语义。

G9 不根据现有 Markdown frontmatter 猜 final machine schema。

---

## 3. G9-02A｜PASS / CLOSED

已证明：

```text
Source Asset
→ per-game bind + lineage
→ Game-local typed definition mutation
→ definition revision
→ Product canonical projection
→ Save / Restore / Branch / Recovery
```

关键“重要信件” reference case、two-game isolation、source immutability、Item/Character/Scene typed mutation、Recovery exactly-once、hidden disclosure 与 ordinary zero mutation call 均通过。

非阻塞历史迁移说明：pre-G9 数据没有 exact `createdRevision`，0011 migration 只能使用既有 `createdTurn` 作为历史回填种子；future consumer 不得将该 legacy 回填值视为可证明的历史 Event revision。

---

## 4. G9-02BC｜PASS / CLOSED

Final code：`5962e6f5933f245693e090cbdfd2f79791820ef1`

Independent Review：`PASS / CLOSED`。

已正式建立：

```text
程序内置领域模块宿主
↓
包 / 功能 / 模块绑定与启用
↓
按所有者区分的长期定义记录 / 运行状态
↓
类型化候选 / 正式变化 / 事件 / 交接
↓
最小路由目录
↓
选择校验
↓
只对选中模块按需生成上下文
↓
保留所有者身份的有界上下文组合
```

已证明：

- 资产不能注入任意 JS / callback / eval / query / state path；
- G9-02A Game-local identity / lineage / revision 继续是唯一 common metadata seam；
- 长期定义记录与运行状态分离；
- `Package Included != Feature Enabled != Module Enabled`；
- disabled module 对 candidate/change/routing/projection/handoff 全部 fail closed；
- hard dependency 不自动扩大 Context；
- Domain change 与 Formal Turn / Event / Narrative / Time / durable checkpoint 原子提交；
- Save / Restore / Branch / Recovery exactly-once；
- 100-module fixture 只投影显式选中的 1 个模块；
- migration 0012 rollback 与 0011 lineage preservation 通过。

02BC 没有实现完整 Model-first Router，也没有冻结 external schema。

---

## 5. G9-02B breadth｜ACTIVE / NEXT

默认执行者：**Grok Build**，按 G9-02BC 已冻结的内部轨道扩展。

目标：

- package / feature / module registry breadth；
- concrete built-in domain modules against frozen Host；
- definition registry / G8 UI Host contribution binding；
- **Player-known Entity / Character Directory**；
- persistence / migration / regression breadth。

必须保持：

```text
Canonical Character Truth
!= Player-known Character Directory
!= Current Scene Visible Character Set
```

People Surface 只消费长期 player-known / last-known safe projection；未认识 NPC 不泄露；角色离场后仍保留；死亡、失踪或远行不因为“不在当前场景”而自动从目录删除。

不得在 02B 提前实现完整 Model-first Router，也不得冻结 G9-03 external schema。

---

## 6. G9-02C breadth｜BLOCKED BY 02B

02C 重点：

```text
玩家输入
+ 已启用且预筛后的最小路由目录
+ 最小当前上下文
→ 模型判断当前相关领域
→ Program 校验 + 必要状态补充
→ 只对选中所有者按需投影
→ 有界组合
→ 领域处理 / 正式结果
→ 结果成立后才激活下游继续处理
```

必须证明：

- Model-first routing，不用关键词重复 NLP；
- state-mandatory augmentation；
- disabled fail-closed；
- deterministic background zero-model-call；
- no transitive prompt expansion；
- Bounded != Starved；
- Open Attempt；
- G8 Materialization Need 不回滚；
- large registry / long session 不导致普通 Turn 上下文线性增长；
- People Directory scale stress。

02BC Review 明确保留的 02C 深水区：

1. 玩家语义 → 模块选择的授权/evidence 闭环；
2. 大量 enabled modules 时进一步预筛 routing working set；
3. outcome-gated downstream continuation。

---

## 7. Sol / Grok Build 资源策略

正式治理：`G9及后续阶段_Agent资源分配与GrokBuild代码协作裁定_v1.0_2026-08-19.md`。

当前新增资源安排：

```text
G9-02BC                    Sol         PASS / CLOSED
G9-02B breadth             Grok Build  ACTIVE / NEXT
GPT                        exact-SHA Independent Review
最后约一次 Sol 深任务       RESERVED for G9-02C core
其余 02C breadth           Grok Build
```

最后一次 Sol **暂不立即消耗在 02B 常规 breadth**。若 02B Grok Build review PASS，优先用于 G9-02C 的模型路由 / 上下文编排核心；若额度因套餐变化提前不可用，02C 仍可由 Grok Build 按 frozen rails 执行，不形成项目 blocker。

同一 repo 默认 serialized writer；Executor != Independent Reviewer。

---

## 8. G9-03 Freeze Gate

只有 02B breadth + 02C breadth + G9-02 Integrated Closure 全部 PASS，才能冻结：

- source asset machine identity/version/hash；
- source → local binding wire；
- immutable/evolvable/private field policy；
- owner / namespace；
- dependency / conditional / handoff / contribution；
- package / feature / module activation；
- routing / context contract；
- UI declaration；
- materialization / mutation eligibility；
- runtime binding metadata；
- compatibility / migration metadata；
- Manifest / Bundle relationship。

禁止：

- 把 Player-known Character Directory 做成 Source Character Card 字段；
- 把 current visibility 与 long-lived acquaintance/knowledge lifecycle 合并；
- 提前冻结 People / Objective / Information UI taxonomy；
- 把 Discussion-only Opening Scenario wire 提前塞进 external schema。

---

## 9. 不可回滚 Authority

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

---

## 10. Owner UAT / Product Backlog 路由

```text
Item durable known-description evolution
→ G9-02A PASS / CLOSED

People persistent known-character directory
→ G9-02B ACTIVE
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

Opening Scenario 保持 Discussion Draft：玩家端入口与 Prologue Runtime 暂不开放；未来先做 Creator 最小 authoring prototype + 首轮真实创作复盘，再决定正式 Runtime / asset-spec。

---

## 11. 当前 Next

> **G9-02B breadth implementation，默认由 Grok Build 执行。**

Implementation Base：`5962e6f5933f245693e090cbdfd2f79791820ef1`。

最后约一次 Sol 深任务保留给 02C core。

G9-03 当前 `NOT AUTHORIZED`。
