---
title: my world｜G4 Expansion Pack v0.1 / Public d20 Decision
status: canonical-architecture-decision
created: 2026-08-31
updated: 2026-08-31
phase: G4
parent_task: G4-08 Expansion Pack v0.1 + First Real Runtime Vertical
semantic_owner: GPT
---

# G4 Expansion Pack v0.1｜判定与检定：公开 d20

## 0. Decision

G4-08 的第一款真实 Expansion 正式冻结为：

```text
Display Name  判定与检定：公开 d20
asset_id      exp.check_core.public_d20
asset_type    expansion_pack
schema        expansion_pack.v0.1
capability    action_check.public_d20.v1
slot          action_resolution
```

它是一个**可选、世界无关、Program-owned RNG 的公开行动判定层**。

未选择该 Expansion 时，G4-07 已通过 Product UAT 的正常 GM 自然语言路线保持不变，不得静默启用判定、DC、骰面或判定卡。

G4-08 的证明目标不是“Expansion 能被加载”，而是：

```text
exact selected Expansion Source
→ Final Create materialization
→ Program-owned capability binding
→ real Player action
→ risk adjudication before RNG
→ Program RNG
→ durable resolution
→ real DeepSeek continuation constrained by that result
→ Save / Continue same reality
```

---

## 1. Product semantics

### 1.1 When to roll

只有同时满足：

```text
结果存在真实不确定性
+
失败存在真实代价
```

才进行检定。

正式“三不掷”：

1. **必然成功不掷**：人物能力与当前处境使失败不构成真实可能；
2. **必然失败不掷**：已有世界事实、人物能力边界或稳定人格底线使该尝试不存在合理随机成功空间；
3. **无代价不掷**：失败后可以无成本立即重复的动作不交给 RNG。

一次检定覆盖一个有意义的行动意图，不把一个自然行动拆成无意义的连续骰子流水线。

### 1.2 Dice boundary

正式原则：

> **Dice decides uncertainty. Dice does not erase reality.**
>
> 骰子裁定真实存在的不确定性，不抹除人物、知识边界、已发生事实或已成立因果。

因此天然高点不能：

- 让 NPC 无缘由跨越稳定人格/利益/义务底线；
- 让人物获得没有来源的知识；
- 让不存在的能力凭空出现；
- 改写已经发生的 durable world fact。

v0.1 **没有 natural 1 / natural 20 自动成败规则**。

---

## 2. Expansion Source v0.1

Expansion 是第三类 reusable Primary Source，与 World / Character 只共享最薄的 exact-generation seam；不得建立 universal mega-schema。

### 2.1 Minimal package

第一代最小形状：

```text
<package-root>/
├─ source.json
└─ sections/
   └─ rules.md
```

Expansion v0.1 顶层只需要表达：

```text
schema_version
asset_id
asset_type
version
display_name
catalog_summary
capability_binding
semantic_sections
```

`capability_binding` 至少包含：

```text
capability_id   = action_check.public_d20.v1
capability_slot = action_resolution
```

`semantic_sections` 延续现有 rich semantic section / disclosure / package-local UTF-8 content / exact byte fingerprint 原则。

### 2.2 Explicit anti-scope

Expansion Source v0.1 不包含：

- GDScript / DLL / executable code；
- 任意脚本执行声明；
- arbitrary Provider tool definition；
- generic UI schema；
- live Game state；
- mutable RNG state；
- Character attributes / level / XP；
- full combat rules engine。

Source 只声明 authored semantics + Host 已知 capability binding。真正能力由 Program registry 拥有。

---

## 3. Composition / compatibility

New Game Composition 长期语义是：

```text
0..N exact Expansion generations
```

G4-08 v0.1 冻结：

- 每个选择都是 exact generation identity；
- 同一个 exact Expansion 不得重复选择；
- 同一个 `capability_slot` 同时只能有一个 Expansion；
- 两个不同 Expansion 都占 `action_resolution` → Compatibility Review fail closed；
- 不根据名字、family、genre、World 年代等做隐藏猜测；
- Public d20 本身没有 World-family restriction，必须可跨 Han / Afterglow 使用。

第一版不建设 dependency solver、version range solver 或 feature graph。

---

## 4. Final Create / provenance

