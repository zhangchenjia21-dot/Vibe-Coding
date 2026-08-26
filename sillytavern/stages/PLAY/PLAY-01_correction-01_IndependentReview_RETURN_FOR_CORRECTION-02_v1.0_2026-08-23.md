---
title: 酒馆游戏｜PLAY-01 correction-01 独立审核｜RETURN FOR CORRECTION-02
status: current-review-decision
version: 1.0
updated: 2026-08-23
project: 酒馆游戏新版主体
reviewed_implementation_sha: 0b638ac940bc78dd083718c42c116513cda61eb6
---

# PLAY-01 correction-01｜独立审核

## 0. Verdict

```text
P0 = 0
P1 = 2
Verdict = RETURN FOR CORRECTION-02
main integration = BLOCKED
Owner UAT = BLOCKED
```

correction-01 已关闭上一轮三个 Provider 闭环 P1：world place 不再替玩家移动；Active Situation 已进入 ContinuityContext；Opening prompt 不再禁止新世界候选。真实 DeepSeek 探针也证明 world_initiative place 可返回 `moveToDestination=false` 并通过 Program validator。

但 exact-SHA 审核 `0b638ac940bc78dd083718c42c116513cda61eb6` 发现两个仍直接阻断核心游玩纵向的问题。

---

## 1. P1-01｜World Initiative 读取的是玩家行动前世界，不是 post-player world frame

冻结架构要求：

```text
Player action / resolution
→ provisional post-player world frame
→ Model World Development / NPC Reaction
```

当前 `FormalTurnSubmissionFlow` 虽然已经计算 `backgroundCompleteDelta`，但随后仍直接：

```text
compileContinuityContext(workingState, ...)
→ narrativeProvider.decideContinuity(...)
→ #prepareWorldDevelopment(workingState, ...)
```

此时 `workingState` 只包含 player-demand materialization / asset mutation 等预处理，并未应用本回合已经接受的 Player Outcome 与 background progression。

结果：

- 玩家刚从 Scene A 移动到 Scene B，World Initiative 仍可能看到 current.location=A；
- Wait / 时间推进后，模型仍可能看到旧 worldTime；
- due background NPC 已移动，模型仍可能看到旧 NPC 位置；
- World Development validation/materialization 也基于旧 frame，而不是本回合真实后果。

这违反 `核心游玩重构_Turn与WorldInitiative架构规格_v1.0_2026-08-23.md` 的 ordinary turn 顺序，也会直接削弱“世界对玩家选择作出反应”。

### Required correction

在不提前写数据库、不双重提交 delta 的前提下，建立一个只用于编排/模型上下文的 **provisional post-player state**：至少反映本回合已接受的 Player Formal Delta、时间变化、background progression，以及其它已经确定且会影响 World Initiative 当前工作集的正式变化。

World Initiative 的 context compilation、proposal validation / materialization 应基于该 provisional frame，而最终仍只做一次原子 commit。

不得为了修复此问题建立第二套 Runtime Store / truth；优先复用或提取纯函数 working-state projector。

---

## 2. P1-02｜真实 T0 Opening 可以 quiet 后永久完成，却没有 durable Opening Situation

原 PLAY-01 产品目标不是“首次进入有一段文字”，而是形成玩家可回应、后续世界可持续推进的 Opening Situation。

correction-01 的真实 DeepSeek probe 中，Opening 两次都合法返回 `beat=quiet`。当前 `ensureOpening()` 会接受这种纯叙事 Opening，提交 `opening_revision/opening_narrative` 并永久标记 Opening exactly-once 完成。

问题在于：

1. quiet 无 durable changes，因此不会创建 Active Situation；
2. `compileContinuityContext()` 不携带 `state.opening.narrative`；
3. 下一回合 World Initiative 因而没有 durable situation，也没有 Opening narrative 作为持续工作集。

这可能产生：

```text
首次进入：有开场文字
↓
无 Active Situation / durable hook
↓
玩家回应后，世界决策阶段拿不到开场钩子
↓
再次退化为“看起来有剧情，但世界没有持续对象”
```

### Required correction

T0 Opening 在 NarrativeProvider 可用时必须形成至少一个 **durable、player-safe、可回应的 Active Situation**。模型仍自由决定局势内容；Program 不得用剧情关键词白名单主持剧情。

允许的实现方向包括为 Opening 使用更窄但正向的结构契约（例如 T0 不允许 quiet、要求 situation=create），或其它能保证同一产品语义的方案。禁止 Program 根据 narrative 文本自行编造剧情语义。

该要求是核心产品输出保证，不是“为防模型犯错而增加预防层”，与 `DEC-P04` 不冲突：Freedom 是“模型自由决定发生什么”，不是“模型可以选择不履行 T0 主持职责”。

---

## 3. correction-01 已确认 PASS 的部分

以下不要求回退：

- world_initiative place Provider 指令明确 `moveToDestination=false`；
- player_need place 旧移动语义保留；
- Active Situation active/changed 进入 bounded ContinuityContext；
- resolved 不进入，hidden related refs 过滤；
- Opening prompt 允许新 Character / Place candidate；
- quiet ordinary continuation 语义已统一；
- focused / full automated validation 全绿；
- 真实 Provider probe 6 calls，最终三探针通过；
- 独立 worktree 已使用；Owner 真实 DB 未用于自动化；
- main 未漂移，仍为 `9212d1fe9a87e07ec2437203562ac333b28e3ab3`。

---

## 4. Same-root classification

本轮两个 P1 属于同一根因族：

> **World Initiative 已有 typed capability，但真实 orchestration 仍没有把“当前可玩的局势”完整交给模型。**

correction-02 必须做一次同根扫描：Opening、post-player reaction、Wait/background progression、move-to-second-scene 四条路径均检查模型实际 working frame，而不是只修单一测试。

若 correction-02 后再次出现同根 P0/P1，不再机械进入 correction-03；先做 root-cause architecture review。

---

## 5. Integration decision

当前禁止移动 `sillytavern/main`。

只有 correction-02 exact-SHA 独立审核达到：

```text
P0 = 0
P1 = 0
```

并再次确认 `origin/main` 未漂移、task branch 为 formal base 的 fast-forward descendant，才允许精确快进集成。

Owner Fun / Playability PASS 仍只能来自真实连续试玩。