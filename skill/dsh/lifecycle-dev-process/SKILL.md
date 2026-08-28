---
name: lifecycle-dev-process
description: 规划与审核多阶段 AI 辅助软件/产品开发的共享基础优先架构、权威状态所有权、迁移纪律、收敛审核、状态机测试、恢复与 UAT 边界。适用于项目生命周期、架构、阶段门、重构、迁移或发布流程工作。
whenToUse: 需要做项目生命周期规划、架构治理、共享基础规划、所有权与权威状态分类、迁移与退役、收敛审核、状态机/恢复序列测试、UAT 边界，或审查这些流程时。
---

# lifecycle-dev-process v1.6

> [!abstract]
> 跨项目 AI 开发生命周期 Skill。
>
> v1.6 保留此前已经验证的：
>
> - 单一事实源；
> - 先真实纵向链再横向扩展；
> - Planner / Generator / Evaluator 分离；
> - Mock / Real Integration / UAT 分层；
> - 阶段止损；
> - Snapshot ≠ live truth；
> - operation revision / business equality / branch identity 分离；
> - immutable request identity；
> - stage-owned recovery；
> - committed-result replay；
> - Domain Model ≠ Product IA；
> - Extension Owner / Contributor；
> - Host layout / safety authority；
> - UI Preference ≠ business state；
> - canonical Product Entry / Engineering Shell separation；
> - deterministic fault injection 优于要求用户撞内部竞态。
>
> v1.6 新增：**共享基础先于高耦合消费者，以及 Authority/Derived/Reference/Cache 分类治理。**

---

# 1. Shared Foundation Before High-coupling Consumers

当多个下游模块预计依赖同一：

- 状态；
- 语义；
-基础服务；
- 数据结构；
-解析逻辑；
-权限模型；

必须先判断是否需要共享 Primitive / Core / Platform capability。

默认流程：

```text
Domain / capability survey
↓
shared primitive identification
↓
canonical owner
↓
vertical proof
↓
downstream consumers
```

禁止：

> 每个消费者先各自做临时实现，最后再从重复代码 / 重复状态中抽基础层。

---

# 2. Provisional Ownership

探索期允许临时 Owner，但必须显式：

- provisional；
- scope；
- expected future consumers；
- extraction trigger；
- migration plan。

临时 Owner：

> 不应被误认为长期架构承诺。

如果第二个消费者出现，应优先重新评估共享基础，而不是复制第二套实现。

---

# 3. Semantic / Domain Namespace Audit

架构审核不能只查：

- 表名；
- API；
- 类名；
-字段。

还要检查：

> 两个模块是否用相同 / 相近业务词表达不同层级事实，导致 Owner 模糊。

建议维护：

```text
term
meaning
owner
scale
kind
```

同名不同尺度必须 namespace / rename。

---

# 4. Authority Classification Gate

所有重要结构分类为：

1. **Authoritative State**
2. **Derived Projection**
3. **Reference / Read Model**
4. **Cache**
5. **Historical / Legacy Evidence**

规则：

- Derived / Reference / Cache 默认不可反向改 Authoritative State；
- Snapshot / cache / projection 不能成为第二 live truth；
- 若 Reference 要产生业务变化，必须走正式 Owner 的 mutation path。

---

# 5. Aggregate vs Instance Boundary

当系统同时存在：

- aggregate / summary state；
- concrete entity / instance state；

必须显式设计 bridge。

至少回答：

- 何时只保留 aggregate？
- 何时 materialize 成 instance？
- 谁拥有 instance truth？
- 双向同步如何避免 double count？
- 删除 /合并 /恢复如何保持一致？

不要用“性能需要”作为建立第二事实源的理由。

---

# 6. Cause / Process / Consequence Boundary

跨模块因果链必须拆清：

```text
Cause
→ Process / Resolution
→ Persistent Consequence
→ Higher-level propagation
```

每层只修改自己的 owner state。

审核时检查：

> 是否为了方便从 Cause 直接跳写远端 Consequence？

如果是，优先增加 Handoff，而不是扩大 Source Owner。

---

# 7. Supersession Before Compatibility

未发布或无真实外部兼容义务时，新架构已完整替代旧实现，应优先：