Final Create 对 selected Expansion 必须像 World / Character 一样尊重 exact immutable ancestry。

创建 Game 时至少冻结：

```text
exact Expansion identity
+ exact generation fingerprint
+ materialized authored rules needed by this Game
+ capability_id
+ capability_slot
```

之后：

```text
mutable SourceLibrary.current
!= Game runtime authority
```

Runtime 使用 Game-local materialized Expansion setup / exact ancestry，不得因为 Source Library 后来安装新版本而静默改变旧 Game。

Expansion Source 本身永不接收 Runtime writeback。

---

## 5. Program-owned capability

Host 第一代只认识一个 bounded capability：

```text
action_check.public_d20.v1
```

Unknown `capability_id` 必须 fail loud；不得把 Source 文本当成可执行指令解释成任意机制。

该 capability 只拥有：

> **这次 Player attempt 的 resolution 是否成功。**

它不拥有成功/失败之后的 Relationship / Injury / Knowledge / Inventory / Faction / World 等长期后果。后果仍由相应 Game-local canonical owner 持有。

---

## 6. Public d20 rules

### 6.1 Formula

```text
d20 + modifier = total

total >= DC → success
total <  DC → failure
```

### 6.2 DC guidance

标准档位：

```text
10  简单
15  中等
20  困难
25  极难
30  近乎不可能
```

允许语义裁定使用档位间整数微调，但 Program 只接受合理有界整数；边界由实现任务固定并测试。

DC 只表示行动本身的难度，不承担全部风险语义。

### 6.3 Modifier guidance

第一代不创建 Attribute / Skill table。

建议量级：

```text
+0  普通 / 无相关训练
+2  有训练 / 有经验
+4  专业行家
+6  顶尖能力
```

modifier 必须来自当前 Character / Game-local durable facts 与当前真实状态；不能来自玩家未知 GM secret，也不能为了“剧情顺”任意补偿骰点。

### 6.4 Advantage / disadvantage

```text
normal        1d20
advantage     2d20 take high
disadvantage  2d20 take low
```

优势与劣势同时成立时抵消为 normal；v0.1 不累计多层 advantage/disadvantage。

优势/劣势主要表达当前方法、准备、对象、环境相对有利/不利；不要把一切都压成细碎 ±1。

---

## 7. Freeze-before-RNG protocol

这是 G4-08 新的硬不变量：

> **Risk structure is frozen before the model can see the random result.**

真正掷骰前必须先得到并冻结一个 Check Proposal，至少语义包含：

```text
action identity
intent
DC
modifier
stance = normal | advantage | disadvantage
modifier reason
situation reason
success intent
failure stakes
```

Proposal 验证通过后才允许 Program RNG。

骰面出现之后：

- GM 不得改变 DC；
- GM 不得改变 modifier；
- GM 不得把 stance 改成 advantage/disadvantage；
- GM 不得把失败 stakes 偷换成另一件事来迁就结果。

---

## 8. Provider turn protocol

选择 Public d20 后，Player turn 采用 conditional two-stage flow，而不是强制每回合双调用。

### 8.1 No-check path

如果 GM 判断本次行动符合“三不掷”或根本不存在需要随机裁定的风险：

```text
Player action
→ adjudication envelope says NO_CHECK
→ same Provider response carries normal GM narrative
→ accept normal continuation
```

因此普通行动仍可保持单次 Provider call。

### 8.2 Check path

如果需要检定：

```text
Player action
→ Provider returns CHECK_REQUIRED + Check Proposal
→ Program validates + freezes Proposal
→ Program RNG
→ Program computes outcome
→ durable resolution written
→ Provider receives frozen proposal + Program result
→ GM writes narrative continuation consistent with outcome
```

第二次 Provider call 只发生在真实需要检定的 turn。

具体 wire encoding 可以由实现选择，但必须是 Program 可严格解析/校验的结构化协议；不能靠从自由 prose 猜 DC 或骰式。

---

## 9. RNG authority

正式原则：

> **The model never owns the die face.**

骰面只由 Program 产生。

AI 文本中的“我掷出了 18”不能作为 authority。

Program 至少保证：

