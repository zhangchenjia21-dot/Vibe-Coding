# G9-02C｜Model-first Routing 与 Context Orchestration 核心规格 v1.0

状态：`CURRENT SPEC / CORE DESIGN FROZEN / IMPLEMENTATION NEXT`
日期：2026-08-19
类型：G9-02C high-coupling core slice
默认核心执行者：Sol
后续 breadth：Grok Build
实现基线：`zhangchenjia21-dot/sillytavern@0ee847e1173ae8d17e643d5b838d238cf889031e`

---

## 0. Freshness / Product Definition Gate

本规格建立于以下 current facts：

```text
sillytavern/main
= 0ee847e1173ae8d17e643d5b838d238cf889031e

G9-02A
= PASS / CLOSED

G9-02BC Shared Foundation
= PASS / CLOSED

G9-02B Player-known Directory
= PASS / CLOSED

G9-02C
= ACTIVE / NEXT

G9-03
= NOT AUTHORIZED
```

当前 `agent/*` 临时分支为 0，G9-02B worktree 已清理。

Skill freshness：

- `Skill/main@ac48934beae20a938ce126014cfee6a20642c1b2`
- `agent-task-packet/SKILL.md` v1.0 current；
- `lifecycle-dev-process/SKILL.md` v2.1 current；
- Large Relation Graph / Deterministic Subgraph Projection gate 已加入 current Tavern Asset Skill。

### Product Definition Gate

02C 不改变酒馆游戏的 Product Promise、核心用户路径或 Source / Game-local / Runtime 三层产品语义；它把 #15 已冻结的上下文产品承诺落成 Runtime internal authority。

因此：

```text
Product Definition Gate
= PASS / unchanged
```

本轮不创建新的产品方向，不冻结 G9-03 external schema。

---

## 1. Outcome

G9-02C Core 的唯一主要 Outcome：

> **冻结并 production-proof“模型如何只看当前需要的路由目录、Program 如何验证并补充模块选择、以及选中后如何获得最小充分上下文”的 Runtime authority boundary。**

正式链：

```text
已启用 Package / Feature / Module
↓
Program 构建有界 Routing Working Set
↓
Model-first immediate routing
↓
Program structural validation
+ state-mandatory augmentation
+ authoritative continuation activation
↓
带 provenance 的最终 Module Selection
↓
当前行动的 authoritative context anchors
↓
selected-only JIT owner projection
↓
owner-preserving bounded join
↓
typed domain candidate
↓
Program validates candidate belongs to this authorized turn
↓
existing Formal Turn / Domain Change authority
```

核心目标不是“Prompt 越短越好”，而是：

```text
Total Enabled Modules ↑↑↑
Total Player-known / Relation Graph ↑↑↑
Long Session ↑↑↑

ordinary unrelated Turn model working set ≈ bounded
```

同时保持：

```text
Bounded != Starved
```

---

## 2. Authority / Source Manifest

优先级：

1. 用户当前明确指令；
2. `15_酒馆游戏_RuntimeContextOrchestration与模块化复杂度控制裁定_v1.2_2026-08-18.md`；
3. `G9-02BC_SharedRuntimeFoundationConvergence规格_v1.0_2026-08-19.md`；
4. `G9-02BC_IndependentReview_共享运行时基础_v1.0_2026-08-19.md`；
5. `17_酒馆游戏_PlayerKnownEntityDirectory与长期资料表面信息架构裁定_v1.0_2026-08-18.md`；
6. `G9-02B_IndependentReview_PlayerKnownDirectory最终收口_v1.0_2026-08-19.md`；
7. `G9及后续阶段_Agent资源分配与GrokBuild代码协作裁定_v1.0_2026-08-19.md`；
8. `代码Agent_Worktree隔离与Main合并治理_v1.0_2026-08-19.md`；
9. `sillytavern/main@0ee847e1173ae8d17e643d5b838d238cf889031e` 当前代码 / tests / `AGENTS.md`；
10. `Skill/main@ac48934beae20a938ce126014cfee6a20642c1b2` current execution methods。

Discussion-only Opening Scenario 不属于 02C authority。

---

## 3. Decision Propagation

### Stage Gate

```text
G9-02B PASS / CLOSED
→ G9-02C ACTIVE / NEXT
```

### Prerequisite

02C 复用：

- 02BC Program-owned Domain Host；
- Package / Feature / Module activation；
- owner-typed Canonical / Runtime State；
- Event / Handoff；
- selected-only JIT Projection；
- bounded owner-preserving join；
- 02B Player-known Directory。

