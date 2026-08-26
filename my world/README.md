# my world

`my world` 是基于 SillyTavern / The World / DSH 多轮开发与长期真实试玩经验启动的独立单人 AI RPG 项目治理目录。

## 项目事实

- 本地项目目录：`D:\AI\Projects\my-world`
- 实现仓库：`https://github.com/zhangchenjia21-dot/my-world`
- Godot：`4.7.2.stable.official.ed1daf0bf` Standard / non-.NET Windows x64
- 当前阶段：`G2 — AI Conversation Spine`
- 当前 Task：以 [`MY_WORLD_G2_CURRENT_STATUS.md`](./MY_WORLD_G2_CURRENT_STATUS.md) 为准
- `G1-01...G1-06`：**PASS**
- `G1-GATE`：**PASS**
- `G2-01 Application / Game Shell`：**PASS — Owner UAT**
- `G2-02 Provider Adapter v0.1`：**PASS — Engineering**

## Authority

1. [`MY_WORLD_项目启动总纲_CURRENT.md`](./MY_WORLD_项目启动总纲_CURRENT.md)
2. [`MY_WORLD_核心设计原则_CURRENT.md`](./MY_WORLD_核心设计原则_CURRENT.md)
3. [`MY_WORLD_时间线存档与可逆性架构_CURRENT.md`](./MY_WORLD_时间线存档与可逆性架构_CURRENT.md) — Save / Restore / Timeline / Reversibility supporting architecture
4. [`MY_WORLD_声明式UIHost架构_CURRENT.md`](./MY_WORLD_声明式UIHost架构_CURRENT.md) — G2-03 / G6 / G8 UI supporting architecture；其中旧的 arbitrary `回到这里` 建议受时间线架构 supersede
5. [`MY_WORLD_G2_CURRENT_STATUS.md`](./MY_WORLD_G2_CURRENT_STATUS.md) — G2 current Task / PASS / UAT status
6. [`MY_WORLD_总体规划路线图_CURRENT.md`](./MY_WORLD_总体规划路线图_CURRENT.md)
7. [`MY_WORLD_Foundation架构决策_v1.0_2026-08-26.md`](./MY_WORLD_Foundation架构决策_v1.0_2026-08-26.md)
8. [`MY_WORLD_独立版Preflight与第一阶段计划_v1.0_2026-08-25.md`](./MY_WORLD_独立版Preflight与第一阶段计划_v1.0_2026-08-25.md)
9. [`MY_WORLD_DSH经验继承矩阵_v1.0_2026-08-25.md`](./MY_WORLD_DSH经验继承矩阵_v1.0_2026-08-25.md)

历史 G1 handoff 只保留历史价值，不再拥有 G2 current execution status。

## 核心产品 / Runtime 原则

> **迁移经验，不迁移宿主债务。**

> **Commodity Foundation, Owned Game Semantics.**

> **Engine-native, not engine-semantic-coupled.**

> **Model freedom first. Reversibility over prevention.**

> **Reversibility ≠ frictionless arbitrary rewind.**

> **Model authors the world; Runtime makes it durable; Player owns the timeline.**

> **Save Point != Timeline Node.**

> **Source provides inertia; actors create history.**

> **Context stays bounded.**

## 第一代 Foundation Architecture

- Godot `4.7.2` Standard / non-.NET；
- 第一代 GDScript；
- Godot same-process Runtime，同时保持 Domain / Provider / Persistence 明确边界；
- JSON/files 用于配置、小型元数据和 portable Source；SQLite 是 G3 authoritative World/Timeline state 的首选评估候选；
- UI、Transcript、Markdown、Godot Resource 不作为 authoritative gameplay DB；
- 极薄 Provider adapter；G2 初始 product-facing Provider = DeepSeek `deepseek-v4-pro`；
- headless parse、最小 focused tests、有界脱敏日志与 Windows exported-executable proof。

## 声明式 UI Host 架构

SillyTavern 第二版后期已经真实实现并关闭过一版 `Runtime-extensible UI Host`。`my world` 不迁移旧 React / Browser / HTTP 代码，但正式继承其经过验证的 Host 语义。

第一代桌面长期骨架：

```text
Left   Player Host
Center Narrative Host
Right  World Surface Host
```

对应产品意图：

```text
左：主角立绘 / 身份 / 属性 / 高频状态
中：AI GM Narrative / 玩家自然语言输入 / Turn actions
右：概览 / 人物 / 关系 / 任务 / 物品 / 地图 / Save / Timeline / Extension Surface
```

具体右侧 Tab 数量和命名当前不冻结。

正式演化路径：

```text
G2    fixed Godot UI + stable Host Slots
↓
G3–G5 real Domain/player-safe projections
↓
G6    Internal Declarative UI Host
↓
G8    external World Pack / Mod declarative UI contract
```

> **Host capability first; external asset protocol second.**

Definition 只声明“表达什么 / 放在哪里 / 使用什么安全组件”；Runtime/Projection 提供 live truth；Godot Host 决定 rendering、layout、responsive、Theme 和安全交互。

外部资产不得执行任意 GDScript、读取任意 Runtime path 或直接修改 authoritative state。

详细 canonical 架构见 [`MY_WORLD_声明式UIHost架构_CURRENT.md`](./MY_WORLD_声明式UIHost架构_CURRENT.md)。

## Save / Timeline / Reversibility

新的正式边界：

```text
Cancel / Regenerate
= 高频、低风险、靠近 Narrative

Save / Load
= 明确玩家意图、长期恢复点

Timeline
= 首先是 Runtime 内部历史 / 恢复基础设施

Arbitrary per-turn rewind
= Deferred，不是第一代默认玩家功能
```

读取旧 Save 时，第一代架构应优先保证当前未来不会被立即不可逆销毁；G3 再选择最小可靠的 recovery checkpoint / old-head / internal-branch 方案。

详细 canonical 架构见 [`MY_WORLD_时间线存档与可逆性架构_CURRENT.md`](./MY_WORLD_时间线存档与可逆性架构_CURRENT.md)。

## 当前 G2

G2-01 已通过 Owner UAT；视觉粗糙属于 deferred polish。

G2-02 Provider Adapter 已 Engineering PASS。

当前 G2-03 Narrative Conversation View 正在建立真实 `输入 → DeepSeek streaming Narrative → Cancel / Regenerate` 主路径与三 Host Slot。该任务不实现 Save / Timeline / arbitrary rewind，因此新的时间线架构裁定不要求 G2-03 返工。
