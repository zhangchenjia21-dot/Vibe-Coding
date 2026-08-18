---
title: 酒馆游戏项目开发核心总纲
status: current-integrated
updated: 2026-08-18
current_path: sillytavern/酒馆游戏项目开发核心总纲_CURRENT.md
supersedes: 酒馆游戏项目开发核心总纲_Obsidian整合版_2026-08-18_第二版经验复盘增量版.md
---

# 酒馆游戏项目开发核心总纲｜CURRENT

> [!abstract] 当前解释层
> 本文件采用固定 `CURRENT` 路径。后续项目总纲更新默认直接更新本文件，不再为每次小增量生成新的时间戳“总纲副本”。历史解释层统一进入 `99_归档/` 与 Git history。

## 0. 当前状态

```text
G1–G7                   PASS / CLOSED
G8                       ACTIVE / STAGE UAT
WEB-04 Host              PASS / CLOSED
WEB-05 Migration         PASS / CLOSED
WEB-08 Multi-action      PASS / CLOSED
Engineering Exit Gate    historical PASS
G8-UAT-01 Implementation PASS / CLOSED
Independent Review       PASS
Stage UAT                AUTHORIZED / NEXT
Current Code Baseline    52d0421bc58449ac8763681816bc7a84de93b385
G9                       NOT AUTHORIZED
```

`G8-UAT-01 v1.1` 已在 exact SHA `52d0421bc58449ac8763681816bc7a84de93b385` 完成独立审核。当前不再有 P0 / P1 engineering blocker；只有项目所有者 Stage UAT 通过后，G8 才可正式 `PASS / CLOSED`。

---

## 1. 当前 active 正式来源

### 当前阶段 / 路线

- `G8_StageUAT_交互响应性与空壳世界阻塞发现_v1.1_2026-08-18.md`
- `G8-UAT-01_PlayableRuntimeSeed与NarrativeAuthority收口规格_v1.1_2026-08-18.md`
- `G8网页产品化启动规划_v1.8_2026-08-18.md`
- `G8阶段收口与G9前置裁定_v1.8_2026-08-18.md`
- `酒馆游戏新版主体重建总路线 v2.0.md`

### 编号核心

- `11_酒馆游戏_G5长期连续性与玩家授权收口裁定_v1.0_2026-08-15.md`
- `12_酒馆游戏_G6_SaveContinueRestore收口裁定_v1.0_2026-08-15.md`
- `13_酒馆游戏_G7_CrashResumeRecovery收口裁定_v1.0_2026-08-16.md`
- `14_酒馆游戏_G8_RuntimeExtensibleUI产品架构裁定_v1.2_2026-08-18.md`
- `15_酒馆游戏_RuntimeContextOrchestration与模块化复杂度控制裁定_v1.1_2026-08-18.md`
- `16_酒馆游戏_RuntimeWorldMaterialization与当局游戏资产演化裁定_v1.1_2026-08-18.md`

### 通用开发治理

- `项目经验/AI驱动项目全生命周期开发流程规范_v1.5_2026-08-18.md`
- `项目经验/第一版_SillyTavern_项目构建经验复盘_Obsidian版.md`
- `项目经验/第二版_SillyTavern_项目构建经验复盘_Obsidian版_2026-08-18.md`
- `Skill/skill/gpt/lifecycle-dev-process/SKILL.md`：current v2.0
- `Skill/skill/gpt/lifecycle-templates/SKILL.md`：current v1.8

历史同族版本不再列入 current source；统一见 `../99_归档/`。

---

## 2. 当前产品与架构核心事实

### 2.1 Engineering Correctness != Playability Completeness

G8 Stage Close 必须同时证明：

```text
Engineering Correctness
+ Real Creation Instance
+ Playability Completeness
+ Integration of Meaning
+ Project Owner UAT
```

Rich Fixture 不能替代正式 Creation → Real Game Instance → Multi-turn UAT。

### 2.2 Narrative / UI Affordance 必须有真实 referent

```text
Visible as concrete interactable
→ authoritative referent + current capability
```

Narrative 不得宣称 Program 未提交的 durable world change；推荐输入不得引用不存在或隐藏的实体 / 地点。

### 2.3 Runtime Context

正式冻结：

```text
Asset Library
!= Package Included
!= Feature Enabled
!= Module Enabled
!= Runtime Relevant
!= Model Visible Working Set
```

以及：

```text
Full Asset Definition != Prompt Payload
Dependency Graph != Context Inclusion Graph
Domain Active != Full Definition Registry Visible
Background deterministic progression != Model Activation
Bounded != Starved
```

模型上下文目标是**最小但充分**，不是最短。

### 2.4 Source Asset / Game-local Asset / Runtime State

```text
Source Asset Library
↓ snapshot / bind
Game-local Canonical Assets
↓ instantiate / project
Authoritative Runtime State
```

