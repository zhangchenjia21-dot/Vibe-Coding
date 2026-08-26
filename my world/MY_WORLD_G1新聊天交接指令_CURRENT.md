---
title: my world｜G1 收口与 G2 新聊天交接指令
status: current-handoff
version: 1.8
created: 2026-08-25
updated: 2026-08-26
current_phase: G2
current_task: G2-01
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
roadmap: MY_WORLD_总体规划路线图_CURRENT.md

---

# my world｜G1 收口与 G2 新聊天交接指令 CURRENT

下面内容用于新聊天接手 `my world` 独立项目开发。

## 可直接复制到新聊天

你现在接手独立游戏项目 **my world**。不要依赖聊天记忆，也不要把前代 DSH 实现直接搬来。先按 Authority / Source Manifest 对 GitHub `main` 做 freshness，再从 Current Phase G2 / Current Task G2-01 开始；没有新的 current Task Packet 不得实现。

### 项目位置

```text
本地实现：D:\AI\Projects\my-world
实现仓库：https://github.com/zhangchenjia21-dot/my-world
治理仓库：https://github.com/zhangchenjia21-dot/Vibe-Coding
治理目录：Vibe-Coding/my world/
前代参考：https://github.com/zhangchenjia21-dot/the-world
Godot：D:\AI\Engine / 4.7.2
```

### Authority / Source Manifest

1. 用户当前明确指令；
2. `Vibe-Coding/AGENTS.md`；
3. `MY_WORLD_项目启动总纲_CURRENT.md`；
4. `MY_WORLD_总体规划路线图_CURRENT.md`；
5. `MY_WORLD_Foundation架构决策_v1.0_2026-08-26.md`；
6. `MY_WORLD_独立版Preflight与第一阶段计划_v1.0_2026-08-25.md`；
7. `MY_WORLD_DSH经验继承矩阵_v1.0_2026-08-25.md`；
8. `my-world` 当前实现、测试与 HEAD。

历史聊天、旧附件、模型记忆与 DSH workaround 不构成当前实现权威。

### 已完成事实

```text
G1-01 Repository Bootstrap                PASS
G1-02 Toolchain & Language Confirmation   PASS
G1-03 Chinese Long Text / Input Spike     PASS
G1-04 Provider Stream / Cancel Spike      PASS
G1-05 Local IO / Image / Windows Export   PASS
G1-06 Foundation Architecture Decision    PASS
G1-GATE Foundation Gate                   PASS
Current Phase                             G2
Current Task                              G2-01
```

G1-05 Owner UAT 已在直接运行的 Windows exported EXE 中证明 `user://` 跨启动 probe、portrait / scene / map 的真实 filesystem 图片加载，以及重新启动后的 probe 保留。它没有冻结正式 Save Schema、asset pipeline 或 World Pack。

### 第一代 Foundation Architecture

Canonical 决策见 `MY_WORLD_Foundation架构决策_v1.0_2026-08-26.md`：

- Host = Godot `4.7.2`；
- Distribution = Standard / non-.NET Windows x64；
- 第一代语言 = GDScript，Domain 不依赖 Scene / Node / Resource 生命周期；
- 第一代 Runtime = Godot same-process，Domain / Provider / Persistence 保持显式内部边界，当前不做 IPC；
- JSON/files 只负责 config、小型元数据和 portable Source；SQLite 是 G3 authoritative World/Timeline 的首选评估候选；Event Log/Snapshot 是语义模式；
- Markdown、Transcript、UI、Godot Resource 不能成为 authoritative gameplay DB；
- Provider adapter 保持 `send / stream / cancel`；G2 初始只运行 DeepSeek `deepseek-v4-pro`，Kimi Code 是已验证 alternate，不是自动 fallback；
- headless parse + 必要时的最小 focused tests + `user://logs/` 有界脱敏日志 + tracked export preset / ignored build；Agent 做 routine QA，Owner 做最终产品 UAT。

业务模块遵循 `L3 -> L2 -> L1 -> L0`，跨模块只经对方 L3。不要为完整感创建空层、通用平台或未来服务。

### 当前 G2-01 边界

当前只确认任务名：

> **G2-01 — Application / Game Shell**

必须先取得/创建 current Task Packet，并在实现前完成 freshness。不得依据路线图摘要提前实现 Provider routing、persistence Schema、World Pack、NPC/Faction evolution、long-session platform，或重构已通过的 G1 Spike。

### 工作方式

```text
current Task Packet
→ freshness
→ smallest complete implementation
→ automated evidence
→ Owner product UAT when required
→ decision propagation
```

若聊天无法运行真人体验，只返回可自动验证的实现、准确命令与 NOT VERIFIED 边界，不得假装 Windows runtime 或 Product UAT PASS。
