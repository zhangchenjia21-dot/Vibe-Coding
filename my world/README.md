# my world

`my world` 是基于 The World / DSH 长期真实试玩经验启动的独立单人 AI RPG 项目治理目录。

## 项目事实

- 本地项目目录：`D:\AI\Projects\my-world`
- 实现仓库：`https://github.com/zhangchenjia21-dot/my-world`
- Godot：`4.7.2.stable.official.ed1daf0bf` Standard / non-.NET Windows x64
- 当前：`G1-04 — Real Provider Streaming / Cancel Foundation Spike`

## Authority

1. [`MY_WORLD_项目启动总纲_CURRENT.md`](./MY_WORLD_项目启动总纲_CURRENT.md)
2. [`MY_WORLD_总体规划路线图_CURRENT.md`](./MY_WORLD_总体规划路线图_CURRENT.md)
3. [`MY_WORLD_G1新聊天交接指令_CURRENT.md`](./MY_WORLD_G1新聊天交接指令_CURRENT.md)
4. [`MY_WORLD_独立版Preflight与第一阶段计划_v1.0_2026-08-25.md`](./MY_WORLD_独立版Preflight与第一阶段计划_v1.0_2026-08-25.md)
5. [`MY_WORLD_DSH经验继承矩阵_v1.0_2026-08-25.md`](./MY_WORLD_DSH经验继承矩阵_v1.0_2026-08-25.md)

```text
Current Phase = G1
Current Task = G1-04
G1-01 = PASS
G1-02 = PASS
G1-03 = PASS
```

G1-01 已证明本机 Git / Godot runtime 正常；Codex 权限失败为 sandbox-only。

G1-02 已证明 Godot CLI、Windows x86_64 export templates 与 ICU Data 可用；GDScript 只是 provisional spike candidate，最终语言 / Runtime boundary 留给 G1-06。

G1-03 已由用户人工 Windows UAT 确认 PASS：中文、长文本滚动、批量 / 持续追加、UI 响应、输入、选择 / 复制、正常退出与 Git clean 均通过。

## G1-04

Outcome：用一个真实 Provider 证明 Godot 可完成真实网络请求、SSE 增量输出、cancel、错误态和 UI 非冻结。

```text
Provider: DeepSeek Chat Completions
POST https://api.deepseek.com/chat/completions
stream = true
default model = deepseek-v4-pro
```

这是 exploratory execution choice，不冻结最终产品 Provider。API key 只从本地 `DEEPSEEK_API_KEY` 读取；UI 只显示是否设置，不显示 key 值。Godot 使用 non-blocking `HTTPClient`。G1-04 只有在真实 Windows Provider + cancel UAT 后才能 PASS。

## 原则

> **迁移经验，不迁移宿主债务。**

> **Commodity Foundation, Owned Game Semantics.**

> **Engine-native, not engine-semantic-coupled.**
