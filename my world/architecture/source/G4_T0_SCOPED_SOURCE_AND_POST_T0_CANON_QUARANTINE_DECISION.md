---
title: my world｜T0-scoped Source / Post-T0 Canon Quarantine Decision
status: current-canonical-architecture-decision
created: 2026-08-29
updated: 2026-08-29
phase: G4 Primary Source Assets & Local Game Creation
owner: GPT
supersedes_when_conflicting:
  - docs/source/World Pack与Character Card合同v0.2_SEMANTIC_FREEZE.md freeze_revision 1 T0/history guidance
---

# T0-scoped Source / Post-T0 Canon Quarantine｜Canonical Decision

## 1. Decision

正式采用：

> **T0-scoped Source / Post-T0 Canon Quarantine.**

核心原因不是节省 Context，而是防止已有 canon 对模型形成低成本未来模板。

旧 DSH 长局已经证明：即使文字规则明确写着“历史不是剧本”“Game-local Reality > Source”，只要 ordinary GM Context 同时能看到完整原历史/人物后半生与当前 Game Reality，模型仍可能选择更省推理成本的 canon 继续推动世界，使玩家改变历史后重新收敛。

因此：

> **Do not show the model a post-T0 answer and then ask it to forget that answer.**

---

## 2. Model-freedom self-check｜PASS

本决策不会通过增加 Narrative validator、固定输出格式或人格状态机来解决收敛问题。

明确不增加：

- 固定叙事长度或格式；
- 反历史 checklist；
- 强制人物偏离原设；
- “历史偏离度”；
- 原历史/略变/大变的概率配额；
- 每回合未来-canon 审批；
- Regex/关键词禁止人物发展。

本决策只改变：

```text
which source-derived information
is eligible to become
ordinary Runtime / Context authority
```

而不改变：

```text
how richly the model may write
what choices actors may make
what new semantics the model may create
```

为了避免 Context starvation，T0 projection 必须保留当时已经真实形成的：

- 完整人格惯性与矛盾；
- 已发生经历及其影响；
- 能力、局限与专业风格；
- 已存在关系历史与社会位置；
- 当时合理知识与信息来源；
- 世界制度、资源、地理、文化与冲突压力；
- 开放目标、风险、选择空间与 deliberate blanks。

结论：

> **Quarantine future answers; preserve present depth.**

---

## 3. Authority layers

必须区分：

```text
Immutable Source Package Total Content
!= Selected T0 Source Projection
!= Game-local Canonical Reality
!= Runtime Relevant Set
!= Model-visible Working Set
```

一个 Source package 可以物理保存多个 Entry/T0 snapshot / Character T0 profile，以支持不同新游戏从不同时间点开始。

但是某一局选择 T0 后：

```text
selected exact Source generation
+ selected Entry/T0
→ T0-scoped source projection
→ Final Create materialization
→ game-local reality
```

普通 Runtime 不能因为 package 中还存在 later profile 就读取它。

---

## 4. World Pack rule

World Pack 可以包含：

1. **always-safe / cross-T0 material**：不依赖未来结果的世界逻辑、相对稳定的地理/文化/制度基底等；
2. **Entry/T0-specific material**：某个 Entry 时点已经成为现实的政治、社会、地理、人物与历史状态；
3. **other T0 profiles / later snapshots**：只服务其它新 Game 开局；
4. optional authoring/reference corpus：可用于创作、研究或专门 Historical Reference capability，但不是 ordinary Runtime future truth。

选择某个 Entry 后，只 materialize：

```text
always-safe material
+ selected Entry/T0 material
```

later Entry snapshot 不进入 ordinary Runtime。

### World post-T0 quarantine

默认不得作为普通 GM future authority：

- T0 后原历史事件结果；
- later regime/state snapshot；
- 后来才成立的联盟、战果、人物命运；
- later social/institutional outcome；
- 原作者为后续年份写下的“世界后来怎样”。

这些内容可以属于另一个 T0 的 starting truth，但不属于当前 T0 的未来。

---

## 5. Character Card rule

Character Card 不再等于“完整一生人格传记交给 Runtime”。

正式理解：

```text
Character Source
= stable character identity/continuity
+ authored semantic material
+ zero or more T0-specific character profiles
```

历史人物/跨时间跨度人物必须避免把后期结果写入早期 ordinary Runtime profile。

例如 191 年孙权 profile 可以拥有：

- 截至 191 已形成的身份与家庭背景；
- 当时人格惯性与可观察矛盾；
- 当时能力与局限；
- 当时已有关系与知识；
- 当时开放的发展可能。

它不能为了“帮助模型理解完整孙权”而同时暴露：

- 后来继承江东后的成熟治理经验；
- 后来形成的政治疑虑；
- 晚年继承斗争；
- 称帝与后世结局；
- 任何 T0 后才由原历史产生的人格证据。

### No back-propagation by description

“后期问题不能倒灌青年人格”不再仅靠一句 prose instruction 保护；普通 Runtime 直接没有权限读取 later profile。

---

## 6. Minimal v0.2 contract consequence

G4-02R1 现在已经有真实 deterministic consumer：

> selected Entry/T0 must determine which Source content is eligible for Final Create / Context.

因此允许从 real consumer 拉出最小 temporal scope contract，而不是继续把它 Deferred 成纯 prose。

推荐 v0.2 revision-2 语义形态：

### World

```text
world.semantic_sections
= always-safe sections

world.entries[]
= entry identity / opening seed
+ entry-scoped semantic sections or exact references to them
```

### Character

```text
character.semantic_sections
= always-safe sections only

character.t0_profiles[]
= profile identity
+ explicit World/Entry binding(s) where authored
+ profile-scoped semantic sections
```

