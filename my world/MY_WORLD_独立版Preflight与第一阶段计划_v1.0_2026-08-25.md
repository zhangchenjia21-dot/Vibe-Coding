---
title: my world｜独立版 Preflight 与第一阶段计划
status: current-plan
version: 1.0
created: 2026-08-25
updated: 2026-08-25
stage: Standalone Preflight
owner: Project Owner + GPT Architecture
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜独立版 Preflight 与第一阶段计划 v1.0

## 1. Why Now

The World / DSH 已经证明核心 RPG 体验可以成立，但长局开始明显受宿主限制：

- 生成上下文越来越重；
- edit / consolidation 越来越慢；
- Workspace projection 阶段性滞后；
- Save / Restore 需要绕 DSH Session；
- RPG UI、Timeline、Game State 都需要通过插件适配通用 Agent Host。

当前判断：

> DSH 版已经完成最重要的产品探索使命，继续优化宿主边界的边际价值开始低于启动独立版的价值。

因此进入 `my world` Standalone Preflight。

---

## 2. Preflight 目标

Preflight 不负责“把游戏做出来”。

它只回答：

> **我们是否已经拥有足够清楚的产品定义、技术基底和首个 Vertical 边界，可以安全开始独立游戏开发，而不把 The World 的宿主债务照搬进新项目？**

Preflight 完成后才进入大规模 Runtime / UI 实现。

---

## 3. 当前已知环境

```text
项目名：my world
本地项目目录：D:\AI\Projects\my-world
实现仓库：https://github.com/zhangchenjia21-dot/my-world
当前 GitHub 状态：G1-01 最小 bootstrap 已建立

Godot 本地位置：D:\AI\Engine
Godot 版本：4.7.2.stable.official.ed1daf0bf
Godot Distribution：Standard / non-.NET Windows x64
Git：2.54.0.windows.1
OS Architecture：X64

前代参考实现：
D:\AI\Projects\the world
https://github.com/zhangchenjia21-dot/the-world
```

当前本地验证 blocker：

- Git 无法写 `.git/FETCH_HEAD`，报 `Permission denied`；
- Godot 可以启动并初始化 Vulkan / NVIDIA GeForce RTX 4070 Laptop GPU；
- Godot 无法创建 `user://logs`、`user://vulkan` 与 shader cache；
- root certificate store 读取失败。

因此 G1-01 的 GitHub bootstrap 已完成，但最小 runtime proof 尚未 PASS。先解决执行账户 / 文件系统写权限，不通过修改游戏架构规避环境问题。

注意：`D:\AI\Engine` 是引擎安装位置，不作为游戏项目目录。

---

## 4. Preflight P0｜项目初始化

### 4.1 建立实现仓库最小骨架

当前已建立：

```text
README.md
AGENTS.md
.gitignore
project.godot
src/main.tscn
```

`src/main.tscn` 是无脚本、语言中立的最小启动场景。`assets/`、`tests/`、`docs/` 等只在出现真实实现 / 验证需求时创建，不为了架构完整提前制造空目录。

### 4.2 记录本地工具链

已确认：

- Godot `4.7.2.stable.official.ed1daf0bf`；
- Standard / non-.NET Windows x64；
- GUI：`D:\AI\Engine\Godot_v4.7.2-stable_win64.exe`；
- Console：`D:\AI\Engine\Godot_v4.7.2-stable_win64_console.exe`；
- Git `2.54.0.windows.1`；
- OS Architecture `X64`。

尚未冻结：

- 第一阶段开发语言候选；
- 第一 Provider 的本地开发接入方式；
- Windows Export 路径；
- Runtime process boundary。

不要提前安装与第一阶段无关的大量 SDK。

---

## 5. Preflight P1｜Godot Foundation Spike

### 5.1 Spike 定位

这是 exploration：

```text
NOT canonical architecture
NOT production commitment
```

目标不是做漂亮 Demo，而是验证最危险的 Host seam。

### 5.2 必须验证的能力

#### A. 基础 2D / UI

- Windows 窗口正常运行；
- 中文字体和长文本正常；
- 文本区域可持续追加大量内容；
- 玩家输入框可用；
- 滚动 / 选择 / 复制等基础阅读体验不异常。

#### B. 流式 AI 输出

