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
G9                             ACTIVE / G9-02C breadth
G9-01 Compatibility Audit      PASS / CLOSED
G9-02A                         PASS / CLOSED
G9-02A final SHA               04603e1e4a3270e9f5740b5957cf545a2bd001d0
G9-02BC Shared Foundation      PASS / CLOSED
G9-02BC final SHA              5962e6f5933f245693e090cbdfd2f79791820ef1
G9-02B Player-known breadth    PASS / CLOSED
G9-02B final SHA               0ee847e1173ae8d17e643d5b838d238cf889031e
G9-02B Independent Review      PASS / CLOSED
G9-02C Core Design             FROZEN / IMPLEMENTATION NEXT
G9-02C breadth                 ACTIVE / NEXT
G9-02 Integrated Closure       BLOCKED BY G9-02C
G9-03                          NOT AUTHORIZED
Current Next                   G9-02C Context Orchestration
```

G9-02A 已完成 Source Binding、Game-local Definition Revision、typed existing-asset mutation、Save / Restore / Branch / Recovery。

G9-02BC 已完成 02B / 02C 共用的运行时领域模块宿主、包/功能/模块启用边界、扩展定义记录与运行状态、正式变化、事件/交接、最小路由目录、按需上下文投影和有界上下文组合，并通过 exact-SHA Independent Review。

G9-02B 已完成长期 Player-known Character Directory、People Surface 迁移、证据边界、修订一致性、老存档兼容与 Save / Restore / Branch / Recovery。当前 P0=0 / P1=0。

G9-02C Model-first Routing / Context Orchestration 核心设计已冻结，当前实现范围不因 #18 资料库裁定临时扩项。

#17 继续冻结：**Current Scene Visible Characters != Player-known Character Directory**。

#18 v1.1 继续冻结：**世界包 / 角色卡 / 拓展包继续是三类主资产；资料库是长期资料资源层，但属于后置增量功能。资料库不得成为 G9 三类主资产首轮协议、Creator 或创建游戏闭环的 blocker。**

---

## 1. 当前 active 正式来源

- `G9-01_资产兼容性审计与G9-02基础门禁_v1.0_2026-08-18.md`
- `G9阶段启动与G9-02实施切片裁定_v1.0_2026-08-18.md`
- `G9-02A_SourceBinding与GameLocalRevisionFoundation规格_v1.0_2026-08-18.md`
- `G9-02A_IndependentReview_SourceBinding与GameLocalRevision_v1.0_2026-08-19.md`
- `G9-02BC_SharedRuntimeFoundationConvergence规格_v1.0_2026-08-19.md`
- `G9-02BC_IndependentReview_共享运行时基础_v1.0_2026-08-19.md`
- `G9-02B_RuntimeDomainBreadth与PlayerKnownDirectory规格_v1.0_2026-08-19.md`
- `G9-02B_IndependentReview_PlayerKnownDirectory最终收口_v1.0_2026-08-19.md`
- `G9-02C_ModelFirstRouting与ContextOrchestration核心规格_v1.0_2026-08-19.md`
- `G9-02C_Agent资源分配增量裁定_v1.0_2026-08-19.md`
- `17_酒馆游戏_PlayerKnownEntityDirectory与长期资料表面信息架构裁定_v1.0_2026-08-18.md`
- `18_酒馆游戏_资料库资源层与世界创作集成裁定_v1.1_2026-08-19.md`
- `G9及后续阶段_Agent资源分配与GrokBuild代码协作裁定_v1.0_2026-08-19.md`
- `代码Agent_Worktree隔离与Main合并治理_v1.0_2026-08-19.md`
- `G8_StageUAT_最终收口与体验Backlog_v1.0_2026-08-18.md`
- `15_酒馆游戏_RuntimeContextOrchestration与模块化复杂度控制裁定_v1.2_2026-08-18.md`
- `16_酒馆游戏_RuntimeWorldMaterialization与当局游戏资产演化裁定_v1.1_2026-08-18.md`
- `酒馆游戏新版主体重建总路线 v2.1.md`

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

- World Pack / Character Card / Expansion 三类主资产分权；
- #18 资料库作为长期资料资源层补充，而不是第四类主资产；
- Source Definition != Game-local Instance != Runtime State；
- 未来 Source Library != Game-local Canonical Definition != Runtime State；
- Program-owned Formal Outcome / Atomic Commit / Save；
- Open Attempt；
- Canonical Owner / typed dependency / handoff / contribution；
- Package / Feature / Module activation semantics；
- Runtime Context Contract；
- UI Surface Owner / Contributor 语义。

当前 G9 首轮只完成三类主资产。资料库不因为产品语义已冻结就提前进入 external schema。

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

#18 未来资料库同样必须继承 Source 不静默反写现有 Game-local / Runtime truth 的原则，但具体资料库绑定协议等待后置增量阶段。

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

## 5. G9-02B｜PASS / CLOSED

Final code：`0ee847e1173ae8d17e643d5b838d238cf889031e`

Final Review：`G9-02B_IndependentReview_PlayerKnownDirectory最终收口_v1.0_2026-08-19.md`。

正式冻结：

```text
Canonical Character Truth
!= Player-known Character Directory
!= Current Scene Visible Character Set
```

已证明：

- `visibleCharacters` 与长期 `knownCharacters` 分离；
- People Surface 使用长期 player-known / last-known safe projection；
- stable `characterRef`，不创建第二 Character Truth；
- 未认识 public NPC 不泄露，hidden Character fail-closed；
- `currentPresence` 只表示当前在场，不决定 membership；
- off-scene Character / Relationship 不读取 live state；
- typed evidence 不自动复制未授权 canonical dossier；
- stable evidence identity、duplicate no-op 与 crash/recovery exactly-once；
- dossier `knownDisplayName / knownDescription` 真实变化同步 Game-local `definitionRevision`；
- `lastSeenTurn != lastInteractionTurn`；
- existing-game compatibility seed 使用升级时真实 turn/revision，不伪造 turn 0 历史；
- Save / Restore / Branch 保留认识历史、last-known state 与 dossier revision。

继续冻结：

```text
Player-known
!= Runtime Relevant
!= Model Visible
```

未来资料库沿用同一信息隔离精神：

```text
资料库包含某事实
!= 玩家知道该事实
!= 模型应该看到该事实
```

---

## 6. G9-02C breadth｜ACTIVE / NEXT

02C Core Authority：`G9-02C_ModelFirstRouting与ContextOrchestration核心规格_v1.0_2026-08-19.md`。

02C 重点：

```text
玩家输入
+ 已启用且预筛后的最小路由目录
+ 最小当前上下文
→ 模型优先判断当前相关领域
→ Program 结构校验 + 必要状态补充
→ 只对选中所有者按需投影
→ 有界且保留所有者身份的上下文组合
→ 领域处理 / 正式结果
→ 结果成立后才激活下游继续处理
```

必须证明：

- Model-first routing，不用关键词重复 NLP；
- 玩家语义 → 模块选择的授权/evidence seam；
- state-mandatory augmentation；
- disabled fail-closed；
- deterministic background zero-model-call；
- no transitive prompt expansion；
- Bounded != Starved；
- Open Attempt；
- G8 Materialization Need 不回滚；
- large registry / long session 不导致 ordinary Turn context 线性增长；
- People Directory scale stress；
- Large Relation Graph relevant-subgraph projection；
- outcome-gated downstream continuation。

### #18 v1.1 对 02C 的 Decision Propagation

```text
资料库长期架构
= APPROVED

