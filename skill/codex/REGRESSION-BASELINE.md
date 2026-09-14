# Minecraft Planner / Builder Regression Baseline

日期：2026-09-14。本文仅记录既有回归结论和证据位置，不增加执行规则，不改变任何 Skill 或 shared contract 行为。

## Stable versions

- minecraft-planner **v0.5**
- minecraft-builder **v1.11**
- Minecraft Planner–Builder Contract **v1.0**
- 固定源提交：[`fc6371361685e2eeaefdef5a513f21dbe64c6696`](https://github.com/zhangchenjia21-dot/Vibe-Coding/commit/fc6371361685e2eeaefdef5a513f21dbe64c6696)

## Recorded result

**Core regression suite = PASS_WITH_NOTES。**

依据15份既有 Independent Review 及对应原始证据，已覆盖历次版本累计的 L0→L4 递归规划，以及最终版本的 Planner→Builder handoff、Builder→Planner 最小修订、stale传播、Planning Fidelity和合法拒绝冻结行为。历史 MP-P05R 的 `FAIL_MODEL_EXECUTION` 保留，其后 MP-P05R2 提供成功复跑证据；没有把历史每轮改写为PASS，也没有宣称所有场景在最终版本上重跑。

MP-I02B建筑仍为 **UNFROZEN_DESIGN_WITH_BLOCKERS**，未达到 `DESIGN_READY`。其正确保留地基、烟道/居住环境和雨水性能问题，不构成核心接口套件失败。正向 `DESIGN_READY` 成功样本、Builder Core、运行时碰撞、工程性能、Finishing及世界施工不在已通过覆盖中。

**World-write / production construction 未授权；world writes = 0。** 稳定版本记录不等于建筑批准或生产施工认证。

## Evidence archive

- [稳定基线归档](https://github.com/zhangchenjia21-dot/assets/tree/c0fb06be12848ccf8e7e591a27d7b78d84fc8c1b/minecraft/PLANNER-BUILDER-REGRESSION-BASELINE-v0.5-v1.11/)
- [总回归报告](https://github.com/zhangchenjia21-dot/assets/blob/c0fb06be12848ccf8e7e591a27d7b78d84fc8c1b/minecraft/PLANNER-BUILDER-REGRESSION-BASELINE-v0.5-v1.11/%E6%80%BB%E5%9B%9E%E5%BD%92%E6%8A%A5%E5%91%8A.md)
- assets归档提交：`c0fb06be12848ccf8e7e591a27d7b78d84fc8c1b`
- 归档内含原审核引用、版本/blob/SHA256索引、覆盖边界、Known limitations和Production usage recommendation。

当前证据不要求修改以上三个稳定版本的行为。本记录仅声明已测试范围内的稳定基线，原始测试、Canon和“建筑师”正式项目内容均保持不变。
