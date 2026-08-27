---
title: my world｜当前状态
status: current-project-status
version: 2.5
created: 2026-08-26
updated: 2026-08-27
phase: G3 Persistent Game / Save / Timeline Foundation
current_task: G3-03 Game Reopen / Resume — READY FOR OWNER UAT
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
G3-03 Game Reopen / Resume     ENGINEERING PASS — READY FOR OWNER UAT
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

Independent Review：**PASS**。

已冻结：atomic World/Timeline/head transaction、expected-head CAS、replay-safe mutation identity、immutable recovery anchors、physical query failure fail-loud 与 successful zero-row 分离。

---

## 5. G3-03｜ENGINEERING PASS / READY FOR OWNER UAT

实现 commit：

```text
929f4ff1e1253a808522d8f559a3cadd01b8d5db  G3-03 current Game reopen / resume
```

Independent Review：**PASS — no engineering blocker found**。

已证明：

- production schema v1 → v2 additive transactional migration；intentional mid-migration failure rollback 保持 v1 Game/World/Timeline 数据与 version；
- current-only `conversation_materializations` 保存有序 accepted Conversation，不保存 Provider messages / Context / partial stream；
- Application runtime 接管 current Game lifecycle，Narrative UI 不再在正常产品路径自建独立 Conversation truth；
- first run 只在 DB 文件真实不存在时创建一个稳定 Game/root identity；existing zero/multi/corrupt/unsupported state 均 fail-loud，不生成替代空白新局；
- Provider completion 采用 prospective candidate → durable Conversation COMMIT → Domain accepted 的 persist-before-accept 顺序；
- New / Regenerate / Correction 的 durable write failure 均保持旧 Domain + DB accepted truth，并进入 retryable persistence failure；
- cancelled / failed / streaming-only partial attempt 不在 reopen 后成为 accepted history；
- 两个独立 OS process 可恢复 exact same Game/head/World/Conversation 并继续下一 Turn；
- 14-Turn reopen 后 Context 仍从 restored Conversation 按 recent-12 规则重建，不读取 stored Prompt，也不把 opaque World JSON 直接塞入 Prompt；
- restored Narrative UI 从 Conversation projection 重绘，latest Regenerate 复用同一 logical Turn/GM block；
- real DeepSeek resumed-history GUI、Windows export、两次 launcher reopen 均已有 Agent evidence。

### Non-blocking follow-up risk

当前 G3-03 验证的是**顺序重启/恢复**。one-current-Game DB 尚未专门冻结“双开两个产品进程同时写 current Conversation”的互斥或 revision-CAS 语义。该问题不阻塞 G3-03 Owner UAT，但进入 G3-06 / standalone hardening 前必须明确选择 single-instance lock、stale-session rejection 或等价最小保护，避免并发实例 last-writer-wins。

---

## 6. Owner UAT｜CURRENT

只验证真实产品价值，不要求查看日志/数据库：

```text
run-game.cmd
→ 连续完成 2–3 个真实 Turn
→ 记住最后剧情
→ 正常退出
→ 再次 run-game.cmd
→ 确认旧 Narrative 完整恢复且顺序正确
→ 再发一个新行动
→ 确认 AI 自然承接重启前剧情
→ 可选：对重启前最后一个 Turn 做一次 Regenerate
```

PASS 关注：

- 直观上确实是“同一局游戏”；
- 旧 Narrative 没有丢失、重复或乱序；
- 下一 Turn 自然承接；
- 没有出现空白替代新局、明显卡死或旧/新内容错乱。

Owner UAT PASS 前不得开始 G3-04。

---

## 7. 当前核心约束

- `Model authors the world; Runtime makes it durable; Player owns the timeline.`
- `Save Point != Timeline Node.`
- `Reversibility != frictionless arbitrary rewind.`
- UI / Transcript / Markdown / Godot Resource 不得成为 authoritative gameplay DB。
- Persistence physical failure 必须 fail-loud；正常 absence 与 storage failure 不得混淆。
- Context 是 derived/rebuildable material，不是另一份存档。

---

## 8. 当前 waiting

```text
Blocking: Owner UAT not yet completed
Current: G3-03 Owner UAT
G3-04: HOLD
Next after Owner UAT PASS: G3-04 Explicit Save / Load / Restore + Context Rebuild
```