- 可以发起一个实际模型请求；
- 文本可逐步流式显示；
- 玩家可以看到生成过程而不是等完整响应；
- 可以取消正在进行的生成；
- 错误不会让整个 UI 失效。

#### C. 后台工作与 UI 响应

- 网络请求进行时 UI 不冻结；
- 本地 persistence / context preparation 进行时 UI 不冻结；
- 能区分主叙事流与后台工作状态。

#### D. 本地 Persistence

验证最小：

```text
write local state
↓
close app
↓
reopen
↓
read same state
```

此处只测试 Host / IO seam，不提前冻结正式数据库或 Schema。

#### E. 美术资源

至少验证：

- 动态加载一张角色立绘；
- 动态加载一张场景图；
- 动态加载一张地图；
- 不要求第一轮有最终美术风格。

#### F. Windows Export

必须验证：

> **不是只在 Godot Editor 里能跑，而是导出的 Windows build 也能完成上述关键路径。**

这是 Foundation Spike 的硬 Gate。

---

## 6. Preflight P2｜Runtime Boundary Spike

Foundation Spike 期间必须比较至少两个边界方案，不必做完整实现：

### 方案 A｜Godot 内部 Runtime

```text
Godot UI
+
游戏 Runtime / Provider / Persistence
同进程
```

优势可能是简单、部署少；风险是 AI / Persistence / UI 逻辑逐渐耦合进 Godot 生命周期。

### 方案 B｜Godot Client + Local Runtime

```text
Godot Game Client
        ↕
Local The World Runtime
        ↕
Model Provider / Persistence
```

优势可能是：

- 游戏语义更独立；
- Provider / Persistence / Agent Context 更容易测试；
- 更符合 Engine Adapter 思路。

风险可能是：

- 增加本地 IPC / 进程生命周期；
- packaging 更复杂。

### 裁定原则

不能因为“架构看起来漂亮”就选择 B，也不能因为“A 写得快”就永久耦合。

按以下问题裁定：

1. 哪个方案让 Godot 更像 presentation / game host，而不是 world semantics owner？
2. 哪个方案对流式 AI、取消、后台任务和测试更自然？
3. 哪个方案的 Windows 打包更可靠？
4. 哪个方案第一阶段开发复杂度更低？
5. 如果未来增加 Local Model / Mod / 自动化测试，哪个边界更有余量？

Spike 后只冻结当前最小可行边界，不设计通用服务平台。

---

## 7. Preflight P3｜第一阶段技术裁定

Foundation Spike 通过后，再冻结以下最小技术决定：

- Godot 是否正式作为第一代 Host；
- Godot 具体版本；
- GDScript / C# / 混合边界；
- Runtime 同进程还是独立本地进程；
- 第一阶段 persistence 技术；
- 第一 Provider；
- 最小测试框架；
- 最小 logging / crash diagnostics；
- Windows build / packaging 路径。

只冻结第一 Vertical 真正依赖的决定。

---

## 8. 第一真实 Vertical Slice

### 8.1 核心问题

第一个 Vertical 不问：

> “我们能不能做很多 RPG 功能？”

只问：

> **离开 DSH 后，我们能不能在自己的 Host 里重新跑出一个长期 AI RPG 的最小完整脊柱？**

### 8.2 必须跑通的路径

```text
启动 my world
↓
新建本地 Game
↓
加载一个极小测试 World Pack
↓
创建角色 / 进入世界
↓
与一个真实模型 GM 对话
↓
发生至少一个 durable world mutation
↓
UI 立即看到对应当前状态
↓
关闭游戏
↓
重新打开
↓
继续同一世界
↓
建立 Save Point
↓
继续产生明显“未来事实”
↓
Restore 到旧 Save
↓
世界回到旧状态
↓
新的 Agent Context 不知道被回滚掉的未来
```

### 8.3 Vertical 必须包含的最小游戏内容

- 一个测试 World Pack；
- 一个玩家角色；
- 2–3 个重要 NPC；
- 一个地点 / 地图；
- 一个可改变的长期世界事实；
- 一个能看出 NPC 自主性的互动；
- 一个需要不同风险结构的 meaningful choice；
- Save / Restore。

不需要大世界、大量内容或最终美术。

---

## 9. 第一 Vertical 的 Product Acceptance

### A. GM Quality

与同模型在 The World / DSH 的简单基线相比，文本质量和自由度不能明显退化。

