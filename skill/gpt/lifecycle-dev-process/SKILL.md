---
name: lifecycle-dev-process
description: Plan and audit multi-stage AI-assisted software and product development from pre-implementation route alignment through architecture, staged implementation, real validation, UAT, release, and reusable closeout. Use for project inception, major redesign, roadmap creation, stage-gate planning, architecture, migration, platform/extension design, AI product validation, game/runtime design, repository governance, or release-process work. Emphasizes task-axis-first planning, two-pass reference audits, Owner-reviewed route freeze, vertical-before-platform sequencing, canonical ownership, real-consumer evidence, independent review, bounded context, recovery, decision propagation, and pre-push freshness.
---

# lifecycle-dev-process v2.3

> [!abstract]
> 跨项目 AI 驱动开发全生命周期 Skill。
>
> 核心目标不是把流程写得更复杂，而是**尽量在产生沉没成本之前发现方向错误**，并让每个阶段都用合适的证据证明“做对了、接通了、值得继续”。

总路径：

```text
开发前路线对齐
→ 架构 / Ownership
→ 分阶段实现
→ 自动化 + 真实集成验证
→ Independent Review
→ Owner / User UAT
→ Decision Propagation
→ 下一阶段 / Release
```

最重要的顺序原则：

> **Vertical before platform.**
>
> **先证明能跑，再证明值得用。**
>
> **先保证关键历史 / 数据不会丢，再让系统变复杂。**
>
> **先有正式产品入口，再放复杂内容选择。**
>
> **Consumer before Creator.**
>
> **先证明 Runtime effect，再抽象外部 Protocol / UI contract。**
>
> **第一消费者拉出最小能力，第二消费者帮助抽象。**
>
> **自动测试证明语义，真实集成证明现实，Owner UAT 证明产品。**

---

# 0. 开发前路线对齐 Gate｜Pre-Implementation Alignment

新项目、重大重构、产品方向重置、会改变主用户路径或大量下游依赖的工作，默认先走本 Gate。

**除低成本 spike / prototype 外，在本 Gate PASS 前不开始正式大规模实现。**

推荐流程：

```text
Primary Purpose / Core Value
↓
Draft Task Axis v0
↓
Reference Audit — Pass 1
↓
Reference Audit — Pass 2
↓
Risk / Omission / Ordering Review
↓
Owner Discussion + Improvement Proposals
↓
Revised Task Axis
↓
Owner Approval
↓
Route Freeze / Product Definition Gate
↓
Implementation
```

## 0.1 先回答产品为什么存在

在列功能、Schema、Protocol、State Model 前，至少明确：

1. **Primary Purpose / Job To Be Done**：用户最主要拿它做什么；
2. **Core Value / Core Experience**：为什么值得使用；
3. **Core Loop / Core User Journey**：价值如何反复产生；
4. **Non-negotiable Core**：哪些体验失败时，工程全绿也仍算产品失败；
5. **Simple Baseline / Current Alternative**：现实中最简单的替代方案；
6. **Hard Constraints**：真正不可越过的安全、平台、时间、成本或兼容边界。

Guardrail 是支撑能力，不得反客为主。任何明显限制 Core Value 的安全、可靠性、兼容或治理规则，都必须说明它保护什么真实风险，以及能否缩窄到真正危险的边界。

## 0.2 先列完整 Task Axis，再开始实现

Draft Task Axis 不是“列模块名”，而是先回答整个项目将怎样变成产品。

每个阶段至少写清：

```text
Stage Outcome
→ 用户/系统获得什么新能力
→ 为什么必须现在做
→ 前置依赖
→ 主要任务
→ 明确 Non-scope / Deferred
→ 最早 Reality Gate / UAT
→ Stage Exit evidence
```

同时标出：

- 高沉没成本决策；
- 一旦做错会被大量下游依赖的 contract / schema / ownership；
- 可能存在产品分歧、需要 Owner 提前裁定的点；
- 哪些阶段必须先有真实 consumer，才能允许抽象平台。

目的：**在还可以低成本改路线时，先把“做什么、先做什么、为什么”暴露出来。**

## 0.3 Reference Audit 必须至少两轮

两轮不是机械“读两份文档”，而是从不同角度主动寻找 Draft Task Axis 的遗漏。

### Pass 1｜针对性经验审计

优先检查：

