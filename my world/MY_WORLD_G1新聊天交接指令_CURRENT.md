---
title: my world｜G1 新聊天交接指令
status: current-handoff
version: 1.0
created: 2026-08-25
updated: 2026-08-25
current_phase: G1
current_task: G1-01
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
roadmap: MY_WORLD_总体规划路线图_CURRENT.md
---

# my world｜G1 新聊天交接指令 CURRENT

下面内容用于新聊天启动 `my world` 独立项目开发。

---

## 可直接复制到新聊天

你现在接手一个新的独立游戏项目：**my world**。

不要依赖本聊天之外的记忆，也不要把前代 DSH 项目的实现直接搬过来。先按以下 Authority / Source Manifest 做 freshness 检查，然后从当前阶段 G1 继续。

### 项目位置

```text
项目名：my world
本地项目目录：D:\AI\Projects\my-world
实现仓库：https://github.com/zhangchenjia21-dot/my-world
治理 / 产品仓库：https://github.com/zhangchenjia21-dot/Vibe-Coding
治理目录：Vibe-Coding/my world/
前代参考实现：https://github.com/zhangchenjia21-dot/the-world
Godot 本地目录：D:\AI\Engine
Godot 版本：v4.7.2
```

当前 `zhangchenjia21-dot/my-world` 已完成最小 GitHub bootstrap，并包含：

```text
README.md
AGENTS.md
.gitignore
project.godot
src/main.tscn
```

已验证 Windows 本地工具链：

```text
Godot: 4.7.2.stable.official.ed1daf0bf
Distribution: Standard / non-.NET Windows x64
GUI: D:\AI\Engine\Godot_v4.7.2-stable_win64.exe
Console: D:\AI\Engine\Godot_v4.7.2-stable_win64_console.exe
Git: 2.54.0.windows.1
OS Architecture: X64
```

当前 G1-01 仍未关闭，因为最新本地运行验证发现环境写权限 blocker：

- `git pull` 无法写 `.git/FETCH_HEAD`，报 `Permission denied`；
- Godot 可以启动并初始化 Vulkan / RTX 4070 Laptop GPU；
- 但 Godot 无法创建 `user://logs`、`user://vulkan` 和 shader cache；
- root certificate store 读取失败。

在上述本地写权限问题解决并重新完成最小运行验证前，不得把 G1-01 runtime verification 标记 PASS，也不得提前进入 G1-02。

### Authority / Source Manifest

按以下顺序读取并以 GitHub `main` 最新版为准：

1. 用户在新聊天中的当前明确指令；
2. `Vibe-Coding/AGENTS.md`；
3. `Vibe-Coding/my world/MY_WORLD_项目启动总纲_CURRENT.md`；
4. `Vibe-Coding/my world/MY_WORLD_总体规划路线图_CURRENT.md`；
5. `Vibe-Coding/my world/MY_WORLD_独立版Preflight与第一阶段计划_v1.0_2026-08-25.md`；
6. `Vibe-Coding/my world/MY_WORLD_DSH经验继承矩阵_v1.0_2026-08-25.md`；
7. `zhangchenjia21-dot/my-world` 当前代码 / 测试 / HEAD；
8. 如需生成正式 Agent 任务，再读取 `zhangchenjia21-dot/Skill` 当前最新版：
   - `skill/gpt/lifecycle-dev-process/SKILL.md`
   - `skill/gpt/agent-task-packet/SKILL.md`

历史聊天、旧附件、模型记忆与 The World 的 DSH workaround 不构成 `my world` 当前实现权威。

### 当前产品结论

必须保持：

```text
2D 对话式 AI RPG / 互动小说
本地优先
长期单人
自然语言玩家输入
优秀 AI GM 流式叙事
角色立绘 / 场景图 / 地图 / RPG UI
World Pack / Mod 一级能力
原生 Game / Timeline / Save / Agent Context
```

核心原则：

> 迁移经验，不迁移宿主债务。

> Commodity Foundation, Owned Game Semantics.

> Engine-native, not engine-semantic-coupled.

> UI is a projection of game truth, not a second truth source.

> Source 定义开局前参考；game-local reality 决定开局后的本局真相。

> Model authors candidates; Program / Domain Owner commits reality.

### 明确不要从 The World / DSH 搬来的实现

不要复制：

- DSH Session workaround；
- fresh-session restore seam；
- `fs.watch` Restore workaround；
- 周期性 consolidation；
- DELTAS + 批量 Markdown edit 作为主状态 Runtime；
- 把 Markdown 当游戏数据库；
- DSH plugin lifecycle；
- 为通用 Agent Workspace 设计的 UI / Owner 结构。

