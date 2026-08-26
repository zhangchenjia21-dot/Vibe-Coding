---
title: my world｜Foundation 架构决策
status: accepted-supporting-decision
version: 1.0
created: 2026-08-26
updated: 2026-08-26
stage_decided: G1
current_stage: G2
canonical_map: ../../MY_WORLD_架构_CURRENT.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# Foundation 架构决策 v1.0

## 0. 状态与解释边界

本文件记录 G1-01～G1-05 真实 Windows 证据后冻结的第一代 Foundation 技术决策。

它不拥有 Current Task；当前状态看 `../../MY_WORLD_CURRENT_STATUS.md`。

G1 之后形成的 `Model freedom first / Runtime makes it durable` 核心原则 supersede 本文件早期可能被解释为 prevention-first 的 `Model authors candidates / Program commits reality` 表述。Foundation 的 Godot、GDScript、same-process、Provider/Persistence 边界继续有效。

当前第一代结果：

```text
Host                         Godot 4.7.2
Distribution                 Standard / non-.NET Windows x64
First-generation language    GDScript
Runtime boundary             Godot same-process Runtime
Persistence candidate        JSON/files + SQLite evaluation + Event Log/Snapshot semantics
Initial product Provider     DeepSeek deepseek-v4-pro
Foundation Gate              PASS
```

这些是第一代最小边界，不是永久技术承诺。

---

## 1. 证据基线

G1 真实证明：

- 普通 Windows 环境的 Git / Godot / `user://` 可写与正常启动退出；
- Godot 4.7.2 Standard / non-.NET、Vulkan/Forward+、CLI、Windows export templates、ICU Data 可用；
- 中文长文本、持续追加、输入、选择/复制、UI 响应可用；
- DeepSeek + Kimi Code 真实 HTTP、incremental stream、cancel、post-cancel request、failure handling、UI 非冻结；
- `user://` 跨启动 IO probe；
- portrait / scene / map 的真实 filesystem 图片加载；
- Windows export 与 exported EXE 直接运行。

没有发现必须放弃 Godot 的 blocker。

DSH 的长局证据要求未来拥有 Timeline/Restore、NPC/Faction 自主演化和 bounded context，但 DSH Session、Markdown Runtime DB、周期 consolidation、Host workaround 不迁移。

---

## 2. 跨决策不变量

- `Game / World / Timeline / Save Point / Agent Context / Conversation / NPC / Faction / World Pack` 是 `my world` Domain，不是 Godot Scene / Node / Resource 别名；
- UI、Transcript、Markdown 与 Godot Resource 不是 authoritative gameplay truth；
- Source 定义可复用起点，开局后 game-local reality 权威；
- Domain / Provider / Persistence 保持明确边界；
- 业务模块内部依赖 `L3 -> L2 -> L1 -> L0`，跨模块经对方 L3；
- 不为形式完整创建空层、空类、IPC、通用 Provider 平台或通用 persistence 平台；
- 模型自由与 Narrative 产品边界以当前 `MY_WORLD_核心设计原则_CURRENT.md` 为准。

---

## 3. DEC-A｜Host = Godot 4.7.2

**Decision**：Godot 4.7.2 是第一代正式 Host。

**Why**：G1 已连续证明窗口、中文 UI、输入、非阻塞 HTTP、stream/cancel、文件 IO、动态图片与 exported EXE。

**Alternatives deferred**：Unity、其它成熟 2D Desktop/Game Foundation。

**Known cost**：必须主动防止 Domain 退化为 SceneTree state。

**Revisit trigger**：出现无法用小型边界修复的核心 UI/输入、streaming、Windows packaging 或长期运行 blocker。

---

## 4. DEC-B｜Standard / non-.NET + GDScript

**Decision**：第一代继续 Standard/non-.NET Windows x64，业务实现使用 GDScript；Domain 不依赖 Scene/Node/Resource 生命周期。