- 当前/前代仓库；
- retrospective / postmortem；
- Owner UAT findings；
- 失败修复记录；
- 真实用户路径；
- 已验证的类似系统 / 简单基线。

目标：找出已经付过学费的边界、真实有价值的功能、历史失败模式。

### Pass 2｜开发路径与反事实审计

不要只读“最后架构”，而要重建实际经历过的开发步骤：

```text
最初怎么排
→ 哪些任务后来被插入
→ 哪些阶段返工最多
→ 哪些 UAT 太晚
→ 哪些能力只有 contract/binding 没有真实效果
→ 哪些 platform/creator 做早了
→ 哪些缺失入口或 lifecycle 导致后面重拆
```

然后逐项对照 Draft Task Axis：

- 是否漏了前置阶段；
- 顺序是否反了；
- 是否把多个高风险变量压进第一次试玩；
- 是否应该更早插入 First Playable / Reality Gate；
- 是否有旧能力值得继承，但当前不该做；
- 是否有当前方案比旧方案更简单、更可证伪。

没有前代项目时，也应做两种独立审计：**相似产品/技术证据** + **失败模式/依赖顺序审计**。

## 0.4 改良建议必须先交 Owner 审核

AI / Planner 不得在发现历史经验后直接静默改 Roadmap。

先向 Owner 提交：

```text
发现了什么
→ 为什么影响当前路线
→ 建议怎么改
→ 不改的真实风险
→ 改动会增加/减少什么范围
→ 建议采纳 / 延后 / 放弃
```

Owner 审核后再形成 Revised Task Axis。

这一步尤其适合提前冻结：

- 创建/导入主路径；
- 用户必须/可选的输入；
- 多种模式是否第一代都支持；
- 主入口与生命周期；
- Source / Instance / Runtime 边界；
- 是否允许外部扩展；
- 第一次试玩包含哪些能力；
- 哪些功能明确 Deferred。

## 0.5 Route Freeze / Product Definition Gate PASS

开始正式实现前，至少满足：

- Primary Purpose / Core Value 可以清楚说明；
- 已有一条完整 Task Axis；
- 已做两轮 Reference / Path Audit；
- Owner 已审核主要改良建议和分歧点；
- Must-have / Deferred / Non-scope 可区分；
- 核心用户路径和最早 First Playable 已明确；
- 高沉没成本架构点已识别；
- 关键 Domain Semantics 足以避免明显 ownership/identity 歧义；
- Stage Gate 与 Owner UAT 节点已放在风险边界，而不是只放项目末尾；
- 唯一 current Product / Roadmap / Status 事实源已指定。

Gate 前允许：UX prototype、technical spike、fixture、mock、feasibility test、exploratory model call。

但必须明确：

```text
EXPLORATION
NOT canonical architecture
NOT production commitment
```

---

# 1. 任务排序原则｜Order by Risk, Not by Module List

Task Axis 应按**风险消除顺序**排，不按组织结构或“模块看起来完整”排。

评估一个新能力放在哪个阶段，先问：

```text
A. 没有它，当前真实纵向是否无法继续？
B. 它依赖的事实 / consumer 是否已经存在？
C. 是否已经有真实 consumer，而不是未来想象？
D. 如果现在做错，会不会被大量下游依赖？
```

默认解释：

- A=是：优先解决；
- B=否：通常后置；
- C=否：先做更薄 seam / internal capability；
- D=是：先 Reality Check / 第二消费者，再冻结长期协议。

常见正确顺序：

```text
feasibility
→ first usable vertical
→ durability/recovery
→ formal product entry / lifecycle
→ real content or data consumption
→ richer domain semantics
→ richer UI
→ scale / long-session
→ creator / plugin / external protocol
→ alpha / release
```

这些是风险顺序，不是所有项目必须照抄的模块名。

特别保持：

> **Platform before product = warning.**
>
> **Creator before consumer = warning.**
>
> **External protocol before proven internal host = warning.**
>
> **Binding / registration before observable behavior = incomplete.**

---

# 2. Canonical Product / Architecture Boundary

## 2.1 Canonical Ownership

重要事实先分类：

1. Authoritative State；
2. Derived Projection / Materialized View；
3. Reference / Read Model；
4. Cache；
5. Historical / Legacy Evidence。

至少能回答：

