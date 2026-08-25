# my world

`my world` 是基于 The World / DSH 长期真实试玩经验启动的独立单人 AI RPG 项目治理目录。

## 项目事实

- 项目名：`my world`
- 本地项目目录：`D:\AI\Projects\my-world`
- 项目实现仓库：`https://github.com/zhangchenjia21-dot/my-world`
- 当前项目仓库状态：G1-01 / G1-02 / G1-03 已 PASS；当前进入 G1-04 真实 Provider stream / cancel / UI 非冻结 Foundation Spike
- 当前优先游戏引擎：Godot `v4.7.2`
- 已验证 Godot：`4.7.2.stable.official.ed1daf0bf`，Standard / non-.NET Windows x64
- Godot 本地位置：`D:\AI\Engine`
- 前代参考实现：`https://github.com/zhangchenjia21-dot/the-world`

## 当前权威入口

### 1. 产品与项目总纲

[`MY_WORLD_项目启动总纲_CURRENT.md`](./MY_WORLD_项目启动总纲_CURRENT.md)

这是 `my world` 当前唯一的滚动产品 / 项目事实入口。产品方向、核心价值、范围、产品原则与 Decision Ledger 以此为准。

### 2. 总体规划路线图

[`MY_WORLD_总体规划路线图_CURRENT.md`](./MY_WORLD_总体规划路线图_CURRENT.md)

这是唯一 current 开发 Roadmap。开发阶段固定为 `G1 → G9`，阶段内任务统一使用 `G<阶段>-<两位序号>` 编码。

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

用于新的工程开发聊天直接接手当前事实、执行 freshness，并继续 G1-04。

### 4. 独立版 Preflight 与第一阶段计划

[`MY_WORLD_独立版Preflight与第一阶段计划_v1.0_2026-08-25.md`](./MY_WORLD_独立版Preflight与第一阶段计划_v1.0_2026-08-25.md)

定义正式写大规模游戏代码前的 Foundation Spike、首个 Vertical Slice、Stage Gate 与暂不做事项。

### 5. DSH 经验继承矩阵

[`MY_WORLD_DSH经验继承矩阵_v1.0_2026-08-25.md`](./MY_WORLD_DSH经验继承矩阵_v1.0_2026-08-25.md)

明确哪些 The World / DSH 经验应继承、哪些应重新设计、哪些只是宿主债务不得迁移。

## 已完成 G1 证据

### G1-01｜Repository Bootstrap — PASS

真实 Windows 证据：Godot `4.7.2.stable.official.ed1daf0bf`、Standard / non-.NET Windows x64、Vulkan / Forward+、RTX 4070 Laptop GPU；普通 PowerShell 下 Git metadata 与 Godot `user://` 可写；最小工程正常启动、显示、退出且 working tree clean。此前 Codex 内写权限错误已定位为 sandbox-only。

### G1-02｜Toolchain & Language Confirmation — PASS

已确认：

- GUI / console executable 与 CLI；
- Windows x86_64 export templates；
- ICU Data；
- GDScript 是当前 Foundation Spike 的 provisional lowest-dependency language candidate；
- C# / .NET 与最终 Runtime boundary 仍由后续证据和 G1-06 决定；
- 完整 Windows functional export proof 仍属于 G1-05。

### G1-03｜2D 中文长文本 / 输入 Foundation Spike — PASS

用户已完成人工 Windows UAT 并确认 PASS。已真实观察：

- 中文显示正常；
- 大量长文本滚动正常；
- 300 段批量追加可用；
- 持续追加时 UI 保持响应；
- 中文输入与 Ctrl+Enter 可用；
- 阅读区选择 / Ctrl+C 可用；
- 没有明显布局崩坏或不可操作；
- 正常退出且 Git clean。

G1-03 的本地模拟追加只证明 UI append seam，不构成真实 Provider streaming 证据。

## 当前 G1-04

`G1-04｜真实 Provider 流式调用 Spike` 当前 Outcome：

> **用一个真实 Provider 证明 Godot Foundation surface 可以完成真实网络请求、SSE 增量输出、cancel、错误态和 UI 非冻结。**

本轮 exploratory provider 选择为 **DeepSeek Chat Completions**，只用于 G1-04 Foundation evidence，不冻结最终产品 Provider：

```text
POST https://api.deepseek.com/chat/completions
stream = true
```

实现约束：

- Provider surface 保持极薄，不建设多 Provider 平台；
- API key 仅从本地 `DEEPSEEK_API_KEY` 环境变量读取；
- 不提交、不显示、不记录 key 值；
- Godot 使用 non-blocking `HTTPClient` + `poll()` / incremental body reads；
- cancel 只验证当前活动生成可被中止并恢复 UI；
- deterministic failure test 不携带凭据；
- same-process networking 是 Spike 证据，不是 G1-06 Runtime boundary 裁定。

G1-04 必须等待用户真实 Windows 网络 UAT 后才能 PASS。

## 当前阶段

```text
The World / DSH 真实长局验证
        ↓
产品经验抽取
        ↓
my world Product Definition Gate PASS
        ↓
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

当前原则：

> **迁移经验，不迁移宿主债务。**

> **Commodity Foundation, Owned Game Semantics.**

> **Engine-native, not engine-semantic-coupled.**

## 仓库职责

- `Vibe-Coding/my world/`：项目长期治理、产品裁定、架构决策、Roadmap、阶段计划、复盘与任务事实。
- `zhangchenjia21-dot/my-world`：代码、测试、构建、运行、项目级 `AGENTS.md` 与真实实现事实。
- `zhangchenjia21-dot/the-world`：DSH 参考实现、真实试玩证据与前代产品实验，不作为新项目代码模板直接搬迁。