### Task DAG

```text
G9-02C Core｜Sol
↓ exact-SHA Independent Review
G9-02C Breadth｜Grok Build
↓ exact-SHA Independent Review
G9-02 Integrated Closure
↓
G9-03
```

### Protocol Timing

02C 只冻结 internal Runtime contracts。External asset-spec / JSON Schema / manifest / compiler wire 仍属于 G9-03+。

### Ownership

Context Orchestrator 是 Program / Runtime authority；Routing Decision 与 Context Projection 是 read/selection artifacts，不是第二 Runtime truth。

---

# 4. Core Invariants

## INV-C01｜Router = Context Selection，不是行为白名单

Router 回答：

> 当前这次输入，需要哪些已启用的领域语义参与？

不得回答：

> 玩家是否有资格尝试这件事？

因此：

```text
Router miss
!= Attempt illegal
```

既有 Player Agency / Open Attempt / Runtime Capability authority 不得被 Router 替换。

---

## INV-C02｜Model-first，不允许 Program 用关键词复制 NLP

玩家自由语言的 immediate domain relevance 由模型判断。

Program 允许：

- enabled / disabled structural filtering；
- Package / Feature / Module hierarchy filtering；
- stable ID validation；
- state-mandatory augmentation；
- authoritative ref / handoff / active-state filtering；
- deterministic exact identity / graph traversal / scope filtering；
- bounds / duplicate / owner validation。

Program 禁止使用自然语言关键词表、正则意图分类或 display-name similarity 重新实现一套 domain NLP router。

---

## INV-C03｜Routing Working Set 必须与 Enabled Module 总量解耦

当前 02BC 的：

```text
buildRoutingDirectory(state)
```

会返回全部 enabled module entries。02C Core 必须新增有界 working-set authority。

正式采用**分层、可细化路由**：

```text
Package / Feature / Module routing hierarchy
```

当 enabled leaves 较少时，可直接给模型 module profiles。

当 enabled leaves 超过有界阈值时：

```text
先给 bounded Package / Feature group profiles
→ Model 选 group
→ Program structural expand
→ 必要时进行 bounded refinement routing
→ 最终得到 module leaves
```

禁止：

- 一次把 100 / 1000 个 module profiles 全塞给模型；
- 用关键词 Program prefilter 代替 model refinement；
- 为了减少一次模型调用而重新让完整目录进入 Prompt。

当前不冻结 token budget；代码必须有清晰 count / serialized-size bounds。

---

## INV-C04｜Routing Profile 是最小语义索引，不是完整 Definition

Internal routing profile 至少表达语义等价项：

```text
stable routing ref
kind: package | feature | module
parent / ownership identity
label
one-line scope
small typical semantics
leaf moduleRef（仅 module）
```

不得包含：

- Domain Canonical Record payload；
- Runtime State；
- Full Definition Registry；
- arbitrary query / callback / script。

02C 只冻结 internal semantic shape，不冻结 G9-03 source wire。

---

## INV-C05｜Routing Decision 必须有 provenance

最终 selection 不能只是一组无来源 module refs。

至少区分：

```text
model_immediate
state_mandatory
authoritative_continuation
```

Program 必须能证明：

- 哪些 module 是模型根据本次输入选中的；
- 哪些是当前 authoritative state 强制加入；
- 哪些是正式 Outcome / Handoff 成立后加入。

未知来源、disabled module、重复 module fail closed。

Selection evidence 是 orchestration artifact，不自动成为长期 world truth。

---

## INV-C06｜State-mandatory Augmentation 是 deterministic Program logic

每个 Program-built module 可通过 bounded internal seam 声明：

> 基于本模块 authoritative active state + 当前 action/context anchors，本回合是否必须参与。

Program 可以扫描大量 enabled modules 做本地 deterministic checks，因为：

```text
Program computation
!= Model Context
```

但每个 module 只能读取它被授权的 owner state / bounded anchors；不得把扫描结果变成全状态 Prompt。

---

## INV-C07｜Domain Orchestration 必须继承“本回合已经授权”的行动证据

当前 `RuntimeDomainTurnOrchestrator.prepare(state, playerInput)` 只有 raw input，02C Core 必须补上一个**玩家安全、结构化的 Authorized Turn Evidence / Context Anchor envelope**。

语义上至少包含：

