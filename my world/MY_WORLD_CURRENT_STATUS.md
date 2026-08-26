---
title: my world｜当前状态
status: current-project-status
version: 2.0
created: 2026-08-26
updated: 2026-08-26
phase: G2 AI Conversation Spine
current_task: G2-06 First Owner Playtest
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
G2-04 Turn/Conversation      PASS — Independent Review
G2-05 Context Assembly       PASS — Independent Review
Current Task                  G2-06 — First Owner Playtest
G2-GATE                       NOT YET
```

---

## 3. G2-05｜CLOSED / PASS

实现 commit：

```text
9c577811fd71d19f514ca4e9455e02321f0aa34d  Context Assembly v0.1
```

Independent Review 结论：**PASS**。

已成立：

- `Conversation` 继续拥有 Turn / accepted player+GM truth / generation lifecycle；
- `Conversation.get_context_projection()` 只返回 derived read model，不再负责 Provider message assembly；
- 独立 `Context Assembly` 成为 GM/system instructions、Conversation working-set 与 Game Context request material 的组装 owner；
- `Conversation.build_provider_messages()` 已退休，无 compatibility fallback；
- 第一代 bounded policy = 最近 12 个完整 accepted Turn + current attempt；按完整 Turn 取舍，不截断单条玩家/GM文本；
- new / retry / regenerate / correction request shape 已用 deterministic state matrix 覆盖；
- Regenerate / Correction 会排除当前旧 accepted pair，request 以当前 user 结束，同时 Domain old accepted truth 在 replacement 成功前保持稳定；
- cancelled / failed partial draft 不进入 Context；
- non-empty `game_context_text` 只作为 system 中 `Current Game Context` derived material；production 当前为空，不伪造尚不存在的 World/NPC authoritative state；
- Context/messages 是 derived copies，不可反向修改 Conversation truth；
- `Narrative richness over artificial brevity` 保持；无 `max_tokens` / output-length cap；
- IR-03 / IR-04、G2-04 Domain、G2-03 UI、G2-02 Adapter 与真实 DeepSeek / Windows export regressions 均保持通过；
- 未越界实现 G3/G4/G5/G7 或 retrieval/summarization/long-memory platform。

G2-05 是工程 ownership/context foundation，不单独要求 Owner UAT。

---

## 4. Current Task｜G2-06 First Owner Playtest

目标：由 Owner 在真实导出 EXE 中体验当前完整 G2 Conversation Spine，而不是继续做工程检查。

当前可真实评价：

- 自然语言行动输入是否舒服；
- AI GM streaming / 多回合连续阅读是否自然；
- Conversation working set 下的短局 continuity 是否可接受；
- Narrative 篇幅、信息密度、沉浸感是否值得继续读；
- Cancel / Regenerate / Retry 是否低摩擦；
- medium typography、Composer、三 Host 布局是否适合持续游玩；
- 整体交互骨架是否适合作为后续长期 AI RPG 的 Conversation Spine。

当前**不要求**评价：

- 长期 World persistence / Save / Timeline；
- 正式 Character/NPC/Faction/World state；
- World Pack；
- 长局 retrieval / summarization；
- 完整 RPG 机制；
- “现在是否已经像完整 AI RPG”。

原因：production `game_context_text` 当前仍诚实为空；G2-05 只建立 Context owner / bounded working set / future Game Context seam，尚未实现正式 Game/World material。

Owner Playtest 只需要真实玩，不需要运行测试、看日志、检查 Git 或验证内部 Context roles。

---

## 5. 当前核心约束

- `Model freedom first. Reversibility over prevention.`
- `Narrative richness over artificial brevity.`
- `Context stays bounded, not starved.`
- `UI is a projection, not a second truth source.`
- Context material / Provider messages are derived request material, not canonical World truth.
- `Transcript != Timeline.`
- G2-06 只做 Product Owner gameplay reality check，不借 UAT 偷做 G3+。

---

## 6. 当前 waiting

```text
Blocking: NONE KNOWN
Current: Owner plays exported game and returns PASS / feedback
Owner UAT entry: D:\AI\Projects\my-world\run-game.cmd
Next after Owner PASS: close G2-06 → evaluate / close G2-GATE → proceed per roadmap
```
