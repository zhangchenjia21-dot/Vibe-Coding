---
title: my world｜G1 新聊天交接指令
status: current-handoff
version: 1.7
created: 2026-08-25
updated: 2026-08-26
current_phase: G1
current_task: G1-05
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
roadmap: MY_WORLD_总体规划路线图_CURRENT.md
---

# my world｜G1 新聊天交接指令 CURRENT

下面内容用于新聊天接手 `my world` 独立项目开发。

## 可直接复制到新聊天

你现在接手一个新的独立游戏项目：**my world**。

不要依赖聊天记忆，也不要把前代 DSH 项目的实现直接搬过来。先按以下 Authority / Source Manifest 做 freshness 检查，然后从当前阶段 G1 / 当前任务 G1-05 继续。

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
7. `zhangchenjia21-dot/my-world` 当前代码 / 测试 / HEAD。

历史聊天、旧附件、模型记忆与 The World 的 DSH workaround 不构成 `my world` 当前实现权威。

### 已完成事实

```text
G1-01 Repository Bootstrap                PASS
G1-02 Toolchain & Language Confirmation   PASS
G1-03 Chinese Long Text / Input Spike     PASS
Current Phase                             G1
G1-04 Provider Stream / Cancel Spike      PASS
Current Task                              G1-05
```

G1-01 已真实验证：普通 Windows PowerShell 下 Git metadata 与 Godot `user://` 可写；最小工程正常启动、显示、退出；exit code 0；Git clean。Codex 内早先的写权限失败是 sandbox-only。

G1-02 已真实验证 Godot `4.7.2.stable.official.ed1daf0bf` Standard / non-.NET Windows x64、CLI、Vulkan / Forward+、Windows x86_64 export templates 与 ICU Data。GDScript 是 provisional lowest-dependency candidate；最终语言 / Runtime boundary 属于 G1-06；functional Windows export 属于 G1-05。

G1-03 已由用户人工 Windows UAT 确认 PASS：中文、长文本滚动、300 段追加、持续追加期间 UI 响应、中文输入、Ctrl+Enter、选择 / Ctrl+C、正常退出与 Git clean 均通过。

### 当前产品结论

保持：2D 对话式 AI RPG / 互动小说、本地优先、长期单人、自然语言玩家输入、优秀 AI GM 流式叙事、角色立绘 / 场景图 / 地图 / RPG UI、World Pack / Mod 一级能力、原生 Game / Timeline / Save / Agent Context。

核心原则：

> 迁移经验，不迁移宿主债务。

> Commodity Foundation, Owned Game Semantics.

> Engine-native, not engine-semantic-coupled.

> UI is a projection of game truth, not a second truth source.

> Source 定义开局前参考；game-local reality 决定开局后的本局真相。

> Model authors candidates; Program / Domain Owner commits reality.

### G1-04 关闭裁定

Owner 已完成真实 Windows-local UAT，G1-04 = **PASS**：

```text
DeepSeek 完整生成 / Cancel / Cancel 后重试  PASS
Kimi Code 完整生成 / Cancel / Cancel 后重试 PASS
请求期间 heartbeat / UI 响应 +1              PASS
Provider idle 切换                           PASS
deterministic 连接失败测试                   PASS
正常退出 / Git clean                         PASS
```

现行 exploratory Provider 事实保持：

```text
DeepSeek
POST https://api.deepseek.com/chat/completions
default model = deepseek-v4-pro
key env = DEEPSEEK_API_KEY

Kimi Code API
POST https://api.kimi.com/coding/v1/chat/completions
default model = k3
key env = KIMI_CODE_API_KEY
```

两家长输出完整生成约 30 秒不是 G1-04 blocker。后续在 G2 分开观察 TTFT 与 generation throughput；当前不优化 Provider latency，不建设 routing / fallback mesh / generic registry，也不冻结 G1-06 Runtime boundary。

### 当前 G1-05 边界

G1-05｜本地 IO / 图片 / Windows Export Spike，目标只证明：

- `FileAccess` / `DirAccess` 最小本地 probe 写入、读回与跨启动保留；
- portrait / scene / map 三类图片从真实 filesystem path decode/load 后显示；
- Godot 4.7.2 Windows export 成功；
- exported executable 不依赖 Editor，仍能完成 IO 与三类图片 proof。

这是 Foundation exploration：

```text
NOT canonical persistence architecture
NOT final asset pipeline
NOT final World Pack schema
NOT production save system
```

不得开始 G1-06、G2，不得重构已通过的 Provider seam。执行 Agent 的最高状态是 `READY FOR OWNER UAT`；Owner 只承担 5 步以内的 exported EXE 体验确认。

### 工作方式

```text
focused exploration
→ real executable proof
→ architecture decision
→ small commit
→ reality check
→ next task
```

聊天无法运行用户本机 exported EXE 时，只完成可自动验证的实现与准确命令，不得假装 Windows runtime PASS。