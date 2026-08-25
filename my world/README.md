# my world

`my world` 是基于 The World / DSH 长期真实试玩经验启动的独立单人 AI RPG 项目治理目录。

## 项目事实

- 项目名：`my world`
- 本地项目目录：`D:\AI\Projects\my-world`
- 项目实现仓库：`https://github.com/zhangchenjia21-dot/my-world`
- 当前项目仓库状态：G1-01 最小仓库与 Godot bootstrap 已建立，当前等待解决 Windows 本地写权限 blocker 后完成 runtime verification
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
Current Task = G1-01
```

### 3. G1 新聊天交接

[`MY_WORLD_G1新聊天交接指令_CURRENT.md`](./MY_WORLD_G1新聊天交接指令_CURRENT.md)

用于新的工程开发聊天直接接手当前事实、执行 freshness，并继续 G1-01。

### 4. 独立版 Preflight 与第一阶段计划

[`MY_WORLD_独立版Preflight与第一阶段计划_v1.0_2026-08-25.md`](./MY_WORLD_独立版Preflight与第一阶段计划_v1.0_2026-08-25.md)

定义正式写大规模游戏代码前的 Foundation Spike、首个 Vertical Slice、Stage Gate 与暂不做事项。Godot 的当前已确认版本以本 README 与 current Roadmap 中登记的 `v4.7.2` 为准。

### 5. DSH 经验继承矩阵

[`MY_WORLD_DSH经验继承矩阵_v1.0_2026-08-25.md`](./MY_WORLD_DSH经验继承矩阵_v1.0_2026-08-25.md)

明确哪些 The World / DSH 经验应继承、哪些应重新设计、哪些只是宿主债务不得迁移。

## 当前 G1-01 Reality Check

GitHub 实现仓库已经不再为空，并已建立最小 Godot 4.7.2 工作面。Windows 本地工具链已确认：

```text
Godot 4.7.2.stable.official.ed1daf0bf
Standard / non-.NET Windows x64
Git 2.54.0.windows.1
OS Architecture X64
```

当前本地运行验证发现环境写权限 blocker：

- Git 无法写 `.git/FETCH_HEAD`；
- Godot 无法创建 `user://logs` / `user://vulkan`；
- shader cache 无法创建；
- root certificate store 读取失败。

因此当前仍保持 `G1-01`，不得把 runtime verification 标记 PASS，直到本机写权限问题被真实解决并重新运行最小工程。

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
>
> **通用基底尽量复用，游戏核心语义必须掌握在自己手里。**

> **Engine-native, not engine-semantic-coupled.**

## 仓库职责

- `Vibe-Coding/my world/`：项目长期治理、产品裁定、架构决策、Roadmap、阶段计划、复盘与任务事实。
- `zhangchenjia21-dot/my-world`：代码、测试、构建、运行、项目级 `AGENTS.md` 与真实实现事实。
- `zhangchenjia21-dot/the-world`：DSH 参考实现、真实试玩证据与前代产品实验，不作为新项目代码模板直接搬迁。
