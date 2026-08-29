---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-08-29
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G4-02R1
current_execution_task: G4-02R1M1
semantic_owner: GPT
current_execution_owner: Codex
---

# my world｜GPT CONTEXT HANDOFF CURRENT

> 本文件是**接管导航 / 最小充分摘要**，不是新的 Product / Architecture / Status 权威。
>
> 新 GPT 接管时必须先做 Freshness，并以当前 GitHub `main` 的 Product / Principles / Architecture / Roadmap / Status / implementation facts 为准；若本文件与 current source 冲突，以 current source 为准并更新/替换本 handoff。

---

## 1. 接管第一动作｜不要从聊天记忆直接继续

先读取两个仓库当前 `main` HEAD：

```text
Implementation: zhangchenjia21-dot/my-world
Governance:     zhangchenjia21-dot/Vibe-Coding
```

然后按顺序读取：

### Governance current core

1. `AGENTS.md`
2. `my world/MY_WORLD_项目启动总纲_CURRENT.md`
3. `my world/MY_WORLD_核心设计原则_CURRENT.md`
4. `my world/MY_WORLD_架构_CURRENT.md`
5. `my world/MY_WORLD_总体规划路线图_CURRENT.md`
6. `my world/MY_WORLD_CURRENT_STATUS.md`

### Current Source architecture decisions

7. `my world/architecture/source/G4_SOURCE_SEMANTIC_OWNERSHIP_AND_REAUDIT_DECISION.md`
8. `my world/architecture/source/G4_GAME_LOCAL_EVOLVABLE_SEMANTICS_DECISION.md`
9. `my world/architecture/source/G4_T0_SCOPED_SOURCE_AND_POST_T0_CANON_QUARANTINE_DECISION.md`

### Implementation current task / contract

10. `my-world/AGENTS.md`
11. `my-world/docs/tasks/G4-02R1_SOURCE_SEMANTIC_REAUDIT_TASK.md`
12. `my-world/docs/tasks/G4-02R1M1_SOURCE_V0_2_R2_MECHANISM_CORRECTION_TASK.md`
13. `my-world/docs/source/World Pack与Character Card合同v0.2_SEMANTIC_FREEZE.md`
14. `my-world/docs/source/G4-02R1_T0_SCOPED_SOURCE_CONTRACT_ADDENDUM.md`
15. `my-world/docs/source/G4-02R1_T0_CHARACTER_INDIVIDUALITY_ADDENDUM.md`
16. `my-world/docs/source/G4-02R1_FIXED_T0_MULTI_ENTRY_CHARACTER_BINDING_CLARIFICATION.md`
17. `my-world/docs/source/G4-02R1_CROSS_FAMILY_PACKAGE_SHAPE_STABILITY_DECISION.md`

只有现有证据不足时再扩大读取范围；不要默认重新阅读整个历史仓库。

---

## 2. 产品目的与不可牺牲核心

`my world` 是 local-first、single-player-first 的长期 AI RPG。

Primary Purpose：

> 让单个玩家通过自然语言，与优秀 AI GM 在一个长期持续、可保存、可恢复、会自主演化的 2D RPG 世界中长期游玩。

长期原则：

> **Model freedom first. Reversibility over prevention.**
>
> **Narrative richness over artificial brevity.**
>
> **Model authors the world; Runtime makes it durable; Player owns the timeline.**
>
> **玩法应该拉出 Schema；不要让玩法迁就先写好的万能 Schema。**

不要为了可靠性把 GM 变成填表机、状态机或固定格式输出器。

---

## 3. 当前最重要的历史教训｜历史收敛 / 人格倒灌

旧 DSH 长局出现过：玩家改变三国历史后，模型仍因原历史是更低成本模板而把世界重新拉回 canon。

因此当前已冻结：

> **T0-scoped Source / Post-T0 Canon Quarantine.**
>
> **Do not show the model a post-T0 answer and then ask it to forget that answer.**

必须区分：