- d20 范围合法；
- advantage/disadvantage 按规则取值；
- total / outcome 由 Program 计算；
- RNG 发生后结果先 durable，再进入 resolution narrative；
- 模型无法预知、指定或改写骰面。

---

## 10. Durable check identity / retry

每个真实检定拥有 stable `check_id`，并绑定 stable Player action identity。

至少 durable：

```text
check_id
action_id
intent
DC
modifier
stance
reasons
success intent
failure stakes
raw_rolls
selected_roll
total
outcome
```

硬不变量：

> **Same Player action retry must never reroll.**

例如 Program 已经掷出失败，但第二次 DeepSeek continuation 因网络错误失败：Retry 必须复用原 Proposal、原骰面、原 outcome，而不是重新获得一次成功机会。

Process restart / Main Menu / Continue 后也一样。

---

## 11. Context boundary

启用 Expansion 时，model-visible Context 只加入当前 Game 所需的：

```text
materialized Public d20 rules
+ current Game-local reality
+ current Player action
```

如果当前 turn 发生检定，第二阶段再加入：

```text
frozen Check Proposal
+ Program-generated Check Result
```

不得每回合把全部历史 check log 塞回 Context。

历史检定的长期意义主要由已经发生的 canonical world consequences 继续存在；check record 是 durable provenance / audit evidence，不是第二份 World truth。

---

## 12. UI boundary

G4-08 不要求玩家点击“掷骰”。第一代流程自动执行 RNG，避免提前引入 Pending Roll interaction state、double-click、roll animation 等 G6 体验复杂度。

但玩家最终必须看到公开判定信息。UI 至少应能投影一个轻量 mechanic card：

```text
判定｜<intent>
DC <n>
修正 <n> · <reason>
优势/普通/劣势 · <reason>
骰面 <raw rolls> → <selected>
总计 <n>
成功 / 失败
失败代价：<stakes>
```

UI 只投影 Program-owned resolution，不重新计算或拥有判定 truth。

UI 工作独立路由 Kimi；Codex mechanism task 不拥有视觉交互。

---

## 13. First real acceptance story

Primary vertical：

```text
World       汉末三国：天下未定
Entry       208 / 赤壁前夕
Player      刘备
Expansion   判定与检定：公开 d20
```

玩家执行一个真实风险行动，例如亲自靠近敌军警戒区域侦察船阵并试图不惊动哨兵。

必须真实出现：

```text
World / Character context
→ CHECK_REQUIRED
→ proposal freezes before RNG
→ Program-generated d20
→ public total/DC/outcome
→ real DeepSeek narrative obeys outcome
→ later turn remembers resulting reality
→ Save / Continue preserves it
```

随后执行一个明显不应检定的普通行动，证明没有退化成 roll-everything pipeline。

Cross-world engineering proof：同一 Expansion 在 Afterglow / Livia route 也能产生语义不同但协议相同的风险检定，证明没有硬编码 Han。

No-Expansion control：同一 build 创建 `Expansion = none` 的 Game，必须保持 G4-07 行为，没有 adjudication protocol、RNG、DC 或 check state。

---

## 14. Deferred

明确不进入 G4-08：

- Player-click roll button / dice animation；
- natural critical system；
- contested checks；
- damage dice；
- initiative / combat rounds；
- HP / attributes / skills / level / XP；
- NPC autonomous dice simulation；
- hidden rolls；
- generic dice language；
- executable Expansion code；
- dependency/version solver；
- generic Mod sandbox / external UI contract / Creator suite。

这些只能由后续真实需求重新立项。

---

## 15. Frozen invariants

G4-08S0 正式冻结：

1. 第一款 Expansion = **判定与检定：公开 d20**；
2. Expansion optional；未选择绝不静默启用；
3. Public d20 占 exclusive `action_resolution` slot；
4. “三不掷”成立；
5. Risk Structure 必须在 RNG 前冻结；
6. RNG / total / outcome authority 永远属于 Program，不属于模型；
7. same action retry / restart 不得重掷；
8. Expansion owns resolution, not downstream World consequences；
9. no-check 普通 turn 不强制第二次 Provider call；
10. G4-08 必须用启用 / 不启用 Expansion 的真实可玩差异验收。

本 Decision 关闭 G4-08S0 的语义问题，并授权下一步 `G4-08M1` 机制实现。