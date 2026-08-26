---
title: my world｜当前状态
status: current-project-status
version: 1.9
created: 2026-08-26
updated: 2026-08-26
phase: G2 AI Conversation Spine
current_task: G2-05 Context Assembly v0.1
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
Current Task                  G2-05 — Context Assembly v0.1
G2-06 Owner Playtest          NOT STARTED
G2-GATE                       NOT YET
```

---

## 3. G2-04｜CLOSED / PASS

最终实现线：

```text
0bf1f012366db7271664a192c1c30e60947cc5c9  Conversation / Turn Domain v0.1

d0d5d47f487fdb75f31de5349894517a830a51e8  IR-03 regenerate Provider context repair

d1acd2a58e00fd99b73ab98bc3ccdc3c79762951  IR-04 empty/whitespace completion integrity repair
```

Independent Review 结论：**PASS**。

已成立：

- Conversation / Turn Domain 是正式 in-memory conversation truth owner；
- UI 不再维护独立 `_history` / duplicated generation-state truth；
- Provider Adapter 保持 transport-only；
- normal Turn / Retry / Regenerate / latest-turn correction 使用稳定 logical turn identity；
- completed Regenerate / correction 成功前旧 accepted pair 保持稳定，成功后同 identity 原子替换；
- IR-01 / IR-02 history/context correctness 保持；
- IR-03：Regenerate replacement request = previous accepted pairs + current user，当前旧 assistant 不进入请求，request 以 user 结束；
- IR-04：zero / whitespace-only completion 不得成为 accepted GM truth；它进入 `empty_generation` failed-equivalent，可同 Turn Retry；任何非空白内容（包括 1 字）仍可 accepted，不构成人为 Narrative 长度限制；
- 中等可读 typography baseline 已完成；
- 未越界实现 G2-05 / G3。

G2-04 是工程 ownership / semantics 任务，不单独要求 Owner UAT；字体与整体体验在 G2-06 再由 Owner 观察。

---

## 4. Current Task｜G2-05 Context Assembly v0.1

Why now：G2-04 已建立稳定 Conversation owner，但当前 Provider messages 仍由 Conversation 内临时 `build_provider_messages()` 直接拼接。正式 Context Assembly 必须在 G2-06 Owner Playtest 前成为独立责任，否则 Conversation Domain 会继续承担 system prompt / working-set / game-context 选择职责。

Outcome：

```text
GM/System Instructions
+
Conversation read projection
+
Current minimal Game Context material
+
Bounded recent Conversation working set
→ Context Assembly v0.1
→ Provider messages
```

关键边界：

- `Context stays bounded, not starved.`
- Context Assembly 是 derived request material，不是第二 truth source；
- Conversation Domain 继续拥有 Turn/accepted truth，不拥有 prompt/context policy；
- Provider Adapter 继续只接收已组装 messages，不拥有 Context 语义；
- 当前最小 Game Context 只建立输入 seam / fixture，不伪造尚不存在的 World/NPC authoritative state；
- G2-05 使用简单 recent-turn working set，不建设 retrieval / summarization / long-memory platform；
- 不把 bounded context 误解成短回答：`Narrative richness over artificial brevity` 继续成立；
- G3 Persistence、G4 World Pack、G5 World/NPC semantics、G7 long-session retrieval 均未授权。

---

## 5. 当前核心约束

- `Model freedom first. Reversibility over prevention.`
- `Narrative richness over artificial brevity.`
- `Context stays bounded, not starved.`
- `UI is a projection, not a second truth source.`
- Context material / Provider messages are derived request material, not canonical World truth.
- `Transcript != Timeline.`
- `Save Point != Timeline Node.`
- G2-05 不得提前实现 G3 / G4 / G5 / G7。

---

## 6. 当前 waiting

```text
Blocking: NONE KNOWN
Current: prepare / execute G2-05 repository-native Task Packet
Owner UAT: not required for G2-05 engineering closeout
Next after G2-05 Independent Review PASS: G2-06 first Owner Playtest
```
