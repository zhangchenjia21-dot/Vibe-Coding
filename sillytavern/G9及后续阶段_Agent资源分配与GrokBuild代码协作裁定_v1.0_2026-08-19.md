# G9 及后续阶段｜Agent 资源分配与 Grok Build 代码协作裁定 v1.0

状态：`CURRENT EXECUTION GOVERNANCE`
日期：2026-08-19
scope:
- agent-allocation
- codex-sol
- grok-build
- implementation-governance
- g9+

## 1. 背景

Project Owner 后续将更多使用 Grok Build，Codex / GPT-5.6 Sol 深度编码额度将成为稀缺资源。项目不应继续默认“所有 code task 都由 Sol 实现”。

当前仓库 `AGENTS.md` 与 `agent-task-packet` 已支持 Codex / Grok formal task；本裁定进一步冻结不同 Agent 的风险分工。

---

## 2. 总原则

```text
Best Reasoning Model
!= Default Code Writer
```

稀缺深度模型应优先用于：
- 高耦合 shared foundation；
- Canonical Owner / state model / authority boundary；
- persistence / recovery / idempotency；
- Context Orchestration / AI responsibility boundary；
- irreversible migration / schema-seam；
- architecture convergence / reference vertical。

Grok Build 默认成为**冻结合同后的主要 implementation executor**。

GPT 继续负责：
- Product / Architecture Lead；
- Freshness Preflight；
- Decision Propagation；
- Canonical Spec；
- Agent Task Packet；
- exact-SHA Independent Review；
- Stage Gate / UAT routing。

---

## 3. Sol Reserved Work

Sol 默认仅用于以下任一条件成立的任务：

1. 会重新定义 canonical owner；
2. 会改变 Source / Game-local / Runtime 三层模型；
3. 会改变 Formal Turn / atomic commit / Save / Restore / Recovery；
4. 会新增可扩展 Runtime Host / plugin-like execution seam；
5. 会定义 Context Orchestrator 的 routing / projection / handoff / model-visible boundary；
6. 会冻结高耦合 internal contract，而错误实现将导致 G9-03+ 大面积返工；
7. 会执行难以安全拆分的 architecture supersession。

Sol 不应消耗在：
- 常规 React/UI；
- CRUD；
- 已冻结 DTO 的接线；
- repetitive adapters；
- fixtures；
- mechanical migration；
- straightforward tests；
- copy/polish；
- Creator 表单实现。

---

## 4. Grok Build Default Scope

在 Canonical Spec + internal contract 已明确后，Grok Build 可正式拥有：

### Frontend / Product
- React / CSS / layout；
- Product Surface；
- Creator UI；
- settings / library management；
- classification / filter / search；
- responsive / visual work。

### Backend under Frozen Contract
- typed registry / adapter implementation；
- deterministic services；
- CRUD / persistence wiring；
- compiled DTO / projection wiring；
- domain module implementations against a frozen Host interface；
- asset compiler / parser；
- migration scripts with already-decided semantics；
- tests / fixtures / negative cases；
- build / lint / typecheck / smoke closure。

### Not Autonomous Architecture Owner

Grok Build 不得自行：
- 创造第二 canonical truth；
- 改 owner boundary；
- 新增 arbitrary JS/eval/plugin execution；
- 改 Save/Restore/Recovery 语义；
- 把 Dependency Graph 等同 Context Inclusion；
- 为方便实现重写 current architecture decision；
- 在没有 Task Spec 时冻结 external schema。

遇到这些情况必须 return BLOCKED / architecture question，而不是自行扩大 scope。

---

## 5. G9 剩余任务建议分工

当前 G9 DAG（02A 完成后）：

```text
G9-02B Runtime Domain / Expansion Binding Host
       + Player-known Entity Directory
↓
G9-02C Context Orchestration Foundation
↓
G9-02 Integrated Closure
↓
G9-03 asset-spec vNext
↓
G9-04 Adapter / Compiler
↓
G9-05 Creator rebuild
```

### 最后一条 Sol 优先用途

推荐不是让 Sol 完整吞下 G9-02B 或 G9-02C，而是执行一个 bounded：

`G9-02BC Shared Runtime Foundation Convergence`

Outcome：

```text
在 G9-02A current foundation 上
冻结并 production-proof：

Runtime Domain Module Host
↔ owner-typed canonical/state extension seam
↔ typed candidate/event/handoff/projection seam
↔ Package/Feature/Module activation boundary
↔ Context Projection Provider / Orchestrator boundary
```

至少包含一个真实或强 handwritten reference vertical，证明：
- disabled fail-closed；
- owner identity preserved；
- extension state/persistence 不形成第二 authority；
- Context consumer 只能拿 bounded projection；
- Host 不允许 arbitrary asset code。

该任务应只建立 shared primitives + reference vertical，不扩完所有 Domain，不做完整 UI/Creator/asset parser。

### Grok Build 后续

Sol foundation PASS 后：

#### G9-02B breadth completion
Grok Build 主导：
- package / feature / module registries breadth；
- concrete built-in domain modules against frozen Host；
- Player-known Entity Directory implementation；
- UI Host contribution binding；
- persistence/test breadth；
- focused + full regression。

#### G9-02C breadth completion
Grok Build 主导，但 GPT 提供严格 Spec：
- Model-first Router wiring；
- state-mandatory augmentation；
- JIT projections；
- bounded owner-preserving join；
- outcome-gated continuation；
- large registry projection；
- People Directory scale stress；
- background deterministic zero-model-call tests。

若 02C 实现过程中出现 routing authority / model responsibility 新裁定，则停止实现，由 GPT 先裁定；不要求 Grok 自行架构。

