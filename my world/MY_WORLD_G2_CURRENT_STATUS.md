---
title: my world｜G2 当前状态
status: current-stage-status
version: 1.3
created: 2026-08-26
updated: 2026-08-26
phase: G2 AI Conversation Spine
current_task: G2-03 Narrative Conversation View
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜G2 当前状态 CURRENT

## 1. 文档职责

本文件只拥有 **G2 当前执行状态**：当前 Task、已完成 Task 的 Gate 状态、Owner UAT 结论和不阻塞主链的观察项。

产品定义仍由 `MY_WORLD_项目启动总纲_CURRENT.md` 拥有；阶段目标与 Task DAG 由 `MY_WORLD_总体规划路线图_CURRENT.md` 拥有；核心产品 / Runtime 原则由 `MY_WORLD_核心设计原则_CURRENT.md` 拥有；G2-03 以后涉及产品 UI Host 的设计由 `MY_WORLD_声明式UIHost架构_CURRENT.md` 拥有；Save / Restore / Timeline / Reversibility 的更具体产品语义由 `MY_WORLD_时间线存档与可逆性架构_CURRENT.md` 拥有。

当较早 current 文档对 Timeline UX 的表述与 `MY_WORLD_时间线存档与可逆性架构_CURRENT.md` 冲突时，以后者的显式 supersession 为准，直到对应文档下一次收口改写。

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
- 不做 G3 Persistence / Save / Timeline / arbitrary rewind / branch。

G2-03 可以使用**明确标注为 provisional 的最小 in-memory UI/session state 和最小 GM system message**完成真实垂直体验；这些不是正式 Conversation Domain 或 Context Contract，后续必须由 G2-04 / G2-05 接管。

## 6. Reversibility / Narrative UX

当前 G2-03 只建立最早的一层玩家可逆性：

```text
active generation → Cancel
latest generation → Regenerate / Retry
```

推荐 Turn footer：最近完成/取消/失败的 GM 内容下方提供轻量 `重新生成` 操作；不要永久给每条历史内容铺满大按钮。

2026-08-26 Product Owner 进一步裁定：

> **Reversibility ≠ frictionless arbitrary rewind.**
>
> **局部错误低成本纠正；重大历史恢复必须表达明确意图。**

因此：

- 历史 Narrative 默认只用于阅读 / 滚动，不放 `回到这里` 一键回档；
- `Player owns the timeline` 不解释为每个历史 Turn 都必须成为可点击 Load Point；
- `Save Point != Timeline Node`；
- G3 优先实现可靠 current game、明确 Save、明确 Load/Restore、future-memory isolation 与误读档后的可恢复性；
- arbitrary per-turn rewind 当前为 **DEFERRED / NOT DEFAULT FIRST-GENERATION PRODUCT BEHAVIOR**；
- 右侧未来可以有 Save / Timeline Surface，但第一代优先服务“保存重要进度 / 明确读取存档”，不是把内部 Timeline 当调试器暴露。

Canonical supporting architecture：

`MY_WORLD_时间线存档与可逆性架构_CURRENT.md`

该文件明确 supersede `MY_WORLD_声明式UIHost架构_CURRENT.md` 中“旧 Turn 可通过 `回到这里` / timeline affordance 直接恢复”的建议，并收窄 Roadmap 中 `Rewind / Branch` 的旧解释。

当前 G2-03 Task Packet **不失效**：它本来只要求 Cancel / Regenerate，并禁止 G3 rewind/branch 实现，因此执行 Agent 无需返工。

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
- `Reversibility ≠ frictionless arbitrary rewind.`
- `Model authors the world; Runtime makes it durable; Player owns the timeline.`
- `Save Point != Timeline Node.`
- `Host capability first; external asset protocol second.`
- G2 product-facing Provider = DeepSeek `deepseek-v4-pro`；Kimi Code 仍只是 Foundation alternate。
- Secret 只来自本地受保护来源；不得进入 Git、日志、UI、截图或聊天。
- Owner 不承担 routine Godot/Git/build/debug/QA；真实产品体验 Gate 才交给 Owner。
