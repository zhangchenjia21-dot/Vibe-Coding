---
title: my world｜当前状态
status: current-project-status
version: 1.3
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
Base implementation           d736ac9389c2bf23f7f71b0270d6fd8f72db8461
IR-01 repair                  774ab522e48ef1026d622f89e7903e9cb7bab64c
IR-01                         PASS — completed regenerate duplication fixed
IR-02                         RETURNED — regenerate cancel/fail then new-send history integrity
Owner UAT                     RETURNED — wide-screen host scaling / startup mode
G2-GATE                       NOT YET
```

---

## 3. 已关闭 Task

### G2-01｜Application / Game Shell

Implementation: `my-world@4a13deb29a2e9c354530843d23eb48422957033c`

Owner UAT：**PASS**。

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

### 4.1 IR-01｜PASS

原缺陷：completed generation → Regenerate 会形成 `[user, user, new_assistant]`。

修复 commit：

`774ab522e48ef1026d622f89e7903e9cb7bab64c`

复审确认当前实现使用 `_current_turn_in_history` 区分当前 player turn 是否已进入 provisional history；successful completed→regenerate 后 history 保持 `[same user, new assistant]`，并新增 provider-context 只读测试 seam 与回归测试。

当前 `my-world/main == 774ab522e48ef1026d622f89e7903e9cb7bab64c` 时该修复为最新实现事实。

### 4.2 IR-02｜BLOCKING

复审 IR-01 时发现同一 provisional-state 机制仍存在一个边缘路径：

```text
completed turn
→ Regenerate
→ old assistant is popped from history
→ new regeneration Cancel / Fail
→ history temporarily remains [user]
→ player chooses to send a new action instead of Retry
```

此时新 `_on_send_pressed()` 会把 `_current_turn_in_history` 设为 false，但 `_build_messages()` 看到 history 末位已经是 `user`，不会 append 新 player input；结果可能出现：

- provisional history 不再保持 completed player/GM pair；
- 新玩家行动没有进入下一次 Provider context；
- UI 显示的新行动与模型实际收到的 context 不一致。

这违反 G2-03 的 provisional history correctness / failure recovery 目标。

要求最小修复，不引入 G2-04 Domain：

1. completed-turn Regenerate 的替换过程不得让 Cancel / Fail 后留下会污染下一请求的半对 history；
2. 推荐语义：直到新 generation **成功完成**前，原 completed assistant 仍作为 active provisional context 的稳定版本；成功后再原子替换，或采用等价的最小正确实现；
3. Cancel / Fail 后玩家无论选择 Retry 还是直接发送下一条行动，Provider context 与 provisional history 都必须保持一致；
4. 新增 regression：

```text
turn1 completed
→ regenerate
→ cancel or deterministic fail
→ directly send turn2 without retry
→ provider context contains turn1 exactly once and turn2 exactly once
→ no half-pair / duplicate user
→ successful turn2 ends with valid [user,assistant,user,assistant] history
```

不允许为此提前建设正式 Turn Domain / Session framework。

---

## 5. Owner UAT｜RETURNED：三栏伸缩与默认窗口

Owner 已真实运行 exported EXE，并确认基础三栏方向成立，但发现最大化后的布局策略不满足长期 RPG 信息栏需求：

- 默认窗口下比例尚可；
- 最大化后新增横向空间几乎全部给 Narrative；
- 左右 Host 基本只纵向拉高，横向仍接近细栏；
- 左右说明文字出现空间不足 / 边界拥挤；
- 这种行为会让 Player / World Host 在未来真实承载属性、人物、关系、任务、Save 等信息时不可用。

该项是 **functional layout feedback**，不是 deferred visual polish。

当前裁定已传播到 `MY_WORLD_架构_CURRENT.md` 与 `architecture/ui/声明式UIHost设计.md`：

```text
Narrative First != Narrative Only
```

修复要求：

- 默认玩家启动使用 **Maximized Window**，不是 Exclusive Fullscreen；
- 宽屏下三个 Host 全部参与横向 expansion；
- 第一版比例基线约 `18% / 60% / 22%`；
- Player / World 保持约 `250 / 310px` 量级 minimum usable width；
- 空间不足时折叠侧 Host，不靠无限压窄维持三栏；
- 所有侧栏文字正常 wrap / constrain，不越界；
- 1280×720 继续作为 windowed regression；
- 960×540 继续作为 narrow responsive regression；
- 超宽 Narrative 正文避免无限拉长单行，低成本情况下加入 reasonable readable-width constraint。

Owner 不需要在此修复前重复做 UAT。

---

## 6. 当前 Reversibility UX

当前只需要：

```text
active generation → Cancel
latest generation → Regenerate / Retry
```

长期原则：

> **Reversibility != frictionless arbitrary rewind.**
>
> **Save Point != Timeline Node.**

历史 Narrative 默认 read / scroll，不为每条放 `回到这里`；Save / Load 是明确玩家意图；Timeline 首先是 Runtime history / recovery foundation。

---

## 7. 当前 blocker / waiting

```text
Blocking:
- IR-02 completed-regenerate cancel/fail → direct new-send history/context integrity
- Owner UAT wide-screen proportional Host scaling + default maximized startup

Waiting:
- KimiCode K3 focused G2-03 repair
- engineering regression evidence
- then Owner re-UAT

Owner UAT: HOLD until repair
```

该返修仍属于 G2-03，不授权 G2-04 / G2-05 / G3 实现，也不授权大规模 UI 美化。