#### G9-02 Integrated Closure
Grok Build 可执行 regression / cleanup / legacy retirement；GPT independent review / stage gate。

#### G9-03 asset-spec vNext
GPT 主导 schema semantics / ownership freeze；Grok Build 可实现 TypeScript types、JSON Schema、validators、fixtures、docs generation 与 conformance tests。

Opening Scenario 当前仍是 Discussion Draft，玩家 Runtime 未验证，因此 **G9-03 默认不冻结 Prologue Scenario external wire**，除非首轮 Creator authoring pilot + 后续正式裁定在此之前完成。

#### G9-04 Adapter / Compiler
Grok Build 主导。属于冻结协议后的典型 deterministic implementation：Markdown/frontmatter/normalized source → machine contract → Game-local binding；GPT review compatibility / authority。

#### G9-05 Creator rebuild
Grok Build 主导 UI + form + preview + validation；GPT 负责产品流程和真实 Creator UAT。

Opening Scenario 第一轮只做最小 authoring prototype，先获得真实创作经验，再决定最终 Creator/Runtime。

---

## 6. Grok Build Formal Task Rule

Grok Build 与 Codex 一样必须通过：

```text
Freshness PASS
+
Decision Propagation PASS
+
Canonical Task Spec
+
Applicable AGENTS.md
+
Base HEAD
+
bounded Read First
+
Allowed / Prohibited
+
Acceptance Gates
+
Git / Return Protocol
```

不得因为 Grok 更偏 build/execution 就使用模糊指令：

> “把这个功能做了。”

GPT 必须先把架构自由度压缩到合理范围。

---

## 7. Writer Serialization

默认同一 repo / main 同一时刻只允许一个主要代码写入 Agent。

```text
Codex writing
→ Grok no overlapping write task

Grok writing
→ Codex no overlapping write task
```

若未来必须并行：
- 使用独立 branch / PR；
- 文件 ownership 不重叠；
- integration owner 明确；
- 合并前重新 Freshness / Decision Propagation。

不得让两个 Agent 同时直接推 main 的重叠模块。

---

## 8. Review Model

默认：

```text
Executor
!= Independent Reviewer
```

Grok Build 完成代码后：
- GPT 通过 GitHub exact-SHA 做 architecture / contract / diff review；
- 必要时再发 bounded correction task；
- Project Owner 只做产品体验/UAT，不负责技术诊断。

Sol 不应被浪费成常规 reviewer；只有发现深层 architecture conflict 时才考虑调用。

---

## 9. 当前资源决策

在 G9-02A 当前 Codex 任务完成后：

1. GPT 先做 exact-SHA Independent Review；
2. 若 02A PASS，保留约最后一次 Sol 深任务给 `G9-02BC Shared Runtime Foundation Convergence`；
3. Sol foundation 完成并 review 后，后续 G9 默认 Grok Build 主导 implementation；
4. 任一 Grok task 若触及尚未冻结的 architecture seam，停止并回 GPT 裁定；
5. 未来 Plus 使用模式下继续维持该分工，不因执行 Agent 变化降低 Gate。

---

## 10. 与 Project Owner 的沟通语言规则

Project Owner 不是开发者。GPT 与 Project Owner 的讨论、汇报、阶段说明、风险解释和产品建议必须遵守 **中文优先、技术英文最小化**。

### 10.1 面向 Project Owner 的默认表达

- 先用中文解释“这是什么、为什么重要、会带来什么影响”；
- 产品状态、阶段结论、风险、优先级、下一步默认使用中文；
- 不使用大段中英混杂的技术黑话作为主要说明方式；
- 同一概念已有自然中文表达时，优先使用中文；
- 架构图、流程图、状态图若面向 Project Owner，也应优先使用中文节点名称。

例如应优先写：

```text
运行时领域模块宿主
→ 模块启用状态
→ 按需上下文投影
→ 有界上下文组合
```

而不是把整段说明写成连续英文架构术语。

### 10.2 允许保留英文的情况

以下情况可以保留英文，但必须让 Project Owner 不依赖英文也能理解：

1. 真实代码字段名、接口名、类型名、类名；
2. Git commit、文件路径、命令；
3. 已冻结且需要精确引用的技术协议名；
4. 中文翻译会造成技术歧义的行业术语。

推荐格式：

> “模块所有者字段，底层字段名为 `ownerNamespace`。”

> “按需上下文投影，代码接口暂称 `ContextProjectionProvider`。”

而不是只写英文名后要求 Project Owner 自行理解。

### 10.3 正式 Agent 技术指令例外

发给 Codex、Grok Build、Reviewer 的正式技术任务包可以使用更多英文代码标识、字段名、接口名和测试术语，因为执行对象是代码 Agent。

但当 GPT 向 Project Owner **解释这份指令为什么这样设计、执行结果意味着什么、是否通过阶段 Gate** 时，仍必须回到中文优先表达。

### 10.4 报告规则

以后 GPT 给 Project Owner 的阶段报告默认采用：

```text
中文结论
↓
中文解释
↓
必要时补充精确英文代码字段 / 接口名
```

而不是：

```text
大量英文架构术语
↓
少量中文连接词
```

该规则属于长期 AI 协作治理，不只适用于 G9。

---

## 11. 最终原则

> **把稀缺的深度推理能力用于铺设正确轨道，把可扩展的构建 Agent 用于沿轨道实现。**

Grok Build 是正式代码执行 Agent，不只是 UI 助手；但它在本项目中的安全扩权条件是：**先冻结合同、任务边界清晰、写入串行、按精确提交审核**。
