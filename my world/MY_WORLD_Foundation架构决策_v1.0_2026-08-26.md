---
title: my world｜Foundation 架构决策
status: current-architecture-decision
version: 1.0
created: 2026-08-26
updated: 2026-08-26
stage: G2 AI Conversation Spine
decision_scope: first-generation-foundation
implementation_repo: https://github.com/zhangchenjia21-dot/my-world

---

# my world｜Foundation 架构决策 v1.0

## 1. 决策状态

G1-01～G1-05 的真实 Windows 证据已经足以冻结第一代最小技术边界：

```text
Host                         Godot 4.7.2
Distribution                 Standard / non-.NET Windows x64
First-generation language    GDScript
Runtime boundary             Godot same-process Runtime
Persistence candidate        JSON/files + SQLite evaluation + Event Log/Snapshot semantics
Initial product Provider     DeepSeek deepseek-v4-pro
Foundation Gate              PASS
Current Phase                G2
Current Task                 G2-01
```

本文件冻结的是第一代最小边界，不是永久技术承诺。G2-01 尚未开始；任何实现必须等待新的 current Task Packet。

## 2. 证据基线

- G1-01：普通 Windows 环境中 Git / Godot / `user://` 可写，最小工程可启动、退出并保持 Git clean。
- G1-02：Godot `4.7.2.stable.official.ed1daf0bf` Standard / non-.NET、Vulkan / Forward+、CLI、Windows export templates 与 ICU Data 可用；没有安装或必须引入 .NET 的证据。
- G1-03：真实 Windows UAT 证明中文长文本、持续追加、输入、选择 / 复制和 UI 响应可用。
- G1-04：DeepSeek 与 Kimi Code 的真实 HTTP、增量 stream、cancel、cancel 后重试、idle 切换、明确失败路径和 UI 非冻结均通过。
- G1-05：`user://` 跨启动 probe、portrait / scene / map 真实 filesystem 图片加载、Windows export、导出 EXE 直接运行和再次启动后的 probe 保留均通过 Owner UAT。
- DSH 长局证据要求未来支持即时权威 mutation、Timeline / Restore、NPC / Faction 自主演化与 bounded context，但禁止把 DSH Session、consolidation、Markdown Runtime DB 和宿主 workaround 迁入新项目。

## 3. 跨决策不变量

- `Game / World / Timeline / Save Point / Agent Context / Conversation / NPC / Faction / World Pack` 是 `my world` Domain，不是 Godot Scene / Node / Resource 的别名。
- UI、Transcript、Markdown 与 Godot Resource 都不是 authoritative gameplay truth。
- Source 提供起点与惯性；开局后 game-local reality 权威。
- 模型可以提出候选；确定性 Program / Domain Owner 提交权威现实。
- 业务模块依赖方向为 `L3_外交层 -> L2_流程层 -> L1_器件层 -> L0_公理层`；允许向下跳层，禁止向上依赖。
- 跨模块只通过对方 `L3_外交层`；Bootstrap 负责组合根与依赖注入。
- 不为形式完整创建空层、空类、IPC、通用 Provider 平台或通用 persistence 平台。

## 4. DEC-A｜Host

**Decision**

Godot `4.7.2` 正式成为 `my world` 第一代 Host。

**Evidence**

G1-01～G1-05 已在真实 Windows 环境连续证明窗口、中文 UI、输入、非阻塞 HTTP、stream/cancel、文件 IO、动态图片与导出 EXE。没有观察到需要放弃 Godot 的 blocker。

**Alternatives**

Unity；成熟 2D Desktop App Foundation；理论上的其他游戏引擎。

**Why rejected / deferred**

替换 Host 会放弃已经验证的整条 Foundation seam，而当前没有对应的 Godot 失败证据。Unity 或桌面应用基底只保留为未来真实 blocker 出现后的比较对象。

**Known cost**

Domain 与引擎生命周期必须主动隔离；Godot 生态不能自动定义 World / Timeline / Save 语义。

**Failure mode**

产品 Domain 逐渐退化为 Scene Tree 状态，导致测试、恢复或后台演化被引擎对象生命周期绑死。

**Earliest falsification test**

G2-01 / G2-04 检查 Application Shell 与 Turn/Conversation Domain 能否保持“Host 组合、Domain 拥有语义”。

**Revisit trigger**

出现可复现且无法用小型边界修复的 Host blocker：核心 UI/输入、Provider 流式、Windows packaging 或长期运行可靠性无法满足产品路径。

## 5. DEC-B｜Distribution 与语言边界

**Decision**

第一代继续使用 Standard / non-.NET Windows x64；业务实现使用 GDScript。核心 Domain 不得依赖 Scene / Node / Resource 生命周期，Godot-facing adapter 和 Bootstrap 可以使用 Godot API。

**Evidence**

当前安装与全部 G1 证据均来自 Standard build。GDScript 已覆盖 UI、异步 HTTP、文件与导出路径；没有 .NET-enabled Godot、.NET SDK 或 C# 必要性的真实证据。

