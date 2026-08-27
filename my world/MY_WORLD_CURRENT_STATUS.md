---
title: my world｜当前状态
status: current-project-status
version: 2.7
created: 2026-08-26
updated: 2026-08-27
phase: G3 Persistent Game / Save / Timeline Foundation
current_task: G3-04 Explicit Save / Load / Restore + Context Rebuild — READY FOR OWNER UAT
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
G3-04 Save / Load / Restore    ENGINEERING PASS — READY FOR OWNER UAT
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

已冻结：Game/World/Timeline-Save/Conversation 拥有各自业务语义；Persistence 只拥有 durable representation / transaction / schema / migration / backup-recovery mechanics。G3-02 已证明 atomic World/Timeline/head transaction、expected-head CAS、replay-safe mutation identity、immutable recovery anchors 与 physical query failure fail-loud。

---

## 4. G3-03｜CLOSED / PASS — Owner UAT

实现 commit：

```text
929f4ff1e1253a808522d8f559a3cadd01b8d5db  G3-03 current Game reopen / resume
```

Independent Review：**PASS**。Owner UAT（2026-08-27）：**PASS**。

已冻结：production schema v2、one-current-Game runtime、current accepted Conversation durable materialization、persist-before-accept、only accepted truth resumes、Context/Provider messages derived/rebuildable、existing corrupt/schema/ambiguous state fail-loud。

Non-blocking follow-up：双开两个产品进程同时写 current Game 的保护仍需在 G3-06 / standalone hardening 前冻结。

---

## 5. G3-04｜ENGINEERING PASS / READY FOR OWNER UAT

实现 commit：

```text
618fa0f2238114cbe4fc0fe790a1d60c43e99b45  G3-04 explicit Save / Load / Restore + Context rebuild
```

Independent Review：**PASS — no engineering blocker found**。

已证明：

- production schema v2 → v3 additive transactional migration；intentional mid-migration failure rollback 保持 v2 current truth；
- immutable `save_points` 使用 stable `save_id`，display name 不承担 identity；
- Save 在单一 transaction 内捕获 durable active head + accepted Conversation，且不改变 current World/head/Conversation；
- Save Point 引用 Timeline Node immutable World snapshot，不复制第二份 World live truth；
- Restore 在一个 SQLite transaction 内原子更新 current World materialization、Game.active_head 与 current accepted Conversation；任一步失败全部 rollback；
- Conversation Domain 提供 non-mutating validation 与 COMMIT 后 accepted replacement；Persistence 不拥有 accepted Turn 语义；
- Restore COMMIT 后才切 runtime/in-memory/UI；若内存 apply 极端失败，runtime 进入 reopen-required blocking state；
- exact-PID crash-after-Restore-COMMIT / before memory-UI apply 后 reopen 能恢复 durable restored truth；
- future-only marker `FUTURE_ONLY_SECRET_G304` 在 Restore 后从 accepted Conversation 与下一次 Provider messages 中消失；recent-12 与 current-user exactly-once/last 保持成立；
- Context/Prompt 不作为 Save truth，raw opaque World JSON 不直接进入 Prompt；
- historical Timeline Nodes 与其它 Save Points 在 Load 后保留，不实现任意历史 rewind；
- World Surface 提供最小命名 Save、选择、明确 Load confirmation；active generation 中 Save/Load 禁用；
- Restore 后 Narrative UI 从 Conversation projection 全量重绘；
- real DeepSeek post-Restore continuation、Windows export、launcher smoke 与 G3/G2 regressions均已有 Agent evidence。

G3-04 未实现 automatic pre-Load recovery checkpoint、恢复未显式保存的旧 current future、Timeline browser、任意 Turn rewind、Save rename/delete/overwrite manager、G4/G5/G7。

---

## 6. Owner UAT｜CURRENT

只验证真实产品价值，不要求查看日志/数据库：

```text
run-game.cmd
→ 玩出一个容易记住的剧情节点
→ 在右侧“世界”区域创建命名 Save Point S1
→ 再继续玩 1–2 Turn，明确制造一个只有未来才知道的信息/事件
→ 读取 S1，并确认 Load confirmation 文案清楚
→ Narrative 回到 S1 时刻，未来 Turn 不再出现
→ 再输入一个新行动/问题
→ 确认 AI 自然从 S1 继续，且不泄漏刚才 future-only 信息
→ 正常退出并重新打开
→ 确认仍停留在 Restore 后的新当前进度
```

PASS 关注：

- Save/Load 操作是否直观，选中的 Save 是否明确；
- 读取后 World/Narrative 直观上确实回到目标进度；
- 被回滚未来没有残留、重复或泄漏；
- 下一 Turn 的 AI 不知道未来信息；
- 退出重开后仍保持 Restore 后状态；
- 没有空白局、半恢复、明显卡死或错乱。

Owner UAT PASS 前不得开始 G3-05。

---

## 7. 当前核心约束

- `Model authors the world; Runtime makes it durable; Player owns the timeline.`
- `Save Point != Timeline Node.`
- `Reversibility != frictionless arbitrary rewind.`
- 局部错误低成本纠正；重大历史恢复必须表达明确玩家意图。
- UI / Transcript / Markdown / Godot Resource 不得成为 authoritative gameplay DB。
- Context 是 derived/rebuildable material，不是另一份存档。
- Restore 后 future-memory isolation 是 G3-04 blocking acceptance。

---

## 8. 当前 waiting

```text
Blocking: Owner UAT not yet completed
Current: G3-04 Owner UAT
G3-05: HOLD
Next after Owner UAT PASS: G3-05 Recovery / Timeline Foundation
```