- player / actor ref；
- 当前 scene / place relevant refs；
- 已被正式 Semantic + Agency + Capability pipeline 接受的 action kind / intent class；
- 已授权 target / item / destination / relevant refs（存在时）；
- direct / confirmed 等必要授权 provenance；
- expected revision / turn context。

不得给 Domain Router / Candidate Provider：

- hidden state；
- raw private evidence；
- arbitrary Runtime query access；
- 能绕过 core authorization 的“万能授权”标志。

正式规则：

```text
Domain selection
!= new player authorization
```

02C Core 不允许 Domain Candidate 因为“模型觉得相关”就独立制造一个未被玩家授权的额外行动。

---

## INV-C08｜Router miss / Domain fail 不得污染已合法 Core Attempt

Domain routing 是附加语义处理路径。

若 core attempt 本身合法，而 domain router：

- 选空；
- 不确定；
- selected module 最终无 candidate；

不得因此把合法 core attempt 改判为非法。

若 Router 明确报告歧义且该歧义会改变正式世界结果，允许进入既有 clarification gate。

---

## INV-C09｜Context Anchors 必须来自 authoritative / validated refs

JIT Projection 的 anchor 至少来自：

- playerRef / current scene；
- 已授权 core action 的 target / item / destination refs；
- state-mandatory module 的 bounded owner refs；
- authoritative Outcome / Handoff refs。

不得因为一个对象存在于：

- Game-local asset library；
- Player-known directory；
- Relation Graph；

就自动成为 Context anchor。

---

## INV-C10｜Player-known Directory 大小不得变成 Prompt 大小

正式继续保持：

```text
Player-known
!= Runtime Relevant
!= Model Visible
```

至少证明：

```text
Known Character Directory = 1,000
unrelated Turn
→ directory dossiers in ordinary semantic/domain prompt ≈ 0
```

当 authoritative target ref 指向一个已知人物时，Player-known module 可以投影该人物的 bounded last-known dossier；不得投影全部已知人物。

---

## INV-C11｜Large Relation Graph 使用 deterministic relevant subgraph

吸收 current Skill 的稳定 gate：

```text
Whole Relation Graph
!= Model Prompt
```

长期关系图中：

- semantically primary relation / source fact 可持久化；
- 可确定重算的 path / pairwise / aggregate 不得为方便 Prompt 建第二 canonical truth；
- Program 可按 authoritative anchor refs deterministic 选择 current-relevant path / subgraph；
- Model 只看到 owner-safe / player-safe bounded projection。

至少保留一个 scale fixture 证明：图规模扩大后，local query context 只随 current-relevant subgraph 变化。

---

## INV-C12｜Bounded != Starved

禁止通过“只给 count / 只给 module ID”伪装上下文有界。

被选 module 的 projection 必须能回答当前模型职责所需的：

- identity；
- authoritative target refs；
- player-safe current / last-known state；
- 当前约束；
- selected Definition / relevant relation path（需要时）；
- upstream outcome / handoff（需要时）。

如果模型因为缺关键事实只能泛化、猜测或产生 phantom affordance，属于 FAIL。

---

## INV-C13｜Outcome-Gated Continuation 只由正式 Trigger 激活

```text
Potential downstream
!= Current Relevant downstream
```

只有：

- validated upstream formal outcome；
- validated domain event；
- validated handoff；

满足 continuation condition 后，recipient module 才可加入 `authoritative_continuation` selection。

miss / invalid-target / disabled recipient 分支：

```text
downstream routing/profile/projection load = 0
```

02C Core 冻结**activation + context boundary**；不因这个 Gate 自动把 `RuntimeDomainFormalChangePlan` 扩成任意多 owner mutation list。

若真实 downstream vertical 必须在同一 Formal Turn 原子提交多个 Domain mutation，必须单独返回 architecture question；不得在 breadth 中偷偷改变 Formal Turn / Save / Recovery cardinality。

---

## INV-C14｜Background deterministic progression 保持 zero-model-call

Program 可以确定推进的 timer / threshold bookkeeping / routine state：

```text
world tick
→ Program authoritative update
→ no router call
→ no candidate model call
```

只有需要开放语义或正式 handoff continuation 时才触发相应模型 Context。

---

## INV-C15｜不同模型职责继续使用不同 Context

至少保持：

```text
Core Semantic Context
!= Domain Routing Context
!= Selected Domain Candidate Context
!= Narrative Context
```