```text
retire old
→ keep legacy evidence
→ migrate unique deltas
→ rebind consumers
→ delete obsolete path
```

而不是：

```text
old
→ adapter
→ v2
→ compatibility adapter
→ permanent dual path
```

只有存在真实用户 / 数据 /第三方依赖时才为兼容付长期成本。

---

# 8. Dependency Granularity

依赖不只分：

- depends / not depends。

应根据实际语义区分：

- required / hard；
- optional；
- handoff；
- feature-conditional；
- read-only / reference；
- UI contributor / owner。

一个模块“理论上支持很多能力”：

> 不等于整个 package hard-dep 所有 provider。

---

# 9. Convergence Audit Cadence

不要只做：

> 单模块测试 + 项目末期总审。

推荐：

- 每能力完成：local audit；
- 形成 2–5 个高耦合模块：cluster convergence；
- milestone：system convergence；
- release：full compatibility / migration audit。

Cluster Audit 重点检查：

- owner duplication；
- dependency inversion；
- semantic namespace；
- stale version；
- data flow；
- authority；
- UI ownership；
- migration residue。

---

# 10. Decision Ledger to Reduce Document Churn

不是每个小发现都立即升级：

- master spec；
- roadmap；
- architecture；
- skill；
- template。

可先进入：

> Decision / Pending Integration Ledger。

到原子任务、cluster 或 milestone 结束时批量回写。

立即改主事实源的条件：

- 当前执行若不更新就会错误；
- 安全 /数据 /权限边界改变；
- 下游马上依赖。

这样降低 stale reference 与版本同步成本。

---

# 11. State-machine Sequence Testing

继续强化之前 Restore / Recovery 的经验。

凡是状态机 / 生命周期能力至少测试：

- positive；
- negative；
- repeated action；
- idempotent replay；
- invalid order；
- interruption；
- return-to-origin / baseline；
- fault injection；
- partial-stage recovery。

单步 happy path：

> 不能证明状态机正确。

---

# 12. Logical Identity Before Technical Uniqueness

数据库 unique key / cache key / file name 不等于业务 identity。

设计 ID 前先回答：

> 业务上“同一个东西 / 同一次请求 / 同一个资源”到底是什么？

然后才设计：

- unique constraint；
- idempotency；
- fingerprint；
- cache key；
- ref ID。

---

# 13. UAT Boundary

真人 UAT 负责：

- 产品路径；
- 可理解性；
- 真实任务；
- 关键体验；
- 项目所有者接受。

不要要求用户：

- 精准刷新；
- kill 在毫秒窗口；
- 手工改数据库；
- 承担内部状态调试。

内部竞态：

> deterministic fault injection / automated sequence test。

---

# 14. Exploration vs Current

推荐架构 /文档状态：

```text
exploration
candidate
current
deprecated
legacy-reference
archived
```

下游只能依赖：

> 与其风险级别相匹配的上游状态。

不得把探索稿静默当 Current Contract。

---

# 15. v1.6 Change Log

新增：

- shared foundation before consumers；
- provisional owner；
- semantic namespace audit；
- Authoritative / Derived / Reference / Cache classification；
- aggregate vs instance bridge；
- cause / process / consequence；
- supersession before compatibility；
- dependency granularity；
- cluster convergence cadence；
- decision ledger；
- logical identity before technical uniqueness；
- lifecycle status discipline。

---

## DSH 执行适配

本 Skill 与工具无关，在 DeepSeek Harness 下按如下映射落地：

- 读取/恢复事实：用 `read`（含行号）、`glob`（路径匹配）、`grep`（内容检索）定位权威事实源，勿凭记忆断言。
- 落盘治理文档与决策记录：用 `write` 写入当前工作区（本部署 `D:\AI\deepseekharness`）。
- 用户裁定（授权门）：用 `ask_user_question`，一次一问、选项带推荐。
- 独立审核 / Evaluator：用 `subagent_fork` 派生子代理做隔离审核。
- 外部研究：用 `web_search`（附来源 URL）。
- 多阶段任务跟踪：用 `todo_write` 维护步骤清单。
- Planner / Generator / Evaluator 角色分离：映射为父代理规划 + 子代理生成/评估，评审结论写回工作区文件。