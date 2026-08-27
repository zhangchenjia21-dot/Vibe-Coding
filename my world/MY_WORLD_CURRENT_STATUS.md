---
title: my world｜当前状态
status: current-project-status
version: 2.4
created: 2026-08-26
updated: 2026-08-27
phase: G3 Persistent Game / Save / Timeline Foundation
current_task: G3-03 Game Reopen / Resume
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
Current Task                   G3-03 — Game Reopen / Resume
G3-GATE                        NOT YET
```

---

## 3. G3-01｜CLOSED / PASS

已接受第一代 persistence route：

```text
SQLite
+ 2shady4u/godot-sqlite v4.9 GDExtension
+ Godot 4.7.2 Standard / non-.NET Windows x64
+ GDScript / same-process Runtime
```

正式 ownership：Game/World/Timeline-Save/Conversation 各自拥有业务语义；Persistence 只拥有 SQLite durable representation、transaction、schema/migration、backup/recovery mechanics。

---

## 4. G3-02｜CLOSED / PASS

实现线：

```text
bda2a8877297c51365cd6581536875b68c81cb85  production durable World mutation kernel
ee768ca6ec8abdb2d65c994da4e7287886153bff  IR-01 query failure propagation repair
```

Independent Review 最终结论：**PASS**。

已证明并冻结的 production persistence kernel：

- production schema 与 G3-01 fixture 分离；
- current World 是唯一 writable live materialization；Timeline snapshot 是 immutable recovery anchor；
- initial Game / root anchor 创建为原子事务；
- World + Timeline Node + active head 同一 SQLite transaction；
- expected-head stale writer rejection；
- stable mutation identity、exact replay、conflicting mutation-id reuse；
- late-step SQL failure 全量 rollback；
- post-COMMIT lost-ACK exact-PID replay 不重复 node/effect；
- opaque JSON World materialization，不提前冻结 G5 NPC/Faction/Item schema；
- physical query failure 与 successful zero-row 明确区分：storage failure fail-loud，正常 absence 才是 `not_found`；
- L3 read API 不以空结果或 runtime error 隐藏 SQLite failure。

G3-02 没有实现 Resume、Conversation persistence、Save/Load/Restore、Timeline browser 或 G5 World semantics。

---

## 5. Current Task｜G3-03 Game Reopen / Resume

Outcome：让真实产品路径第一次拥有跨进程的同一 Game continuity：

```text
启动 / 首次建立 Game
→ 连续 accepted Conversation
→ accepted truth durable
→ 正常退出
→ 重新启动
→ 恢复同一 Game identity
→ 恢复 current World materialization
→ 恢复 accepted Conversation
→ Context 从 restored truth 重建
→ 可以继续下一 Turn
```

第一代边界：

- 只恢复 accepted Conversation truth；streaming draft、cancelled/failed partial attempt 不作为 accepted resume truth；
- Agent Context / Provider messages 不持久化为 truth，启动后从 restored Conversation + 当前可用 Game material 重建；
- 当前 production 还没有正式 World semantic context renderer，因此不得把 opaque World JSON 直接倾倒进 Prompt 充当新语义；
- Conversation Domain 继续拥有 accepted Turn semantics；Persistence 只保存 durable representation；
- accepted Conversation durability 必须 fail-loud，不能出现“UI 已宣布成功但 durable write 失败仍继续玩”的静默分叉；
- G3-03 只解决 current resume，不实现旧 Save Restore / historical Conversation visibility / future-memory isolation；这些属于 G3-04；
- 不提前做多 Game picker / World Pack / G5 schema / arbitrary Timeline rewind。

---

## 6. 当前核心约束

- `Model authors the world; Runtime makes it durable; Player owns the timeline.`
- `Save Point != Timeline Node.`
- `Reversibility != frictionless arbitrary rewind.`
- UI / Transcript / Markdown / Godot Resource 不得成为 authoritative gameplay DB。
- Persistence physical failure 必须 fail-loud；正常 absence 与 storage failure 不得混淆。
- Context 是 derived/rebuildable material，不是另一份存档。

---

## 7. 当前 waiting

```text
Blocking: NONE KNOWN
Current: prepare / execute G3-03 repository-native Task Packet
Owner UAT: required after Engineering + Independent Review, because restart/resume is now a real product path
Next after G3-03 PASS: G3-04 Explicit Save / Load / Restore + Context Rebuild
```
