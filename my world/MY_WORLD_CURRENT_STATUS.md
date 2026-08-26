---
title: my world｜当前状态
status: current-project-status
version: 1.2
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
G2-03 Implementation          COMMITTED at d736ac9
Independent Review            RETURNED — completed-regenerate history defect
Owner UAT                     NOT READY
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

Implementation commit：

`d736ac9389c2bf23f7f71b0270d6fd8f72db8461 — G2-03: add narrative conversation view`

该实现之后的 `my-world/main` 增量仅修改 `AGENTS.md`、`README.md`、`docs/CORE_DESIGN_PRINCIPLES.md`，未覆盖 G2-03 游戏代码。

执行 Agent Final Report 报告：parse / offline tests / GUI real-DeepSeek tests / cancel / regenerate / second turn / export / run-game / secret checks 均通过，并返回 `READY FOR OWNER UAT`。

### Independent Review Finding｜IR-01 — BLOCKING

静态 Review 对照 Task Packet AC-07 / AC-08 发现：

```text
completed history
= [user, assistant]

Regenerate completed generation
→ pop assistant
→ history becomes [user]
→ start new generation
→ on completed() unconditionally append user + assistant
→ history becomes [user, user, assistant]
```

因此“**completed GM generation → Regenerate**”会重复写入同一个 player turn，违反：

- AC-07：regenerate 不得重复制造第二个 player entry；
- AC-08：旧 GM 与新 GM 不得形成错误 active context / provisional history。

现有 GUI test 的 Regenerate 路径发生在 Cancel 之后，当时 history 为空，所以无法覆盖这个缺陷；其后直接发送 second turn，也没有测试“completed → regenerate”。

### Required Repair

执行 Agent必须做最小修复，不扩张到 G2-04 Domain：

1. completed-generation regenerate 后，provisional history 必须仍恰好为同一个 `user + new assistant` 对；
2. 不得重复 user；
3. 新 Provider request context 只含一次该 user；
4. 新增 focused regression test，明确覆盖：

```text
first turn completed
→ regenerate completed turn
→ history size == 2
→ roles == [user, assistant]
→ same player input appears exactly once
→ second turn completed
→ history size == 4
```

5. 重新运行相关 offline / GUI tests、parse、secret/git hygiene；若修复触及真实 Provider path，重新证明 real regenerate；
6. 新 commit + push 后重新提交 Final Report。

修复前：

> **NOT READY FOR OWNER UAT**

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
Blocking: IR-01 completed-generation Regenerate duplicates player history entry
Waiting: KimiCode K3 focused repair + regression evidence + revised Final Report
Owner UAT: HOLD
```

该返修只属于 G2-03 provisional session correctness，不授权 G2-04 / G2-05 / G3 实现。
