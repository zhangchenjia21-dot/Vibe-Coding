# my world

`my world` 是基于 The World / DSH 长期真实试玩经验启动的独立单人 AI RPG 项目治理目录。

## 项目事实

- 本地项目目录：`D:\AI\Projects\my-world`
- 实现仓库：`https://github.com/zhangchenjia21-dot/my-world`
- Godot：`4.7.2.stable.official.ed1daf0bf` Standard / non-.NET Windows x64
- 当前：`G2-01 — Application / Game Shell`（尚未开始实现，等待 current Task Packet）
- `G1-01...G1-06`：**PASS**
- `G1-GATE`：**PASS**

## Authority

1. [`MY_WORLD_项目启动总纲_CURRENT.md`](./MY_WORLD_项目启动总纲_CURRENT.md)
2. [`MY_WORLD_核心设计原则_CURRENT.md`](./MY_WORLD_核心设计原则_CURRENT.md)
3. [`MY_WORLD_总体规划路线图_CURRENT.md`](./MY_WORLD_总体规划路线图_CURRENT.md)
4. [`MY_WORLD_Foundation架构决策_v1.0_2026-08-26.md`](./MY_WORLD_Foundation架构决策_v1.0_2026-08-26.md)
5. [`MY_WORLD_G1新聊天交接指令_CURRENT.md`](./MY_WORLD_G1新聊天交接指令_CURRENT.md)
6. [`MY_WORLD_独立版Preflight与第一阶段计划_v1.0_2026-08-25.md`](./MY_WORLD_独立版Preflight与第一阶段计划_v1.0_2026-08-25.md)
7. [`MY_WORLD_DSH经验继承矩阵_v1.0_2026-08-25.md`](./MY_WORLD_DSH经验继承矩阵_v1.0_2026-08-25.md)

`MY_WORLD_核心设计原则_CURRENT.md` 汇总 SillyTavern / World OS / FC2、The World / DSH 与当前 Owner 裁定中跨阶段长期有效的产品与 Runtime 设计。它明确采用 **Model freedom first / Reversibility over prevention**，并修正旧的 prevention-first Narrative / Confirmation / No-Phantom 硬 Gate 解释；G1 已验证的 Godot / GDScript / same-process 等 Foundation 技术结论不受影响。

```text
Current Phase = G2
Current Task = G2-01
G1-01 = PASS
G1-02 = PASS
G1-03 = PASS
G1-04 = PASS
G1-05 = PASS
G1-06 = PASS
G1-GATE = PASS
```

## G1 Closeout

G1 的真实 Windows 证据覆盖 Godot/Git 最小运行、中文长文本与输入、DeepSeek + Kimi Code 的 stream/cancel/UI 非冻结、本地 IO、三类真实 filesystem 图片、Windows export 与 exported EXE 跨启动 probe。没有发现需要放弃 Godot 的 blocker。

## 第一代 Foundation Architecture

- Godot `4.7.2` Standard / non-.NET；
- 第一代 GDScript；
- Godot same-process Runtime，同时保持 Domain / Provider / Persistence 的明确边界；
- JSON/files 用于配置、小型元数据和 portable Source；SQLite 是 G3 authoritative state 的首选评估候选；Event Log/Snapshot 是语义模式；
- UI、Transcript、Markdown、Godot Resource 不是 authoritative gameplay DB；
- 极薄 `send / stream / cancel` Provider adapter；G2 初始使用一个 concrete Provider；
- headless parse、最小 focused tests、有界脱敏本地日志与 Windows exported-executable proof。

## 核心产品 / Runtime 原则

> **迁移经验，不迁移宿主债务。**

> **Commodity Foundation, Owned Game Semantics.**

> **Engine-native, not engine-semantic-coupled.**

> **Model freedom first. Reversibility over prevention.**

> **Model authors the world; Runtime makes it durable; Player owns the timeline.**

> **Source provides inertia; actors create history.**

> **Context stays bounded.**

G2-01 必须由新的 current Task Packet 启动；G1-06 没有实现 G2。