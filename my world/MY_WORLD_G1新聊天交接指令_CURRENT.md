---
title: my world｜G1 新聊天交接指令
status: current-handoff
version: 1.4
created: 2026-08-25
updated: 2026-08-25
current_phase: G1
current_task: G1-04
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
roadmap: MY_WORLD_总体规划路线图_CURRENT.md
---

# my world｜G1 新聊天交接指令 CURRENT

下面内容用于新聊天接手 `my world` 独立项目开发。

## 可直接复制到新聊天

你现在接手一个新的独立游戏项目：**my world**。

不要依赖聊天记忆，也不要把前代 DSH 项目的实现直接搬过来。先按以下 Authority / Source Manifest 做 freshness 检查，然后从当前阶段 G1 / 当前任务 G1-04 继续。

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

### 已完成事实

```text
G1-01 Repository Bootstrap                         PASS
G1-02 Godot 4.7.2 Toolchain & Language            PASS
G1-03 2D Chinese Long Text / Input Spike          PASS
Current Phase                                      G1
Current Task                                       G1-04
```

G1-01 已真实验证：普通 Windows PowerShell 下 Git metadata 与 Godot `user://` 可写；最小工程正常启动、显示、退出；exit code 0；Git clean。Codex 内早先的写权限失败是 sandbox-only。

G1-02 已真实验证：

- Godot `4.7.2.stable.official.ed1daf0bf`；
- Standard / non-.NET Windows x64；
- GUI / console executable 与 CLI；
- Vulkan / Forward+；
- NVIDIA GeForce RTX 4070 Laptop GPU；
- Windows x86_64 export templates；
- ICU Data；
- GDScript = 当前 Foundation Spike provisional lowest-dependency language candidate；
- 完整 Windows functional export proof 仍属于 G1-05；
- GDScript/C#/mixed 与 Runtime boundary 仍由 G1-06 决定。

G1-03 已由用户完成人工 Windows UAT 并确认 PASS：

- 中文正常显示；
- 大量长文本滚动正常；
- 300 段批量追加正常；
- 持续追加期间 UI 保持响应；
- 中文输入与 Ctrl+Enter 正常；
- 阅读区选择 / Ctrl+C 正常；
- 没有明显布局崩坏、卡死或不可操作；
- 正常退出且 Git clean。

G1-03 的 timer 模拟追加不算真实 Provider streaming evidence。

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

不要提前大规模实现 G2–G9。

### 现在执行 G1-04

`G1-04｜真实 Provider 流式调用 Spike` 的 Outcome：

> **用一个真实 Provider 证明 Godot 可以完成真实网络请求、真实 SSE 增量输出、cancel、错误路径，并在请求期间保持 UI 主循环可响应。**

当前 exploratory provider：

```text
DeepSeek Chat Completions
POST https://api.deepseek.com/chat/completions
stream = true
```

这是 G1-04 execution choice，不是最终产品 Provider 决定。

实现边界：

- 使用当前 provisional GDScript Foundation surface；
- 使用 Godot non-blocking `HTTPClient`；
- `poll()` 驱动网络状态；
- incremental response body → SSE `data:` 解析；
- `data: [DONE]` 完成；
- real streamed text 直接追加到 Godot reading surface；
- cancel 通过关闭活动 transport 验证中断与 UI recovery；
- UI heartbeat + 手动 response counter 验证网络期间主循环仍活着；
- `127.0.0.1:1` 无凭据连接用于 deterministic failure path；
- 不建设多 Provider routing/fallback/retry platform；
- 不提前冻结 same-process vs local runtime process。

### Secret 规则

真实 API key 只允许存在于用户本机进程环境变量：

```text
DEEPSEEK_API_KEY
```

可选 G1-04 本地模型覆盖：

```text
MY_WORLD_G1_04_MODEL
```

严禁：

- 把 key 写进 Git；
- 把 key 写进 `.gd` / `.tscn` / `project.godot`；
- 在 UI / console / screenshot 中显示 key 值；
- 把 key 发到聊天。

### G1-04 PASS 必须由真实 Windows UAT 证明

1. 本地 key 已设置，但 UI 不显示 key 值；
2. 真 Provider 返回 HTTP 2xx；
3. 内容在生成尚未完成时逐步出现，而不是最后一次性出现；
4. streaming 期间 heartbeat 持续增加；
5. streaming 期间 `UI 响应 +1` 可点击；
6. cancel 真正中止活动生成并迅速恢复 UI；
7. cancel 后可再次发起真实请求；
8. deterministic failure path 有明确反馈且不冻结；
9. Provider/API 错误有可读错误态而非 silent hang；
10. 正常退出；
11. `git status --short` clean。

没有真实 Provider + cancel 证据不得判 G1-04 PASS。

### G1 剩余边界

G1-05：local IO / dynamic portrait-scene-map images / functional Windows export。

G1-06：根据 G1 真实 Spike 证据裁定 Godot Host、Standard/.NET、GDScript/C#/mixed、Runtime process boundary 与第一阶段 persistence candidate range。

在 G1-GATE 前，不冻结 G3 数据库模型、G4 完整 World Pack Schema、G5 NPC Runtime 或 G6 完整 UI。

### 工作方式

```text
focused exploration
→ real executable proof
→ architecture decision
→ small commit
→ reality check
→ next task
```

如果聊天无法真正运行用户本机 Godot / Provider，则只完成 GitHub-side implementation，并给出普通 Windows PowerShell 的最小验证命令；不得假装本地网络 PASS。
