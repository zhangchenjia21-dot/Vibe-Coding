---
title: my world｜当前状态
status: current-project-status
version: 2.3
created: 2026-08-26
updated: 2026-08-27
phase: G3 Persistent Game / Save / Timeline Foundation
current_task: G3-02 Durable World Mutation Path — IR-01 focused repair
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
G3-02 Durable World Mutation   RETURNED — IR-01 focused repair
G3-GATE                        NOT YET
```

---

## 3. G3-01｜CLOSED / PASS

实现 commit：

```text
1fc1cba76ade63a05e4b7ba9009264696ad45b1a  G3-01 SQLite persistence architecture spike
```

Independent Review：**PASS**。

已接受的第一代 persistence route：

```text
SQLite
+ 2shady4u/godot-sqlite v4.9 GDExtension
+ Godot 4.7.2 Standard / non-.NET Windows x64
+ GDScript / same-process Runtime
```

真实 spike 已证明 open/reopen、parameter binding、COMMIT/ROLLBACK、pre-COMMIT crash/reopen、transactional migration failure rollback、corrupt fail-loud、Timeline/Save/recovery reference 关系和 Windows exported EXE packaging。

正式 ownership：Game/World/Timeline-Save/Conversation 各自拥有业务语义；Persistence 只拥有 SQLite durable representation、transaction、schema/migration、backup/recovery mechanics。

---

## 4. G3-02｜Independent Review RETURNED

实现 commit：

```text
bda2a8877297c51365cd6581536875b68c81cb85  G3-02 production durable World mutation kernel
```

已通过且无需重做：

- production schema 与 G3-01 fixture 分离；
- current World 是唯一 writable live materialization，Timeline snapshot 是 immutable recovery anchor；
- Game + World + Timeline Node + active head 同一 SQLite transaction；
- expected-head stale writer rejection；
- stable mutation identity、exact replay、conflicting reuse；
- late-step SQL failure 全量 rollback；
- post-COMMIT lost-ACK exact-PID replay 不重复 effect/node；
- opaque JSON World materialization，不提前冻结 G5 NPC/Faction/Item schema；
- 无 ORM/DI/EventBus/full event sourcing/G3-03+ 越界。

### IR-01｜BLOCKING — Query failure propagation

当前 L1 `query_rows()` 对两种完全不同的结果都返回空 Array：

```text
SELECT 成功，但 0 rows
SELECT/SQLite 执行失败
```

L2 因此可能：

- 把真实 storage/query failure 错报成 `not_found`；
- 在 mutation replay/head preflight 中把失败读误解成“没有记录”；
- `timeline_node_count()` 在 query failure 后访问空 rows，产生 runtime error，而不是稳定 `storage_failure`。

这违反 Persistence hard boundary：**physical/storage failure 必须 fail-loud，并与正常业务 absence 区分。** G3-03 Resume 将直接依赖这些 read APIs，因此不能把该缺口带入下一任务。

Focused repair packet：

```text
docs/tasks/G3-02_IR-01_QUERY_FAILURE_PROPAGATION_REPAIR.md
```

repair packet commit：

```text
dd9ac9797a65d241f454f872269748ab44c204f2
```

---

## 5. 当前核心约束

- `Model authors the world; Runtime makes it durable; Player owns the timeline.`
- `Save Point != Timeline Node.`
- `Reversibility != frictionless arbitrary rewind.`
- UI / Transcript / Markdown / Godot Resource 不得成为 authoritative gameplay DB。
- Persistence hard integrity focuses on atomicity / explicit failure / migration / recovery, not Narrative censorship。
- G3-02 IR-01 只修 query success/empty/failure contract；不得借机开始 G3-03+ 或重设计 production schema。

---

## 6. 当前 waiting

```text
Blocking: IR-01 query failure propagation
Current: G3-02 focused repair
Owner UAT: not required
G3-03: HOLD until G3-02 Independent Review PASS
```
