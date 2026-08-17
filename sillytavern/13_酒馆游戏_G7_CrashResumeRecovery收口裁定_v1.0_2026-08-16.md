---
title: 酒馆游戏 G7 Crash / Resume / Recovery 收口裁定
aliases:
  - G7 Recovery Closure
  - G7 Durable Execution 收口
  - G7 Crash Resume 收口
status: current
version: 1.0
created: 2026-08-16
updated: 2026-08-16
project: 酒馆游戏新版主体
scope:
  - Durable Execution
  - Crash Resume
  - Response-loss Recovery
  - Semantic at-most-once
  - Dice no-reroll
  - Formal Commit exactly-once
  - Idempotency Identity
  - Recovery UAT
---

# 酒馆游戏 G7 Crash / Resume / Recovery 收口裁定 v1.0

> [!abstract] 裁定结论
> `G7｜Crash / Resume / Recovery` 已完成工程实现、两轮独立审核、自动 Crash Injection、真实 DeepSeek Recovery Smoke 与项目所有者真人 UAT，于 2026-08-16 正式记为 **PASS / CLOSED**。
>
> 当前正式阶段进入 `G8｜网页产品化`。
>
> G7 冻结的是**异常执行的最小 Durable Execution / Recovery 语义**，不是分布式任务系统、多设备恢复、完整 Error Center、Undo 产品体验或多 Execution 管理器。

## 一、阶段状态

```text
G1｜真实模型兼容性             PASS / CLOSED
G2｜最小世界运行内核           PASS / CLOSED
G3｜正式世界能力扩展           PASS / CLOSED
G4｜程序判定与 Dice            PASS / CLOSED
G5｜长期世界连续性             PASS / CLOSED
G6｜Save / Continue / Restore  PASS / CLOSED
G7｜Crash / Resume / Recovery  PASS / CLOSED
G8｜网页产品化                 ACTIVE
```

G7 关闭后，除非出现重复 Semantic 执行、Dice 重掷、重复 Formal Commit、Response-loss 重复世界副作用、Recovery 污染 G6 Branch、未授权执行被恢复、隐藏信息泄露或失败 Execution 污染正式世界等硬问题，否则不重新扩张 G7 架构。

## 二、Execution 与 Formal Turn 的正式边界

### 2.1 Durable Execution 只是运行过程

G7 允许建立最小 Durable Execution Journal，用来保存 execution identity、idempotency identity、已稳定完成的阶段、恢复继续所需的最小结构化 artifact、committed / terminal failure 状态，以及 player-safe recovery status。

Durable Execution **不是世界历史**，也不是第二套 Game State。

### 2.2 Formal Turn 仍只在 Atomic Commit 后成立

以下状态不构成 Formal Turn：

- accepted；
- semantic_ready；
- resolution_ready；
- formal_outcome_ready；
- narrative_ready；
- terminal_failed。

只有正式世界、Event、Dice/Resolution ownership、G6 pending Branch 与 committed execution checkpoint 在同一原子提交成功以后，才存在新的 Formal Turn。

新版继续禁止把 `pending / processing / failed Formal Turn` 作为长期核心模型。

## 三、阶段所有权与“只补缺失阶段”

Recovery 只能继续**尚未完成的执行阶段**。

如果已有 durable evidence 证明某阶段完成：

- Semantic Proposal 已持久化 → 不重新调用 Semantic Provider；
- Dice / Resolution 已锁定 → 不重掷、不重新 Judge；
- Program Final Outcome 已持久化 → 不重新决定 Outcome；
- Narrative 已持久化 → 不重新生成；
- Formal Turn 已 committed → 不再次提交，只恢复同一个 committed result。

> **Recovery 不是“从头再跑一次”，而是由 Program 根据 durable stage ownership 补齐缺失阶段。**

## 四、Semantic at-most-once 与 Dice no-reroll

同一 Durable Execution 如果已经拥有合法 Semantic artifact，Crash / refresh / retry 后直接复用，不重新解释玩家原始 world-changing intent。

如果 Execution 只有 `accepted`，尚无 durable Semantic artifact，当前最小 G7 选择 **fail closed**，不得重新调用 Semantic Provider 冒充恢复。

已经锁定的 Dice 与 Program Resolution 保持 ID / 值不变；Recovery 不重掷、不重判，Dice 继续只是 Program Resolution 的证据，不是第二 Outcome owner。

## 五、Formal Commit exactly-once 与 Response-loss

同一 logical Execution 最多产生：

- 一个 Formal Turn；
- 一次 worldTime / authoritative revision 的业务提交；
- 一组匹配的 Event / Outcome / Dice ownership；
- 一次 G6 pending Branch 消费。

Response-loss 正式支持：

```text
Atomic Commit 成功
→ HTTP / client response 丢失
→ 页面刷新 / Recovery
→ 读取同一个 committed result
```

不得再次调用 Semantic、重掷 Dice、生成第二 Outcome、创建第二 Formal Turn、推进第二次 worldTime 或创建第二 Branch。

当前 minimal G7 使用 committed Turn / revision 等 canonical evidence 确认 Execution 仍属于当前正式历史；没有建设完整 immutable response archive。

## 六、Idempotency Identity 正式语义

review-01 发现：若公开 Submit 入口只按 `(gameId, idempotencyKey)` 找到旧 Execution 就直接返回，新的不同玩家输入可能被旧 committed result 静默吞掉。

review-02 正式修复为：

```text
logical submission identity
=
gameId
+ idempotencyKey
+ expectedRevision
+ canonical playerInput（当前采用 trim 后原始输入）
```

同一个 `(gameId, idempotencyKey)` 若出现不同 `expectedRevision` 或不同 canonical `playerInput`，必须在 Provider / RNG / Commit / Recovery 之前失败关闭。

