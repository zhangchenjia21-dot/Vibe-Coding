---
title: my world｜当前状态
status: current-project-status
version: 1.8
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
G2-04 base implementation    0bf1f012366db7271664a192c1c30e60947cc5c9
IR-03 repair                  d0d5d47f487fdb75f31de5349894517a830a51e8 — PASS
Independent Review            RETURNED — IR-04 empty-completion accepted-truth defect
G2-05 Context Assembly        HOLD
G2-GATE                       NOT YET
```

---

## 3. G2-04 已确认通过的部分

当前 `my-world/main == d0d5d47f487fdb75f31de5349894517a830a51e8` 时：

- Conversation / Turn Domain 成为正式 in-memory owner；
- UI 已移除旧 `_history` / duplicated generation-state truth；
- Provider Adapter 保持 transport-only；
- normal Turn / Retry / Regenerate / latest-turn correction 的 logical identity 与 accepted-truth 原子替换语义成立；
- IR-01 / IR-02 回归保持；
- **IR-03 已修复**：completed latest Turn 的 Regenerate Provider request 只保留 previous accepted pairs + current user，当前旧 assistant 不进入 replacement request，request 以 current `user` 结束；
- typography medium baseline 已完成；
- 未越界实现 G2-05 / G3。

这些部分不要求重做。

---

## 4. IR-04｜Empty completion must not become accepted GM truth — BLOCKING

IR-03 真实 GUI 回归中，执行 Agent 已观察到一次 DeepSeek 偶发快速 `[DONE]` / 近空响应：没有产生可接受的正文 draft，就结束了 stream。当前 Adapter 会正常发 `completed()`，而 `Conversation.complete_generation()` 无条件执行：

```text
pending_player_text → accepted player_text
draft_text          → accepted_gm_text
has_accepted_response = true
```

因此空 / 纯空白 draft 也会成为 accepted GM truth。

影响：

### 新 Turn

```text
player action
→ provider completed with no meaningful content
→ accepted pair becomes [player, ""]
```

玩家得到一个没有 GM Narrative 的“完成回合”。

### Regenerate / Correction

更严重：

```text
old accepted pair stable
→ regenerate/correct
→ provider emits completed but zero/whitespace content
→ old accepted GM is replaced by ""
```

这破坏了我们刚建立的“旧 accepted truth 在成功 replacement 前保持稳定”语义。

现有 G2-04 T09 甚至把“没有任何 delta 仍 complete 成 accepted 空 GM”编码成合法行为，因此测试无法抓住它。

### 产品边界

这**不是 Narrative 长度限制**，也不得演化成 minimum-word-count guardrail。

正式区分：

```text
1 character / very short content
→ model-authored content
→ allowed

zero characters / whitespace-only content
→ no GM Narrative produced
→ generation attempt is not acceptable completion
```

`Narrative richness over artificial brevity` 继续成立；IR-04 只防止“完全没有输出”破坏 accepted truth。

---

## 5. IR-04 Required Repair

只做 G2-04 focused integrity repair：

1. Conversation Domain 在接受 generation completion 前检查当前 `draft_text` 是否存在非空白内容。
2. 若 `draft_text.strip_edges().is_empty()`：
   - **不得**写入/覆盖 `player_text`、`accepted_gm_text`、`has_accepted_response`；
   - attempt 进入可 Retry 的 failed-equivalent state；
   - 发出明确的 domain failure code，例如 `empty_generation`（命名可等价）；
   - 对 Regenerate / completed Correction，旧 accepted pair 必须原样保留；
   - 对新 Turn，仍无 accepted response，可直接 Retry 同一 logical Turn。
3. UI 给 `empty_generation` 一个正常玩家可读错误，例如“本次没有生成有效叙事，可点击重新生成重试”；不要展示工程细节。
4. 不设置最小字符数；任何非空白内容都允许 accepted。
5. 增加 focused tests，至少覆盖：

```text
new turn → empty completion
→ FAILED/retryable
→ no accepted entry
→ retry same turn → non-empty completion → accepted
```

```text
completed turn → regenerate → empty completion
→ old player + old GM unchanged
→ same turn identity
→ retry/regenerate again → non-empty completion → atomic replacement
```

```text
completed latest correction → empty completion
→ old accepted player + GM unchanged
→ corrected text not partially accepted
```

以及 whitespace-only draft。
6. 原 IR-03 request-context assertions、IR-01 / IR-02 / correction / typography / G2-03 regressions保持通过。
7. Real DeepSeek 不要求强行复现偶发空响应；必须用 deterministic Domain test 证明该路径。真实 GUI 正常 completed→Regenerate 继续 smoke PASS 即可。
8. 默认不修改 Provider Adapter；Adapter 的 `completed` 仍可表示 transport/API 正常结束，Domain 决定该 attempt 是否产生可接受的游戏 Narrative。

---

## 6. Scope

允许：

- `src/domain/会话.gd` 最小 completion acceptance 修复；
- `src/ui/叙事对话视图.gd` 增加 `empty_generation` 玩家可读错误；
- 直接相关 G2-04 / G2-03 tests。

禁止：

- arbitrary minimum length / word count；
- Narrative quality classifier；
- Provider retry platform；
- 修改 Provider Adapter，除非发现独立、可证实的 transport bug 并先返回说明；
- G2-05 Context Assembly；
- G3 Persistence / Save / Timeline；
- 大规模重构。

---

## 7. 当前 waiting

```text
Blocking: IR-04 empty/whitespace completion can overwrite accepted truth
Waiting: KimiCode K3 focused repair + deterministic regression evidence + revised Final Report
Owner UAT: not required
Next only after Independent Review PASS: close G2-04 → G2-05 Context Assembly v0.1
```
