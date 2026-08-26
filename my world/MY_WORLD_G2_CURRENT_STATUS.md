---
title: my world｜G2 当前状态
status: current-stage-status
version: 1.2
created: 2026-08-26
updated: 2026-08-26
phase: G2 AI Conversation Spine
current_task: G2-03 Narrative Conversation View
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜G2 当前状态 CURRENT

## 1. 文档职责

本文件只拥有 **G2 当前执行状态**：当前 Task、已完成 Task 的 Gate 状态、Owner UAT 结论和不阻塞主链的观察项。

产品定义仍由 `MY_WORLD_项目启动总纲_CURRENT.md` 拥有；阶段目标与任务 DAG 仍由 `MY_WORLD_总体规划路线图_CURRENT.md` 拥有；核心产品 / Runtime 原则由 `MY_WORLD_核心设计原则_CURRENT.md` 拥有；G2-03 以后涉及产品 UI Host 的设计由 `MY_WORLD_声明式UIHost架构_CURRENT.md` 拥有。

## 2. 当前状态

```text
Current Phase                 G2 — AI Conversation Spine
G2-01 Application/Game Shell PASS — Owner UAT
G2-02 Provider Adapter v0.1  PASS — Engineering
Current Task                  G2-03 — Narrative Conversation View
G2-GATE                       NOT YET
```

## 3. G2-01 Closeout

Implementation commit:

`my-world@4a13deb29a2e9c354530843d23eb48422957033c`

Owner UAT on 2026-08-26:

- exported `build/windows/my-world.exe` opens as a normal game shell rather than a Foundation test tool;
- window resize remains usable;
- normal exit works;
- overall result accepted as **PASS**.

Owner observation:

> 当前 UI 功能和布局整体正常，但视觉仍较粗糙。

该观察为 **deferred visual polish / non-blocking**。不得为了美化 UI 阻塞 G2 Conversation Spine；后续适合交给 KimiCode 在有明确 UI polish 任务时处理。

## 4. G2-02 Closeout

Implementation commit:

`my-world@ec0617195cbd71ba49e9c3e4ff834aee83e82fd3`

G2-02 在 Task Packet 定义下属于支撑性工程任务；全部 Engineering Acceptance 满足后可直接关闭，不需要额外 Owner UAT。

2026-08-26 KimiCode K3 Final Report + repository audit 证明：

- 正式 DeepSeek Provider Adapter v0.1 已建立，公开 seam 为 `start_stream / text_delta / completed / cancel / cancelled / failed / is_busy`；
- Godot `HTTPClient` 使用 non-blocking `poll()` + incremental SSE；
- missing-key 在联网前明确失败；
- credential-free deterministic transport failure 明确失败且可恢复；
- 真实 DeepSeek streaming 成功并产生多个 text delta；
- active generation cancel 后无双终止，随后真实请求再次成功；
- G2 product config 已收口为 `DEEPSEEK_API_KEY` + optional `MY_WORLD_DEEPSEEK_MODEL`；
- G1-04 Kimi Code product config 已从当前启动路径退休；
- G2-01 Shell 无回归；
- secret / Git hygiene PASS。

因此：

> **G2-02 = ENGINEERING PASS / CLOSED。**

非阻塞性能观察：真实 Provider 的 TTFT 波动仍可能较大；继续记录 TTFT 与 generation throughput，但不得在 G2-03 为此提前建设 telemetry/optimization platform。

## 5. G2-03 Current Boundary

G2-03 是第一个真正改变玩家主路径的 G2 产品任务。

目标体验：

```text
打开游戏
→ 在正式 Narrative Host 输入自然语言行动
→ DeepSeek 真正流式生成 AI GM Narrative
→ 玩家可 Cancel
→ 最近一次生成可 Regenerate / Retry
→ 错误后仍可继续
```

同时必须把当前 Game Shell 重构为稳定三 Host Slot：

```text
Left   = PlayerPanelHost
Center = NarrativeHost
Right  = WorldSurfaceHost
```

边界：

- Center Narrative 永远是视觉和交互重心；
- 宽窗口显示三 Host，窄窗口优先保留 Narrative，并折叠/隐藏侧 Host；
- 左右 Host 当前只能显示诚实的最小空状态 / placeholder，不得伪造 Character / World / Timeline 数据或无行为 Tab；
- 当前固定手写 Godot UI，不做通用 Declarative Renderer；
- 不做外部 World Pack / Mod UI schema；
- 不做 G2-04 正式 Turn / Conversation Domain；
- 不做 G2-05 正式 Context Assembly；
- 不做 G3 Persistence / Save / Timeline / rewind / branch。

G2-03 可以使用**明确标注为 provisional 的最小 in-memory UI/session state 和最小 GM system message**完成真实垂直体验；这些不是正式 Conversation Domain 或 Context Contract，后续必须由 G2-04 / G2-05 接管。

## 6. Reversibility / Narrative UX

当前 G2-03 只建立最早的一层玩家可逆性：

```text
active generation → Cancel
latest generation → Regenerate / Retry
```

推荐 Turn footer：最近完成/取消/失败的 GM 内容下方提供轻量 `重新生成` 操作；不要永久给每条历史内容铺满大按钮。

`回到这里 / edit-and-retry / branch / Timeline navigation` 归 G3 后续能力，不在 G2-03 模拟假实现。

正式产品原则继续为：

> **Model freedom first. Reversibility over prevention.**

不要为了当前 UI 建 Narrative whitelist、Regex 授权、Confirmation 或世界一致性 Validator。

## 7. Owner UAT Requirement

G2-03 是直接改变核心用户路径的产品面任务。

执行 Agent 完成工程验收后的最高状态：

> **READY FOR OWNER UAT**

Owner UAT 应使用可直接运行的 Windows 产品路径，不要求 Owner 打开 Godot Editor、运行 Git、PowerShell 调试或执行工程测试。

Owner 重点判断：

- 第一眼是否开始像一个 AI RPG，而不是工程测试工具 / 普通聊天客户端；
- Narrative 是否真正流式出现且阅读自然；
- 输入、Cancel、重新生成是否直观；
- 三栏/折叠布局是否让 Narrative 保持主角；
- 整体是否值得继续进入 G2-04 / G2-05。

## 8. Current Core Constraints

- `Model freedom first. Reversibility over prevention.`
- `Model authors the world; Runtime makes it durable; Player owns the timeline.`
- `Host capability first; external asset protocol second.`
- G2 product-facing Provider = DeepSeek `deepseek-v4-pro`；Kimi Code 仍只是 Foundation alternate。
- Secret 只来自本地受保护来源；不得进入 Git、日志、UI、截图或聊天。
- Owner 不承担 routine Godot/Git/build/debug/QA；真实产品体验 Gate 才交给 Owner。