本轮不强制把 Domain Router 合并进既有 G1 Semantic Provider，也不为了省一次调用修改已冻结 G1 语义协议。

默认优先：

> **独立 Model-first Router，输入有界 working set；只有选中 module 后才调用 domain candidate provider。**

这让普通模型职责边界更清晰，并把 G1 回归半径控制在最小范围。

---

# 5. Core Internal Contract Shape

精确 TypeScript 名称可由 Sol 在实现调查后决定，但语义上必须形成以下 seams。

## 5.1 Routing Catalog / Working Set

需要一个 Program-validated internal catalog，能表达 package / feature / module routing nodes，并生成 bounded working set + refinement cursor / parent identity。

不得依赖 external asset parser。

## 5.2 Routing Decision

Selector 不再只返回 `string[]`。

Decision 至少表达：

- selected routing refs；
- selected module refs（leaf 后）；
- needsClarification / uncertainty；
- refinement request（如选中 group）；
- bounded count。

Program 对每一层 selection 都重新校验 enabled / hierarchy / bounds。

## 5.3 Selection Provenance

最终 Module Selection 至少保留三类来源：

- model immediate；
- state mandatory；
- authoritative continuation。

## 5.4 Authorized Turn / Context Anchors

`RuntimeDomainTurnOrchestrator` 不再只接受 raw `playerInput`；必须接受 Formal Turn 已验证的 bounded action evidence / refs。

## 5.5 Module State-mandatory Hook

Program-built module 可提供 deterministic mandatory-activation decision；未知 / disabled / cross-owner query fail closed。

## 5.6 JIT Projection

继续复用 02BC `projectSelectedContext` owner-preserving boundary，但 Projection Request 必须能得到当前 validated anchors；不得退回 eager all-state projection。

## 5.7 Continuation Activation Hook

从 validated Outcome / Event / Handoff 构造 continuation activation evidence，并只对真实 recipient 做 routing/projection。

本 Core 不自动修改 Formal Turn mutation cardinality。

---

# 6. Sol Core Slice｜允许范围

Sol 只负责高耦合核心：

1. internal routing profile / hierarchy / working-set contracts；
2. adaptive bounded routing/refinement Host rails；
3. typed routing decision + clarification / uncertainty；
4. final selection provenance；
5. state-mandatory deterministic hook；
6. Authorized Turn / Context Anchor envelope；
7. Orchestrator wiring 到 existing Formal Turn，确保 Domain path 不能绕过既有 player authorization；
8. one reference vertical / fixture 证明 Model-first selection → JIT projection → typed candidate；
9. one outcome-gated continuation boundary proof；
10. regression / scale tests 证明核心 contract。

Sol 不负责把所有真实 Domain、所有 asset family 或所有 stress matrix 做完。

---

# 7. Grok Breadth Slice｜Core PASS 后

Core exact-SHA Review PASS 后，Grok Build 才可沿 frozen rails 扩：

### 02C-B1｜Routing Breadth

- concrete package / feature / module routing profile fixtures；
- large enabled registry hierarchy stress；
- refinement branch breadth；
- unknown / disabled / over-select / clarification cases。

### 02C-B2｜Projection / Scale Breadth

- Player-known 1,000 entry stress；
- targeted known-character bounded dossier projection；
- large relation graph deterministic subgraph fixture；
- large Definition Registry selected-only projection；
- cross-owner bounded join；
- anti-starvation cases。

### 02C-B3｜Continuation / Background Breadth

- outcome-gated continuation branch matrix；
- disabled recipient / miss branch no-load；
- deterministic background zero-router / zero-model-call；
- threshold/handoff activation tests。

### 02C-B4｜Integrated Regression

- G5/G6/G7/G8/G9-02A/02BC/02B；
- long session；
- recovery / replay；
- context-size / model-call-count metrics；
- real Provider routing smoke after offline gates。

---

# 8. Core Acceptance Gates

## AC-C01｜Small Registry

少量 enabled modules：

```text
single bounded routing pass
→ exact enabled module selection
→ selected-only JIT projection
```

## AC-C02｜Large Registry

例如 100+ enabled module leaves：

```text
first model-visible routing working set <= bounded constant
```

模型只看到 group/profile working set；选中 group 后才 bounded refinement。

`full enabled module directory serialized into one model request = 0`。

## AC-C03｜Unknown / Disabled

Model 输出 unknown / disabled / wrong-parent routing ref：fail closed，不产生 projection / candidate / change。

