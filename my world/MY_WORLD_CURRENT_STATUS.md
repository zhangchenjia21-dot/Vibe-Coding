---
title: my world｜当前状态
status: current-project-status
version: 2.8
created: 2026-08-26
updated: 2026-08-27
phase: G3 Persistent Game / Save / Timeline Foundation
current_task: G3-05 Recovery / Timeline Foundation
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜CURRENT STATUS

## 1. 文档职责

本文件只拥有当前执行状态：Current Phase、Current Task、已完成 Gate、Owner UAT 结论、当前 blocker / waiting。

其它事实 owner：

- 产品目的：`MY_WORLD_项目启动总纲_CURRENT.md`
- 跨阶段原则：`MY_WORLD_核心设计原则_CURRENT.md`
- 架构地图：`MY_WORLD_架构_CURRENT.md`
- 阶段 / Task DAG：`MY_WORLD_总体规划路线图_CURRENT.md`

---

## 2. 当前状态

```text
G1 Foundation                  PASS / CLOSED
G2 AI Conversation Spine       PASS / CLOSED
G2-GATE                        PASS

Current Phase                  G3 — Persistent Game / Save / Timeline Foundation
G3-01 Persistence Architecture PASS — Independent Review
G3-02 Durable World Mutation   PASS — Independent Review
G3-03 Game Reopen / Resume     PASS — Owner UAT
G3-04 Save / Load / Restore    PASS — Owner UAT
Current Task                   G3-05 — Recovery / Timeline Foundation
G3-GATE                        NOT YET
```

---

## 3. G3-01 / G3-02｜CLOSED

已接受第一代 persistence route：

```text
SQLite
+ 2shady4u/godot-sqlite v4.9 GDExtension
+ Godot 4.7.2 Standard / non-.NET Windows x64
+ GDScript / same-process Runtime
```

已冻结：Game/World/Timeline-Save/Conversation 各自拥有业务语义；Persistence 只拥有 durable representation / transaction / schema / migration / backup-recovery mechanics。G3-02 已证明 atomic World/Timeline/head transaction、expected-head CAS、replay-safe mutation identity、immutable Timeline recovery anchors 与 physical query failure fail-loud。

---

## 4. G3-03｜CLOSED / PASS — Owner UAT

实现 commit：

```text
929f4ff1e1253a808522d8f559a3cadd01b8d5db  G3-03 current Game reopen / resume
```

Independent Review：**PASS**。Owner UAT（2026-08-27）：**PASS**。

已冻结：production schema v2、one-current-Game runtime、current accepted Conversation durable materialization、persist-before-accept、only accepted truth resumes、Context/Provider messages derived/rebuildable、existing corrupt/schema/ambiguous state fail-loud。

---

## 5. G3-04｜CLOSED / PASS — Owner UAT

实现 commit：

```text
618fa0f2238114cbe4fc0fe790a1d60c43e99b45  G3-04 explicit Save / Load / Restore + Context rebuild
```

Independent Review：**PASS**。Owner UAT（2026-08-27）：**PASS**。

Owner 已真实验证：命名 Save、继续产生未来、明确 Load、Narrative 回退、AI future-memory isolation、Restore 后继续游戏与退出重开均符合预期。

已冻结 G3-04 事实：

- production schema v3；
- immutable player Save Point 使用 stable `save_id` + display name + Timeline anchor + accepted Conversation recovery material；
- Save 捕获 coherent durable head + accepted Conversation，不复制第二份 World truth；
- Restore 原子切换 current World + Game.active_head + accepted Conversation；
- Conversation recovery material 由 Conversation Domain 预验证；
- durable COMMIT 先于 memory/UI switch；
- crash-after-Restore-COMMIT / before UI apply 可通过 reopen 恢复 durable restored truth；
- Restore 后 Context 重新组装，被回滚 future 不进入 Provider request；
- historical Timeline Nodes / unrelated Save Points 不因 Load 删除；
- Save/Load 是明确高影响操作，不提供 arbitrary per-Turn rewind。

Non-blocking follow-up：双开两个产品进程同时写 current Game 的保护仍需在 G3-06 / standalone hardening 前冻结。

---

## 6. Current Task｜G3-05 Recovery / Timeline Foundation

Outcome：让“读取旧存档”本身也可恢复。玩家不需要为了防止一次误读档而事先知道自己必须另存当前未来。

第一代目标路径：

```text
current progress Fcurrent
→ player explicitly Loads old Save S1
→ Runtime atomically preserves Fcurrent as internal Recovery Checkpoint R1
→ current switches to S1
→ player realizes this was the wrong Restore
→ explicit Recover Previous Progress
→ World/head/Conversation atomically return to R1
→ Context rebuilds from recovered truth
→ AI continues from the recovered future
```

冻结方向：

- Recovery Checkpoint != player Save Point；它是 Runtime 自动创建的 safety material，不进入普通 Save 列表；
- 每次真正发生高影响 progress switch 前，在**同一个 SQLite transaction**里先捕获被替换的 durable `active_head + accepted Conversation`，再切换目标状态；如果 switch 失败，Recovery Checkpoint 也不得单独留下；
- Recovery Checkpoint 引用已有 immutable Timeline Node 作为 World anchor，并保存该时刻 accepted Conversation recovery material；不复制整份 World DB；
- 第一代允许 append-retained internal recovery records，但普通 UI 只暴露最近一个“恢复读取前进度”入口，不做 recovery history browser；
- successful recovery switch 也应保护被替换的当前进度，使最近两条 active futures 可以安全来回切换，而不是恢复一次就销毁另一边；
- no-op Load/Recover（目标 head + Conversation 已等于 current）不得覆盖或制造无意义 recovery material；
- historical Timeline Nodes 保留；从 restored old node 继续新的 World mutation 时允许自然形成 internal branch，旧 future 不删除；不建设 branch graph / branch picker；
- Retry / Regenerate / latest-turn Correction 在 Load/Recover 前后仍遵守 durable accepted truth 边界；Recovery 必须恢复这些已经 accepted 的最终版本，而不是旧 draft/partial；
- active generation 中不得执行 Load/Recover；
- Context/Prompt 继续 derived/rebuildable，Recover 后也必须满足 future-memory isolation；
- G3-06 的物理 DB corruption/backup、interrupted-write hardening、双进程并发保护不在本任务提前实现。

---

## 7. 当前核心约束

- `Model authors the world; Runtime makes it durable; Player owns the timeline.`
- `Save Point != Timeline Node.`
- `Recovery Checkpoint != Save Point.`
- `Reversibility != frictionless arbitrary rewind.`
- 局部错误低成本纠正；重大历史恢复必须表达明确玩家意图。
- Restore / Recover 都不得半切换 current head / World / Conversation。
- UI / Transcript / Markdown / Godot Resource 不得成为 authoritative gameplay DB。
- Context 是 derived/rebuildable material，不是另一份存档。
- internal branch capability 只服务 correctness / recovery；不得演化成默认 Timeline debugger。

---

## 8. 当前 waiting

```text
Blocking: NONE KNOWN
Current: G3-05 repository-native Task Packet / implementation
Owner UAT: required after Engineering + Independent Review
G3-06: HOLD
Next after G3-05 PASS: G3-06 Crash / Interrupted Write Recovery
```
