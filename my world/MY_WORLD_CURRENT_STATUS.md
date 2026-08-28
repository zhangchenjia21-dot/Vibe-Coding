---
title: my world｜当前状态
status: current-project-status
version: 3.1
created: 2026-08-26
updated: 2026-08-28
phase: G3 Persistent Game / Save / Timeline Foundation
current_task: G3-06 Crash / Interrupted Write Recovery — READY FOR OWNER UAT
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
G3-06 Crash / Write Recovery   ENGINEERING PASS — READY FOR OWNER UAT
G3-GATE                        NOT YET
```

---

## 3. G3-01..G3-05｜CLOSED

已接受并逐步验证第一代 persistence route：

```text
SQLite
+ 2shady4u/godot-sqlite v4.9 GDExtension
+ Godot 4.7.2 Standard / non-.NET Windows x64
+ GDScript / same-process Runtime
```

G3-03、G3-04、G3-05 均已通过 Independent Review + Owner UAT。

已冻结：current Game resume、persist-before-accept、immutable named Save Point、atomic World/head/Conversation Restore、future-memory isolation、Recovery Checkpoint、reciprocal Recover、historical Timeline retention / internal branch correctness。

---

## 4. G3-06｜ENGINEERING PASS / READY FOR OWNER UAT

实现 commit：

```text
7e2e622f03782a1d66f5f8837d739f900615b775  G3-06 crash / interrupted-write recovery hardening
```

Independent Review：**PASS — no engineering blocker found**。

已证明：

- dedicated sibling SQLite coordination DB 使用 process-lifetime `BEGIN IMMEDIATE` 实现 single-writer；第二 product process 在 gameplay DB mutation/migration 前 fail-fast，首实例 normal exit / exact-PID crash 后锁由 SQLite/Windows 自动释放；不依赖裸 PID / wall-clock lease；
- gameplay DB 不持有 lifetime transaction；
- production physical backup 使用 godot-sqlite v4.9 `SQLite.backup_to(path)`，replacement staging 使用 `SQLite.restore_from(path)`；不 ordinary-copy open WAL DB；
- `latest.sqlite` / `previous.sqlite` / `backup-staging.sqlite` staged publication：staging 先完成 SQLite open、quick_check、foreign_key_check、schema、current truth 与 JSON structural verification 后才发布；
- first READY 建立初始 verified backup；明确 player Save 成功后刷新；graceful close 尽力刷新；backup refresh failure 不撤销已 committed Save，UI 返回准确 warning，旧 verified backup 保留；
- existing schema migration 前 verified pre-migration backup 是 blocking gate；backup creation/verification failure 时 migration 不开始；intentional migration failure 后 current old schema + prebackup 均保持有效；
- startup 区分 normal missing、already running、physical corruption、unsupported newer schema、logical invalid 与 ordinary storage failure；physical corruption 不会变成 first-run blank Game；
- disaster recovery 只从 verified whole-DB generation 恢复：先构造并验证 replacement staging，再 quarantine corrupt current，最后 publish replacement；成功后进入 reopen-required，不让旧 Runtime 继续；
- invalid latest 可 fallback verified previous；无 verified backup 时 fail-loud 且不创建空局；
- backup staging/rotation、quarantine/replacement publication 等 exact-PID interruption 后仍至少保留 current-corrupt / verified backup / staged replacement 中的安全可重试组合；
- normal SQLite pre-COMMIT / post-COMMIT crash 继续由 transaction/WAL/replay 语义恢复，不误触 physical-corruption UX；
- Windows Desktop exported EXE 已验证 single-instance、crash-release、staged recovery 与 coherent reopen；
- G3-05..G3-01 与 G2 核心离线/进程/UI回归通过。

本轮额外尝试的既有 G3-05 real-provider continuation 返回 `transport`，因此没有冒充 real-provider PASS。G3-06 SQLite/single-writer/backup/recovery Engineering Acceptance 不依赖真实 Provider；G3-07 Persistence Reality Test 必须重新验证真实 Provider continuation。

G3-06 未实现 cloud sync、backup/history browser、manual import/export platform、backup encryption、Timeline browser、G4/G5/G7。

---

## 5. Owner UAT｜CURRENT

必须使用 implementation repository 提供的隔离 fixture，不破坏真实 `user://my-world/current-game.sqlite`：

```text
PowerShell:
& 'tests\g3_06\启动隔离Owner_UAT.ps1'

→ real product UI 打开 task-owned corrupted current DB
→ 明确看到“当前数据损坏 / 可恢复安全备份 / 备份后进度可能丢失 / 损坏原件保留”
→ 点击“恢复最近安全备份”
→ 阅读二次确认并确认
→ 应进入“恢复完成，需要重新打开”状态

随后：
& 'tests\g3_06\启动隔离Owner_UAT.ps1' -Reopen

→ recovered fixture 正常打开
→ Narrative / Save / Recovery 状态与 backup generation 一致
→ 可以继续正常使用
```

PASS 关注：

- 灾难恢复文案是否清楚、保守，不像普通 Save/Load；
- 不需要玩家找 `.sqlite`、跑 SQL 或理解 WAL；
- 恢复后不是空白局、半历史或混合 generations；
- reopen 后产品路径正常；
- 整个 UAT 明确只作用于 `build/g3_06_owner_uat`。

Owner UAT PASS 前不得进入 G3-07。

---

## 6. 当前核心约束

- `Model authors the world; Runtime makes it durable; Player owns the timeline.`
- `Save Point != Timeline Node.`
- `Recovery Checkpoint != Save Point.`
- `Physical Backup != Save Point / Recovery Checkpoint.`
- authoritative current DB 是唯一 live truth；backup/quarantine/staging 只服务恢复。
- physical integrity failure / ambiguous concurrent writer 必须 fail-loud，不能静默开新局或继续写。
- migration / backup / restore recovery 都必须保留明确原子边界和可重试性。
- UI / Transcript / Prompt / Cache 不参与灾难恢复 authoritative reconstruction。

---

## 7. 当前 waiting

```text
Blocking: Owner UAT not yet completed
Current: G3-06 Owner UAT
G3-07: HOLD
Next after G3-06 PASS: G3-07 Persistence Reality Test
Next implementation agent: do not assume Codex; user authorized Grok Build or KimiCode due current Codex quota exhaustion
```