## AC-C04｜State Mandatory

模型不选某 module，但 authoritative active state 明确要求参与：Program deterministic 加入，provenance = `state_mandatory`。

Program 不使用玩家文本关键词完成该加入。

## AC-C05｜Player Agency

Domain router / candidate 不能把一个 core 已判 unauthorized 的行动变成 authorized world change。

Router miss 也不能把一个合法 core action 改判 illegal。

## AC-C06｜Context Anchors

selected module projection 只能消费 validated anchors / owner state；无关 Game-local / known entity 不进入 projection。

## AC-C07｜People Scale

1,000 known characters + unrelated action：

```text
ordinary domain context does not contain 1,000 dossiers
```

## AC-C08｜Bounded Sufficiency

Targeted known character / relation case：模型获得足以完成职责的 bounded target dossier / relation slice，而不是只获得 count。

## AC-C09｜Continuation

Potential downstream 未触发：recipient profile/projection 0。

正式 trigger/handoff 成立：仅对应 recipient 进入 `authoritative_continuation` selection。

## AC-C10｜Background

纯 deterministic background progression：router call = 0；domain candidate model call = 0。

## AC-C11｜Recovery

semantic-ready 之后 recovery 复用已持久化的 domain orchestration result / formal plan；不重复正式 Domain change / event / handoff。

不得为 02C 随意新增 recovery authority。

## AC-C12｜No External Freeze

无 JSON Schema / external manifest / asset parser / Creator wire。

---

# 9. Non-scope

G9-02C Core 禁止扩张为：

- G9-03 external asset-spec；
- World Pack / Expansion external compiler；
- Creator；
- Opening Scenario Runtime；
- Objective Engine；
- People UI search/filter/taxonomy；
- 全部真实 Expansion module；
- 任意脚本 / eval / generic query runtime；
- 通用向量数据库 / speculative RAG platform；
- 把所有模型职责合并成一个不断增长的大 Prompt；
- 未经单独裁定改变 `RuntimeDomainFormalChangePlan` 为任意多 owner mutation；
- 为了“智能”让模型执行 deterministic graph traversal / common ancestor / exact scope filtering。

---

# 10. Validation Strategy

Core focused test 至少覆盖：

1. bounded routing working set；
2. group refinement；
3. unknown / disabled selection fail closed；
4. selection provenance；
5. state-mandatory augmentation；
6. Authorized Turn anchors；
7. router miss does not block legal core action；
8. unauthorized core action cannot be revived by Domain path；
9. selected-only JIT projection；
10. bounded-sufficient target projection；
11. outcome-gated recipient no-load / activation；
12. background zero-router/model；
13. recovery / replay non-duplication；
14. 02BC/02B regression。

然后至少：

```powershell
npm run g9:02c:core:test
npm run g9:02bc:test
npm run g9:02b:test
npm run g9:02a:test
npm run g5:test
npm run g6:test
npm run g7:test
npm run g8:test
npm test
npm run typecheck
npm run lint
npm run product:build
npm run launcher:smoke
npm run g2:disclosure
git diff --check
```

真实 Provider routing smoke：Core offline PASS 后允许做 bounded smoke；Stage closure 前必须至少有真实 Model-first routing evidence，但不得在离线失败时浪费调用。

---

# 11. Stop / Architecture Question

Sol / Grok 必须停止并返回 GPT，而不是自行扩张，如果发现：

1. 正确 outcome-gated continuation 必须改变 Formal Turn / Save / Recovery 的多 owner mutation cardinality；
2. 现有 G1 Semantic / Capability authority 无法让 Domain path在不绕过 Player Agency 的情况下工作；
3. 必须把整个 Player-known Directory / Relation Graph 给模型才能完成 reference vertical；
4. hierarchical routing 需要 external asset schema 才能成立；
5. 需要 arbitrary query / vector DB / plugin code 才能满足本轮；
6. current main / spec / AGENTS 在执行期间发生同 Owner 冲突变化。

---

# 12. Closure Semantics

Sol Core 完成后只能宣布：

```text
G9-02C CORE READY FOR INDEPENDENT REVIEW
```

Core Review PASS 后：

```text
G9-02C Core PASS
G9-02C Breadth ACTIVE / NEXT
```

只有 Breadth + Integrated Regression + required real routing evidence 全部 PASS，GPT 才能宣布：

```text
G9-02C PASS / CLOSED
```

G9-03 在此之前继续 `NOT AUTHORIZED`。