**Alternatives**

C#/.NET；GDScript + C# mixed。

**Why rejected / deferred**

现在引入会增加工具链、导出与调试矩阵，却没有解决已观察到的问题。mixed 会在没有真实职责分界前增加跨语言契约成本。

**Known cost**

GDScript 的静态类型、测试生态和 CPU 密集逻辑能力可能早于产品目标遇到上限；必须通过边界与 focused tests 抑制动态语言漂移。

**Failure mode**

复杂 World / Timeline 规则难以可靠测试，或 G5/G7 的确定性计算与长期任务出现可量化性能瓶颈。

**Earliest falsification test**

G3-01 的 persistence/domain proof：用纯数据、确定性规则和 focused automated tests 验证 GDScript 是否足以表达权威 mutation 与恢复不变量。

**Revisit trigger**

G3/G5/G7 出现可复现的测试性、性能或成熟库接入缺口，且小型 GDScript 边界无法解决；届时再比较 C#/.NET 或 mixed，并明确具体职责。

## 6. DEC-C｜Runtime Process Boundary

**Decision**

第一代采用 **Godot same-process Runtime**。Domain、Provider、Persistence 以内部清晰边界协作，不把 World / Timeline 放入 Scene Tree；当前不建设 IPC 或第二进程。

**Evidence**

G1-04 已证明同进程非阻塞 Provider stream/cancel 不冻结 UI；G1-05 已证明同进程文件与导出路径可靠。单进程是已执行的简单基线。

**Alternatives**

Godot Client + Local Runtime Process。

**Why rejected / deferred**

独立进程会立即增加 IPC、协议版本、Windows packaging、进程发现、退出与崩溃恢复成本；目前没有主循环阻塞、隔离或库依赖证据证明这些成本合理。

**Known cost**

Host 崩溃与 Runtime 共享故障域；后台任务必须主动调度；未来提取进程需要把内部边界转换为序列化协议。

**Failure mode**

权威提交、SQLite 访问、世界演化或上下文维护阻塞帧循环；或长任务/崩溃隔离无法在同进程安全实现。

**Earliest falsification test**

G2 在真实 stream 期间持续验证 UI 帧响应；G3-01 最小事务 proof 同时测量提交/读取是否阻塞主循环，并验证 Domain 不依赖 Node 生命周期。

**Revisit trigger**

出现以下任一真实证据：无法避免的主循环阻塞；需要独立崩溃隔离；成熟 SQLite/本地模型库只能在独立 Runtime 自然接入；G5/G7 后台长期任务无法在 Godot 调度边界内满足响应与生命周期要求。

**Core Value / DSH / complexity check**

同进程保持最短的玩家叙事反馈路径；显式 Domain 边界避免复制 DSH Host ownership；当前复杂度最低；G2/G3 可以低成本证伪。它是第一代选择，不是永久承诺。

## 7. DEC-D｜Persistence Candidate Range

**Decision**

- JSON / ordinary files：用于设置、少量本地元数据、可移植 World Pack Source 输入与非权威缓存；写入仍需明确失败与原子替换策略。
- SQLite：G3 authoritative World / Timeline 的首选评估候选，重点验证事务提交、约束、查询、备份/恢复与迁移；本阶段不冻结库、绑定或 Schema。
- Event Log / Snapshot：作为 Timeline、审计与恢复的语义模式评估，可与 SQLite 组合；不默认采用全量 event sourcing。
- Markdown、Transcript、UI state、Godot Resource：不得成为 authoritative gameplay database。

**Evidence**

G1-05 证明普通文件 IO seam 可用，但明确没有证明正式 Save。DSH 已证明周期 consolidation 与 Markdown DB 无法提供可靠即时权威 mutation；G3 需要事务型候选。

**Alternatives**

纯 JSON 保存全部世界；Markdown owner；全量 event sourcing；直接以 Godot Resource 保存 Domain。

**Why rejected / deferred**

纯 JSON 对复杂权威状态的并发提交、查询、约束与迁移风险过高；Markdown 与 Transcript 已被 DSH 证伪；全量 event sourcing 与 Resource-owned Domain 都会在 Schema 证据出现前过度冻结实现。

**Known cost**

SQLite 引入绑定、迁移、备份和事务设计成本；Event Log/Snapshot 组合要求明确事件与快照一致性。

**Failure mode**

数据库或日志结构反过来定义产品语义；Restore 恢复世界却泄露未来 Agent Context；写入失败产生半提交状态。

**Earliest falsification test**

G3-01 用最小 fixture 验证一次权威 mutation、关闭/重开、Timeline point、恢复与 future-memory isolation；比较 SQLite 候选和小文件职责是否自然。

**Revisit trigger**

G3 proof 证明 SQLite 绑定在 Godot Standard 下不成熟、事务/备份路径不可靠，或另一嵌入式事务存储以更小复杂度满足同一不变量。

**Core Value / DSH / complexity check**

候选范围直接保护长期可保存/恢复世界；排除 DSH 已证伪主状态路径；不在 G1 设计 Schema；G3 可用真实 fixture 快速证伪。

