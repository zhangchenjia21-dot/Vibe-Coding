---
title: 酒馆游戏 G6 Save / Continue / Restore 收口裁定
aliases:
  - G6 Save Restore Closure
  - G6 存档恢复收口
  - G6 Branch 语义收口
status: current
version: 1.0
created: 2026-08-15
updated: 2026-08-15
project: 酒馆游戏新版主体
scope:
  - G6 Save
  - Continue
  - Restore
  - Branch
  - Canonical Snapshot
  - Timeline Navigation
  - 阶段复盘
---

# 酒馆游戏 G6 Save / Continue / Restore 收口裁定 v1.0

> [!abstract] 裁定结论
> `G6｜Save / Continue / Restore` 已完成工程 Gate、三轮独立审核收口与项目所有者真人 UAT，于 2026-08-15 正式记为 **PASS / CLOSED**。
>
> 当前正式阶段进入 `G7｜Crash / Resume / Recovery`。
>
> G6 冻结的是**权威状态保存、恢复、继续与分支的正式语义**，不是完整 Save Center、Timeline UI、Undo 产品体验或 Crash Recovery。

---

## 一、阶段状态

```text
G1｜真实模型兼容性             PASS / CLOSED
G2｜最小世界运行内核           PASS / CLOSED
G3｜正式世界能力扩展           PASS / CLOSED
G4｜程序判定与 Dice            PASS / CLOSED
G5｜长期世界连续性             PASS / CLOSED
G6｜Save / Continue / Restore  PASS / CLOSED
G7｜Crash / Resume / Recovery  ACTIVE
```

G6 关闭后，除非出现正式状态恢复错误、时间线误分支、原子性破坏、隐藏状态泄露、Save 成为第二事实源或恢复绕过正式权限等硬问题，否则不重新扩张 G6 架构。

---

## 二、G6 已冻结的权威状态语义

### 2.1 Save 只发生在稳定 committed boundary

Save 只能捕获已经完成 Atomic Commit 的正式世界。

不得把以下执行中间态保存为可恢复世界：

- failed / cancelled Execution；
- awaiting Confirmation；
- incomplete Resolution；
- 未提交 Narrative；
- read-only ephemeral response；
- Provider transient state；
- UI draft / loading state。

Save 的语义是：

> **保存一个已经成立的权威世界，而不是保存一次正在运行的请求。**

### 2.2 Canonical Snapshot 是恢复材料，不是第二实时事实源

G6 使用 canonical snapshot 保存恢复所需完整状态，但当前运行世界继续由正式 normalized Runtime State / 持久化结构拥有。

Snapshot：

- immutable；
- versioned；
- integrity checked；
- 用于 Save / Restore / Branch comparison；
- 不参与普通 Turn 的实时写入权威。

因此不得形成：

```text
Live Runtime State
+
Live Snapshot State
```

两个并行事实源。

### 2.3 Snapshot 必须覆盖完整恢复语义

G6 已验证恢复至少覆盖：

- worldTime；
- Region / Place / Scene / Position；
- Item Placement / state；
- Character State；
- Character Knowledge；
- Relationship；
- Commitment；
- Pending World Action；
- Background Progression state；
- Event / Formal history；
- G4 Dice / Resolution history；
- G5 continuity structures；
- 当前正式叙事与恢复所需历史边界；
- state/schema/world/runtime version metadata。

不得通过“把聊天重新发给模型”重建这些事实。

### 2.4 Snapshot 明确不保存临时或危险内容

不进入 canonical Save authority：

- API Key / Authorization；
- raw Provider response；
- reasoning；
- 完整系统 Prompt；
- bounded context cache；
- stale Confirmation execution authority；
- read-only ephemeral response；
- 浏览器临时 UI 状态。

旧 Confirmation 在 Restore 后不得重新获得执行权。

---

## 三、Continue 正式语义

Continue 从当前最新稳定 committed world 继续。

Continue 不等于：

- 重新发送整段 transcript；
- 恢复 Provider 内存；
- 恢复浏览器缓存；
- 从某个旧 Save 猜测当前状态。

正式流程是：

```text
读取当前 committed authoritative state
→ 重建玩家安全投影
→ 重新编译 bounded continuity context
→ 等待下一次玩家输入
```

G5 的 bounded Context 仍保持：

- recent Turns `<= 4`；
- relevant Events `<= 6`；
- current authoritative state 优先于 old history。

---

## 四、Restore 正式语义

### 4.1 Restore 是 Program-owned、无模型的权威状态恢复