```text
Fact / Object
→ Canonical Owner
→ Authorized Writers
→ Readers
→ Persistence / Lifecycle
```

Derived / Snapshot / Projection / Cache 默认不能反向成为第二 live truth。

UI、Prompt、Transcript、推荐、声明式定义都不因“用户看得到”自动获得事实所有权。

## 2.2 Source / Local Instance / Runtime

可复用 Source 进入长期实例时，优先检查三层：

```text
Reusable Source
↓ exact bind / snapshot
Local Canonical Instance
↓ current operation
Runtime State
```

必须定义：

- stable logical identity；
- exact source generation / provenance（若 Source 可更新）；
- Source 更新是否影响旧 Instance；
- 哪些字段 immutable / evolvable / runtime-owned；
- 动态新增对象如何获得 stable identity；
- Save / Restore / Branch 如何保持实例独立。

**Stable name / folder / display label != exact business identity。**

## 2.3 Shared Foundation 只在真实共享需求出现时建设

多个真实消费者依赖相同 state / semantics / host capability 时：

```text
shared need
→ canonical owner
→ minimal internal contract
→ first real vertical
→ second consumer / convergence
→ stable abstraction
```

只有一个消费者时，不要为了“以后可能复用”先造大型 framework。

## 2.4 Internal capability 先于 External contract

对 Plugin / Mod / Creator / Declarative UI / SDK：

```text
internal real capability
→ production consumer
→ second/third consumer
→ stable vocabulary
→ external schema / protocol
→ creator / authoring helper
```

外部协议一旦发布，错误设计的修正成本远高于内部 seam。

---

# 3. AI / Runtime / Context 边界

模型、Planner、Generator 可以负责开放语义、内容、候选方案、自然语言理解和创造；Program / Domain Owner 负责真正不可妥协的完整性边界：

- stable identity；
- permission / authority；
- atomic durable write；
- persistence / recovery；
- external irreversible side effect；
- secrets / OS / filesystem boundary；
- schema / reference integrity where structurally required。

不要把 Program 变成无限扩张的“内容审查委员会”，也不要让自由文本无边界地替代 authoritative state。

Context 必须长期区分：

```text
System Total State
!= Enabled Set
!= Relevant Set
!= Model-visible Working Set
```

同时：

> **Bounded context != starved context.**

模型必须得到完成当前职责所需的最小充分材料；不要用字数、格式或 Prompt 小修掩盖数据/Context 不足。

---

# 4. Stage / Task 设计

每个 Stage 必须有一个清楚 Outcome，不把“完成若干模块”当 Outcome。

Stage Exit 缺口按四类判断：

- **A Architecture Prerequisite**：会改变 owner / public contract / schema / state model / safety boundary；当前 REQUIRED。
- **B Stage Minimum**：本阶段承诺的最小闭环尚未成立；默认 REQUIRED。
- **C Product/UX Maturity**：底层成立，缺更完整管理或低摩擦体验；可显式 DEFER。
- **D Polish / Scale / Future Platform**：像素 polish、极端规模、speculative platform；早期默认不阻塞。

决定 Primary Purpose / Stage Outcome 是否成立的缺口，不能伪装成 C/D。

高风险 Task 在编码前优先做 **state / failure matrix**，尤其是：

- persistence / migration；
- create / import / publish；
- identity / version selection；
- multi-instance switching；
- Save / Restore；
- concurrent writer；
- source update isolation；
- external side effects。

Matrix 要回答：

> **每个失败点发生时，系统最终应该留下什么 authoritative truth？**

---

# 5. 单个任务执行循环｜S0 → S10

正式 Task 默认：

```text
S0  Freshness / Takeover
S1  Read current Authority / Scope
S2  Relevant history + seam audit
S3  Contract / state / failure matrix before code
S4  Minimal implementation
S5  Self validation
S6  Automated regression
S7  Real integration / platform / Provider / crash evidence where relevant
S8  Independent Review
S9  Owner / User UAT when product-facing
S10 Decision Propagation / Closeout
```

Routine Git、build、fixture、日志、DB proof、crash injection、export、自动化验证属于 Agent 工作。

Owner 负责：

- 产品方向；
- 真实体验；
- 不可由工程证据替代的主观价值判断；
- secrets / credentials；
- 真正不可消除的产品分歧。

不要把 routine engineering 转嫁给 Owner，也不要让 Owner 从长篇 AI 方案里自行找完所有隐含风险。

