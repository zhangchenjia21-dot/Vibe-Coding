---
title: my world｜独立版 Preflight 与第一阶段计划
status: current-plan
version: 1.6
created: 2026-08-25
updated: 2026-08-26
stage: G2 AI Conversation Spine (G1 closed)
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

因此启动的 Standalone Preflight / G1 Foundation Spike 已于 2026-08-26 完成并通过；项目现进入 G2。

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
当前 GitHub 状态：G1-01～G1-06 与 G1-GATE 全部 PASS；Current Phase G2；Current Task G2-01

Godot 本地位置：D:\AI\Engine
Godot 版本：4.7.2.stable.official.ed1daf0bf
Godot Distribution：Standard / non-.NET Windows x64
GUI：D:\AI\Engine\Godot_v4.7.2-stable_win64.exe
Console：D:\AI\Engine\Godot_v4.7.2-stable_win64_console.exe
Git：2.54.0.windows.1
OS Architecture：X64
Renderer：Vulkan / Forward+
GPU：NVIDIA GeForce RTX 4070 Laptop GPU

前代参考实现：
D:\AI\Projects\the world
https://github.com/zhangchenjia21-dot/the-world
```

G1-01 已通过真实 Windows runtime verification：普通 PowerShell 下 Git metadata 与 Godot `user://` 可写；最小工程正常启动并显示 `my world / G1 Foundation Spike`；Godot exit code = 0；退出后 `git status --short` 无输出。

此前在 Codex 执行环境里出现的 `.git/FETCH_HEAD`、`user://logs`、`user://vulkan` 与 shader cache 写入失败已定位为 Codex execution sandbox 边界，不是 Windows ACL 或项目 blocker。不要为 sandbox-only 错误修改系统 ACL 或游戏架构。

注意：`D:\AI\Engine` 是引擎安装位置，不作为游戏项目目录。

---

## 4. Preflight P0｜项目初始化

### 4.1 建立实现仓库最小骨架

当前已建立并持续演进：

```text
README.md
AGENTS.md
.gitignore
project.godot
src/main.tscn
src/<current G1 spike script>
```

`src/main.tscn` 最初是无脚本、语言中立的最小启动场景；从 G1-03 开始允许把它演进为立即需要的 Foundation Spike 测试面。`assets/`、`tests/`、`docs/` 等只在出现真实实现 / 验证需求时创建，不为了架构完整提前制造空目录。

G1-01 / Gate MW-F0 已 PASS。

### 4.2 记录本地工具链

已确认：

- Godot `4.7.2.stable.official.ed1daf0bf`；
- Standard / non-.NET Windows x64；
- GUI：`D:\AI\Engine\Godot_v4.7.2-stable_win64.exe`；
- Console：`D:\AI\Engine\Godot_v4.7.2-stable_win64_console.exe`；
- Git `2.54.0.windows.1`；
- OS Architecture `X64`；
- Vulkan / Forward+ 可正常初始化；
- CLI 提供 `--export-release` / `--export-debug` / `--export-pack`；
- Godot 4.7.2 Windows x86_64 export templates 已安装并验证；
- ICU Data 已安装并验证，可用于后续中文相关导出测试。

G1-06 final first-generation decision：

- 第一代 Host = Godot 4.7.2 Standard / non-.NET；
- 第一代 Language = GDScript，Domain 不依赖 Scene / Node / Resource 生命周期；
- 第一代 Runtime = Godot same-process，Domain / Provider / Persistence 保持显式内部边界；
- 只有 G3/G5/G7 的真实证据才重审 C#/.NET/mixed 或 Local Runtime Process；
- 当前不安装 .NET，不实现 IPC。

**G1-02 已 PASS。G1-05 exported-executable proof 也已由 Owner UAT PASS。**

G1-03 也已由用户真实 Windows UAT 确认 PASS：中文显示、长文本滚动、300 段批量追加、持续追加期间 UI 响应、中文输入、Ctrl+Enter、选择 / Ctrl+C、正常退出与 Git clean 均通过。

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

对应任务：`G1-03`，当前 **PASS**。

#### B. 流式 AI 输出

当前用户修正决定：G1-04 不再只验证一个 Provider，而是必须验证 **DeepSeek + Kimi** 两个真实 Provider。

必须证明：

- DeepSeek 可以发起真实请求并获得 HTTP 2xx；
- DeepSeek 文本可逐步流式显示；
- Kimi 可以发起真实请求并获得 HTTP 2xx；
- Kimi 文本可逐步流式显示；
- 两家都不只是等待完整响应后一次性显示；
- 活动生成可以取消；
- Provider/API 错误不会让整个 UI 失效。

当前 G1-04 concrete endpoints：

```text
DeepSeek
POST https://api.deepseek.com/chat/completions
stream = true
default model = deepseek-v4-pro
key env = DEEPSEEK_API_KEY

Kimi Code API
POST https://api.kimi.com/coding/v1/chat/completions
stream = true
default model = k3
key env = KIMI_CODE_API_KEY
optional model override = MY_WORLD_G1_04_KIMI_MODEL
```

两者复用极薄的 OpenAI-compatible HTTP/SSE seam，但 host/path/key/model 显式分离。Kimi Code 替换当前 G1-04 的旧 concrete 配置，不保留兼容 fallback。**不建设自动路由、fallback mesh、负载均衡、Provider plugin framework 或 account system。**