资料库实现
= DEFERRED

02C Core Design
= NO REOPEN

02C Implementation Scope
= NO Library import/index/retrieval/Creator implementation
```

真实资料库 Provider、索引、检索、Creator / 游戏创建绑定不属于当前 02C 验收范围。

---

## 7. Sol / Grok Build 资源策略

正式治理：

- `G9及后续阶段_Agent资源分配与GrokBuild代码协作裁定_v1.0_2026-08-19.md`
- `G9-02C_Agent资源分配增量裁定_v1.0_2026-08-19.md`

当前安排：

```text
G9-02BC                    Sol         PASS / CLOSED
G9-02B breadth             Grok Build  PASS / CLOSED
GPT                        exact-SHA Independent Review  PASS
G9-02C high-risk core      Sol         RESERVED / NEXT
其余 02C breadth           Grok Build
```

最后约一次 Sol 深任务优先用于 G9-02C 的模型路由 / 上下文编排核心。其余已经有冻结答案的 breadth 默认交给 Grok Build。

若额度或套餐变化使剩余 Sol 不可用，02C 仍可由 Grok Build 按 frozen rails 执行，不形成项目 blocker。

同一 repo 默认 serialized writer；Executor != Independent Reviewer；正式代码 Agent 使用隔离 worktree，不直接写 main。

---

## 8. G9-03 Freeze Gate｜三类主资产首轮协议

只有 02C breadth + G9-02 Integrated Closure 全部 PASS，才能冻结三类主资产首轮机器协议。

首轮范围：

```text
World Pack
Character Card
Expansion Pack
```

至少覆盖：

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

### #18 v1.1 时序修正

资料库明确**不进入**本轮 G9-03 Freeze Gate。

当前禁止为资料库冻结：

- source identity / version / hash；
- World Pack → Library 机器绑定；
- 资料检索 / 投影协议；
- Creator 资料库字段；
- 资料库 Bundle / import / export；
- 资料查询 DSL。

这些全部等待“三类主资产纵向闭环门槛”PASS 后的资料库增量阶段。

继续禁止：

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
- Bounded != Starved；
- Whole Relation Graph != Model Prompt；
- 三类主资产继续是 World Pack / Character Card / Expansion；
- 资料库是长期资料资源层，不是第四类主资产；
- 资料库实现属于后置增量；
- 三类主资产纵向闭环不得被资料库阻塞。

---

## 10. Owner UAT / Product Backlog 路由

```text
Item durable known-description evolution
→ G9-02A PASS / CLOSED

