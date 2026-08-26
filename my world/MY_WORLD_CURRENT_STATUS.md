---
title: my world｜当前状态
status: current-project-status
version: 1.0
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
G2-GATE                       NOT YET
```

---

## 3. 已关闭 Task

### G2-01｜Application / Game Shell

Implementation:

`my-world@4a13deb29a2e9c354530843d23eb48422957033c`

Owner UAT：**PASS**。

已确认：

- exported `build/windows/my-world.exe` 是正常游戏壳，不是 Foundation 工具；
- resize 可用；
- 正常退出；
- UI 功能整体正常。

Owner observation：视觉仍较粗糙。该项为 **deferred visual polish / non-blocking**，后续可由 KimiCode 在明确 UI polish 任务中处理。

### G2-02｜Provider Adapter v0.1

Implementation:

`my-world@ec0617195cbd71ba49e9c3e4ff834aee83e82fd3`

状态：**ENGINEERING PASS / CLOSED**。

已证明：

- 正式 DeepSeek Provider Adapter v0.1；
- `start_stream / text_delta / completed / cancel / cancelled / failed / is_busy`；
- non-blocking `HTTPClient.poll()` + incremental SSE；
- missing-key 在发网前失败；
- credential-free deterministic failure 可恢复；
- 真实 DeepSeek streaming；
- active cancel 无双终止；
- cancel 后真实请求再次成功；
- product config 收口到 `DEEPSEEK_API_KEY` + optional `MY_WORLD_DEEPSEEK_MODEL`；
- secret / Git hygiene PASS。

TTFT 波动是观察项，不阻塞 G2-03，也不授权建设 telemetry / optimization platform。

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

同时建立固定产品骨架：

```text
Left   = PlayerPanelHost
Center = NarrativeHost
Right  = WorldSurfaceHost
```

当前约束：

- Center Narrative 是视觉 / 交互中心；
- 宽窗口三 Host，窄窗口 Narrative 优先、侧栏折叠 / 隐藏；
- 左右只显示诚实最小空状态，不伪造 Character / World / Timeline 数据；
- 固定手写 Godot UI，不做通用 Declarative Renderer；
- 不做 G2-04 正式 Turn Domain；
- 不做 G2-05 正式 Context Assembly；
- 不做 G3 Persistence / Save / Timeline / arbitrary rewind / branch。

G2-03 是产品面任务。执行 Agent 完成 Engineering Acceptance 后最高状态：

> **READY FOR OWNER UAT**

Product PASS 必须由 Owner 真实运行 Windows 产品路径后确认。

---

## 5. 当前 Reversibility UX

当前只实现：

```text
active generation → Cancel
latest generation → Regenerate / Retry
```

长期产品原则：

> **Reversibility ≠ frictionless arbitrary rewind.**
>
> **Save Point != Timeline Node.**

因此：

- 历史 Narrative 默认用于阅读 / 滚动，不放每条 `回到这里`；
- Save / Load 属于明确玩家意图；
- Timeline 首先是 Runtime history / recovery foundation；
- arbitrary per-turn rewind 当前 **DEFERRED**；
- G3 优先 reliable persistence、resume、explicit Save、explicit Load/Restore、future-memory isolation、误读档 recoverability。

详细设计由 `MY_WORLD_架构_CURRENT.md` 导航到 `architecture/persistence/`。

---

## 6. 当前核心约束

- `Model freedom first. Reversibility over prevention.`
- `Reversibility ≠ frictionless arbitrary rewind.`
- `Model authors the world; Runtime makes it durable; Player owns the timeline.`
- `Save Point != Timeline Node.`
- `Source provides inertia; actors create history.`
- `Context stays bounded.`
- `Host capability first; external asset protocol second.`
- G2 product-facing Provider = DeepSeek `deepseek-v4-pro`；Kimi Code 是 Foundation alternate，不自动 fallback。
- Secret 不进入 Git、日志、UI、截图或聊天。
- Owner 不承担 routine Godot/Git/build/debug/QA；真实产品体验 Gate 才交给 Owner。

---

## 7. 当前 blocker

```text
Blocking: NONE KNOWN
Waiting: G2-03 implementation → READY FOR OWNER UAT
```

文档目录整理属于治理维护，不改变 G2-03 Outcome / Scope；当前 Task Packet 在引用路径完成同步后继续有效。
