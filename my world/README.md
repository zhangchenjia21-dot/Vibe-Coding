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

### 1. 产品与项目总纲
[`MY_WORLD_项目启动总纲_CURRENT.md`](./MY_WORLD_项目启动总纲_CURRENT.md)

### 2. 总体规划路线图
[`MY_WORLD_总体规划路线图_CURRENT.md`](./MY_WORLD_总体规划路线图_CURRENT.md)

当前：

```text
Current Phase = G1
Current Task = G1-04
G1-01 = PASS
G1-02 = PASS
G1-03 = PASS
```

### 3. G1 新聊天交接
[`MY_WORLD_G1新聊天交接指令_CURRENT.md`](./MY_WORLD_G1新聊天交接指令_CURRENT.md)

### 4. 独立版 Preflight 与第一阶段计划
[`MY_WORLD_独立版Preflight与第一阶段计划_v1.0_2026-08-25.md`](./MY_WORLD_独立版Preflight与第一阶段计划_v1.0_2026-08-25.md)

### 5. DSH 经验继承矩阵
[`MY_WORLD_DSH经验继承矩阵_v1.0_2026-08-25.md`](./MY_WORLD_DSH经验继承矩阵_v1.0_2026-08-25.md)

## 已完成 G1 证据

### G1-01 — PASS
普通 Windows PowerShell 下 Git metadata 与 Godot `user://` 可写；最小工程正常启动、显示、退出且 Git clean。Codex 内早先的权限失败已定位为 sandbox-only。

### G1-02 — PASS
Standard / non-.NET Godot 4.7.2 Windows x64、GUI / console / CLI、Windows x86_64 export templates、ICU Data 均已验证。GDScript 为当前 provisional spike candidate；最终语言 / Runtime boundary 留给 G1-06；functional Windows export 留给 G1-05。

### G1-03 — PASS
用户已完成人工 Windows UAT：中文显示、长文本滚动、300 段批量追加、持续追加期间 UI 响应、中文输入、Ctrl+Enter、选择 / Ctrl+C、正常退出与 Git clean 均通过。模拟追加不算真实 Provider streaming evidence。

## 当前 G1-04

Outcome：**用一个真实 Provider 证明 Godot Foundation surface 可以完成真实网络请求、SSE 增量输出、cancel、错误态和 UI 非冻结。**

当前 exploratory provider：

```text
DeepSeek Chat Completions
POST https://api.deepseek.com/chat/completions
stream = true
default model = deepseek-v4-pro
```

这是 G1-04 execution choice，不冻结最终产品 Provider。当前 DeepSeek 官方 API 已使用 `deepseek-v4-pro` / `deepseek-v4-flash`；旧 `deepseek-chat` / `deepseek-reasoner` 已退役。

实现约束：

- Provider surface 保持极薄，不建设多 Provider 平台；
- API key 仅从本地 `DEEPSEEK_API_KEY` 环境变量读取；
- UI 只允许显示 key 是否存在，绝不显示 key 值；
- Godot 使用 non-blocking `HTTPClient` + `poll()` / incremental reads；
- cancel 只验证活动生成可中止并恢复 UI；
- deterministic failure test 不携带凭据；
- same-process networking 是 Spike 证据，不是 G1-06 Runtime boundary 裁定。

G1-04 必须等待用户真实 Windows Provider UAT 后才能 PASS。

## 当前原则

> **迁移经验，不迁移宿主债务。**

> **Commodity Foundation, Owned Game Semantics.**

> **Engine-native, not engine-semantic-coupled.**

## 仓库职责

- `Vibe-Coding/my world/`：项目长期治理、产品裁定、架构决策、Roadmap、阶段计划、复盘与任务事实。
- `zhangchenjia21-dot/my-world`：代码、测试、构建、运行、项目级 `AGENTS.md` 与真实实现事实。
- `zhangchenjia21-dot/the-world`：DSH 参考实现、真实试玩证据与前代产品实验，不作为新项目代码模板直接搬迁。
