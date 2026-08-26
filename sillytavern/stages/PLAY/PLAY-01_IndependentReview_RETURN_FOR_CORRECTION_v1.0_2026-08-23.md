---
title: PLAY-01 独立审核｜真实 Provider 世界主动性返修
status: current-review
version: 1.0
updated: 2026-08-23
project: 酒馆游戏新版主体
reviewed_base_sha: 9212d1fe9fa8878230949d3b371222c2ed8254f6c
reviewed_implementation_sha: ff23174018cb79a5bf4de08146b406c22216e408
result: RETURN_FOR_CORRECTION
---

# PLAY-01 独立审核｜RETURN FOR CORRECTION

## 1. 结论

PLAY-01 当前实现 **不得合并 main**。

```text
P0 = 0
P1 = 3
P2 = 1（同根问题）
Process finding = 1
Independent Review = FAIL / RETURN FOR CORRECTION-01
Owner UAT / Playability = NOT READY
```

已验证的正向基础：T0 Opening non-turn commit、revision/turnNumber 分离、World Initiative 与 Player Authorization 分权、Active Situation 持久化与玩家投影、Save/Restore/Recovery/Disclosure 回归覆盖、world-source Character 物化 fixture 纵向均已形成有效工程基础。

失败集中在**真实 DeepSeek Provider 能否真正消费这些能力**，属于本轮产品核心，不得以 fixture / 自动化全绿覆盖。

## 2. P1-01｜真实 world_initiative 地点物化提示词与校验器互相矛盾

文件：
- `src/运行时/L3_外交层/DeepSeek世界语义物化提供者.ts`
- `src/运行时/L1_器件层/世界语义物化校验器.ts`

现状：共享 `SYSTEM_INSTRUCTION` 仍写：

```text
need.kind=place 时只创建 destination 并设置 moveToDestination=true
```

但 PLAY-01 world-source 校验明确规定：

```text
world_initiative -> moveToDestination 必须 false
```

因此真实模型按提示词正确输出时，world_initiative 新地点会被 `WORLD_INITIATIVE_PLAYER_MOVE_FORBIDDEN` 拒绝。

现有测试使用手工 `validWire()` 固定 `moveToDestination='no'`，只证明解析/校验能接受正确 wire，没有证明 Provider 指令会生成该 wire。

影响：
- world-source Place / Scene / Connection 真实纵向不成立；
- 直接命中 Owner “无法进入第二场景”的核心阻塞；
- 违反 PLAY-01 Required Deliverable 4 与架构规格 dynamic Place/Scene/Connection 目标。

## 3. P1-02｜Active Situation 没有进入下一回合 World Initiative 模型上下文

文件：
- `src/运行时/L0_公理层/连续性契约.ts`
- `src/运行时/L1_器件层/连续性上下文编译器.ts`
- `src/运行时/L3_外交层/DeepSeek叙事提供者.ts`

PLAY-01 新增了 `ActiveSituationState`，并持久化/投影给玩家；但 `ContinuityContext` 没有 situation 字段，`compileContinuityContext()` 也不选择 active/changed situations。

DeepSeek 的 `decideContinuity` / `world_development` 只能从该上下文决定下一步，而 `change/resolve` 又必须提交 `situation_ref`。

结果：真实模型看不到当前 Situation 的 stable ref、summary、status，无法可靠 change / resolve / escalate 已存在局势。fixture 测试可以人工注入 ref，因此没有暴露真实模型上下文缺口。

影响：
- `Active Situation` 退化成“数据库 + UI 有、模型碰不到”的设计；
- 违反 DEC-06“给下一回合 World Initiative 一个可持续对象”；
- 违反 AC-09 active→changed/resolved 的真实纵向含义；
- 容易重新出现本次事故的同类问题：能力存在但模型没有工作集。

## 4. P1-03｜Opening 提示词仍直接禁止创造请求中不存在的新人物/地点

