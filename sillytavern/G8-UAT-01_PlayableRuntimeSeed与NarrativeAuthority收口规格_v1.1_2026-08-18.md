# G8-UAT-01｜Playable Runtime Seed + Narrative Authority Convergence 规格 v1.1

状态：`CURRENT IMPLEMENTATION BASELINE`
日期：2026-08-18
触发：项目所有者 G8 Stage UAT
代码基线：`sillytavern@3ad5b419e9fb289488a18ded82c5a77f2cdd376c`
supersedes: `G8-UAT-01_PlayableRuntimeSeed与NarrativeAuthority收口规格_v1.0_2026-08-18.md`

> v1.1 完整继承 v1.0 的 P0 Narrative Authority、Minimum Playable Runtime T0、No Phantom Interactable、Bounded Rich Narrative Context、Narrative Freedom Envelope、Player Agency 与真实 UAT corpus 要求，并恢复此前只冻结合同但未实现的 Session Dynamic Suggested Inputs。
>
> 本任务仍不是 G9 Asset Runtime Binding，不是 procedural world generator，也不是单纯 Narrative prompt polish。

## 1. v1.1 新增裁定｜动态五推荐输入是 G8 Stage UAT Required

早期产品基线要求中央输入区存在 `3–6 个行动方向 + 自由输入`。项目所有者当前明确恢复为：

> **输入框上方固定显示 exactly 5 个当前回合推荐输入。**

当前实现不满足该要求。

现状：

```text
ProductSessionView.center.suggestedActions
=
session.availableMoves.map(...)
```

因此：

- 旧 fixture / 老存档有 2 条 public connection → 长期只显示 2 个“前往 XX”；
- AI-created game 当前 `connections=[]` → 推荐区完全为空；
- 这不是 Dynamic Suggested Inputs，只是 movement shortcut placeholder。

代码中已存在 `PlayerRoleplayAssistanceContract / SuggestedPlayerInputContext / SuggestedPlayerInput` 消费合同，但原注释明确只冻结后续 Session 边界，没有实现人格分析或建议生成。

该遗漏现升级为 G8 Stage UAT 的 **P1 Stage Objective Minimum**，与 Playable Runtime T0 一并收口。

## 2. Dynamic Five Suggested Inputs｜正式产品语义

每次正式 Session 需要提供：

```text
exactly 5 SuggestedPlayerInput
```

每项：

```text
suggestionId
playerInput
player-safe label / display text
```

点击行为：

```text
suggestion
→ prefill composer only
→ player may edit
→ player explicitly sends
→ only then becomes Formal Player Input
```

禁止：

- 点击建议直接提交；
- 建议本身推进 Turn / worldTime；
- 建议本身创建 Event / Knowledge / Relationship；
- 未发送建议成为 Personality evidence。

保留既有 provenance 语义：

- `manual`
- `modified_suggestion`
- `verbatim_suggestion`

只有真正提交的 Player Input 才进入玩家人格/选择证据。

## 3. Recommendation Working Set｜Bounded != Fixed

建议生成只能消费当前玩家安全 working set：

```text
Current authoritative Scene
+
Visible authoritative NPCs
+
Visible / carried authoritative Items
+
Available authoritative Destinations
+
Player-safe Knowledge
+
Player public identity / background / goals / personality / language style
+
Recent committed Player Inputs
+
optional CurrentPersonalityProfile
```

不得消费：

- hidden/private Character Knowledge；
- hidden state；
- internal motive truth；
- future event；
- secret destination；
- Prompt / Provider raw；
- Narrative-only phantom entity。

推荐生成属于 player assistance，不拥有世界事实。

## 4. Context-sensitive，不得继续固定字段

推荐必须从**当前 Session state**重新计算。

至少在：

- Session initial load；
- successful Formal Turn commit 后；
- Save Restore 后；
- authoritative current Scene / participants / inventory / availableMoves / player-safe knowledge 改变后；

获得与当前事实一致的推荐。

不要求为了无关 UI render 重复调用模型。

如果 authoritative context 没变化，推荐可以部分保持；但不得因为实现偷懒而永久绑定为固定 movement fields。

必须证明：

```text
move to another Scene
→ recommendation context changes
→ at least relevant suggestions change
```

## 5. Five suggestions 的多样性边界

五项建议应尽量覆盖当前真实可玩的不同方向，而不是五条同义移动。

可用语义池包括：

- observation / investigation；
- dialogue / social interaction；
- authoritative movement；
- item interaction / show / transfer；
- inner expression / player-goal-driven roleplay；
- 当前安全事实支持的其他开放输入。

约束：