```text
Immutable Source Package Total Content
!= Selected T0 Source Projection
!= Game-local Canonical Reality
!= Runtime Relevant Set
!= Model-visible Working Set
```

World 普通 Runtime 只获得：

```text
top-level always-safe sections
+ exact selected Entry sections
```

Character 普通 Runtime 只获得：

```text
top-level always-safe sections
+ exact matching T0 profile sections
```

禁止 latest / nearest-year / later-profile / complete-life biography fallback。

原历史可以自然重现，但没有特殊收敛权重：

> **No convergence force. No divergence force. Causality first.**

---

## 4. Character 早期个体性｜特别不要遗漏

防 future leakage 不能把早期人物削成 generic child / generic youth。

正式语义：

> **Age / developmental stage is a capability boundary, not a personality template.**

每个 T0 Character 必须保留截至 T0 已形成的 starting individuality，例如：

- 家庭位置与照料关系；
- 已经历事件；
- 教育 / 社会化环境；
- 已可观察气质、兴趣、恐惧、习惯；
- 能力与局限；
- 知识来源与误解；
- 当前内部张力与开放成长空间。

证据层级允许：

```text
attested pre-T0 evidence
reasonable inference
authored completion
deliberate blank
```

但禁止：

```text
成年 stereotype → 反推童年人格
后期结局 → 证明早期能力
未来 canon → 伪装成 starting disposition
```

人物区分度必须随年龄/证据缩放：2 岁儿童的差异应落在依恋、习惯、感官与即时压力反应，而不是伪造政治人格。

---

## 5. Source v0.2-r2｜语义已经冻结

当前语义阶段：

> **PASS / FROZEN FOR MECHANISM**

两类真实资产族已经完成 full-fidelity 语义迁移与 family joint audit：

```text
World x2
- 汉末三国：天下未定
- 埃瑟维亚：诸界余辉

Character x6
- 刘备
- 曹操
- 孙权
- 莉维娅·塞兰
- 阿德里安·维尔克
- 杜恩·石痕
```

World / Character 使用 rich `semantic_sections` + package-local Markdown/TXT；`section_type` 是开放 hint，不是 universal ontology。

Disclosure：

```text
gm_reference
gm_private
```

但：

```text
Disclosure != Character Knowledge != Player Knowledge
```

复杂人物人格、关系、行为、技能/法术表、知识边界继续允许使用 rich prose / tables，不因为 parser 方便而机器化成巨大 schema。

---

## 6. Game-local evolvable semantics｜不要重新封死世界

Source schema 不是 Living World 的可能性上限。

Final Create 后：

```text
immutable Source
→ Game-local canonical object
→ lived history
→ model/runtime may evolve local semantics
```

模型可以让本局人物产生 Source 未预见的长期语义，例如新价值观、创伤、政治伦理、学派、制度等。

但：

- 不能修改原 Source；
- 不能修改 global Source contract；
- 不能让模型 `ALTER TABLE`；
- stable identity / authority / durability 仍由 Program 管；
- 若 Location / Relationship / Knowledge / Injury / Inventory / Faction 等已有 Domain owner，则 existing Domain wins；
- semantic evolution 必须 durable 且 Save/Restore/Timeline 可逆。

---

## 7. 当前执行状态｜不要重复做 GPT 已冻结的 Meaning

Parent：`G4-02R1`  
Semantic owner：**GPT**  
Semantic/full-fidelity phase：**PASS / FROZEN FOR MECHANISM**  
Overall：**IMPLEMENTATION PENDING**

Current execution task：

> **G4-02R1M1 — Source v0.2-r2 Mechanism Correction**

Current execution owner：**Codex**

Codex 任务只实现 Mechanism：

- v0.2-r2 validator / loader；
- exact-generation fingerprint；
- exact selected World Entry projection；
- exact Character T0 profile projection；
- explicit temporal compatibility states；
- G4-03 regression；
- preserved G4-05 Wizard/Composition regression；
- Godot/Windows evidence。

Codex 不得：

