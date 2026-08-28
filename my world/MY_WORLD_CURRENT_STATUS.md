---
title: my world｜当前状态
status: current-project-status
version: 3.0
created: 2026-08-26
updated: 2026-08-28
phase: G3 Persistent Game / Save / Timeline Foundation
current_task: G3-06 Crash / Interrupted Write Recovery
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
G3-05 Recovery / Timeline      PASS — Owner UAT
Current Task                   G3-06 — Crash / Interrupted Write Recovery
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

## 4. G3-03 / G3-04｜CLOSED / PASS — Owner UAT

```text
929f4ff1e1253a808522d8f559a3cadd01b8d5db  G3-03 current Game reopen / resume
618fa0f2238114cbe4fc0fe790a1d60c43e99b45  G3-04 explicit Save / Load / Restore + Context rebuild
```

G3-03、G3-04 均已通过 Independent Review + Owner UAT。

已冻结：current Game cross-process resume、persist-before-accept、immutable named Save Point、atomic World/head/Conversation Restore、COMMIT-before-memory/UI switch、Restore Context rebuild/future-memory isolation，以及 historical Timeline retention。

---

## 5. G3-05｜CLOSED / PASS — Owner UAT

实现 commit：

```text
bf8c35fdf76c4ea3b8ad2560d93c89c2f84c07b0  G3-05 Recovery / Timeline Foundation
```

Independent Review：**PASS**。Owner UAT（2026-08-28）：**PASS**。

Owner 按规定真实产品路径完成 UAT 并返回 PASS。

已冻结 G3-05 事实：

- production schema v4；
- immutable internal `Recovery Checkpoint != Save Point`；
- Load/Recover 均在单一 transaction 内先捕获 displaced current，再原子切换 current World/head/accepted Conversation；
- no-op progress switch 不制造 Recovery；
- reciprocal Recovery 允许最近两个 active futures 安全往返，historical recovery rows 不 consume/delete；
- durable monotonic recovery ordering 不依赖 wall-clock；
- existing immutable Timeline DAG 可自然承载 internal branch，不新增 branch registry/browser；
- Regenerate / Correction 最终 accepted truth 可 exact recovery，partial/cancelled/failed material 不进入 Recovery；
- Recover 后 Context 对另一条 future 保持隔离；
- crash-after-COMMIT / before memory-UI apply 后 reopen 仍以 durable switched state 为准。

---

## 6. Current Task｜G3-06 Crash / Interrupted Write Recovery

Outcome：把已经成立的 durable Game/Save/Recovery 从“正常路径正确”硬化成第一代灾难恢复能力。产品必须在异常退出、物理 DB 损坏或重复启动另一个写进程时，优先保护长期进度，而不是继续运行在歧义或损坏状态。

第一代目标路径：

```text
healthy current Game
→ only one product writer may own it
→ SQLite-native verified recovery backup exists
→ process / write interruption does not create half truth
→ next startup validates current DB before trusting it
→ if current DB is physically corrupt, do not create blank Game
→ if a verified backup exists, offer explicit disaster recovery
→ preserve corrupt original + recover through staged verified copy
→ reopen into one coherent Game/World/Timeline/Save/Recovery/Conversation truth
```

冻结方向：

- `Physical Backup != Save Point != Recovery Checkpoint`；whole-DB backup 只服务灾难恢复，不进入普通 Save/Recovery UI；
- `godot-sqlite v4.9` 已提供 SQLite online `backup_to` / `restore_from` API，production 不直接在打开的 WAL 数据库上做普通文件复制；
- before any schema-changing migration，先建立并验证可恢复 backup；backup 失败则 migration 不开始；
- healthy runtime 至少保留一个 verified last-known-good backup；第一代允许 latest + previous 两代与 staging 文件，具体命名由 G3-06 最小实现冻结；
- backup refresh 必须先生成 staging、验证成功后再发布，失败/崩溃不得摧毁旧 verified backup；
- startup 必须区分 physical corruption、unsupported newer schema、normal absence 与 logical/application failure；不能把所有失败都解释成“用备份覆盖”；
- current DB 物理损坏时必须 fail-loud；只有存在 verified recovery copy 时才暴露明确恢复动作；没有有效 backup 时不得创建空白替代 Game；
- disaster recovery 必须保留/隔离损坏原件，再通过 staged verified copy 恢复；不得一开始就覆盖唯一 current DB；
- double-running 两个 product processes 必须在 G3-06 冻结为 single-writer safety：第二个写实例应在接触 gameplay DB mutation/migration 前被阻止；首实例 crash 后 guard 必须由 OS/SQLite 自动释放，不采用仅靠 wall-clock lease 或裸 PID 文件的脆弱方案；
- 不允许通过在 gameplay DB 上持有整局 lifetime write transaction 来实现 single-instance；
- G3-06 不实现 cloud sync、backup browser、手工导入导出平台、加密备份或 G4/G5/G7。

---

## 7. 当前核心约束

- `Model authors the world; Runtime makes it durable; Player owns the timeline.`
- `Save Point != Timeline Node.`
- `Recovery Checkpoint != Save Point.`
- `Physical Backup != Save Point / Recovery Checkpoint.`
- authoritative current DB 仍是 live truth；backup 只是 verified recovery copy，不得成为并行 fallback truth。
- physical integrity failure / ambiguous concurrent writer 必须 fail-loud，不能静默开新局或继续写。
- migration / backup / restore recovery 都必须保留明确原子边界和可重试性。
- UI / Transcript / Prompt / Cache 不得参与灾难恢复 authoritative reconstruction。
- destructive test 只允许 task-owned isolated DB，不得损坏真实 `user://my-world/current-game.sqlite`。

---

## 8. 当前 waiting

```text
Blocking: NONE KNOWN
Current: G3-06 repository-native Task Packet / implementation
Owner UAT: required after Engineering + Independent Review via isolated recovery fixture
G3-07: HOLD
Next after G3-06 PASS: G3-07 Persistence Reality Test
```
