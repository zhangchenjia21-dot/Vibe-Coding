# my world

`my world` 是基于 The World / DSH 长期真实试玩经验启动的独立单人 AI RPG 项目治理目录。

## 项目事实

- 项目名：`my world`
- 本地项目目录：`D:\AI\Projects\my-world`
- 项目实现仓库：`https://github.com/zhangchenjia21-dot/my-world`
- 当前项目仓库状态：G1-01 / G1-02 / G1-03 已 PASS；当前 G1-04 真实 Provider stream / cancel / UI 非冻结 Foundation Spike
- 当前优先游戏引擎：Godot `v4.7.2`
- 已验证 Godot：`4.7.2.stable.official.ed1daf0bf`，Standard / non-.NET Windows x64
- Godot 本地位置：`D:\AI\Engine`
- 前代参考实现：`https://github.com/zhangchenjia21-dot/the-world`

## 当前权威入口

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

## 已完成 G1 证据

G1-01 已证明普通 Windows PowerShell 下 Git metadata 与 Godot `user://` 可写，最小工程正常启动 / 显示 / 退出且 Git clean；Codex 权限失败是 sandbox-only。

G1-02 已证明 Standard / non-.NET Godot 4.7.2 Windows x64、GUI / console / CLI、Windows x86_64 export templates 与 ICU Data 可用。GDScript 为 provisional spike candidate；最终语言 / Runtime boundary 留给 G1-06；functional Windows export 留给 G1-05。

G1-03 已由用户人工 Windows UAT 确认 PASS：中文、长文本滚动、批量 / 持续追加、UI 响应、中文输入、Ctrl+Enter、选择 / Ctrl+C、正常退出与 Git clean 均通过。模拟追加不算真实 Provider stream。

## 当前 G1-04

Outcome：**用一个真实 Provider 证明 Godot Foundation surface 可以完成真实网络请求、SSE 增量输出、cancel、错误态和 UI 非冻结。**

```text
Provider: DeepSeek Chat Completions
POST https://api.deepseek.com/chat/completions
stream = true
default model = deepseek-v4-pro
```

这是 G1-04 exploratory execution choice，不冻结最终产品 Provider。API key 只从本地 `DEEPSEEK_API_KEY` 读取；UI 只显示 key 是否存在，绝不显示值。Godot 使用 non-blocking `HTTPClient`；same-process networking 仅是 Spike 证据。

G1-04 必须等待真实 Windows Provider UAT 后才能 PASS。

## 当前阶段

```text
G1 Foundation & Project Bootstrap
↓
G2 AI Conversation Spine
↓
G3 Persistent Game & Timeline
↓
G4 World Pack & Local Content Foundation
↓
G5 World Semantics & GM Runtime
↓
G6 RPG Experience & 2D Presentation
↓
G7 Long-session Context & Performance
↓
G8 Mod / Authoring Ecosystem
↓
G9 Standalone Alpha / Release Validation
```

## 当前原则

> **迁移经验，不迁移宿主债务。**

> **Commodity Foundation, Owned Game Semantics.**

> **Engine-native, not engine-semantic-coupled.**

## 仓库职责

- `Vibe-Coding/my world/`：长期治理、产品裁定、架构决策、Roadmap、阶段计划与任务事实。
- `zhangchenjia21-dot/my-world`：代码、测试、构建、运行与实现事实。
- `zhangchenjia21-dot/the-world`：DSH 参考实现与产品证据，不作为新项目代码迁移模板。