保留的是这些实现背后的产品经验，而不是代码形态。

### 当前 Roadmap

阶段固定为：

```text
G1 Foundation & Project Bootstrap
G2 AI Conversation Spine
G3 Persistent Game & Timeline
G4 World Pack & Local Content Foundation
G5 World Semantics & GM Runtime
G6 RPG Experience & 2D Presentation
G7 Long-session Context & Performance
G8 Mod / Authoring Ecosystem
G9 Standalone Alpha / Release Validation
```

任务编码统一为：

```text
G<阶段>-<两位序号>
```

当前：

```text
Current Phase = G1
Current Task = G1-01
```

不要提前大规模实现 G2–G9。

### 现在继续 G1-01

G1-01 的 Outcome：

> **把 `zhangchenjia21-dot/my-world` 初始化成一个可以承载 Godot v4.7.2 Foundation Spike 的最小、干净、可继续开发的项目仓库，并用真实 Windows 运行证据证明 bootstrap 可启动。**

当前 GitHub-side bootstrap 已完成；剩余工作是解决 Windows 本地写权限 blocker 后重新执行 runtime verification。

先做 freshness：

1. 读取上述 current sources；
2. 检查 `my-world` 当前 HEAD / contents；
3. 不可覆盖未知并行改动；
4. 本地工作目录统一使用 `D:\AI\Projects\my-world`；
5. 真实运行 Godot 并保留原始日志；
6. 若仍出现文件写权限错误，先诊断权限环境，不修改游戏架构规避环境问题。

G1-01 允许建立的最小工作面：

```text
README.md
AGENTS.md
.gitignore
project.godot
src/                          # 只创建立即需要的最小内容
tests/                        # 只创建实际测试入口
docs/                         # 只存实现仓库直接需要的说明
```

不要为了“架构完整”创建几十个空目录、万能接口或未来模块。

### G1 的完整目标

G1 必须最终验证：

1. Godot v4.7.2 Windows 工程可运行；
2. 中文长文本与输入体验可用；
3. 一个真实 Provider 可以流式输出到 UI；
4. cancel 可用；
5. 网络请求 / 后台工作不冻结 UI；
6. 本地读写可用；
7. 动态加载立绘 / 场景 / 地图类图片可用；
8. Windows Export 后上述核心能力仍可运行；
9. 基于 Spike 决定 Standard/.NET、GDScript/C# 或混合边界；
10. 基于证据决定 Runtime 与 Godot 同进程还是 Local Runtime Process。

在 G1-GATE 之前，不要冻结 G3 的数据库模型、G4 的完整 World Pack Schema、G5 的 NPC Runtime 或 G6 的完整 UI。

### 非范围

当前明确不要做：

- Multiplayer；
- Server backend；
- Cloud account / cloud save；
- 3D 自由移动；
- 自动生成地图；
- Universal ECS；
- 全世界逐 tick simulation；
- Steam Workshop；
- Local LLM Hosting；
- TTS / STT；
- 复杂脚本沙箱；
- 多 Provider 路由平台；
- 为未来理论需求预造大型兼容层。

### 工作方式

遵循：

```text
focused exploration
→ real executable proof
→ architecture decision
→ small commit
→ reality check
→ next task
```

不要一开始写完整游戏架构。

如果当前聊天具备合法 GitHub 写权限并且用户没有给出相反指令，可以围绕当前明确任务完成必要的创建 / 修改并提交；禁止 destructive Git 操作、覆盖未知并行改动、删除无法确认归属的内容。

如果当前聊天无法真正访问本机 `D:\AI\Engine` 或运行 Godot，则不要假装完成本地验证。此时：

1. 可以完成 GitHub 侧工作；
2. 把需要本地执行的部分整理成一个最小任务包；
3. 明确告诉用户哪一步需要本地 Codex/KimiCode/终端执行；
4. 等真实运行证据回来后再判对应 Gate。

### 第一次回复应做什么

不要重新讨论整个产品愿景。

先完成 freshness / repo-state 检查，然后简短报告：

```text
已读取哪些 current sources
my-world 当前 HEAD / contents
当前 G1-01 是否仍有效
最新本地运行证据与 blocker
接下来准备执行的最小动作
```

如果没有 blocker，继续当前任务；如果有 blocker，先解决 blocker，不提前进入下一任务。