- 不要求每回合机械固定五个类别；
- 不存在某类 capability 时不得伪造；
- movement suggestion 默认不应占满五项；
- 同一组建议不得出现近义重复；
- 不能推荐不存在的 NPC / Item / Place；
- 不能因为玩家曾经提到“酒馆”就推荐一个 canonical world 中不存在的酒馆。

## 6. Provider / Fallback

Dynamic recommendation 失败不得阻塞 Session 或自由输入。

允许：

```text
preferred:
small bounded recommendation model call

fallback:
deterministic player-safe suggestion composer
```

No-key 环境仍必须保持 composer 可用。

G8 Stage UAT 的核心要求是：

- configured-key path 能产生 5 个真实动态建议；
- recommendation provider failure 不影响正常 Turn；
- no-key 不得伪造 hidden/world facts。

若 deterministic fallback 可以基于 Minimum Playable T0 安全产出 5 项，则优先保证推荐区不为空；若某一极端场景确实不足以产生五个不同合法建议，必须显式降级而不是重复/幻觉凑数，并记录为 bounded fallback case。

## 7. Personality / Player Context

当前 Creation 已拥有：

- player identity；
- public origin/background；
- current goals / attachments；
- important past experiences；
- initial personality style；
- language expression style。

这些 player-safe facts 应进入 `SuggestedPlayerInputContext` 的当前玩家上下文。

`CurrentPersonalityProfile` 如实现动态分析：

- 只能基于 committed Player Input evidence；
- Narrative / NPC opinion / unsent suggestion 不得作为玩家人格证据；
- 分析失败不阻塞建议与 Turn。

本任务不要求建设复杂人格成长系统，但不得把“人格辅助建议”退化成固定按钮。

## 8. 与 G8-UAT-01 v1.0 其它 blocker 的顺序

仍按：

```text
P0 Narrative Authority hole
↓
Minimum Playable Runtime T0
↓
No Phantom Interactable + Bounded Rich Context
↓
Dynamic Five Suggested Inputs
↓
Narrative responsiveness / Real Provider Gate
↓
Stage UAT
```

原因：动态建议必须建立在真实 NPC / Item / Destination 和可靠 player-safe context 上，不能对空壳世界生成五条更漂亮的幻觉。

## 9. Required Regression

至少增加：

### A｜旧 fixture 不再只有 2 个 move shortcut

即使当前有 2 条 availableMoves：

```text
suggestedActions.length = 5
```

并且不能五项全部是 movement。

### B｜AI-created Minimum Playable T0

Final Create 后第一次 Session：

```text
suggestedActions.length = 5
```

至少能基于真实 NPC / destination / player context 提供不同方向。

### C｜Turn 后变化

完成一次 authoritative move / dialogue / item interaction 后：

- recommendation working set 使用新 Session；
- 至少相关建议发生变化；
- 不保留已经不可用的 target。

### D｜No Phantom

推荐中出现的具体 NPC / Item / Place 必须能在当前 player-safe authoritative projection 找到对应 referent。

### E｜No Hidden Disclosure

隐藏 Knowledge / hidden destination / private motive 不得出现在推荐文本。

### F｜Click semantics

点击推荐：

- 只 prefill composer；
- revision / turnNumber / worldTime 不变；
- 修改后提交记录 `modified_suggestion`；
- 原样提交记录 `verbatim_suggestion`。

### G｜Provider Failure

suggestion generation failure：

- composer 保持可用；
- Session 不进入 error-blocked；
- Formal Turn 正常工作。

## 10. Real DeepSeek Gate 增量

除 v1.0 的真实 NPC / unsupported location / valid move / self-context Gate 外，再验证：

1. initial Session 返回 5 条 suggestions；
2. suggestions 与 current T0 实体/地点/玩家背景一致；
3. 完成一回合后 suggestions 随 context 改变；
4. 不出现 hidden fact；
5. 不推荐 canonical world 中不存在的实体或地点；
6. 点击 recommendation 只 prefill，不自动提交。

## 11. Exit Criteria v1.1

G8-UAT-01 PASS 现在需要同时满足：

- v1.0 全部 P0/P1 Exit Criteria；
- `suggestedActions` 不再直接等于 `availableMoves.map(...)`；
- configured-key Session 正常返回 exactly 5 个 context-sensitive suggestions；
- suggestions 只使用 player-safe authoritative working set；
- suggestions 随 Session state 更新；
- suggestions click only prefills composer；
- recommendation failure non-blocking；
- old 2-fixed-move placeholder retired；
- AI-created world recommendation area no longer empty；
- Real DeepSeek targeted Gate PASS；
- G5–G8 / Creation / Host / Multi-action / Save-Restore-Recovery regression PASS；
- no G9 implementation started。

完成后重新由项目所有者执行 Stage UAT；只有真实 UAT PASS 后 G8 才能 CLOSED。
