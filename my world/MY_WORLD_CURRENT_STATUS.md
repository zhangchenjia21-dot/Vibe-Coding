---
title: my world｜当前状态
status: current-project-status
version: 1.4
created: 2026-08-26
updated: 2026-08-26
phase: G2 AI Conversation Spine
current_task: G2-03 Narrative Conversation View
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
Current Phase                 G2 — AI Conversation Spine
G2-01 Application/Game Shell PASS — Owner UAT
G2-02 Provider Adapter v0.1  PASS — Engineering
Current Task                  G2-03 — Narrative Conversation View
Base implementation           d736ac9389c2bf23f7f71b0270d6fd8f72db8461
IR-01 repair                  774ab522e48ef1026d622f89e7903e9cb7bab64c — PASS
IR-02 + Host scaling repair   6690461e1dca80096e4d4ba27e7acbed4f109f23 — PASS (engineering review)
Owner UX closeout             REQUIRED — Composer ergonomics + Narrative richness
G2-GATE                       NOT YET
```

---

## 3. G2-03 已确认通过的工程事实

`my-world/main == 6690461e1dca80096e4d4ba27e7acbed4f109f23` 时：

- real DeepSeek stream / Cancel / Regenerate / failure recovery 已有工程证据；
- IR-01 completed→Regenerate duplicate player history 已修复；
- IR-02 completed→Regenerate→Cancel/Fail→direct new-send history/context integrity 已修复；
- maximized desktop 三 Host 约为 `18% / 60% / 22%`；
- Player / World 保持可用 minimum width；
- 1280×720 三栏、960×540 collapse/toggle 回归已证明；
- 默认玩家启动为 Maximized Window（非 Exclusive Fullscreen）；
- Narrative 正文已有 bounded readable width；
- Windows export / run-game / secret / Git hygiene 已有工程证据。

这些通过不等于 G2-03 Product PASS。

---

## 4. Owner UX Closeout｜当前唯一剩余工作

### UX-01｜Composer 不能在最大化后仍是 64px 窄输入条

Owner 已实际体验并指出：三栏比例修复后，最大化窗口中 Player input 仍然过矮，不符合自然语言 RPG 的输入需求。

当前实现事实：

```text
src/main.tscn
PlayerInput.custom_minimum_size.y = 64
```

且 `src/应用壳.gd` 没有按 viewport height 调整 Composer 高度的逻辑。

因此本项属于 G2-03 功能性 usability feedback，不是 deferred visual polish。

目标语义：

```text
Narrative First != Composer Tiny
```

第一代基线：

- 1280×720：Composer / TextEdit 应至少舒适容纳约 3–4 行自然语言行动；
- maximized 1080p/更高：输入区应适度增高，而不是始终保持 64px；
- 推荐量级：约 `100–120px`（720p）→ `130–160px`（1080p/maximized），并设置合理上限，不能随超高分辨率无限增长；
- 960×540 narrow 仍需保持 Narrative + Composer 可用，不被侧栏或按钮挤坏；
- 不要求本阶段实现复杂 auto-growing editor、Splitter 或可持久化 UI preference。

允许简单 responsive rule / clamp；目标是舒服输入长行动，而不是精确像素公式。

### UX-02｜Narrative richness over artificial brevity

Owner 明确产品偏好：**不要人为限制模型 Narrative 输出量。** 长一点的输出可以承载更多环境、人物、对话、后果与信息，提高沉浸感。

当前 Provider Adapter 没有 `max_tokens` / 字符数硬上限，保持这一点。

G2-03 provisional GM prompt 可增加正向倾向：

> 充分展开对当前场景有价值的环境、人物、行动、对话与后果，不必刻意简短；根据场景节奏自然决定叙事篇幅。

必须同时保持：

```text
no hard minimum length
no fixed per-turn word count
no artificial short-answer instruction
no new max_tokens cap merely for UI / latency convenience
```

篇幅由模型、场景与可用 Context 自然决定。真正的 Narrative quality / length 将在 G2-05 Context Assembly 后继续通过 Owner gameplay 判断。

---

## 5. Scope

当前只允许 G2-03 Owner UX closeout：

- Composer responsive height / multiline usability；
- 与其直接相关的布局回归；
- provisional GM prompt 的 Narrative-richness 微调；
- focused tests / GUI evidence / export。

禁止借机实现：

- G2-04 formal Turn / Conversation Domain；
- G2-05 formal Context Assembly；
- Persistence / Save / Timeline / Branch；
- generalized UI framework / Splitter preference system；
- 大规模视觉美化；
- Provider registry / output-length control platform。

---

## 6. Owner UAT Gate

工程完成后最高状态：

> **READY FOR OWNER UAT**

最终 Owner UAT 重点：

1. 默认 maximized 启动后三栏比例是否自然；
2. Composer 是否足够舒服地输入多行行动；
3. 输入 / streaming Narrative / Cancel / Regenerate 是否自然；
4. Narrative 是否没有被人为压成短回答；
5. resize 到普通与窄窗口后仍可用。

只有 Owner 明确 PASS 才关闭 G2-03 并推进 G2-04。

---

## 7. 当前 waiting

```text
Blocking: UX-01 Composer ergonomics
Required closeout: UX-02 Narrative richness prompt / no artificial output cap
Waiting: KimiCode K3 focused G2-03 UX repair → engineering evidence → Owner UAT
```
