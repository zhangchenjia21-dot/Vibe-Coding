# my world

`my world` 是基于 The World / DSH 长期真实试玩经验启动的独立单人 AI RPG 项目治理目录。

## 项目事实

- 本地项目目录：`D:\AI\Projects\my-world`
- 实现仓库：`https://github.com/zhangchenjia21-dot/my-world`
- Godot：`4.7.2.stable.official.ed1daf0bf` Standard / non-.NET Windows x64
- 当前：`G1-05 — Local IO / Image / Windows Export Foundation Spike`
- G1-04：**PASS**（DeepSeek + Kimi Code Owner Windows UAT）

## Authority

1. [`MY_WORLD_项目启动总纲_CURRENT.md`](./MY_WORLD_项目启动总纲_CURRENT.md)
2. [`MY_WORLD_总体规划路线图_CURRENT.md`](./MY_WORLD_总体规划路线图_CURRENT.md)
3. [`MY_WORLD_G1新聊天交接指令_CURRENT.md`](./MY_WORLD_G1新聊天交接指令_CURRENT.md)
4. [`MY_WORLD_独立版Preflight与第一阶段计划_v1.0_2026-08-25.md`](./MY_WORLD_独立版Preflight与第一阶段计划_v1.0_2026-08-25.md)
5. [`MY_WORLD_DSH经验继承矩阵_v1.0_2026-08-25.md`](./MY_WORLD_DSH经验继承矩阵_v1.0_2026-08-25.md)

```text
Current Phase = G1
G1-04 = PASS
Current Task = G1-05
G1-01 = PASS
G1-02 = PASS
G1-03 = PASS
```

G1-01 已证明本机 Git / Godot runtime 正常；Codex 权限失败为 sandbox-only。

G1-02 已证明 Godot CLI、Windows x86_64 export templates 与 ICU Data 可用；GDScript 只是 provisional spike candidate，最终语言 / Runtime boundary 留给 G1-06。

G1-03 已由用户人工 Windows UAT 确认 PASS：中文、长文本滚动、批量 / 持续追加、UI 响应、输入、选择 / 复制、正常退出与 Git clean 均通过。

## G1-04 Closeout

Owner Windows UAT 已证明 DeepSeek 与 Kimi Code 的真实 HTTP success、增量 stream、cancel、cancel 后重新发送、请求期间 heartbeat / UI 响应、idle 切换、deterministic failure、正常退出与 Git clean。G1-04 = **PASS**。

两家约 30 秒的长输出完整生成耗时不是 G1-04 blocker；后续在 G2 分开观察 TTFT 与 generation throughput。Provider seam 仍保持极薄，不构成 generic multi-provider platform commitment，也不冻结 G1-06 Runtime boundary。

## G1-05

当前目标：用 Godot 原生能力证明最小 local IO、跨启动 probe、portrait / scene / map 三类真实 filesystem dynamic image load、Windows export 与 exported executable runtime。该 Spike 不冻结正式 persistence、asset pipeline、World Pack schema 或 Save 系统。
## 原则

> **迁移经验，不迁移宿主债务。**

> **Commodity Foundation, Owned Game Semantics.**

> **Engine-native, not engine-semantic-coupled.**