Restore 本身：

- 不调用模型；
- 不形成 Formal Turn；
- 不推进 worldTime；
- 不创建普通世界 Event；
- 不创建新的游戏事实；
- 不让 Narrative 决定恢复结果。

真实 DeepSeek Smoke 已验证：

> `Restore Provider calls = 0`。

### 4.2 Restore 必须原子

Restore 的 canonical validation、world state replacement、formal history replacement、timeline metadata 与 pending branch metadata 必须处于同一原子事务边界。

任何 Snapshot 损坏、版本错误、引用错误、integrity failure 或注入故障都必须：

> **fail closed，当前世界保持原样。**

不得出现：

- 世界回去了但 pending metadata 没回去；
- Branch metadata 改了但世界没改；
- 只恢复公开状态，没有恢复隐藏知识；
- 部分表成功、部分表失败。

### 4.3 Domain state 与 operation revision 分离

G6 采用单调 revision / state version 继续保护 stale request、old Confirmation、并发覆盖与 execution identity。

因此 Restore 后：

- **领域世界可以回到旧 Save；**
- **执行 revision 不必倒退。**

关键裁定：

> `revision` 是执行与并发控制元数据，不是 Timeline identity，也不是“世界是否发生分歧”的事实源。

---

## 五、Branch 正式语义

### 5.1 Restore 本身不创建 Branch

正式 Branch 不是“调用过 Restore”，而是：

> **玩家离开原本的正式未来，并从旧状态完成第一次新的成功 Formal Turn。**

```text
Restore older state
→ pending divergence only
→ Branch count = 0

successful new Formal Turn
→ exactly one Branch
```

### 5.2 Canonical divergence 才能建立 pending branch

当不存在 pending origin 时：

```text
current canonical state/history == target save
→ Restore allowed
→ pendingBranch = false

current canonical state/history != target save
→ archive current original future
→ pendingBranch = true
```

Canonical equality 可以忽略纯执行层顶层 `sourceRevision`，但不能忽略会改变真实世界或正式历史的内容。

### 5.3 第一次离开正式未来时冻结 immutable Branch Origin

第一次 divergent Restore：

```text
Original Future
→ immutable pending Branch Origin
```

在第一次新的成功 Formal Turn 之前，后续 Restore 只是 timeline navigation。

不得每 Restore 一次就创建新 origin，也不得用当前回退节点覆盖 original future。

### 5.4 返回 original future 会取消 pending

```text
T1
→ Restore T0
→ pending = true
→ Restore T1
→ pending = false
→ next successful Turn = normal continuation
→ NO Branch
```

这保证“读旧档看看，再回到原进度”不会污染时间线。

### 5.5 连续回溯保持第一次 Origin

```text
T3
→ Restore T1
→ Restore T0
```

只保留第一次离开时的 T3 original future 作为 pending origin；后续成功新 Turn 的 Branch 继续引用 T3，而不是 T1。

### 5.6 只有成功 Formal Turn 消费 pending

read-only query、failed/cancelled execution、model/narrative/validation failure 与 Restore navigation 本身都不能消费 pending。

只有：

```text
pending exists
+
successful Atomic Commit of new Formal Turn
```

才创建 exactly one Branch 并消费 pending。

---

## 六、玩家安全与 UI 边界

浏览器只能得到 player-safe Save / Restore projection，不得发送 raw snapshot、private Character Knowledge、hidden plans、Provider raw 或内部数据库内容。

G6 Playtest Shell 只需要 Save、Continue、Restore 与最小 Branch pending 状态提示。

正式 Save Center、Timeline 浏览器、Archive GC 与完整历史可视化不属于 G6。

---

## 七、G6 最终工程 Gate

最终 review-03 证据记录：

- branch：`main`；
- commit：`47556b7ad8a0fc071795b6996abec8bc1fb604cd`；
- subject：`fix: preserve restore branch origin`；
- tracked worktree：clean；
- push：NO。

自动化：

- `npm run g6:test`：4 files / **15 tests PASS**；
- `npm run g6:check`：53 files / **438 tests PASS**；
- regression corpus：**68 valid**；
- lint / typecheck / build / disclosure scan：PASS；
- launcher smoke：PASS；
- DeepSeek `deepseek-v4-pro` Smoke：PASS；
- Provider calls：15；
- Restore Provider calls：**0**；
- exact world restore：PASS；
- Commitment restored：PASS；
- next committed Turn Branch：PASS；
- hidden state sent to browser：false。

