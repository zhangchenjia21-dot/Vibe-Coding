---
title: my world｜当前状态
status: current-project-status
version: 2.2
created: 2026-08-26
updated: 2026-08-26
phase: G3 Persistent Game / Save / Timeline Foundation
current_task: G3-02 Durable World Mutation Path
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
Current Task                   G3-02 — Durable World Mutation Path
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

真实 spike 已证明：

- SQLite open / schema / parameter binding / close / reopen；
- stable Game / Timeline fixture identity 跨 reopen 保持；
- multi-record transaction COMMIT / explicit ROLLBACK；
- transaction 已写但未 COMMIT 时 exact-PID 终止，reopen 后只有 last committed state；
- schema migration success 与 intentional mid-migration failure rollback；
- missing-path 显式创建与 corrupt DB fail-loud；
- Save Point → Timeline Node reference、旧 future recovery reference 与新 future parent relationship；
- GDExtension 能随 Windows exported main EXE 打包并完成 open/write/reopen/read；
- dependency provenance / MIT license / upstream release digest 已记录并核对。

正式结论：

> **SQLite 从“G3 preferred evaluation candidate”升级为 G3 v0.1 accepted authoritative persistence storage route。**

但本次 `g3_fixture_*` schema、每 mutation 一个 snapshot 等 spike 形态**不是 production schema**，不冻结 G5 NPC/Faction/Item/World 数据结构。

### Independent Review canonicalization clarification

存储层不因为“负责把对象写进 SQLite”就成为所有业务语义的 canonical owner。

正式 ownership 方向：

```text
Game Domain / lifecycle
→ owns Game identity and active-game semantics

World Domain
→ owns game-local authoritative World meaning/state

Timeline / Save Domain
→ owns Timeline Node / Save Point / restore semantics

Conversation Domain
→ owns accepted conversation truth

Persistence
→ owns durable representation, transaction, migration, backup/recovery mechanics
→ does not redefine Game/World/Conversation semantics
```

这只是对 task working note 中 `Persistence Domain owns Game lifecycle` 表述的收口修正；G3-01 spike 代码无需返修。

---

## 4. Current Task｜G3-02 Durable World Mutation Path

Outcome：把 G3-01 已证明的 SQLite/transaction 能力变成第一条正式 production durable mutation path。

目标链：

```text
Game-local World mutation input
→ stable mutation / node identity
→ authoritative current World materialization changes
→ new Timeline Node
→ Game active head changes
→ one SQLite transaction
→ COMMIT 后才 publish success
```

G3-02 只建立最小 Game/World/Timeline persistence kernel，不提前设计完整 NPC/Faction/Item schema，也不实现 Resume、Save/Load UI、Restore product flow 或 arbitrary rewind。

关键边界：

- storage transaction != business semantic owner；
- World mutation 原子提交，不允许 half-new/half-old；
- stale / replayed mutation 必须有明确处理，不能因 crash-after-commit ambiguity 重复制造 Timeline Node；
- Snapshot / checkpoint 不得成为第二 live truth；
- SQLite binding / provenance 继续使用 G3-01 已验证路线；
- Conversation / Context 仍保持 G2 ownership，不在 G3-02 偷做持久化 resume。

---

## 5. 当前核心约束

- `Model authors the world; Runtime makes it durable; Player owns the timeline.`
- `Save Point != Timeline Node.`
- `Reversibility != frictionless arbitrary rewind.`
- UI / Transcript / Markdown / Godot Resource 不得成为 authoritative gameplay DB。
- Persistence hard integrity focuses on atomicity / migration / recovery, not Narrative censorship。
- G3-02 不提前实现 G3-03+ 或 G5 production World schema。

---

## 6. 当前 waiting

```text
Blocking: NONE KNOWN
Current: prepare / execute G3-02 repository-native Task Packet
Owner UAT: not required for G3-02 engineering closeout
Next after G3-02 Independent Review PASS: G3-03 Game Reopen / Resume
```
