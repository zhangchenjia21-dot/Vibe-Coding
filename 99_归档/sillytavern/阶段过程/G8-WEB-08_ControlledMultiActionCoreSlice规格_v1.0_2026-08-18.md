# G8-WEB-08｜Controlled Multi-action Core Slice 规格 v1.0

状态：`CURRENT IMPLEMENTATION BASELINE`
日期：2026-08-18
阶段：G8 网页产品化
上游：G5 Authorization / G6 Save-Restore / G7 Durable Execution / G8 Product UI

> 本规格只冻结 G8 进入 G9 前必须证明的最小多动作 Core semantics。
> 它不是通用 Planner、workflow DSL、agent loop，也不冻结 G9 asset-spec 的最终 action schema。

---

## 1. 产品目标

支持少量、顺序明确、玩家一次明确授权的高频多动作表达，例如：

```text
回到酒馆，然后睡到第二天。
```

正式语义：

```text
one Player Input
→ bounded ordered action sequence
→ Program preflight / simulation
→ one atomic Formal Delta
→ one Formal Turn
```

不得把两个子动作拆成两个 Product submit / 两个 Formal Turn。

---

## 2. G8 Slice 范围

### 2.1 Sequence 长度

G8 vertical proof 使用小型 bounded sequence；默认最大 `2` steps。

该上限是 G8 internal capability limit，不是未来 external asset protocol 的永久业务上限。

### 2.2 合法 Step

只允许现有 `AuthorizedPlayerAction` 已覆盖、Program 可以 exact authorization 的 atomic action family：

- move；
- item_transfer；
- show_item；
- item_interaction；
- wait。

但若任何 step 在实际 transient state 中需要 Dice / Resolution：

> 本 G8 slice 整段 fail closed，不掷骰、不部分执行。

Resolved multi-action 属于未来 capability extension；不得为了 WEB-08 重写 G7 Durable Resolution artifact。

### 2.3 不属于本 Slice

- 任意长期计划；
- branching workflow；
- `if / else`；
- alternatives (`还是 / 或者`)；
- 循环；
- autonomous agent loop；
- 多 Turn plan；
- unresolved/implicit subgoals；
- sequence 中的 mandatory Dice / Resolution；
- Expansion-specific action language。

---

## 3. Semantic Contract

当前 Runtime Semantic Proposal 只有一个 candidate。WEB-08 应扩展 runtime-only candidate vocabulary，而不修改冻结的 legacy player-intent candidate v0。

推荐内部结构：

```text
RuntimeSemanticCandidate
=
Atomic Runtime Candidate
|
action_sequence {
  ordered steps: Atomic Executable Runtime Candidate[]
}
```

约束：

- steps 有序；
- 2 steps for G8 slice；
- 不嵌套 sequence；
- 不包含 `no_action / needs_clarification / read_only_query`；
- 每个 step 必须对应玩家 raw input 中独立、逐字存在的 evidence；
- `worldChangingIntentEvidence` 对 sequence 应完整覆盖全部 steps；
- Model 不得省略一个动作再让 Program 猜；
- alternatives / conditional / question / wish / negation 不得升级成 sequence execution。

Model 只负责识别有序语义；Program 仍拥有 authorization 与正式执行权。

---

## 4. Authorization

现有 `PlayerActionAuthorization.authorizedActions[]` 已允许多个 action，因此继续作为唯一正式授权结果，不新建第二套 sequence authorization owner。

Sequence authorization 必须：

1. raw input agency = execute now；
2. 每个 step evidence 是 raw input 子串；
3. evidence 与 step 顺序一致；
4. 每个 step exact candidate 可被 Program 校验；
5. 没有未报告额外 world-changing intent；
6. 没有 alternative / conditional / hypothetical / negation；
7. 任何 step 不明确 → 整段不执行。

对不满足强授权条件的 sequence：

> 返回 clarification / conservative failure；本 slice 不新增 sequence confirmation state machine。

现有单动作 confirmation 行为不回归。

---

## 5. Program Preflight / Transient Simulation

禁止：

```text
for step in sequence:
  submit(step)
```

正确链：

```text
initial RuntimeGameState
↓
step 1 capability/authorization/resolution-necessity check
↓
compute deterministic step delta
↓
apply to transient state only
↓
step 2 check against transient state
↓
compute deterministic step delta
↓
compose canonical aggregate delta
↓
validate aggregate
↓
one commitFormalTurn
```

Transient state：

- 不是 authoritative state；
- 不写 SQLite world tables；
- 不创建 Formal Event history；
- 不消耗 idempotency；
- 只用于 Program preflight / composition。

如果需要状态 reducer，应抽取共享 pure Program reducer / delta composition primitive；不得复制一套与 SQLite commit 不一致的世界 mutation 规则。

