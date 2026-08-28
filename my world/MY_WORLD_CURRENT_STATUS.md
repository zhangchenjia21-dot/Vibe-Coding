---
title: my world｜当前状态
status: current-project-status
version: 2.9
created: 2026-08-26
updated: 2026-08-28
phase: G3 Persistent Game / Save / Timeline Foundation
current_task: G3-05 Recovery / Timeline Foundation — READY FOR OWNER UAT
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
G3-05 Recovery / Timeline      ENGINEERING PASS — READY FOR OWNER UAT
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

已冻结：production schema v3、immutable player Save Point、atomic World/head/Conversation Restore、Conversation-owned recovery validation、COMMIT-before-memory/UI switch、crash-after-commit reopen correctness、future-memory isolation、historical Timeline retention 与明确高影响 Load UX。

---

## 6. G3-05｜ENGINEERING PASS / READY FOR OWNER UAT

实现 commit：

```text
bf8c35fdf76c4ea3b8ad2560d93c89c2f84c07b0  G3-05 Recovery / Timeline Foundation
```

Independent Review：**PASS — no engineering blocker found**。

已证明：

- production schema v3 → v4 additive transactional migration；intentional mid-migration failure rollback 保持 v3 current/Save/Timeline truth；
- immutable `recovery_checkpoints` 使用 stable `recovery_id`；`AUTOINCREMENT recovery_sequence` 仅用于 durable monotonic latest ordering，不承担业务 identity；
- Load old Save 时，Recovery capture + World/head/Conversation switch 位于同一个 SQLite transaction；INSERT/World/head/Conversation/COMMIT 任一步失败均 rollback，且不留下 orphan Recovery；
- Recover Previous Progress 也是同样的 protected switch：先捕获被替换 current 为 reciprocal Recovery，再原子切回 latest Recovery target；历史 Recovery 不 consume/delete；
- exact no-op Load/Recover 返回 `already_current`，不提交、不增加 Recovery、不改变 latest useful Recovery；
- transaction 内重新解析并 exact 校验 latest Recovery，防止 Runtime 预检与 COMMIT 之间目标漂移；
- latest Recovery 依据 durable monotonic sequence，不依赖 wall-clock；同一秒连续 Load/Recover 仍保持无歧义顺序；
- historical Timeline DAG 直接承载 internal branch：从 H1 恢复后可形成 H1→H2 与 H1→H3 两个 immutable future，不新增 branch registry/browser；
- Regenerate / Correction 的最终 durable accepted truth 可被 Recovery exact round-trip；cancelled/failed partial 不进入 Recovery；
- Recover 后 Context 对另一条 displaced future 保持对称隔离，recent-12 / current-user exactly-once-last 仍成立，Prompt/raw World JSON 不作为 truth；
- protected Load 与 Recover 的 crash-after-COMMIT / before memory-UI apply 均可在 exact-PID termination 后 reopen 到正确 target，并保留 displaced Recovery；
- World Surface 只暴露 latest “恢复上一进度”入口，与 named Save 分离；使用明确 confirmation，active generation 时禁用；正常 reopen 后 availability 从 durable truth 重建；
- real DeepSeek post-Recover continuation、Windows export、launcher smoke 与 G3/G2 regressions均已有 Agent evidence。

G3-05 未实现 Recovery history browser、branch picker、arbitrary per-Turn rewind、G3-06 backup/corruption/interrupted-write hardening、双进程并发保护或 G4/G5/G7。

---

## 7. Owner UAT｜CURRENT

只验证真实产品价值，不要求查看日志/数据库：

```text
run-game.cmd
→ 在当前进度创建一个旧 Save S1
→ 从 S1 之后继续玩 1–2 Turn，形成容易辨认的 Future A
→ 不为 Future A 额外 Save
→ Load S1
→ 确认 UI 出现“可恢复上一进度”能力
→ 在 S1 上继续 1 Turn，形成明显不同的 Future B
→ Recover Previous Progress
→ 确认 Narrative 完整回到 Future A，Future B 不残留
→ 再 Recover Previous Progress
→ 确认又能完整回到 Future B
→ 正常退出并重开
→ 确认当前进度与 Recovery availability 都仍正确
```

PASS 关注：

- 玩家不必事先另存 Future A，也能在误读档后找回；
- Load 与 Recover 的确认和“上一进度”概念直观，不像 Timeline debugger；
- A↔B 往返时 Narrative 没有叠加、重复或混线；
- AI 在每条 current future 中只记得该 future 的 accepted history，不泄漏另一边；
- 退出重开后 current 与 Recovery 能力仍成立；
- 没有 half-switch、空白局、明显卡死或错乱。

Owner UAT PASS 前不得开始 G3-06。

---

## 8. 当前核心约束

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

## 9. 当前 waiting

```text
Blocking: Owner UAT not yet completed
Current: G3-05 Owner UAT
G3-06: HOLD
Next after Owner UAT PASS: G3-06 Crash / Interrupted Write Recovery
```