### B. Persistent World

关闭 / 重开后世界仍是同一个世界。

### C. Immediate State Consistency

一次 durable mutation 完成后，对应 canonical state 和玩家 projection 应立即一致，不依赖“等十回合 consolidation”。

### D. NPC Agency

至少一个 NPC 必须出现：

- 自己的目标；
- 非玩家触发的行动；
- 对玩家方案有真实独立判断；
- 不因一次高骰点轻易丢掉人格底线。

### E. Meaningful Choice

至少一个场景中的不同方案应拥有不同：

- 可行性；
- DC / 能力门槛；
- 优势 / 劣势；
- 或失败代价。

不能只是不同文案 → 同一个骰子。

### F. Native Save / Restore

Restore 后：

- world state 回滚；
- UI 回滚；
- Agent Context 回滚；
- 不存在未来聊天知识泄漏。

### G. Player Experience

玩家主阅读流应始终优先：

> **叙事、角色、场景和游戏状态，而不是 Agent 工程工作噪音。**

---

## 10. World Pack v0.1 的 Preflight 边界

World Pack 必须从第一 Vertical 出现，但初版保持极薄。

概念上只要求能承载：

```text
identity / manifest
world instructions
source lore
characters
map
portraits / scenes
optional mechanic references
```

第一阶段不做：

- 通用脚本市场；
- 复杂 Mod dependency solver；
- Steam Workshop；
- 任意代码沙箱；
- 自动地图生成；
- 通用内容编辑器。

先证明：

> **一个作者制作的 World Pack 能被安装 / 加载，并成为新 Game 的 Source。**

---

## 11. 第一阶段不做事项

在 Vertical Slice PASS 前，禁止主线扩张到：

- Multiplayer；
- Server backend；
- Account system；
- Cloud save；
- 3D；
- 自动地图生成；
- 语音；
- Local LLM hosting；
- 全世界 tick simulation；
- 通用 ECS；
- 大型事件总线；
- Universal Schema；
- 完整插件市场；
- 大型内容编辑器；
- 为“以后可能需要”提前做的大量扩展点。

---

## 12. Stage Gates

### Gate MW-F0｜Project Bootstrap

PASS 条件：

- `my-world` 仓库初始化；
- 项目级 `AGENTS.md` 生效；
- Godot 版本登记；
- 最小运行项目可启动；
- Git / build 基础路径明确。

当前：仓库初始化、AGENTS、版本登记与 GitHub-side 最小工程已完成；“最小运行项目可启动”仍因本地写权限 blocker 未证明，因此 MW-F0 / G1-01 尚未 PASS。

### Gate MW-F1｜Foundation Spike

PASS 条件：

- 中文长文本；
- 玩家输入；
- 流式真实模型；
- cancel；
- UI 不冻结；
- local IO；
- portrait / scene / map asset；
- Windows export；
- Runtime boundary 有实验证据。

### Gate MW-V1｜First Real Vertical

PASS 条件：第 8、9 节完整路径实际通过，并由玩家人工确认：

> **“这是一个比 DSH 更原生、但没有失去 The World 核心乐趣的独立 AI RPG 脊柱。”**

---

## 13. 当前执行顺序

```text
1. 完成 my-world G1-01 bootstrap runtime proof（当前：先解决本地写权限 blocker）
2. 完成 Godot 工具链 / 语言候选确认
3. Godot Foundation Spike
4. Runtime Boundary Spike
5. 冻结第一阶段最小技术栈
6. 实现 First Real Vertical
7. 玩家真实试玩
8. 只修实际阻塞
9. Vertical PASS 后再扩地图 / 立绘 / 人物 UI / World Pack 作者体验
```

---

## 14. Stop Conditions

出现以下情况时，不得因为“已经写了很多代码”继续推进：

- Godot Host seam 明显妨碍流式 AI RPG；
- Windows export 与 Editor 行为严重不一致；
- Runtime boundary 让游戏语义过度依赖 Godot 内部对象；
- persistence 方案导致 Save / Timeline 语义无法自然成立；
- Vertical 实际体验明显差于 DSH baseline；
- 为了解决一个小问题开始预造大量通用基础设施。

遇到 Stop Condition：回到 Foundation / Architecture 裁定，不用功能堆叠掩盖问题。