Owner Windows UAT 已证明 DeepSeek 与 Kimi Code 的 real stream / cancel / cancel 后重试、请求期间 heartbeat / UI 响应、idle 切换、deterministic failure、正常退出与 Git clean；G1-04 = PASS。G1-05 也已由 exported-executable Owner UAT PASS。两家长输出完整生成约 30 秒不是本 Gate blocker，后续在 G2 拆分 TTFT 与 generation throughput 观察。

#### C. 后台工作与 UI 响应

- DeepSeek 请求进行时 UI 不冻结；
- Kimi 请求进行时 UI 不冻结；
- 本地 persistence / context preparation 进行时 UI 不冻结；
- 能区分主叙事流与后台工作状态。

G1-04 通过 UI heartbeat + manual response counter 先证明网络请求不冻结主循环。

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

G1-05 已通过该 Host / IO seam；它没有冻结正式数据库或 Schema。G1-06 只把 SQLite 冻结为 G3 authoritative state 的首选评估候选。

#### E. 美术资源

至少验证：

- 动态加载一张角色立绘；
- 动态加载一张场景图；
- 动态加载一张地图；
- 不要求第一轮有最终美术风格。

G1-05 已在 exported Windows EXE 中验证 portrait / scene / map 三类真实 filesystem 图片并 PASS。

#### F. Windows Export

必须验证：

> **不是只在 Godot Editor 里能跑，而是导出的 Windows build 也能完成上述关键路径。**

该硬 Gate 已由 G1-05 exported-executable Owner UAT PASS。

---

## 6. Preflight P2｜Runtime Boundary Decision

G1-06 已比较 Godot 内部 Runtime 与 Godot Client + Local Runtime Process，并裁定第一代采用 **Godot same-process Runtime**。

主要理由：G1-04 已证明 non-blocking Provider stream/cancel 不冻结 UI，G1-05 已证明同进程 IO/export 可靠；独立进程会现在引入没有证据支撑的 IPC、协议、Windows packaging 与生命周期成本。

明确代价：Host 与 Runtime 共享故障域，后台任务必须主动调度；未来提取时要把显式内部边界序列化。Domain / Provider / Persistence 必须保持独立于 Scene Tree 的职责边界，业务模块遵循 `L3 -> L2 -> L1 -> L0`，跨模块只经 L3。

最早证伪：G2 持续检查 stream 期间 UI 响应；G3-01 检查事务提交、SQLite 接入和 Domain testability。出现不可避免的主循环阻塞、崩溃隔离需求、成熟库障碍或 G5/G7 长任务生命周期问题时，重审 Local Runtime Process。当前不做 IPC。

完整记录：`MY_WORLD_Foundation架构决策_v1.0_2026-08-26.md`。

---

## 7. Preflight P3｜第一阶段技术裁定

当前：**PASS**。

```text
Host                  Godot 4.7.2
Distribution          Standard / non-.NET Windows x64
Language              GDScript
Runtime               Godot same-process Runtime
Persistence           JSON/files + SQLite evaluation + Event Log/Snapshot semantics
Initial Provider      DeepSeek deepseek-v4-pro
Tests/Diagnostics     headless parse + focused tests + bounded redacted local logs
Packaging             tracked export preset + ignored build + exported EXE proof
```

Kimi Code 是已验证 Foundation alternate，不是自动 fallback。SQLite 的正式 binding / Schema / migration 留给 G3；本阶段没有实现它们。Canonical 记录见 `MY_WORLD_Foundation架构决策_v1.0_2026-08-26.md`。

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
- 自动 Provider 路由 / fallback mesh / 通用 Provider marketplace；
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

当前：**PASS**。

### Gate MW-F1｜Foundation Spike

PASS 条件：

- 中文长文本；
- 玩家输入；
- DeepSeek 真实流式模型输出；
- Kimi 真实流式模型输出；
- cancel；
- UI 不冻结；
- local IO；
- portrait / scene / map asset；
- Windows export；
- Runtime boundary 有实验证据。

当前：**PASS**。G1-01～G1-06 与 G1-GATE 已全部关闭。

### Gate MW-V1｜First Real Vertical

PASS 条件：第 8、9 节完整路径实际通过，并由玩家人工确认：

> **“这是一个比 DSH 更原生、但没有失去 The World 核心乐趣的独立 AI RPG 脊柱。”**

---

## 13. 当前执行顺序

```text
1. G1-01 Project Bootstrap                          PASS
2. G1-02 Godot 工具链 / 语言候选确认               PASS
3. G1-03 2D 中文长文本 / 输入 Foundation Spike    PASS
4. G1-04 DeepSeek + Kimi real stream / cancel      PASS
5. G1-05 local IO / image / Windows Export Spike   PASS
6. G1-06 Foundation Architecture Decision          PASS
7. G1-GATE Foundation Gate                         PASS
8. G2-01 Application / Game Shell                  NEXT / NOT STARTED
```

G2-01 必须先有新的 current Task Packet；本 Preflight closeout 不授权实现。

---

## 14. Stop Conditions

出现以下情况时，不得因为“已经写了很多代码”继续推进：

- Godot Host seam 明显妨碍流式 AI RPG；
- DeepSeek 或 Kimi 的真实流式 / cancel seam 暴露出当前 Host 方案的结构性 blocker；
- Windows export 与 Editor 行为严重不一致；
- Runtime boundary 让游戏语义过度依赖 Godot 内部对象；
- persistence 方案导致 Save / Timeline 语义无法自然成立；
- Vertical 实际体验明显差于 DSH baseline；
- 为了解决一个小问题开始预造大量通用基础设施。

遇到 Stop Condition：回到 Foundation / Architecture 裁定，不用功能堆叠掩盖问题。
