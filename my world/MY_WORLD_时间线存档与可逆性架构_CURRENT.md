---
title: my world｜时间线、存档与可逆性架构
status: current-canonical-supporting-architecture
version: 1.0
created: 2026-08-26
updated: 2026-08-26
scope: G2 reversibility UX / G3 persistence-save-timeline / G6 UI
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜时间线、存档与可逆性架构 CURRENT

## 0. 文档定位

本文件冻结 `my world` 第一代 **Reversibility / Save / Restore / Timeline** 的产品语义与 UX 边界。

它基于：

- SillyTavern / The World / DSH 的 Save-Restore / Timeline 实践；
- `my world` 已冻结的 `Model freedom first. Reversibility over prevention.`；
- Product Owner 对“任意历史位置一键返回可能造成误触、降低选择重量并扩大存档/时间线复杂度”的最新裁定。

本文件是对以下旧表述的**明确收窄 / superseding clarification**：

- “每个历史 Turn 都可以通过 `回到这里` 快捷回档”；
- “Timeline 成立后应默认把任意历史节点直接暴露成玩家一键恢复点”；
- “Player owns the timeline = 所有历史位置都应无成本、低摩擦地可逆”。

这些解释不再是 `my world` 的产品方向。

正式原则：

> **Reversibility over prevention does not mean frictionless arbitrary rewind.**
>
> **局部错误应低成本纠正；重大历史恢复必须表达明确玩家意图。**
>
> **Timeline is first a Runtime capability, not automatically a player-facing debugger.**

---

# 1. 重新解释 Player owns the timeline

`Player owns the timeline` 继续有效，但其正式含义是：

> 玩家不会因为模型或游戏的一次错误而被永久锁死；玩家对最终保留、继续和恢复哪段游戏历史拥有最终决定权。

它**不意味着**：

- 每个 GM Turn 都有一个显眼的 `回到这里`；
- 点击任意旧消息即可立即替换当前进度；
- 所有后果都应随时零成本撤销；
- Timeline 的每一个内部节点都必须成为公开 Load Point；
- 玩家必须像操作调试器一样管理游戏历史。

玩家主权与无摩擦任意回档不是同一个产品目标。

---

# 2. 可逆能力分级

第一代产品明确区分以下操作。

## 2.1 Cancel｜取消当前生成

作用：

```text
active AI generation
→ stop
→ return to usable state
```

特征：

- 风险极低；
- 应直接暴露；
- 不等同于 Load / Timeline restore；
- 不应产生复杂历史管理心智。

G2-03 即可提供。

## 2.2 Regenerate / Retry｜重新生成最近一次 GM 输出

作用：

```text
same latest player input
→ discard / supersede latest unsatisfactory generation according to current Turn semantics
→ generate again
```

特征：

- 局部纠错；
- 低摩擦；
- 只针对最近生成；
- 不意味着玩家可任意跳转到很久以前。

G2/G3 逐步冻结其正式 Turn/持久化语义。

## 2.3 Edit latest input & retry｜修改最近一次输入后重试

这是局部修正能力，不等同于任意历史 Rewind。

它只有在 G2-04 Turn Domain 与 G3 persistence boundary 能明确保证状态一致时才进入正式产品路径。

当前不要求 G2-03 实现。

## 2.4 Save Point｜玩家主动存档

Save Point 表达明确玩家意图：

> “这个进度对我重要，我以后可能希望从这里恢复。”

它应具有玩家可理解的身份，例如：

```text
洛阳入城前
王允宴会之前
第一次见到曹操
```

Save Point 是长期、玩家可管理的恢复点。

## 2.5 Load / Restore Save｜读取存档

读取旧存档会改变当前游戏未来，属于高影响操作。

它不得被设计成隐藏在普通 Narrative 历史中的无意识单击。

产品应让玩家明确知道：

- 正在读取哪个 Save；
- 当前进度将不再是 active current timeline；
- 是否存在未显式保存的新进度；
- 当前未来是否仍可恢复。

是否使用确认弹窗、二阶段按钮或专门 Save Surface 由 G3/G6 UX 决定，但必须满足：

> **Load 是明确意图，不是易误触的文本附近快捷动作。**

## 2.6 Timeline Node｜内部时间线节点

Timeline 可以拥有比玩家 Save Point 更细的内部边界，例如 durable Turn / commit / snapshot anchor。

但：

> **Timeline Node != Player Save Point.**

内部存在一个可恢复节点，不代表 UI 必须把它显示为一个可点击存档。

## 2.7 Arbitrary Historical Rewind｜任意历史回退

“从任意旧 Turn 一键回到这里”当前正式状态：

> **DEFERRED / NOT DEFAULT FIRST-GENERATION PRODUCT BEHAVIOR**

未来只有在真实长局 UAT 证明其产品价值明显高于误触风险、选择重量损失和状态复杂度后，才重新讨论。

不得因为 Runtime 技术上可以恢复历史，就自动把该能力暴露给玩家。

---

# 3. 中央 Narrative 的操作边界

中央 Narrative Host 是玩家高频主路径，因此只暴露高频、低风险动作。

近期允许：

```text
active GM generation
→ 取消生成

latest GM generation
→ 重新生成
```

未来在正式 Domain 成立后可评估：