完全相同的 logical submission 可安全重放原 committed result，不重复副作用。

## 七、Recovery key 与新玩家输入分离

客户端的 pending recovery key 只属于等待恢复的旧提交，只供 `getRecoveryStatus()` / `recover()` 使用。

新的玩家 Action 如果没有显式给 key，必须生成新的 idempotency key，不能自动继承 pending recovery key。

Recovery 尚未处理完时，Engineering Playtest Shell 可以暂时禁用新 Action 输入。完整的“放弃恢复并开始另一行动”属于 G8 产品 UX。

## 八、G6 Branch 与 Recovery 共存

Recovery 不拥有 Timeline / Branch 正常语义。

当 G6 已存在 pending Branch Origin 时：

- Recovery 不重建或覆盖 origin；
- read-only / failed Execution 不消费 pending；
- 只有第一次成功新 Formal Turn 与 committed execution checkpoint 同事务时消费 pending；
- response-loss 后恢复同一个 committed Turn 不再创建第二 Branch。

## 九、玩家安全

浏览器只得到 player-safe Recovery Projection，例如 none、recoverable、committed/result available、failed 与用户可理解错误。

不得发送 raw Provider response、reasoning、完整 Prompt、private Game State、API Key / Authorization、内部数据库路径或不必要的 Execution artifact。

## 十、最终工程 Gate

review-02 证据记录：

- branch：`main`；
- implementation commit：`6e665f0bccab8fcc82bb5bf223a818aa06eab3eb`；
- subject：`fix: bind recovery idempotency identity`；
- tracked worktree：clean；
- push：NO。

自动化与真实集成：

- `npm run g7:test`：3 files / **20 tests PASS**；
- `npm run g7:check`：56 files / **458 tests PASS**；
- R1–R13：PASS；
- regression corpus：**68 valid**；
- lint / typecheck / frontend build / disclosure scan / launcher smoke：PASS；
- Real DeepSeek `deepseek-v4-pro` Recovery Smoke：PASS；
- Provider calls：6；
- Semantic calls：2；
- Narrative calls：4；
- Durable Resume：PASS；
- Semantic at-most-once：PASS；
- Response-loss exactly-once：PASS；
- Hard Safety failures：0。

> [!note] 证据边界
> 上述 Git、测试和 Provider 数字来自 G7 review-02 审核包；本裁定未直接访问项目所有者 Windows 工作区重新执行命令。

## 十一、真人 UAT 与项目所有者裁定

项目所有者在行动提交后立即刷新页面，实际命中的是 Commit 前、尚无足够 durable Semantic artifact 的不可恢复路径。

玩家看到：

> **无法恢复**  
> 这项行动无法安全恢复，世界没有因此改变。

同时普通输入框重新可用，没有错误生成 Formal Turn，也没有可见世界污染。

原计划曾建议增加 deterministic Playtest trigger，让项目所有者再亲手命中 response-loss 的正向 Recoverable 路径。项目所有者明确裁定：

> **不再增加这项 UAT 工具；本阶段按通过处理。至少刷新后没有出现大问题，可以进入下一阶段。**

因此：

- 正向 Recoverable / response-loss 路径继续由 R1–R13、Crash Injection 与真实 DeepSeek Smoke 提供工程证据；
- 真人 UAT 直接验证了不可恢复路径的 fail-closed 产品表现；
- “项目所有者未亲手命中正向 recoverable 路径”登记为 **UAT 覆盖债**，不是 Runtime blocker；
- 不为此继续增加 G7 Playtest fault-control UI。

## 十二、阶段止损

G7 关闭后不继续建设：

- Queue / Worker / Redis；
- Distributed Lease；
- 多设备 / 多 Tab Recovery 协调；
- 多 Execution Manager；
- immutable response archive；
- 正式 Error Center；
- Undo / Re-input；
- 完整 Chaos Platform。

这些要么属于 Future，要么属于 G8 产品化。

## 十三、对 G8 的正式上游能力

G8 可以直接依赖 G5 玩家代理权与 bounded continuity、G6 authoritative Save / Restore / Branch、G7 durable execution / idempotency / response-loss recovery、player-safe Recovery Projection 与 exactly-once 提交边界。

G8 负责把这些工程能力变成正式 Player Product UX，包括正式网页信息架构、Save Center、Recovery/Error UX、玩家可见撤回/重新输入、减少过度 Confirmation、受控多动作自然输入与 Runtime-extensible UI Host。

G8 不重新设计 G5–G7 的正式状态权威。

## 十四、Lifecycle / Harness 可泛化经验

1. **幂等键必须绑定 immutable logical request identity。**
2. **Recovery 应由 durable stage ownership 驱动。**
3. **已提交结果的 response-loss 应重放 committed result，而不是重新执行业务。**
4. **没有足够 durable artifact 时，安全 fail closed 优于重新调用非确定性外部系统猜一次。**
5. **Recovery key 与 new submission key 必须分离。**
6. **人工 UAT 不应要求用户通过毫秒级竞速命中内部状态。**
7. **若硬不变量已由自动化、独立审核和真实集成充分证明，真人实际路径没有安全/数据问题，且项目所有者明确接受，未亲手覆盖的子路径可登记为 UAT coverage debt 而不是 blocker。**

酒馆项目专属的 Formal Turn、G6 Branch Origin、具体 Execution enum 不写入通用 Skill。

## 十五、生效结论

自 2026-08-16 起：

```text
G7 = PASS / CLOSED
G8 = ACTIVE
```

> **执行过程可以失败和恢复，但正式历史只能提交一次。**
>
> **幂等身份绑定的是同一次逻辑请求；Recovery 只补缺失阶段，不重做已经成立的世界语义。**
