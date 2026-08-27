---
title: my world｜当前状态
status: current-project-status
version: 2.6
created: 2026-08-26
updated: 2026-08-27
phase: G3 Persistent Game / Save / Timeline Foundation
current_task: G3-04 Explicit Save / Load / Restore + Context Rebuild
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
Current Task                   G3-04 — Explicit Save / Load / Restore + Context Rebuild
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

Owner 已真实验证：

```text
run-game.cmd
→ 连续真实 Turn
→ 正常退出
→ 再次启动
→ 旧 Narrative 恢复
→ 继续下一 Turn
```

产品结论：顺序重启后直观上仍是同一局 Game，Narrative continuity / resumed Context 主路径成立。

已冻结 G3-03 事实：

- production schema v2；
- one-current-Game runtime composition；
- current accepted Conversation durable materialization；
- prospective candidate → durable COMMIT → Domain accepted；
- New / Regenerate / Correction 写失败不会产生 accepted/durable silent divergence；
- only accepted truth resumes；
- Context / Provider messages 不持久化为 truth；
- existing corrupt/schema/ambiguous state fail-loud，不创建替代空局。

Non-blocking follow-up：双开两个产品进程同时写 current Game 的保护仍需在 G3-06 / standalone hardening 前冻结。

---

## 5. Current Task｜G3-04 Explicit Save / Load / Restore + Context Rebuild

Outcome：第一次提供玩家明确可操作的长期恢复点，并证明 Restore 后 World、Conversation 与 AI working context 同时回到目标历史状态：

```text
当前进度
→ 创建命名 Save Point S1
→ 继续产生后续 World / Conversation future
→ 明确选择并 Load S1
→ current World 回到 S1 的 Timeline anchor
→ accepted Conversation 回到 S1 snapshot
→ Context 从 restored truth 重建
→ 被回滚未来不进入下一次 Provider request
→ 从恢复点继续新的当前进度
```

第一代边界：

- `Save Point != Timeline Node`；Save 是玩家命名恢复引用，不复制整份 World DB；
- World restore 使用已有 immutable Timeline snapshot；Save 还必须携带该时刻的 accepted Conversation recovery material，否则 future-memory isolation 不成立；
- Restore 的 World/head/current Conversation durable change 必须是一个原子边界；COMMIT 后才让 in-memory Conversation/UI 切换；
- Context / Prompt 仍然 derived/rebuildable，不作为 Save payload truth；
- Load 是高影响明确意图，UI 必须清楚显示正在读取哪个 Save；active generation 中不得静默 Restore；
- 历史 Timeline Node 不因 Load 物理删除；但“未显式保存的 pre-Load current future 自动恢复”属于紧接着的 G3-05，不在 G3-04 宣称完成；
- arbitrary per-Turn rewind / Timeline debugger 继续 Deferred；
- 不提前实现 G4/G5/G7。

---

## 6. 当前核心约束

- `Model authors the world; Runtime makes it durable; Player owns the timeline.`
- `Save Point != Timeline Node.`
- `Reversibility != frictionless arbitrary rewind.`
- 局部错误低成本纠正；重大历史恢复必须表达明确玩家意图。
- UI / Transcript / Markdown / Godot Resource 不得成为 authoritative gameplay DB。
- Context 是 derived/rebuildable material，不是另一份存档。
- Restore 后 future-memory isolation 是 G3-04 的 blocking acceptance，而不是后续 polish。

---

## 7. 当前 waiting

```text
Blocking: NONE KNOWN
Current: G3-04 repository-native Task Packet / implementation
Owner UAT: required after Engineering + Independent Review
G3-05: HOLD
Next after G3-04 PASS: G3-05 Recovery / Timeline Foundation
```
