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

#18 v1.2 最新冻结：**世界包 / 角色卡 / 拓展包继续是三类主资产；资料库是资料资源层。资料库协议与三类主资产在 G9-03 同批完成，但 Creator、我的资产库、创建游戏绑定和 Runtime 资料检索等资料库产品功能后置。**

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
- `18_酒馆游戏_资料库资源层与世界创作集成裁定_v1.2_2026-08-19.md`
- `G9及后续阶段_Agent资源分配与GrokBuild代码协作裁定_v1.0_2026-08-19.md`
- `代码Agent_Worktree隔离与Main合并治理_v1.0_2026-08-19.md`
- `G8_StageUAT_最终收口与体验Backlog_v1.0_2026-08-18.md`
- `15_酒馆游戏_RuntimeContextOrchestration与模块化复杂度控制裁定_v1.2_2026-08-18.md`
- `16_酒馆游戏_RuntimeWorldMaterialization与当局游戏资产演化裁定_v1.1_2026-08-18.md`
- `酒馆游戏新版主体重建总路线 v2.2.md`

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
- #18 资料库作为资料资源层补充，而不是第四类主资产；
- Source Definition != Game-local Instance != Runtime State；
- Source Library != Game-local Canonical Definition != Runtime State；
- Program-owned Formal Outcome / Atomic Commit / Save；
- Open Attempt；
- Canonical Owner / typed dependency / handoff / contribution；
- Package / Feature / Module activation semantics；
- Runtime Context Contract；
- UI Surface Owner / Contributor 语义。

最新时序：G9-03 对三类主资产与资料库统一设计协议；资料库协议完成不等于资料库产品功能同时实现。

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

#18 资料库协议必须继承 Source 不静默反写现有 Game-local / Runtime truth 的原则；G9-03 冻结资料库源身份与版本语义，真实当局游戏资料库绑定生命周期仍后置。

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

资料库沿用同一信息隔离精神：

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

### #18 v1.2 对 02C 的 Decision Propagation

```text
资料库长期架构
= APPROVED

资料库协议
= G9-03

资料库产品实现
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

## 8. G9-03 Freeze Gate｜统一资产 / 资料资源协议

只有 02C breadth + G9-02 Integrated Closure 全部 PASS，才能冻结 external asset/resource contract。

### 三类主资产

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

### #18 v1.2｜资料库协议同批冻结

至少处理：

1. 资料库源身份、版本与完整性；
2. 条目稳定身份与来源；
3. 世界包 / 角色卡 / 拓展包对资料库的引用；
4. 世界包绑定 `0..N` 资料库的语义；
5. 默认 / 推荐 / 可选资料组合；
6. 独立 / 内嵌 / 引用的机器表达；
7. 资料可见性与信息边界；
8. 资料引用与当局游戏正式定义的边界；
9. Bundle / Manifest / import / export 关系；
10. 兼容 / 更新 / 迁移基础语义；
11. 有界检索所需最小声明能力；
12. 未来 Creator / 游戏实现所需最小协议挂点。

同时在该批次同步 `tavern-asset` 的资料资源层语义，避免语义治理与机器协议分叉。

G9-03 不冻结任意资料查询 DSL，也不授权资料库 Creator / Runtime 产品功能。

继续禁止：

- 把 Player-known Character Directory 做成 Source Character Card 字段；
- 把 current visibility 与 long-lived acquaintance/knowledge lifecycle 合并；
- 提前冻结 People / Objective / Information UI taxonomy；
- 把 Discussion-only Opening Scenario wire 提前塞进 external schema。

---

## 9. G9-04｜三类主资产完整实现 + 资料库协议最小验证

三类主资产按正常主线完成 parser / adapter / compiler / Game-local Binding。

资料库只做：

```text
Library sample
→ parse
→ validate
→ serialize / round-trip
→ cross-reference validation
```

资料库在 G9-04 不实现“我的资产库”资料页面、新游戏资料选择、当局游戏资料绑定生命周期、运行时索引 / 检索、模型资料 Provider 或 Creator 资料库编辑器。

---

## 10. Owner UAT / Product Backlog 路由

```text
Item durable known-description evolution
→ G9-02A PASS / CLOSED

People persistent known-character directory
→ G9-02B PASS / CLOSED
→ G9-02C bounded-context / scale proof

三类主资产 + 资料库统一机器协议
→ G9-03

三类主资产 Adapter / Compiler / Game-local Binding
→ G9-04

资料库协议最小 parse / validate / round-trip proof
→ G9-04
→ 不等于资料库产品功能上线

三类主资产 Creator 基础创作链
→ G9-05

“我的资产库 → 创建游戏 → 完整游玩”三类主资产纵向闭环
→ G9 主资产产品闭环门槛
→ 必须早于资料库产品功能开发

Reference Library Resource Layer
→ #18 v1.2 ARCHITECTURE APPROVED
→ PROTOCOL IN G9-03
→ PRODUCT IMPLEMENTATION DEFERRED
→ only after Primary Asset End-to-End Closure Gate PASS
→ future incremental product stage number TBD

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

## 11. #18 v1.2｜协议与产品实现分轨

正式冻结：

```text
三类主资产
= 世界包 + 角色卡 + 拓展包

资料库
= 可复用 / 可绑定 / 可检索的资料资源层
```

协议状态：

```text
世界包 / 角色卡 / 拓展包 / 资料库
→ G9-03 同批协议冻结
```

产品实现状态：

```text
三类主资产 Creator / 我的资产库 / 创建游戏 / 完整游玩
→ 先完成

资料库 Creator / 我的资产库 / 创建游戏绑定 / Runtime 检索
→ DEFERRED
```

### 资料库产品功能开发门槛

必须全部 PASS：

1. 统一资产 / 资料资源协议完成；
2. 资料库协议最小解析、校验、往返验证完成；
3. 三类主资产适配 / 编译 / 当局游戏绑定完成；
4. Creator 三类主资产基础创作链完成；
5. “我的资产库”可查看、导入、管理三类主资产；
6. 可从“我的资产库”选择三类资产创建新游戏；
7. 真实资产组合可以正常初始化并完整游玩；
8. Save / Continue / Restore / Branch / Recovery 不回滚；
9. 主资产产品链没有 P0/P1 blocker。

之后才开发：

- “我的资产库”资料库管理；
- 世界包资料绑定 / 推荐 / 可选流程；
- Creator“资料与参考”；
- 资料库创建、编辑、导入、AI 整理、提取；
- 当局游戏资料库绑定生命周期；
- Runtime 索引 / 检索与信息安全投影；
- 汉末三国历史与时代资料库；
- `穿越与系统` 历史辅助真实消费。

---

## 12. 当前 Next

> **G9-02C Context Orchestration Core implementation。**

Implementation Base：`0ee847e1173ae8d17e643d5b838d238cf889031e`。

当前 02C Core Spec 已冻结；由 Sol 在隔离 worktree 中实现，完成后执行 exact-SHA Independent Review；PASS 后再开放其余 Grok Build breadth。

#18 v1.2 不改变当前 02C Task DAG。G9-03 将统一冻结三类主资产与资料库协议；资料库产品功能继续后置。

G9-03 当前 `NOT AUTHORIZED`。