## 8. DEC-E｜Provider / Product Configuration Boundary

**Decision**

- Provider adapter 保持极薄，只覆盖 `send / stream / cancel` 与必要错误翻译。
- endpoint / model 等非秘密 product config 与 key 来源分离。
- secrets 只来自本地环境或未来受保护本地来源，不进入 Git、日志、UI、截图或聊天。
- G2 初始只运行一个 concrete Provider：DeepSeek `deepseek-v4-pro`。
- Kimi Code `k3` 保留为已验证 Foundation alternate，不自动 routing/fallback，也不作为 G2 同时维护的 product-facing Provider。

**Evidence**

G1-04 证明同一小型 OpenAI-compatible seam 可以支持两个真实 Provider，也证明双 Provider evidence 不需要 generic platform。

**Alternatives**

G2 同时产品化 DeepSeek 与 Kimi Code；generic registry；自动 routing / fallback mesh；账户平台。

**Why rejected / deferred**

这些选项增加配置、错误矩阵和产品行为不确定性，而 G2 首先需要证明一个高质量 AI Conversation Spine。

**Known cost**

单 Provider 会暴露供应商可用性与质量风险；切换 alternate 需要明确的配置与验证步骤，而不是自动兜底。

**Failure mode**

Provider DTO 泄露进 Domain，或秘密混入配置/日志；自动 fallback 在不同模型之间制造不可解释的 Turn 行为。

**Earliest falsification test**

G2-02 以一个 concrete Provider 验证 adapter、错误边界、cancel 与配置分离；G2-06 Owner Playtest 验证实际 GM 质量。

**Revisit trigger**

出现真实的质量、成本或可用性需求，且单 Provider 无法满足；第二正式 Provider 必须有具体产品用例、验收标准和维护预算。

## 9. DEC-F｜Testing / Diagnostics / Packaging

**Decision**

- Parse：每次相关变更运行 Godot console/headless parse 或等价最小启动验证。
- Tests：当出现确定性 Domain 逻辑时，建立最小 command-line test scene/script；不先建设通用测试平台。
- Logs：使用 `user://logs/` 的有界本地文本日志，至少包含 timestamp、severity、subsystem、event 与脱敏 cause；默认不记录 key、Authorization 或完整 prompt。
- Error UX：UI 显示可读失败状态；日志保留脱敏后的原始 cause，不静默失败。
- Packaging：跟踪 `export_presets.cfg`，忽略 `build/` 和生成产物；Windows release 必须从导出 EXE 直接验证。
- Ownership：Agent 完成 routine build、Git、静态检查、自动测试和 debug evidence；Owner 只负责真人产品体验与最终 UAT 裁定。

**Evidence**

G1 的 headless/CLI 验证、导出模板与真实 exported-executable UAT 已证明该最小路径可执行。

**Alternatives**

只做人工测试；现在引入大型测试框架、遥测/崩溃服务或安装器平台。

**Why rejected / deferred**

只做人工测试会把 routine QA 转嫁给 Owner；大型平台在当前代码与失败模式出现前缺乏依据。

**Known cost**

Godot 内最小 test harness 需要维护；本地日志不能替代正式 crash dump；直接 EXE 验证仍需要真人体验边界。

**Failure mode**

自动验证只证明 parse 而遗漏真实产品失败，或日志泄露秘密/无限增长，或 Editor 成功被错误当成 exported runtime 成功。

**Earliest falsification test**

G2-01 运行 headless parse、最小启动与 Windows export；G2 首次确定性 Domain 规则加入 focused test；错误路径检查日志脱敏与有界性。

**Revisit trigger**

真实 crash 无法靠本地 cause 定位；focused tests 规模证明需要成熟框架；发布分发需要安装器、符号或 crash dump 管线。

## 10. G1-GATE 评估

```text
Godot 4.7.2 稳定运行最小 Windows 工程        PASS — G1-01 / G1-02
中文长文本主路径                            PASS — G1-03 Owner UAT
真实模型 stream / cancel                    PASS — G1-04 Owner UAT
后台请求不冻结 UI                           PASS — G1-04 heartbeat / manual response
local IO / dynamic image                     PASS — G1-05 Owner UAT
Windows export 后关键 Spike                  PASS — G1-05 exported EXE UAT
Runtime boundary                             PASS — DEC-C
第一代语言 / toolchain                       PASS — DEC-B
需要放弃 Godot 的 blocker                    NONE OBSERVED
```

结论：**G1-GATE = PASS**。G1-01～G1-06 全部关闭，Current Phase 推进到 G2，Current Task 推进到 G2-01。

## 11. G2 启动边界

G2-01 的唯一当前名称为：

> **G2-01 — Application / Game Shell**

本决策不授权实现 G2-01。新的 Task Packet 必须先完成 freshness，并具体定义 shell、公开边界、验证与 Non-scope。不得从本架构摘要推导并预建 Provider routing、SQLite Schema、World Pack、NPC/Faction evolution 或 long-session platform。