```text
latest player turn
→ 修改最近输入并重试
```

历史 Turn 默认：

```text
read / scroll / inspect
```

**不默认提供：**

```text
每条历史内容
→ 回到这里
→ 从这里分支
```

因此旧的“每条 Narrative 附近放 `回到这里` / timeline affordance”设计被本文件 supersede。

---

# 4. Save / Timeline Surface 的产品职责

右侧 `World Surface Host` 未来可以承载 Save / Timeline，但两者不要求一开始就是同一个复杂界面。

第一代优先顺序：

```text
Save
→ Load Save
→ 当前游戏 / 当前时间线状态
→ 必要的恢复信息
→ 只有真实需要后再增加高级 Timeline 浏览
```

目标是让玩家理解：

> “我保存了哪些重要进度，我现在在哪个进度，我要恢复哪个进度。”

而不是让玩家理解内部 Event Log、Snapshot、Commit ID 或任意 Turn DAG。

---

# 5. Restore 本身也应尽量可恢复

为了避免误操作导致真正不可逆的数据损失，正式方向是：

> **切换到旧 Save 时，不应立即物理销毁当前未来。**

G3 应评估最小可靠方式，例如：

- 自动 recovery checkpoint；
- 保留旧 current head；
- 内部 branch / detached future；
- 其它等价、简单、可靠的恢复标记。

具体实现由 G3 Persistence Architecture 决定，不在本文件预选 SQLite schema。

但语义目标明确：

```text
Load old save
!=
irreversibly delete everything after it immediately
```

这样即使玩家误读档，也有机会恢复“刚才的进度”。

这比在所有游戏操作前不断增加 Confirmation 更符合 `Reversibility over prevention`。

---

# 6. Save Point 不等于完整世界副本

本文件不冻结具体 persistence 技术，但明确禁止一个错误假设：

> “每一个可恢复位置都必须复制一份完整世界文件。”

G3 可评估成熟的本地持久化方式，例如：

```text
transactional authoritative state
+ durable mutations / event records where useful
+ periodic snapshot / checkpoint where useful
```

因此：

```text
internal timeline granularity
!=
full-save-file count
```

磁盘空间不是第一产品风险；更重要的是：

- Restore correctness；
- future-memory isolation；
- branch/recovery semantics；
- Context rebuild；
- migration；
- 玩家是否理解当前进度。

不要为了理论存储压力提前牺牲正确性，也不要因为存储便宜就无限暴露 Timeline 操作。

---

# 7. G3 正式优先级

G3 应按以下优先级推进：

```text
1. reliable current Game / World persistence
2. reopen / resume current game
3. explicit player Save Point
4. explicit Load / Restore Save
5. Restore 后 Agent Context / Conversation future isolation
6. Load 前后的 current-future recoverability
7. retry / regenerate / latest-turn correction 与 persistence 对齐
8. internal branch semantics only as needed for correctness/recovery
9. arbitrary historical rewind only if later Product UAT proves it necessary
```

因此 G3 不再以“任意 Turn 回到这里”为首要产品目标。

---

# 8. 对现有 current 文档的 supersession

截至本文件 v1.0：

## 8.1 `MY_WORLD_核心设计原则_CURRENT.md`

其中：

- `Player owns the timeline`；
- “回到稳定时间点”；
- `restore / branch`

继续有效，但应按本文件解释：**优先通过局部 Retry 和明确 Save/Load 实现玩家主权，不推导出任意历史节点裸露回档。**

## 8.2 `MY_WORLD_声明式UIHost架构_CURRENT.md`

其 Section `Timeline / Turn Action 与 UI Host` 中关于：

```text
历史 Turn → 回到这里
旧 Turn → timeline affordance
```

的产品建议被本文件 supersede。

保留：

- latest generation 的 Cancel / Regenerate；
- 右侧未来可拥有 Save / Timeline Surface；
- UI 仍是 Runtime truth 的 Projection。

## 8.3 `MY_WORLD_总体规划路线图_CURRENT.md`

G3 中“Rewind / Branch Foundation”与关键路径中的 `Restore / Rewind` 不再解释为必须提供任意历史 Turn 一键回退。

下一次 Roadmap 收口时应改写为：

> **Save / Restore / Recovery / Timeline Foundation**

并把 arbitrary per-turn rewind 标记为 Deferred。

## 8.4 G2-03

当前 G2-03 Task Packet 不受破坏：它本来只要求 Cancel / Regenerate，并明确禁止 G3 rewind/branch 实现。

执行 Agent 无需返工或停止。

---

# 9. 第一代成功标准

第一代 Save / Timeline 体验成功，不取决于“玩家能不能随便穿越到任何一轮”。

更重要的是：

1. **错误容易修正**：生成不满意可以 Cancel / Regenerate；
2. **重要进度容易保存**：Save 行为清晰；
3. **读档不令人害怕**：Load 语义明确，状态正确；
4. **误操作可恢复**：旧 current future 不被立即不可逆销毁；
5. **Restore 后 AI 没有未来记忆**；
6. **玩家理解自己当前在哪条进度上**；
7. **游戏选择仍然有心理重量，不因为无成本任意回档变成调试器。**

正式总结：

> **局部纠错要近、快、低成本；历史恢复要明确、有意识、可恢复。**
