---
title: my world｜G1 新聊天交接指令
status: current-handoff
version: 1.3
created: 2026-08-25
updated: 2026-08-25
current_phase: G1
current_task: G1-03
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
roadmap: MY_WORLD_总体规划路线图_CURRENT.md
---

# my world｜G1 新聊天交接指令 CURRENT

下面内容用于新聊天接手 `my world` 独立项目开发。

---

## 可直接复制到新聊天

你现在接手一个新的独立游戏项目：**my world**。

不要依赖聊天记忆，也不要把前代 DSH 项目的实现直接搬过来。先按以下 Authority / Source Manifest 做 freshness 检查，然后从当前阶段 G1 / 当前任务 G1-03 继续。

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

### 已完成事实

`G1-01｜实现仓库初始化` 已 **PASS**。

真实 Windows 证据：

```text
Godot: 4.7.2.stable.official.ed1daf0bf
Distribution: Standard / non-.NET Windows x64
GUI: D:\AI\Engine\Godot_v4.7.2-stable_win64.exe
Console: D:\AI\Engine\Godot_v4.7.2-stable_win64_console.exe
Git: 2.54.0.windows.1
OS Architecture: X64
Renderer: Vulkan / Forward+
GPU: NVIDIA GeForce RTX 4070 Laptop GPU
```

G1-01 runtime verification 已确认：

- 普通 Windows PowerShell 下 `git pull --ff-only` 正常；
- `.git` metadata 可写；
- Godot `user://` 可写；
- 最小工程正常启动；
- 窗口显示 `my world` / `G1 Foundation Spike`；
- Godot 正常退出，exit code = 0；
- `git status --short` 退出后无输出。

此前 Codex 内出现的 `.git/FETCH_HEAD`、`user://logs`、`user://vulkan` 写入失败已经定位为 Codex execution sandbox 边界，不是 Windows ACL 或 Godot 项目 blocker。不要为此修改系统 ACL 或游戏架构。

`G1-02｜Godot 4.7.2 工具链与语言确认` 已 **PASS**。

已确认：

- Standard / non-.NET Godot 4.7.2 Windows x64；
- GUI / console executable 与 CLI 正常；
- CLI 提供 `--export-release`、`--export-debug`、`--export-pack`；
- Windows x86_64 export templates 已安装并验证；
- ICU Data 已安装并验证；
- GDScript 是当前 Foundation Spike 的最低依赖 provisional language candidate，但不是 G1-06 最终语言裁定；
- C# 若进入真实候选，需要另行引入 .NET-enabled Godot editor + .NET SDK；
- external local runtime process 不是 G1-03 前置条件；最终 Runtime boundary 仍由 G1-04 / G1-05 证据与 G1-06 决定；
- 完整 Windows functional export proof 仍属于 G1-05。

### Authority / Source Manifest

按以下顺序读取并以 GitHub `main` 最新版为准：

1. 用户在新聊天中的当前明确指令；
2. `Vibe-Coding/AGENTS.md`；
3. `Vibe-Coding/my world/MY_WORLD_项目启动总纲_CURRENT.md`；
4. `Vibe-Coding/my world/MY_WORLD_总体规划路线图_CURRENT.md`；
5. `Vibe-Coding/my world/MY_WORLD_独立版Preflight与第一阶段计划_v1.0_2026-08-25.md`；
6. `Vibe-Coding/my world/MY_WORLD_DSH经验继承矩阵_v1.0_2026-08-25.md`；
7. `zhangchenjia21-dot/my-world` 当前代码 / 测试 / HEAD；
8. 如需生成正式 Agent 任务，再读取 `zhangchenjia21-dot/Skill` 当前最新版。

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

当前：

```text
Current Phase = G1
Current Task = G1-03
G1-01 = PASS
G1-02 = PASS
```

不要提前大规模实现 G2–G9。

### 现在执行 G1-03

`G1-03｜2D 中文长文本 / 输入 Foundation Spike` 的 Outcome：

> **用最小 Godot/GDScript 可执行测试面证明中文文本、长文本滚动、持续追加、玩家输入、选择/复制以及长文本下 UI 可用。**

允许建立的最小工作面：

- 在 `src/main.tscn` 上形成立即可运行的 Spike UI；
- 一个专用 GDScript 脚本；
- Windows 系统字体 fallback 仅作为 G1-03 本地 Host seam 验证，不等于最终发行字体策略；
- 本地模拟逐块追加只用于测试 UI append/scroll，不等于真实 Provider streaming。

G1-03 必须人工观察并记录：

1. 中文无乱码、无明显缺字方块；
2. 初始化大量中文段落后滚动正常；
3. 可继续批量追加文本；
4. 可模拟逐块持续追加且 UI 仍可操作；
5. 玩家输入框可输入中文，并能追加到阅读区；
6. 阅读区文字可鼠标选择并 `Ctrl+C` 复制；
7. 在较大文本量下没有明显卡死、布局崩坏或不可操作；
8. 正常退出后 `git status --short` 只反映预期 GitHub 提交，不出现生成缓存污染。

G1-03 不做：

- 真实 Provider；
- API key / secret；
- persistence；
- World Pack；
- 正式 RPG UI architecture；
- Windows functional export；
- Runtime process boundary 冻结。

### G1 的完整目标

G1 最终必须验证：

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

如果当前聊天无法真正运行本机 Godot，则不要假装完成本地验证；可以完成 GitHub 侧 Spike，实现后给出普通 Windows PowerShell 的最小验证命令，等待真实证据再判 G1-03。

### 第一次回复应做什么

不要重新讨论产品愿景。先完成 freshness / repo-state，然后简短报告：

```text
两个仓库当前 HEAD
G1-01 / G1-02 PASS 是否仍为 current fact
G1-03 是否仍有效
当前实现仓库是否已经有 G1-03 Spike surface
下一步最小动作
```

若无 superseding decision，直接继续 G1-03。