规则：

- T0 profile 是 authored starting snapshot，不是 future script；
- profile 内只允许截至该 T0 已成为事实/人格惯性的内容；
- 同一 content file 可以被多个 profile 复用，只要语义在这些 T0 都成立；
- later profile bytes 仍属于 exact Source generation，但不因此获得 current Game Runtime visibility；
- 第一代不建设 universal calendar algebra / arbitrary-year interpolation engine；
- 第一代真正支持哪些 T0，由实际 World Entry 与 authored profile 证明。

### Cross-family / unmatched profile

本决策不恢复“必须同资产家族”硬限制。

若玩家选择的 Character 没有针对该 World/Entry 的 explicit T0 profile：

- 不得偷偷选择“最近的 later profile”；
- 不得回退到 complete-life biography；
- 只能使用 always-safe Character material，并由 Compatibility Review/后续 materialization 明确该组合缺少 authored exact T0 profile；
- 是否作为 warning 还是后续更强 compatibility 规则，由真实 G4-06/G4-07 consumer 决定，不在本决策预造平台。

---

## 7. No convergence force / no divergence force

原历史/原作仍然可以自然重现。

禁止把“历史开放”实现成强制随机偏离。

正确因果：

```text
current actors
+ current knowledge
+ current relationships
+ current resources
+ geography/institutions
+ uncertainty/randomness
+ player actions
→ next world result
```

如果这些当前前提自然产生一个与原历史非常接近的结果，该结果是合法的。

如果关键前提已改变，原历史结果没有任何特殊权重。

> **Causality first. Canon is not a probability target.**

随机性应作用于当前不确定因果，不直接抽签决定“是否按原历史”。

---

## 8. Pretrained model knowledge

对三国等现实历史世界，模型参数本身知道大量 T0 后历史；Runtime 无法物理删除这些参数知识。

因此 Context 中保留一条短而高权重的 authority invariant：

> **T0 之后的预训练/外部 canon 不具有本局事实、人物动机或未来预测权威；未来只能由 current Game-local Reality 与 current causality 推出。**

不得为了“提醒模型别收敛”在 Prompt 中枚举赤壁、夷陵、司马代魏等大量未来答案。

这条规则不是要求模型假装不知道历史名词；它只规定这些知识在 ordinary GM 推演中的**authority = none**。

---

## 9. Explicit historical-reference exception

未来如果本局明确存在：

- 穿越者角色的未来记忆；
- GM-only Historical Reference capability；
- 原作未来知识作为玩法资源；

则 post-T0 canon 可以作为**knowledge/reference**进入对应 owner。

仍必须保持：

```text
Reference knows X
!= World future is X
```

随着 Game Reality 分歧，reference value 可以折旧、失效或变形；它永远不能反向成为事件调度器。

这继承历史《汉末三国：历史参照与分歧》最重要的单向关系：

```text
Game Reality
→ reevaluate reference

NOT
reference
→ force Game Reality
```

---

## 10. Game-local character development

T0 profile 是初始条件，不是人格轨道。

游戏开始后，人物的长期变化由 lived history author：

- 可能强化原来的倾向；
- 可能缓慢修正；
- 可能在重大经历后发生明显变化；
- 可能形成 Source 从未写过的新人格维度；
- 也可能自然走到与原作者/原历史 later depiction 很相似的位置。

唯一要求：结果必须由本局经历、当前因果与人物连续性支撑。

因此本决策与 `G4_GAME_LOCAL_EVOLVABLE_SEMANTICS_DECISION.md` 互补：

```text
T0 quarantine removes future answer leakage
+
Game-local evolvability provides future creative capacity
```

---

## 11. Timeline / Restore consequence

Post-T0 semantic development 属于 Game lived history。

Restore 到更早节点时：

- later personality evolution 必须回滚；
- later local semantic facets 必须回滚；
- later knowledge 不得残留在 Context；
- Source later T0 profile 不能被拿来“补回”已回滚未来。

> **Player owns the timeline, including who a character has become.**

---

## 12. Validation / UAT requirements

自动化至少应证明：

- selected Entry 只暴露 selected World T0 content；
- later Entry marker 不进入 T0 projection；
- selected Character profile只暴露匹配 T0 sections；
- later Character profile marker 不进入 Setup/Runtime Context；
- no matching profile 时不 fallback 到 later/complete-life content；
- exact fingerprint 仍覆盖 package 内所有 declared profiles/content；
- Source Library retention/immutability 不受影响。

真实 Owner UAT 必须包含 **anti-convergence pressure test**：

1. 选择一个早期 T0；
2. 改变一个或多个对原历史结果有决定意义的前提；
3. 连续推进多个高影响节点；
4. 观察模型是否通过“替代人物”“替代事件”“年份到了就发生”“把人格自动长成后期史实形象”等方式重新贴回 canon；
5. 同时观察 Narrative 是否因 quarantine 变薄、人物失真或缺乏自主性。

通过标准不是“必须偏离历史”，而是：

> **when current causes diverge, canon has no convergence privilege; when current causes align, similar outcomes remain possible.**

---

## 13. Decision propagation

本决策立即改变 G4-02R1 当前迁移工作：

- 已迁移的历史 Character package 若仍把 complete-life / later-personality future 作为普通 T0 section，必须重审；
- 孙权作为首个 temporal pressure asset；
- 《汉末三国：天下未定》作为首个 World temporal pressure asset；
- 在这两项通过后再继续批量迁移；
- Codex 继续无 active task，直到 v0.2 revision-2 package shape 经真实资产证明稳定。

G4-06 仍 HOLD；G4-07 必须加入 anti-convergence Product UAT gate。
