---
title: my world｜当前状态
status: current-project-status
version: 2.1
created: 2026-08-26
updated: 2026-08-26
phase: G3 Persistent Game / Save / Timeline Foundation
current_task: G3-01 Persistence Domain Architecture
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
G2-06 Owner Playtest           PASS — Owner UAT
G2-GATE                        PASS

Current Phase                  G3 — Persistent Game / Save / Timeline Foundation
Current Task                   G3-01 — Persistence Domain Architecture
G3-GATE                        NOT YET
```

---

## 3. G2｜CLOSED / PASS

G2 已完成：

- G2-01 Application / Game Shell — PASS — Owner UAT
- G2-02 Provider Adapter v0.1 — PASS — Engineering
- G2-03 Narrative Conversation View — PASS — Owner UAT
- G2-04 Turn / Conversation Domain v0.1 — PASS — Independent Review
- G2-05 Context Assembly v0.1 — PASS — Independent Review
- G2-06 First Owner Playtest — PASS — Owner UAT

Owner 对真实 exported product path 的结论：**PASS，可以进入下一步。**

因此 G2-GATE 正式通过。当前已证明的产品脊柱：

```text
启动游戏
→ 自然语言输入
→ AI GM real streaming Narrative
→ 连续多回合
→ Cancel / Regenerate / Retry
→ bounded Context Assembly
→ failure 后可继续
```

G2 Gate 只证明 Conversation Spine 值得继续建设；它不声称当前已经拥有持久 World / Save / Timeline / World Pack / NPC/Faction runtime。

---

## 4. Current Phase｜G3 Persistent Game / Save / Timeline Foundation

G3 Outcome：建立长期世界的 durable backbone，让退出 / 重开、Save / Load / Restore、Context future isolation 和 recovery 成为原生能力。

必须持续区分：

```text
Game
World State
Timeline
Save Point
Conversation
Agent Context
UI Preference
```

长期不变量：

- `Model authors the world; Runtime makes it durable; Player owns the timeline.`
- `Save Point != Timeline Node.`
- `Reversibility != frictionless arbitrary rewind.`
- UI / Transcript / Markdown / Godot Resource 不得成为 authoritative gameplay DB。
- Load 旧 Save 不应立即不可逆销毁当前 future。
- Restore 后 Context 不得泄漏被回滚未来。

---

## 5. Current Task｜G3-01 Persistence Domain Architecture

目标不是立刻做 Save UI，而是先用真实 fixture / spike 冻结：

```text
Authoritative ownership
+ durable mutation transaction boundary
+ persistence storage candidate
+ checkpoint / snapshot role
+ migration boundary
+ interrupted-write / recovery boundary
+ future G3 task integration seams
```

当前首选评估候选仍是：

```text
JSON/files
→ settings / small metadata / portable source / non-authoritative cache

SQLite
→ G3 authoritative World / Timeline 首选评估候选

Event Log / Snapshot
→ 可组合的 timeline/recovery semantic pattern
→ 不默认 full event sourcing
```

SQLite 仍是 candidate，不是必须硬上的结论。如果在 Godot 4.7.2 Standard / non-.NET Windows x64 的真实 spike 中证明接入、事务、打包或恢复路径不成熟，G3-01 必须以证据重开存储选择，而不是为了服从旧偏好制造宿主债务。

G3-01 不提前实现 G3-02 Durable World Mutation Path、G3-03 Resume、G3-04 Save/Load/Restore，也不建设任意 Turn rewind。

---

## 6. 当前 waiting

```text
Blocking: NONE KNOWN
Current: prepare / execute G3-01 repository-native architecture + technical-spike Task Packet
Owner UAT: not required for G3-01
Next after G3-01 Independent Review PASS: G3-02 Durable World Mutation Path
```