---

# 6. 证据阶梯｜Semantics → Reality → Product

任何“已支持”能力都标注成熟度：

```text
Contract only
→ Fixture / automated semantics proven
→ Real production/integration vertical proven
→ Owner / User UAT proven
```

三类证据职责不同：

### Automated / deterministic evidence

证明：

- state machine；
- identity；
- transaction；
- positive / negative behavior；
- failure / retry / replay；
- no-regression。

### Real integration evidence

证明真实路径没有被 mock 掩盖，例如：

- real Provider / model；
- real OS / filesystem；
- real exported app；
- real database driver；
- real imported/created instance；
- real network/API behavior。

### Owner / User UAT

证明：

- 核心价值自然发生；
- 用户知道下一步做什么；
- UI/交互可理解；
- 结果像产品而不是工程 demo；
- 与 simple baseline 相比没有明显退化；
- 用户愿意继续使用。

因此：

> **Engineering PASS != Product PASS.**

## 6.1 Independent Review 不只是重跑测试

Reviewer 应主动找：

- assertion 是否真正覆盖目标；
- negative assertion 是否 vacuous；
- marker / fixture 是否真的进入被证明的路径；
- mock 是否绕过 production seam；
- test helper 是否比 production contract 更宽松；
- UI selection 是否 exact 对齐 authoritative identity；
- “registered / bound / parsed” 是否被误报成真实 behavior。

Push success 也不等于 Independent Review PASS。

---

# 7. Reality Gate / UAT Cadence

真人体验不要只放项目末尾。

推荐在**风险结构变化后**插入短 Reality Gate：

```text
first core loop runnable
→ short Reality Check

first durable vertical
→ persistence/recovery reality

first real creation/import/content consumer
→ First Playable / Real Instance UAT

new mechanic/runtime effect
→ focused product UAT

stage exit
→ integrated UAT

long-term contract freeze
→ long-session / stress / second-consumer evidence
```

如果第一次 Reality Check 已出现：

- 用户不知道下一步；
- 核心结果不自然发生；
- 需要用户自己补系统缺失；
- simple baseline 明显更好；
- 产品需要大量解释才能工作；

优先重开 Product / Task Axis / Architecture，而不是继续堆外围功能。

---

# 8. 何时继续修，何时重新设计

采用 correction budget：

```text
correction-01
→ focused root fix

correction-02
→ audit neighbor boundary / crash window / sibling path

same seam still failing
→ stop patching
→ redesign seam / task order / contract
```

未发布阶段若 v0.x contract 明显错误：

> **优先修正确模型，不为不存在的用户提前建设 compatibility forest。**

Supersession 默认优于无真实义务的兼容层：

```text
retire old path
→ preserve history/evidence
→ migrate unique data if needed
→ rebind consumers
→ delete obsolete production authority
```

---

# 9. Decision Propagation / Freshness / Repository Hygiene

决策不能只留在聊天。

若新结论改变：

- Primary Purpose / Product Promise；
- Core User Journey；
- Scope / Non-scope；
- Stage / Task order；
- Canonical Owner / identity / state model；
- schema / protocol timing；
- safety / persistence boundary；
- Success / Acceptance；

则必须：

```text
update canonical Product / Architecture / Roadmap / Status
→ recompute prerequisites / current task
→ supersede stale task packet
→ update Agent instructions if needed
→ only then continue implementation
```

**Freshness != Propagation。** 读到最新 HEAD 但继续按旧路线执行仍然是错误。

正式写回前必须再次检查：

```text
Task Base HEAD
!= Current HEAD ?
```

- 无变化：正常写；
- 有无关变化：吸收后验证；
- 有影响当前事实源/架构/任务的变化：重新 audit + Decision Propagation。

Repository 治理保持：

> **Active current only.**

同一事实族只保留一个 current；历史依靠 Git history / archive。Root 用作 map，深度下沉子目录，不让 Agent 在 `final2/latest/new/v1.8/v1.9...` 中猜 current。

---

# 10. Stage Close Gate

Stage 关闭前至少回答：