**Why**：全部 G1 evidence 都来自 Standard build；GDScript 已覆盖当前 UI、网络、IO、export，没有 .NET 必要性的真实证据。

**Alternatives deferred**：C#/.NET、mixed。

**Known cost**：复杂规则的类型、测试和 CPU 性能可能将来遇到上限。

**Revisit trigger**：G3/G5/G7 出现可复现的 testability/performance/library gap，且小型 GDScript 边界无法解决。

---

## 5. DEC-C｜Same-process Runtime

**Decision**：第一代采用 Godot same-process Runtime；内部保留 Domain / Provider / Persistence 边界，当前不做 IPC。

**Why**：G1 已证明同进程 non-blocking Provider 不冻结 UI、同进程 IO/export 可靠；独立进程现在只会增加 IPC、协议、packaging、生命周期成本。

**Known cost**：Host 与 Runtime 共享故障域；后台任务必须主动调度。

**Revisit trigger**：不可避免主循环阻塞、需要独立 crash isolation、成熟库只能在独立 runtime 自然接入，或 G5/G7 长任务生命周期无法满足响应要求。

---

## 6. DEC-D｜Persistence Candidate Range

**Decision**：

- JSON/files：设置、小型 metadata、portable World Pack Source、非权威 cache；
- SQLite：G3 authoritative World/Timeline 的首选评估候选；
- Event Log/Snapshot：可组合的 Timeline/恢复语义模式，不默认全量 event sourcing；
- Markdown、Transcript、UI state、Godot Resource：不得成为 authoritative gameplay DB。

**Why**：G1-05 只证明文件 IO seam；DSH 已证伪周期 consolidation + Markdown DB 作为可靠即时主状态机制。

**不在 G1 冻结**：SQLite binding、Schema、migration、backup format。

**Revisit trigger**：G3 proof 证明 SQLite 在当前 Godot Standard 路径不成熟或另一嵌入式事务存储以更小复杂度满足相同不变量。

Save/Timeline 的当前产品语义看 `../persistence/时间线存档与可逆性设计.md`。

---

## 7. DEC-E｜Provider / Product Configuration

**Decision**：Provider Adapter 极薄，只覆盖必要 `send/start / stream / cancel / completion / failure`；endpoint/model 与 secrets 分离。

G2 初始 product-facing Provider：

```text
DeepSeek
model = deepseek-v4-pro
key env = DEEPSEEK_API_KEY
optional model override = MY_WORLD_DEEPSEEK_MODEL
```

Kimi Code `k3` 是 Foundation 已验证 alternate，不自动 routing/fallback，也不是当前同时维护的产品 Provider。

**Hard boundary**：secret 不进入 Git、日志、UI、截图或聊天。

**Deferred**：generic registry、fallback mesh、account platform、marketplace。

---

## 8. DEC-F｜Testing / Diagnostics / Packaging

第一代工程基线：

- 相关变更运行 Godot console/headless parse 或等价最小启动；
- 确定性 Domain 逻辑出现时建立最小 focused command-line test；
- 日志有界、脱敏，不能记录 key/Authorization；
- Error UX 可读，不能 silent fail；
- export preset 可跟踪，`build/` ignored；
- 产品面变更最终必须由真实 exported product path 验证；
- routine QA 由执行 Agent 完成，Owner 只做真实体验 Gate。

GUI automation 必须精确识别 executable/PID，不能只按模糊窗口标题匹配，更不能终止身份不明进程。

---

## 9. G1-GATE 结论

```text
Godot Windows foundation              PASS
Chinese long text/input               PASS
real model stream/cancel              PASS
UI responsiveness during network      PASS
local IO / filesystem images          PASS
Windows exported EXE                  PASS
Runtime boundary                      PASS
language/toolchain                     PASS
blocker requiring host replacement    NONE OBSERVED
```

因此：**G1-GATE = PASS**。

此后所有阶段按 Roadmap 和固定 `MY_WORLD_CURRENT_STATUS.md` 推进；本 Foundation 决策只作为长期技术底座和重审触发器。