- Source Asset 不被本局反写；
- Game-local Canonical Assets 是本局可持续演化的世界定义；
- Runtime State 管理位置、数值、持有、timer、Turn / Event 等运行状态；
- Model 可以 author candidate / typed patch；
- Program / Domain Owner 负责 validation、stable identity、atomic commit、persistence、recovery 与 disclosure boundary。

### 2.5 World Growth != Prompt Growth

当局游戏资产和 Runtime 世界可以持续增长；普通回合 Context 仍必须保持 current-relevant、bounded、purpose-built。

---

## 3. G8-UAT-01 独立审核结论

Exact SHA：`52d0421bc58449ac8763681816bc7a84de93b385`

结论：

```text
G8-UAT-01 Implementation = PASS / CLOSED
P0 = 0
P1 = 0
Stage UAT = AUTHORIZED / NEXT
```

已独立确认：

1. non-world-changing Candidate 不再吞掉模型已报告的 world-changing intent；
2. Narrative Formal Outcome 强制携带 location / interactable authority；
3. 正式 Creation compiler 生成 Minimum Playable T0：真实 NPC、相邻 Scene、Connection，以及有资源语义时的真实 Item；
4. Narrative Context 恢复 player-safe 背景 / 目标 / 经历 / 人格 / 语言风格与 NPC public description；
5. 五推荐不再等于 `availableMoves.map(...)`，而是 server-side grounded Player Assistance；
6. 点击建议只 prefill，正式发送后才进入 Player Input；
7. Exact UAT 语料与 Real DeepSeek smoke 均有 production-equivalent vertical evidence；
8. 未启动 G9 / WEB-06 / WEB-07 / general JIT world generation。

当前非阻塞事项：

- **P2**：`manual / modified_suggestion / verbatim_suggestion` 的分类、传输与 best-effort hook 已实现，但正式 Launcher 尚未注入持久 Player Assistance evidence sidecar；当前人格分析未依赖该数据，因此不阻塞 Stage UAT。
- **P2**：Recommendation Provider 在成功与 throw-fallback 路径会缓存，但部分“结构错误但未抛异常”的 fallback 分支未写 cache，可能在相同上下文重复产生 Provider 费用；不影响正确性或可玩性。
- **P3**：`人格与快捷输入契约.ts` 顶部仍保留旧阶段“不实现建议生成”的注释，属于过时注释清理。

---

## 4. 当前 Stage UAT Gate

项目所有者只需验证产品体验，不承担内部技术 QA。

重点：

- 正式 Creation 创建的新世界首次进入 Session 时，不再是空壳；
- 输入框上方稳定出现 5 个与当前场景相关的建议；
- 建议点击只填入输入框，可原样发送或修改后发送；
- 与真实 NPC 普通对话能得到自然口头回应；
- 玩家主动回忆经历 / 目标时能使用真实 Creation 内容；
- 不存在的具体地点不会被 Narrative 假装已进入；
- 前往真实可达 destination 后，Header / Narrative / 后续推荐都反映新的 authoritative Scene；
- 不再出现具体但不可交互的 Narrative-only phantom entity。

Stage UAT 若无 blocker：

```text
G8 PASS / CLOSED
→ G9 前架构缺口审计
→ G9-01 Compatibility Audit
```

---

## 5. G9 当前 DAG

```text
G9-01 Compatibility Audit
→ G9-02 Runtime Asset Binding
      + Context Orchestration Foundation
      + Game-local Canonical Asset Layer
      + Runtime World Materialization Foundation
→ G9-03 asset-spec vNext Machine Contract
→ G9-04 Game Asset Adapter / Compiler
→ G9-05 Creator rebuild
```

G9-02 必须先用内部 / handwritten runtime profiles 证明真实能力，再冻结 G9-03 machine schema。

---

## 6. 当前 Next

```text
Project Owner Stage UAT re-run
↓
G8 PASS / CLOSED（若 UAT PASS）
↓
G8 UAT 暴露问题总复盘 + G9 架构缺口审计
↓
G9-01 Compatibility Audit
```

---

## 7. 文档治理与版本规则

### Active-only

同一文档族在 active 目录只能保留一个 current。superseded 旧版移入：

`../99_归档/`

独立有效的编号核心不因日期旧而归档。

### Rolling current

高频滚动解释层优先固定路径，例如本文件 `*_CURRENT.md`，避免每个小变化生成新文件。

### 人类可读版本号

本项目治理文档不采用 SemVer 多位 minor，采用一位小版本：

```text
v1.8 → v1.9 → v2.0 → v2.1
```

`N` 只允许 `0–9`；不得再生成 `v1.10 / v1.11 / v1.12`。版本号只表示演进顺序，重大变化由 `status / supersedes / change_class / ADR` 说明。
