---
title: my world｜G1 新聊天交接指令
status: current-handoff
version: 1.5
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
7. `zhangchenjia21-dot/my-world` 当前代码 / 测试 / HEAD。

历史聊天、旧附件、模型记忆与 The World 的 DSH workaround 不构成 `my world` 当前实现权威。

### 已完成事实

```text
G1-01 Repository Bootstrap                PASS
G1-02 Toolchain & Language Confirmation   PASS
G1-03 Chinese Long Text / Input Spike     PASS
Current Phase                             G1
Current Task                              G1-04
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

### 当前 G1-04 修正决定

用户已明确修正：G1-04 **不能只接 DeepSeek，还必须加入 Kimi**。

因此当前 required real Providers = **DeepSeek + Kimi**。这项决定 supersede 任何旧的“G1-04 只接一个实际 Provider / 当前 concrete Provider 只有 DeepSeek”的表述。

```text
DeepSeek
POST https://api.deepseek.com/chat/completions
stream = true
default model = deepseek-v4-pro
key env = DEEPSEEK_API_KEY
optional model override = MY_WORLD_G1_04_DEEPSEEK_MODEL

Kimi / Moonshot AI
POST https://api.moonshot.ai/v1/chat/completions
stream = true
default model = kimi-k3
key env = MOONSHOT_API_KEY
optional model override = MY_WORLD_G1_04_KIMI_MODEL
```

Kimi 当前官方 API 与 OpenAI API 格式兼容，并支持 `stream: true`。本轮只复用极薄的共同 HTTP/SSE seam，不建设通用多 Provider 平台。

### G1-04 实现边界

- provisional GDScript；
- Godot non-blocking `HTTPClient`；
- `poll()` 驱动网络状态；
- incremental response body → SSE `data:`；
- `data: [DONE]` 完成；
- Provider 下拉框显式选择 DeepSeek / Kimi；
- 两家 host/path/key/model 分开配置；
- real streamed text 直接追加到 Godot reading surface；
- cancel 通过关闭活动 transport 验证中断与 UI recovery；
- UI heartbeat + 手动 response counter 验证主循环；
- `127.0.0.1:1` 无凭据 deterministic failure path；
- 不做自动路由、fallback mesh、负载均衡、账户系统、通用 Provider registry；
- same-process networking 不等于 G1-06 Runtime boundary。

### Secret 规则

真实 key 只允许存在于用户本机进程环境变量：

```text
DEEPSEEK_API_KEY
MOONSHOT_API_KEY
```

UI 可以显示 `已设置 / 未设置`，**绝不能显示 key 值**。禁止把 key 写进 Git、`.gd` / `.tscn` / `project.godot`、console、截图或聊天。

### G1-04 PASS 必须由真实 Windows UAT 证明

1. UI 只显示两个 key 是否已设置，不显示值；
2. DeepSeek 返回真实 HTTP 2xx；
3. DeepSeek 内容在生成未完成时增量出现；
4. Kimi 返回真实 HTTP 2xx；
5. Kimi 内容在生成未完成时增量出现；
6. 两家请求期间 heartbeat 持续增加，`UI 响应 +1` 可点击；
7. 对一个真实活动生成执行 Cancel，生成停止且 UI 立即恢复；
8. Cancel 后至少再成功完成一次真实请求；
9. idle 时可在 DeepSeek / Kimi 间切换，无需重启；
10. deterministic failure path 明确且不冻结；
11. Provider/API 错误可读；
12. 正常退出；
13. Git clean。

**任一家没有真实 stream 证据，G1-04 都不能 PASS，也不能进入 G1-05。**

### G1 剩余边界

G1-05：local IO / dynamic portrait-scene-map images / functional Windows export。

G1-06：根据真实 Spike 裁定 Godot Host、Standard/.NET、GDScript/C#/mixed、Provider/product configuration boundary、Runtime process boundary 与第一阶段 persistence candidate range。

### 工作方式

```text
focused exploration
→ real executable proof
→ architecture decision
→ small commit
→ reality check
→ next task
```

聊天无法运行用户本机 Godot / Provider 时，只完成 GitHub-side implementation 并给出普通 Windows PowerShell 最小验证命令，不得假装本地网络 PASS。