People persistent known-character directory
→ G9-02B PASS / CLOSED
→ G9-02C bounded-context / scale proof

三类主资产机器协议
→ G9-03

三类主资产 Adapter / Compiler / Game-local Binding
→ G9-04

三类主资产 Creator 基础创作链
→ G9-05

“我的资产库 → 创建游戏 → 完整游玩”三类主资产纵向闭环
→ G9 主资产产品闭环门槛
→ 必须早于资料库增量开发

Reference Library Resource Layer
→ #18 v1.1 ARCHITECTURE APPROVED
→ IMPLEMENTATION DEFERRED
→ only after Primary Asset End-to-End Closure Gate PASS
→ future incremental stage number TBD

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

## 11. #18 v1.1｜资料库长期产品语义与开发门槛

长期产品语义仍冻结：

```text
三类主资产
= 世界包 + 角色卡 + 拓展包

资料库
= 可复用 / 可绑定 / 可检索的资料资源层
```

但当前实现状态：

```text
ARCHITECTURE APPROVED
IMPLEMENTATION DEFERRED
```

### 三类主资产纵向闭环门槛

资料库正式开发前必须全部 PASS：

1. 世界包 / 角色卡 / 拓展包相关 `tavern-asset` 语义与治理规则完成；
2. 三类主资产首轮机器协议完成；
3. 三类主资产适配 / 编译 / 当局游戏绑定完成；
4. Creator 三类主资产基础创建、编辑、AI 辅助、校验、保存、导入导出可实际使用；
5. “我的资产库”可查看、导入、管理三类主资产；
6. 可从“我的资产库”选择世界包、角色卡、拓展包创建新游戏；
7. 真实资产组合可以正常初始化并完整游玩；
8. Save / Continue / Restore / Branch / Recovery 不因资产组合而回滚；
9. 当前主资产链不存在 P0/P1 blocker。

只有上述门槛 PASS，资料库才允许进入正式实现。

### 届时才开发

- 世界包绑定 0..N 资料库；
- 资料库版本 / 更新 / 迁移；
- 资料检索与有界上下文；
- Creator“资料与参考”；
- “我的资产库”资料库管理；
- 资料库导入导出；
- 汉末三国历史与时代资料库；
- 穿越与系统历史辅助真实消费。

正式阶段编号到时再定，不现在预造。

---

## 12. 当前 Next

> **G9-02C Context Orchestration Core implementation。**

Implementation Base：`0ee847e1173ae8d17e643d5b838d238cf889031e`。

当前 02C Core Spec 已冻结；由 Sol 在隔离 worktree 中实现，完成后执行 exact-SHA Independent Review；PASS 后再开放其余 Grok Build breadth。

#18 v1.1 不改变当前 02C Task DAG，也不进入 G9-03/04/05 首轮范围。资料库要等三类主资产“我的资产库 → 创建游戏 → 完整游玩”纵向闭环 PASS 后再启动。

G9-03 当前 `NOT AUTHORIZED`。