项目所有者随后完成真人 Save / Continue / Restore 路径并明确确认：**测试通过。**

> [!note] 证据边界
> 上述 Git、测试和 Provider 数字来自 G6 review-03 审核包；本裁定未直接访问项目所有者 Windows 工作区重新执行命令。

---

## 八、G6 阶段复盘

### 8.1 原目标

```text
Save
→ mutate through normal committed Turns
→ Restore complete authoritative state
→ Continue
→ new successful Turn after true rewind creates Branch
```

### 8.2 做对了什么

1. 没有复制旧版 Turn/Timeline 实现，只复用了已经想清楚的产品语义。
2. Snapshot 保持为恢复材料，没有变成第二实时事实源。
3. Restore 从一开始就保持 Program-owned、无模型、原子事务。
4. G4 Dice / G5 Knowledge / Commitment / Continuity 没有在恢复中降级。
5. 玩家只在工程 Gate 和独立审核完成后做一次有价值 UAT，没有承担 branch 语义调试循环。

### 8.3 最大返工

最大返工不在“如何保存数据库”，而在 **Branch eligibility / Timeline identity**。

review-01 发现“恢复同一当前状态仍会 Branch”；review-02 进一步发现“连续 Restore 会覆盖 Branch Origin，回到原未来仍 pending”。最终才收口为：

```text
Canonical divergence
→ immutable original future
→ navigation
→ first new committed result creates Branch
```

### 8.4 被推翻的假设

**被推翻：** 比较 current snapshot 与 target snapshot 就足以决定所有 Branch 行为。

**新结论：** 一旦进入 pending rewind，比较基准必须变为第一次离开时冻结的 original future；当前回退节点不再是 Branch identity 的基准。

### 8.5 测试经验

只测试“旧档 → 新 Turn → Branch”不足以证明 Branch 正确。恢复/回滚状态机必须同时覆盖：

- positive case；
- negative case；
- repeated navigation；
- return-to-origin；
- read-only / failed operation；
- transaction fault injection。

> **状态机的不变量必须通过序列测试证明，而不是只靠单步 happy path。**

### 8.6 用户负担

Branch 的两轮语义问题由独立 Review + deterministic regression 发现；真人 UAT 只在 review-03 无 blocker 后进行。用户不需要查看数据库、日志或重复跑技术 case。

### 8.7 阶段止损

review-03 以后不再继续扩张 Archive GC、Timeline Manager、Save Center、Undo 或 Crash Recovery；它们要么不是 blocker，要么属于 G7/G8。

---

## 九、对 G7 的正式边界

G7 可以建立在 G6 以下稳定能力之上：stable committed state、canonical snapshot、atomic restore、monotonic execution revision、immutable branch origin、player-safe restore projection。

G7 负责 idempotency、durable execution、semantic at-most-once、Dice locking in recovery、crash/resume、response-loss recovery 与 narrative recovery。

**G7 不重新设计 Save / Restore / Branch 的正常语义。**

---

## 十、对 G8“撤回 / 重新输入”的影响

G6 现在为 G8 Undo / Re-input 提供完整 authoritative restore、Branch/original future 语义与原子恢复基础；但仍需 G7 的 idempotency、recovery ownership 与 crash/response-loss 边界后才能产品化。

---

## 十一、Lifecycle / Harness 可泛化经验

1. **Snapshot 是不可变恢复材料，不应成为第二套 live truth。**
2. **执行 revision / version 与业务状态等价性必须分离。** 版本号变化不自动代表业务世界产生分歧。
3. **Restore / rollback 是导航；Branch / fork 需要真实的新提交。**
4. **第一次离开原状态时的 origin 必须稳定。** 新提交前的重复导航不应不断重写 origin。
5. **恢复状态机必须测试反向路径和操作序列。** 同状态恢复、连续恢复、返回 origin、失败/只读、故障注入与 happy path 同等重要。
6. **Timeline / recovery metadata 与业务状态应在同一原子边界。**

这些经验进入跨项目 Lifecycle Harness；酒馆游戏专属的 Formal Turn、World Time、Character Knowledge 名称不写入通用 Skill。

---

## 十二、生效结论

自 2026-08-15 起：

```text
G6 = PASS / CLOSED
G7 = ACTIVE
```

> **Save 保存已经成立的世界；Restore 原子恢复完整权威世界；Continue 从当前 committed truth 继续；Branch 只在真正从恢复点提交新未来时成立。**
>
> **恢复操作、执行 revision 与业务分歧是三个不同概念，不得混为一个状态。**
