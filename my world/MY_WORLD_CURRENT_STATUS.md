---
title: my world｜当前状态
status: current-project-status
version: 1.1
created: 2026-08-26
updated: 2026-08-26
phase: G2 AI Conversation Spine
current_task: G2-03 Narrative Conversation View
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜CURRENT STATUS

## 1. 文档职责

本文件只拥有**当前执行状态**：Current Phase、Current Task、已完成 Task 的 Gate 状态、Owner UAT 结论、当前 blocker 与不阻塞主链的观察项。

本路径跨阶段长期固定，不再为 G3 / G4 / G5 分别新建阶段状态文件。

其它事实 owner：

- 产品目的：`MY_WORLD_项目启动总纲_CURRENT.md`
- 跨阶段原则：`MY_WORLD_核心设计原则_CURRENT.md`
- 当前架构地图与专题导航：`MY_WORLD_架构_CURRENT.md`
- 阶段 / Task DAG：`MY_WORLD_总体规划路线图_CURRENT.md`

---

## 2. 当前状态

```text
Current Phase                 G2 — AI Conversation Spine
G2-01 Application/Game Shell PASS — Owner UAT
G2-02 Provider Adapter v0.1  PASS — Engineering
Current Task                  G2-03 — Narrative Conversation View
G2-03 Implementation          COMMITTED — awaiting closeout / Owner UAT
G2-GATE                       NOT YET
```

---

## 3. 已关闭 Task

### G2-01｜Application / Game Shell

Implementation: `my-world@4a13deb29a2e9c354530843d23eb48422957033c`

Owner UAT：**PASS**。

Owner observation：功能整体正常但视觉较粗糙，记录为 **deferred visual polish / non-blocking**。

### G2-02｜Provider Adapter v0.1

Implementation: `my-world@ec0617195cbd71ba49e9c3e4ff834aee83e82fd3`

状态：**ENGINEERING PASS / CLOSED**。

已证明正式 DeepSeek thin adapter、real stream、cancel、post-cancel recovery、missing-key/transport failure、secret/config separation 与 Shell regression safety。

TTFT 波动继续作为观察项，不阻塞当前主链，也不授权建设 telemetry platform。

---

## 4. Current Task｜G2-03 Narrative Conversation View

目标玩家路径：

```text
打开游戏
→ 在 Narrative Host 输入自然语言行动
→ DeepSeek 真正流式生成 GM Narrative
→ 玩家可 Cancel
→ 最近一次 generation 可 Regenerate / Retry
→ 错误后继续使用
```

固定产品骨架：

```text
Left   = PlayerPanelHost
Center = NarrativeHost
Right  = WorldSurfaceHost
```

当前约束：

- Center Narrative 是视觉 / 交互中心；
- 宽窗口三 Host，窄窗口 Narrative 优先；
- 左右只显示诚实最小空状态，不伪造 Character / World / Timeline 数据；
- fixed handwritten Godot UI，不做 generalized Declarative Renderer；
- 不做 G2-04 Turn Domain、G2-05 Context Assembly；
- 不做 G3 Persistence / Save / Timeline / arbitrary rewind / branch。

### 当前实现事实

2026-08-26 `my-world/main` 已出现 implementation commit：

`d736ac9389c2bf23f7f71b0270d6fd8f72db8461 — G2-03: add narrative conversation view`

可见增量包括 Narrative View、玩家 `run-game` 启动路径与窗口基线调整。该 commit 的存在只表示实现已提交；**在执行 Agent Final Report / Engineering Evidence 与 Owner UAT 明确关闭前，G2-03 仍是 Current Task，不得自动标记 Product PASS 或推进 G2-04。**

G2-03 是产品面任务。最高 pre-UAT 状态：

> **READY FOR OWNER UAT**

---

## 5. 当前 Reversibility UX

当前只需要：

```text
active generation → Cancel
latest generation → Regenerate / Retry
```

长期原则：

> **Reversibility != frictionless arbitrary rewind.**
>
> **Save Point != Timeline Node.**

因此：

- 历史 Narrative 默认 read / scroll，不为每条放 `回到这里`；
- Save / Load 是明确玩家意图；
- Timeline 首先是 Runtime history / recovery foundation；
- arbitrary per-turn rewind 当前 **DEFERRED**；
- G3 优先 reliable persistence、resume、explicit Save、explicit Load/Restore、future-memory isolation 与误读档 recoverability。

详细设计从 `MY_WORLD_架构_CURRENT.md` → `architecture/persistence/`。

---

## 6. 当前核心约束

- `Model freedom first. Reversibility over prevention.`
- `Reversibility != frictionless arbitrary rewind.`
- `Model authors the world; Runtime makes it durable; Player owns the timeline.`
- `Save Point != Timeline Node.`
- `Source provides inertia; actors create history.`
- `Context stays bounded.`
- `Host capability first; external asset protocol second.`
- G2 product-facing Provider = DeepSeek `deepseek-v4-pro`；Kimi Code 是 Foundation alternate，不自动 fallback。
- Secret 不进入 Git、日志、UI、截图或聊天。
- Owner 不承担 routine Godot/Git/build/debug/QA；真实产品体验 Gate 才交给 Owner。

---

## 7. 当前 blocker / waiting

```text
Blocking: NONE KNOWN
Waiting: G2-03 Final Report / engineering review → Owner UAT
```

文档目录整理不改变 G2-03 Outcome / Scope，也不要求已提交实现返工。
