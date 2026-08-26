---
title: my world｜当前状态
status: current-project-status
version: 1.6
created: 2026-08-26
updated: 2026-08-26
phase: G2 AI Conversation Spine
current_task: G2-04 Turn / Conversation Domain v0.1
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
G2-03 Narrative View         PASS — Owner UAT
Current Task                  G2-04 — Turn / Conversation Domain v0.1
G2-05 Context Assembly        NOT STARTED
G2-GATE                       NOT YET
```

---

## 3. G2-03｜CLOSED / PASS

最终实现主线：

```text
d736ac9  initial Narrative Conversation View
774ab52  IR-01 completed-regenerate history repair
6690461  IR-02 + wide-screen Host scaling repair
81e7ce0  Composer responsive height + Narrative richness closeout
```

Owner 已在导出 EXE 中完成真实 UAT，并明确：

> **PASS，可以进入下一步。**

已成立的产品/工程事实：

- 默认 Maximized Window；
- `Player | Narrative | World` 三 Host 宽屏约 `18% / 60% / 22%`，窄屏折叠；
- Composer 支持舒服的多行自然语言输入并按窗口高度在合理范围内响应；
- real DeepSeek streaming / Cancel / Regenerate / failure recovery；
- IR-01 / IR-02 provisional history correctness；
- Narrative 长文本 readable width；
- `Narrative richness over artificial brevity`，Provider 无人为 `max_tokens` / 固定字数限制；
- Windows export / direct player launcher / secret hygiene 已验证。

G2-03 不再要求“现在已经像完整 AI RPG”。当前仅证明：Conversation Spine / Narrative UI 是未来 AI RPG 的正确交互骨架，而不是把产品锁死成普通聊天 UI。

### 非阻塞 Owner observation｜字体偏小

Owner 真实体验指出：当前整个页面字体仍整体偏小。

裁定：

```text
G2-03 PASS 不受阻塞
→ G2-04 携带一个小型 UX carry-forward
→ 当前默认字体调整到中等可读量级
→ 不在 G2 建设完整字体设置系统
→ 未来在 RPG Experience / UI Preference 阶段允许玩家自由选择字体大小
```

该项是已知、低风险、明确的小修，不得扩张成 Theme / Settings framework 重构。

---

## 4. Current Task｜G2-04 Turn / Conversation Domain v0.1

Why now：G2-03 已证明真实交互，但 Conversation 真相仍主要由 UI 内 provisional `_history` / generation flags 管理。G2-05 Context Assembly 不能建立在 UI 私有状态上，因此先建立最小正式 Conversation Domain。

目标：

```text
Player Turn
+ GM Generation
+ Conversation Entry / ordered Turn state
+ Generation State
+ Retry / Regenerate
+ latest-turn correction semantics
→ 由独立 Domain 拥有
```

关键边界：

- UI 只负责输入与投影，不再拥有第二套 Conversation truth；
- Provider Adapter 继续只负责 transport；
- G2-05 才拥有正式 Context Assembly / system instructions / working-set selection；
- Conversation / Transcript **不是 Timeline**；
- 不实现 Persistence / Save / Branch / arbitrary historical rewind；
- completed Regenerate / latest-turn correction 继续遵守“成功前保留旧稳定结果，成功后原子替换”的可逆语义；
- 本任务附带完成上一节的中等字体基线小修。

---

## 5. 当前核心约束

- `Model freedom first. Reversibility over prevention.`
- `Narrative richness over artificial brevity.`
- `Context stays bounded, not starved.`
- `UI is a projection, not a second truth source.`
- `Transcript != Timeline.`
- `Reversibility != frictionless arbitrary rewind.`
- `Save Point != Timeline Node.`
- G2-04 不得提前实现 G2-05 / G3。

---

## 6. 当前 waiting

```text
Blocking: NONE KNOWN
Current: prepare / execute G2-04 repository-native Task Packet
Owner UAT: not required for G2-04 engineering closeout; font/readability can be re-observed at G2-06 playtest
Next after G2-04 review PASS: G2-05 Context Assembly v0.1
```
