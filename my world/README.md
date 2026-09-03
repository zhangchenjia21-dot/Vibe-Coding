# my world｜项目治理入口

`my world` 是独立单人 AI RPG 项目。实时任务状态以 `MY_WORLD_CURRENT_STATUS.md` 为准。

## Start Here

1. [`MY_WORLD_项目启动总纲_CURRENT.md`](./MY_WORLD_项目启动总纲_CURRENT.md)
2. [`MY_WORLD_核心设计原则_CURRENT.md`](./MY_WORLD_核心设计原则_CURRENT.md)
3. [`MY_WORLD_架构_CURRENT.md`](./MY_WORLD_架构_CURRENT.md)
4. [`MY_WORLD_总体规划路线图_CURRENT.md`](./MY_WORLD_总体规划路线图_CURRENT.md)
5. [`MY_WORLD_CURRENT_STATUS.md`](./MY_WORLD_CURRENT_STATUS.md)

> **Root is map; subfolders are depth.**

## 当前状态

```text
G1 Foundation                         PASS / CLOSED
G2 AI Conversation Spine              PASS / CLOSED
G3 Persistence / Save / Timeline     PASS / CLOSED
G4 Primary Source Assets & Local Game PASS / CLOSED
G4-10 Runtime Asset Resolution        DEFERRED / MOVED TO G6

G5 World Semantics & GM Runtime       ACTIVE
G5-01 World Turn                      PASS / CLOSED
G5-02 Knowledge Provenance            PASS / CLOSED
G5-03 NPC / Faction Agency            ACTIVE
G5-03M1 old single-NPC task           SUPERSEDED / DO NOT EXECUTE
G5-03M1 Multi-Actor Agency Cycle      ACTIVE — KIMI
G5-03M2 Stable Actor Materialization  NEXT AFTER M1 REVIEW
G5-04 Event / Priority Evolution      NOT YET
```

## 当前路线

```text
G5-03M1 Multi-Actor Agency Cycle
→ GPT Independent Review
→ G5-03M2 Minimal Stable Actor Materialization / Registry Expansion
→ decide remaining Faction slice from actual consumer
→ G5-04 Event / Priority-driven World Evolution
```

## G5 当前核心

G5 已证明：

```text
free-form Narrative
→ durable world consequences

World / GM truth
!= actor knowledge
```

G5-03 现在开始证明：

> **Source provides inertia; actors create history.**

Owner 已明确纠正旧的 one-NPC-per-turn 设计。当前 Agency Cycle 是：

```text
accepted ordinary turn
→ existing semantic-analysis request performs Agency Selection
→ 0..4 relevant stable NPC candidates
→ separate actor-scoped execution per selected NPC
→ selected requests may progress concurrently
→ several NPC actions may become durable in the same world window
```

不再使用 round-robin 单 NPC 作为主要 scheduler。

每个 actor execution 只能使用该 actor 自己的 Character Source、自己的 durable Knowledge Provenance 与自己的 agency history；不能因为 GM 全知就继承别人的私密知识。

Agency 始终是 background/fail-soft：玩家下一行动、Restore/Recovery 或 timeline 切换优先；未提交的旧 agency 必须失效，不能把 Provider 延迟变成玩家输入 Gate。

当前 M1 用 Guaranteed NPC 作为已有稳定身份的 bootstrap pool。Guaranteed NPC 不是永久 agency 边界。Owner 提出的曹操 / 孙权 / 诸葛亮同一世界窗口可能同时行动，已经为下一步 G5-03M2 stable actor materialization 提供了真实 consumer。

Canonical current agency decision：

`architecture/world/G5_MULTI_ACTOR_AGENCY_CYCLE_V0_2_DECISION.md`

Current implementation packet：

`my-world/docs/tasks/G5-03M1_MULTI_ACTOR_AGENCY_CYCLE_TASK.md`

Historical `G5_STABLE_NPC_AGENCY_V0_1_DECISION.md` 与 `G5-03M1_STABLE_NPC_INDEPENDENT_AGENCY_TASK.md` 已 superseded，不得执行旧的 single-actor rule。

## 长期原则速览

> **Vertical before platform. Consumer before infrastructure.**
>
> **Model Freedom First.**
>
> **Narrative richness over artificial brevity.**
>
> **Model authors the world; Runtime makes it durable; Player owns the timeline.**
>
> **Source provides inertia; actors create history.**
>
> **GM omniscience must not become actor omniscience.**
>
> **Context stays bounded, not starved.**

Visual runtime remains deferred to G6.