```text
1. Stage Outcome 是否真的成立？
2. Primary Purpose / Core Value 是否仍被保护？
3. Engineering evidence 是否足够？
4. Real production/integration vertical 是否存在？
5. 真实 Instance / Consumer 是否工作？
6. 是否存在 contract-only / binding-only / fixture-only 假完成？
7. Canonical ownership / identity / recovery 是否清楚？
8. Context / performance 是否满足当前阶段，而非把问题推给未来？
9. Owner/User UAT 是否在需要时完成？
10. Deferred 是否记录 reason + revisit trigger？
11. Current docs / Task / Agent rules 是否已传播？
12. 下一阶段依赖是否真实成立？
```

对可增长系统，在冻结长期 contract 前再做：

```text
10 interactions later?
50 interactions later?
100 interactions later?
```

检查 identity、history、recovery、context、source update、discovery、performance 是否会线性失控。

但长期可运行不能掩盖短期没人愿意使用。

---

# 11. 高频反模式

出现以下信号应立即 audit：

1. **Platform before product**：协议/框架很多，核心 vertical 还没跑。
2. **Schema drives experience**：真实玩法/流程被迫迁就早期字段。
3. **Creator before consumer**：编辑器先于正式消费路径。
4. **Contract/Parser PASS = Product PASS**。
5. **Binding = Behavior**：注册成功但真实行为无变化。
6. **Late Owner UAT**：大量沉没成本后才首次体验。
7. **Missing product entry/lifecycle**：先做 selector/feature，后补正式入口。
8. **Mutable Source silently changes old instances**。
9. **Display label / latest version guessing identity**。
10. **Chooser visibility = selection**：打开列表就自动选值。
11. **Empty successful instance**：创建成功但没有可继续的真实价值。
12. **Mock-only proof for model/integration semantics**。
13. **UI / cache / transcript becomes second truth**。
14. **Restore state but not dependent context**。
15. **Context starvation disguised by prompt/format rules**。
16. **Speculative shared foundation with one consumer**。
17. **External protocol before internal host is proven**。
18. **Infinite correction chain**：同一 seam 不断加 patch。
19. **Fresh HEAD, stale decision**：代码最新但 Roadmap/Task 语义旧。
20. **Engineering green, simple baseline better**。

---

# 12. 最小可复用 Checklist

## 开发前

```text
Purpose / Core Value / Baseline
✓
Draft Task Axis with stage outcomes
✓
Reference Audit Pass 1
✓
Reference / Path Audit Pass 2
✓
Risks / disagreements / omissions surfaced
✓
Owner reviewed improvement proposals
✓
Revised Task Axis approved
✓
Current Product / Architecture / Roadmap propagated
```

## 每个阶段

```text
Prerequisite real
→ minimal vertical
→ deterministic evidence
→ real integration evidence
→ Independent Review
→ Owner UAT where product-facing
→ Decision Propagation
```

## 抽象前

```text
first real consumer exists?
second consumer or strong convergence evidence exists?
runtime effect proven?
internal host stable?
```

否则默认后置 platform / creator / public protocol。

---

# 13. Human-facing Document Version Policy

人工版本采用一位小版本：

```text
v1.0 → ... → v1.9 → v2.0 → v2.1
```

`vM.N` 中 `N ∈ [0,9]`；`vM.9` 后进入 `v(M+1).0`。版本号表示文档演进顺序，不承担 SemVer breaking-change 语义。

---

# 14. v2.3 Change Log

相对 v2.2：

- 把原先偏泛的 Product Discovery 前置步骤升级为正式 **Pre-Implementation Alignment Gate**；
- 新增“Draft Task Axis → 两轮 Reference/Path Audit → Owner 改良审核 → Revised Task Axis → Route Freeze”作为正式实现前默认流程；
- 明确开发前优先暴露实现范围、阶段 Outcome、依赖、Non-scope、分歧点与高沉没成本决策；
- 提升 `Vertical before platform`、`Consumer before Creator`、`Reality Gate before abstraction` 为跨项目顺序原则；
- 增加“第一消费者拉出最小能力，第二消费者帮助抽象”；
- 将自动化、真实集成、Independent Review、Owner UAT 的职责重新压缩成一条证据阶梯；
- 保留 Canonical Ownership、Source/Instance/Runtime、Shared Foundation、Bounded Context、Recovery、Supersession、Freshness、Decision Propagation、Repository Current 等关键规则，但合并重复章节；
- 将原 27 个平级章节收敛为 14 个高频执行章节，目标是**更短、更可查、更能直接用于项目启动与阶段审计**。