- 改写 2 World + 6 Character semantic content；
- 重新设计 Source semantics；
- 压缩 rich prose；
- 增加 same-family restriction；
- 增加 latest/nearest/later fallback；
- 提前实现 G4-06；
- 恢复被 supersede 的 G4-05R1；
- 假造 portrait；
- destructive rollback。

Codex return ceiling：

> **READY FOR INDEPENDENT REVIEW**

---

## 8. 如果 Codex 还没有回包

新 GPT 不要重复实现 `G4-02R1M1`，也不要重新打开 frozen semantics。

职责是：

- 保持治理 current；
- 回答 Owner 的产品/语义问题；
- 如果 Owner 要派 Codex，使用 repository-native packet；
- 若新讨论改变 frozen architecture，先走 Decision Propagation，再决定是否 supersede task。

---

## 9. 如果 Codex 已回包｜下一个 GPT 的首要任务

进入 **Independent Review**，不是信任“tests pass”。

至少主动检查：

1. real 2 World + 6 Character fixtures 是否真正进入 production loader/projection path；
2. assertion 是否检查实际 projection inventory/content，而不只是数量/存在；
3. early Han Entry 是否真的排除 later Entry/profile 内容；
4. unselected/private bytes 是否仍进入 fingerprint；
5. private/unselected content 是否同时保持 Runtime visibility 隔离；
6. closed temporal coverage 是否正确：刘备 229+、曹操 220+、孙权 263/280 hard incompatible；
7. Afterglow 三个 1287 Entry 是否都 exact match 同一个 1287 profile，而未被误解成 location restriction；
8. cross-world zero-coverage 是否没有被 same-family hard block；
9. no portrait Character 是否可正常加载；
10. G4-03 immutable library / tamper invariants 是否真实回归；
11. G4-05 preserved Wizard/Composition mechanics 是否真实回归；
12. frozen semantic fixture 是否被 Codex 为了过测试偷偷改写。

若发现根问题，按 lifecycle correction budget 处理；同 seam 重复失败不要无限打补丁。

IR 最大可把工程推进到正确的工程状态；不要把 Engineering PASS 冒充未来 G4-07 的 Product PASS。

---

## 10. 后续顺序｜当前不要跳跃

```text
G4-02R1M1 Codex mechanism
→ GPT Independent Review
→ if PASS: resume G4-05 closure/regression
→ G4-06 Atomic Final Create
→ G4-07 First Playable A — Owner UAT
```

G4-07 Owner UAT 必须特别验证：

- Narrative richness；
- Character individuality；
- T0 后人物是否根据本局经历继续发展；
- 改变关键历史前提后是否仍偷偷向 canon 收敛；
- 不强制反历史；
- Context bounded 但不 starved。

G4-05 当前仍 `REWORK / HOLD`；G4-06+ 当前仍 `HOLD`。

---

## 11. 历史资产 / identity 注意事项

Historical evidence repo：

`zhangchenjia21-dot/sillytavern-assets@4a5364a042e41f4c8a69621fc4467956a78703c0`

它是证据，不是 current implementation authority。

`ashtervia` stable asset IDs 已冻结；不要为了拼写美观静默 rename。

汉末三国 authored Entry 采用固定 Entry/T0，而不是构造任意年份 calendar engine。

---

## 12. Git / governance 纪律

- 正式判断前重新读取当前 `main`；
- current explicit Owner instruction > current Product/Principles/Architecture/Roadmap/Status > implementation facts > governance skill > history/memory；
- Freshness != Decision Propagation；
- chat summary / 本 handoff 不能覆盖 current repo authority；
- GPT owns Meaning; Codex owns Mechanism；
- routine build/test/Git/Windows evidence 是 Agent 工作；
- Owner 负责产品方向和真实 UAT；
- 不做 destructive rollback；repair forward。

接管完成后，用一小段文字向 Owner 报告：读取到的 current task、两个 HEAD、是否存在 drift/blocker、下一动作。不要让 Owner重新复述项目历史。