---

## 6. Atomic Delta Composition

多个 step 只产生一个最终 `FormalDelta`。

Composition 必须：

- durationMinutes = ordered steps 的 Program duration 总和；
- position / item placement / state change 以 initial → final canonical effect 表达；
- step public events 保持有序，可用于 Narrative 理解发生顺序；
- 同一 owner/slot 被多 step 修改时最终 Delta 不得产生 mutually-invalid fromValue；
- aggregate 必须继续通过现有 formal outcome validation；
- SQLite 只看到一个最终 transaction。

任何 step preflight / composition 失败：

```text
world mutation = 0
Formal Turn = 0
partial event = 0
```

---

## 7. Resolution Boundary

G8 slice 明确不处理 sequence 内需要 Resolution 的 step。

Program 在 preflight 时先调用现有 resolution necessity gate。

若任一 step requires dice/resolution：

```text
sequence unsupported in current slice
→ fail closed
→ dice calls = 0
→ world mutation = 0
```

单动作 Resolution 流程完全保持当前 G4/G7 权威。

未来若产品确实需要 resolved multi-action，必须单独做 capability review，并先扩 Runtime/Durable artifact，再允许 asset-spec 声明。

---

## 8. Background Progression / Time

Sequence 内部时间由 Program 计算。

Background Progression：

- 不按每个 step 调模型；
- 不因为 sequence 有多个 step 就产生多轮 background model work；
- 在 aggregate time advance 上做一次 deterministic due progression composition；
- 继续遵守第 15 号核心 v1.1：`Background deterministic progression != Model Activation`。

例如：

```text
move 10 min
+
wait until next morning
→ Program 计算 total/final world time
→ 一次 compose due background progression
→ one atomic commit
```

---

## 9. Durable Execution / Recovery

WEB-08 不重开 G7 架构。

Sequence 作为一个 Runtime semantic candidate / execution identity：

- one idempotency key；
- one DurableExecutionRecord；
- one semantic artifact；
- no intermediate Formal Commit；
- final aggregate delta 进入现有 resolution/formal/narrative stages；
- crash / retry 不得重新执行已持久化 semantic work；
- response-loss replay 仍只返回同一 committed Formal Turn。

因为本 slice 不允许 resolved sequence，现有单 `resolution?:` durable artifact 不需要升级为 resolution list。

至少增加 sequence-specific fault/recovery regression，证明 no partial commit / exactly-once。

---

## 10. Narrative

Narrative Provider 只能读取 Program 已确定的 aggregate Formal Outcome / ordered public events。

不得：

- 重新决定是否执行 step 2；
- 改变顺序；
- 把失败的 preflight step 写成已经发生；
- 补造未提交副作用。

Player Input 仍显示为一次输入，Turn 仍只有一次正式叙事结果。

---

## 11. Product UI

WEB-08 不新增复杂 UI。

现有 Composer 继续提交一条自然语言 input。

Product UI 最低要求：

- sequence 成功后仍显示一个 Formal Turn；
- Timeline 不拆成两个玩家提交；
- Program public events / narrative 足以让用户理解先后动作；
- sequence unsupported / clarification 使用现有 player-safe error/clarification language。

不新增 Planner editor / step list editor。

---

## 12. Required Vertical Proof

至少证明：

### Positive

```text
move + wait
→ ordered sequence
→ second step evaluated against transient post-move state
→ one Formal Turn
→ final position correct
→ final time correct
→ no partial commit
```

### Atomic failure

构造 step1 valid、step2 invalid/unsupported：

```text
result = fail closed
revision unchanged
turnNumber unchanged
position unchanged
worldTime unchanged
no events committed
```

### Resolution rejection

step2 requires Resolution：

```text
sequence rejected before dice
Program RNG calls = 0
world unchanged
```

### Recovery

对 sequence 在 semantic/formal/narrative durable stage 注入 crash：

- recovery exactly-once；
- no duplicate Semantic；
- no partial intermediate world state；
- committed replay 不重复世界变化。

### Single-action regression

现有单动作 Authorization / Confirmation / Dice / Save / Recovery 全部保持。

---

## 13. Exit Criteria

WEB-08 PASS 需要：

- bounded ordered sequence runtime contract；
- strong Program authorization；
- transient preflight；
- deterministic-only G8 sequence scope；
- aggregate atomic FormalDelta；
- one Formal Turn；
- no partial commit；
- no-dice sequence rejection；
- background deterministic progression remains Program-only；
- G6/G7 regression；
- player-safe disclosure；
- Product UI minimal proof；
- full tests / lint / typecheck / build PASS。

完成后进入 G8 Exit Gate。