文件：`src/运行时/L3_外交层/DeepSeek叙事提供者.ts`

`buildDeepSeekWorldOpeningRequestBody()` 当前仍写：

```text
只能使用请求中的公开字段；不得编造请求中不存在的角色、地点、物品或隐藏信息。
```

其中“不得编造请求中不存在的角色、地点、物品”与当前产品/架构的核心裁定直接冲突：Opening / World Initiative 必须能够合理提出尚未预枚举的新 NPC / Place，并交给 Program 物化。

安全目标应只禁止：伪造既有 canonical fact、隐藏信息、玩家行动，以及把未提交 durable content 当成已存在事实；不能重新禁止 World Initiative author 新内容候选。

该提示词可能使真实 DeepSeek 在 T0 主动压制 `materialization_kind=character|place`，从而让 Opening 再次变成只消费既有世界。

## 5. P2-01｜quiet beat 的 Provider 方言与 Program 校验不一致

`DeepSeek叙事提供者.ts` 指示 `beat=quiet` 时 `player_safe_summary` 留空；解析器也接受空字符串。

但 `validateWorldDevelopmentProposal()` 在判断 quiet 之前先要求 `playerSafeSummary.trim() !== ''`，因此真实 quiet proposal 会稳定被 `BEAT_WITHOUT_SUMMARY` 拒绝。

虽然最终世界不变化的外部效果接近，但 artifact / accepted semantics 与架构“允许合法 quiet continuation”不一致。应在 correction-01 与同一 Provider-contract 根因一并修正。

## 6. Review Gate｜真实 Provider 证据缺失

架构规格 PLAY-01 明确包含 authorized real Provider playability probe；当前最终报告记录：

```text
real Provider playability probe: NOT RUN
```

由于本次 P1 恰好集中在真实 Provider 指令/上下文，而 fixture 全绿未发现，因此 correction-01 在离线测试通过后必须增加**受控真实 Provider 契约探针**（若执行环境已有合法配置），至少验证 world_initiative place candidate 与 Opening / situation context 的实际 tool output，不泄露 key、完整 prompt、raw private state。

Owner 的最终 Fun / Playability 仍在集成后真人试玩，不由该探针代替。

## 7. Process Finding｜未遵守隔离 worktree

任务包明确要求：

```text
使用隔离 worktree；不得修改 Owner 的 D:\AI\Projects\sillytavern 主工作树。
```

Kimi 最终报告却写明本轮在：

```text
D:\AI\Projects\sillytavern
```

工作树上切到任务分支操作。

没有证据显示 main 被 push 或真实 Owner DB 被修改，因此本项不升级为代码 P1；但属于明确治理违规。correction-01 必须使用独立 worktree，不得继续使用 Owner 主工作树。

## 8. Correction-01 目标

只修上述同根问题，不扩大为 PLAY-02：

1. source-aware materializer Provider 指令，world place 必须 create=true / move=false；
2. Active Situation 进入 World Initiative 的最小充分 player-safe model context；
3. Opening prompt 允许 World Initiative 提出新 durable candidates，同时保留 No Phantom Player Action / Durable Consequence / Interactable Identity；
4. quiet semantics 对齐；
5. 增加能防止 fixture 掩盖 Provider 指令/上下文错误的 regression tests；
6. 有合法 Provider 配置时执行受控真实 Provider 契约探针；
7. 全量回归；
8. P0=0/P1=0 后才重新进入 Independent Review。

## 9. Integration Decision

```text
Current origin/main = 9212d1fe9a87e07ec2437203562ac333b28e3ab3
Reviewed Implementation = ff23174018cb79a5bf4de08146b406c22216e408
Fast-forward ancestry = PASS
Engineering test report = broadly PASS
Independent product/architecture review = FAIL
Integration = BLOCKED
```

不合并 `ff23174018cb79a5bf4de08146b406c22216e408`。
