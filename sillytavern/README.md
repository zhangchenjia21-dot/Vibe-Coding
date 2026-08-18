# SillyTavern 项目治理入口

本目录 active 层只保存**当前仍有效的项目事实源**。历史版本与已关闭过程材料统一进入 `../99_归档/sillytavern/`。

## 当前入口

### Rolling current

- `酒馆游戏项目开发核心总纲_CURRENT.md`：当前项目解释层，优先读取。
- `酒馆游戏新版主体重建总路线 v2.0.md`：当前总路线。
- `G8网页产品化启动规划_v1.8_2026-08-18.md`：当前 G8 产品化 / UAT 修复规划。
- `G8阶段收口与G9前置裁定_v1.8_2026-08-18.md`：当前 G8→G9 阶段边界。
- `G8-UAT-01_PlayableRuntimeSeed与NarrativeAuthority收口规格_v1.1_2026-08-18.md`：当前 G8 实现任务基线。
- `G8_StageUAT_交互响应性与空壳世界阻塞发现_v1.1_2026-08-18.md`：当前 UAT finding。

### 仍有效的编号核心

- `11_酒馆游戏_G5长期连续性与玩家授权收口裁定_v1.0_2026-08-15.md`
- `12_酒馆游戏_G6_SaveContinueRestore收口裁定_v1.0_2026-08-15.md`
- `13_酒馆游戏_G7_CrashResumeRecovery收口裁定_v1.0_2026-08-16.md`
- `14_酒馆游戏_G8_RuntimeExtensibleUI产品架构裁定_v1.2_2026-08-18.md`
- `15_酒馆游戏_RuntimeContextOrchestration与模块化复杂度控制裁定_v1.1_2026-08-18.md`
- `16_酒馆游戏_RuntimeWorldMaterialization与当局游戏资产演化裁定_v1.1_2026-08-18.md`

编号核心不会因为“日期较早”自动归档；只有同一核心出现明确 superseding current 时，旧版本才进入归档。

## 当前阶段

```text
G1–G7              PASS / CLOSED
G8                  ACTIVE / UAT FIX
Stage UAT           FAIL / BLOCKED
Current Next        G8-UAT-01 implementation
G9                  NOT AUTHORIZED
```

## 文档版本规则

本目录的人类治理文档使用一位小版本：

```text
1.8 → 1.9 → 2.0 → 2.1
```

不得再生成 `1.10 / 1.11 / 1.12`。高频滚动解释层优先固定 `*_CURRENT.md`。
