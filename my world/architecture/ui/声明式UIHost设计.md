---
title: my world｜声明式 UI Host 设计
type: supporting-architecture
status: active-supporting-design
version: 1.4
created: 2026-08-26
updated: 2026-09-06
canonical_map: ../../MY_WORLD_架构_CURRENT.md
scope: G2 / G5 UI projection / G6 / G8
historical_evidence: SillyTavern G8 Runtime-extensible UI Host
---

# 声明式 UI Host 设计

## 0. 定位

本文件是 `MY_WORLD_架构_CURRENT.md` 的 UI 深度设计，不是独立顶层 Authority。

当前 G6 若与旧段落冲突，优先服从：

- `G6_SESSION_SHELL_INFORMATION_OWNERSHIP_DECISION.md`
- `G6_CHARACTER_AND_IMPORTANT_EXPERIENCES_V1_0_DECISION.md`
- `G6_MODEL_DRIVEN_INFORMATION_CURATION_AUTHORITY_DECISION.md`
- `MY_WORLD_CURRENT_STATUS.md`

正式原则：

> **Definition declares what should be expressed; Host owns how it is rendered.**
>
> **Runtime / curated projection owns live material; UI remains a projection.**
>
> **Host capability first; external asset protocol second.**

---

## 1. 当前产品骨架

```text
┌────────────────┬──────────────────────────────┬────────────────────┐
│ Player Status  │ Narrative Host               │ World Information  │
│ Host           │                              │ Host               │
│                │                              │                    │
│ [未来立绘]      │ GM Narrative                 │ 概览               │
│ [未来实时状态]  │                              │ 角色               │
│ [未来机制摘要]  │ Player Input                 │ 重要经历           │
│                │                              │ 存档               │
│                │                              │ + future grounded  │
└────────────────┴──────────────────────────────┴────────────────────┘
```

### Player Status Host

回答：**角色现在的高频可玩状态怎样？**

长期承载：

- authored portrait（真实资产存在时）；
- HP / MP / stamina / attributes / injury / buff / resource 等真实 Domain / Expansion contribution；
- 其它已经证明需要常驻的高频 live status。

不再承担：

```text
姓名 / 年龄 / 来历 / 背景 / 性格
能力说明 / 局限 / 长期目标 / Biography
World / Entry identity
recent actions / player-turn count
完整 Inventory / People / Save 信息
```

这些内容进入右侧对应 Surface。没有合法 portrait / mechanic contribution 时，左 Host 可以收窄、折叠或隐藏，不用 biography 填空。

### Narrative Host

回答：**现在发生了什么，我接下来想做什么。**

长期承载 GM Narrative、玩家自然语言输入、streaming、cancel、regenerate/retry 与经过正式边界支持的 contextual contribution。

Narrative 仍是主要视觉/交互区；信息整理、UI curation 或侧栏失败不得反向成为 Narrative acceptance gate。

### World Information Host

回答：**我主动想查看哪些当前角色 / 世界 / 系统信息。**

当前 IA 母版：

```text
概览
角色
重要经历
人物
事务
行囊
系统
地图
存档
```

这不是要求全部现在显示。只有 grounded consumer 才出现。

当前已 grounded：

```text
概览
角色
重要经历
存档
```

其中：

```text
角色
→ 当前 Character Sheet
→ “现在的我是谁”

重要经历
→ protagonist milestone history
→ “我是怎样走到现在的”
```

---

## 2. Responsive / Wide-screen Layout

三栏是桌面长期骨架，不是所有时刻都必须同时占位。

正式规则：

> **Narrative First != Narrative Only.**
>
> **有真实内容的 Side Host 应参与宽屏布局；没有真实内容的 Host 不需要为了三栏外形强占空间。**

当前比例只作为历史调优基线：

```text
Player Status Host   ~18% when active
Narrative Host       ~60% baseline
World Information    ~22% baseline
```

如果 Player Status Host 当前为空，允许隐藏/折叠后让 Narrative + World 重新分配空间。

窄窗口：

```text
wide / sufficient
→ active Side Hosts + Narrative 并列

narrow / insufficient
→ Narrative 保持主区
→ 有内容的 Side Host 用 toggle / drawer / overlay
→ 空 Side Host 不显示无意义 toggle
```

默认启动继续采用 Maximized Window；回归至少覆盖最大化、1280x720 与窄窗口。

Narrative 内部正文仍应保持 bounded readable width，不因超宽桌面无限拉长行宽。

---

## 3. Stable Host slots now; generalized rendering later

当前稳定 placement：

```text
GameShell
├─ PlayerStatusHost
│  ├─ PortraitSlot                 # future grounded visual consumer
│  └─ LiveStatusContributionSlot   # future mechanic consumer
├─ NarrativeHost
│  ├─ NarrativeStream
│  ├─ NarrativeContextualSlot
│  ├─ TurnActionSlot
│  └─ Composer
└─ WorldInformationHost
   ├─ CoreSurfaceNavigation
   ├─ OverviewSurface
   ├─ CharacterSurface
   ├─ ImportantExperiencesSurface
   ├─ SaveSurface
   └─ FutureGroundedSurfaceSlot
```

旧版本中的 `PlayerCharacterDetailSlot` 不再属于左 Host。Character detail 归右侧 `CharacterSurface`。

当前允许继续用手写 Godot Control / Container。关键是 ownership 与 placement 正确，不先制造通用 renderer。

---

## 4. 当前数据流

### 4.1 普通 player-safe projection

```text
Authoritative Runtime / frozen Source
→ player-safe domain projection
→ presentation ViewModel
→ fixed Godot Surface
```

### 4.2 Model-driven information curation

Character / Important Experiences 使用：

```text
accepted Player + GM Narrative
+ bounded current Character / recent milestones
+ frozen starting player-safe profile
↓
Post-turn Information Curator model
↓
bounded structured curation
↓
Program normalization + Timeline currentness
↓
player-safe Character / Important Experiences projection
↓
fixed Godot Surfaces
```

开放语义的重要性、归类与摘要由模型决定；Program 不写 keyword/score/event-rule semantic classifier。

---

## 5. Declarative Structure != Live Data

未来声明式 Definition 不允许通过任意表达式直接读取 Runtime：

```text
game.player.stats.mana
${state.xxx}
任意 NodePath
任意 GDScript expression
任意 SQL/query
```

正式链保持：

```text
Authoritative / curated current material
→ player-safe projection
→ bounded presentation material
→ Declarative UI Host
→ Godot Control tree
```

Definition 不拥有 Runtime truth，也不拥有 semantic curation authority。

---

## 6. Internal Declarative UI Host — CURRENTLY HOLD

正确顺序已经被 Owner 重新确认：

```text
fixed real UI consumers
→ Character / Important Experiences / People / mechanic consumer 等真实模式
→ repeated component patterns
→ Internal Declarative UI Host
→ bounded Action Intent
→ G8 external contract
```

`MW-013 Internal Declarative UI Host v0.1` 当前：

```text
HOLD / NOT AUTHORIZED TO IMPLEMENT YET
```

此前 shaped v0.1 仅尝试过非常小的：

```text
section
status_list
fact_list
```

但即使这些也不能因为“看起来通用”而立即实现；必须等多个真实 consumer 证明重复。

旧版本曾列出的 `card / badge / meter / action_list / map_overlay / secondary_view` 等属于未来可能 vocabulary，不是当前承诺，也不是 MW-013 授权范围。

---

## 7. Surface ownership / contribution

未来扩展 UI 需要区分：

```text
owns surface.X
```

和：

```text
contributes to surface.X
```

但 G6 当前不要为了尚未存在的外部 Mod 先建立 universal surface registry。

真实固定 Surface 先证明产品需求；G8 再把 proven capability 外部化。

---

## 8. Action Intent

未来声明式 UI 可以请求受控 Intent，例如：

- prefill composer；
- open surface；
- open entity detail；
- request retry/regenerate；
- request Save navigation。

Intent 是 UI → Application/Domain 的请求，不是资产直接 mutation。

禁止 arbitrary GDScript callback、arbitrary method dispatch、任意 NodePath execution、filesystem/OS command、World Pack 直接写 authoritative state。

Action Intent 仍位于 Internal Declarative Host 之后，不在当前 Character / Important Experiences fixed UI consumer 中提前建设。

---

## 9. Reversibility 与 UI

中央 Narrative 继续暴露低风险 Cancel / Regenerate 等前台操作；历史恢复由正式 Save / Restore 语义承担。

Character / Important Experiences 也服从相同 current Timeline：

```text
Regenerate replaces history
→ stale curated info disappears

Restore to older Save
→ Character reverts
→ restored-away Important Experiences disappear
```

内部 Timeline Node 不自动成为玩家可点击恢复点。

---

## 10. UI Preference / Typography

UI Preference 默认独立于 canonical World / Timeline State。

未来候选包括 Surface 展开状态、splitter 宽度、字体大小 / UI scale 等。

当前保持 medium readable typography baseline；不为了 Character / Important Experiences task 顺带建设完整 UI Preference persistence。

---

## 11. World Pack / Mod 外部声明时序

必须保持：

```text
真实固定产品 UI
↓
稳定 Host Slots
↓
真实 Runtime / model-curated RPG 数据投影
↓
多个 handwritten consumers
↓
Internal Declarative UI Host（G6 later）
↓
Host vocabulary 收敛
↓
G8 external World Pack / Mod UI declaration schema
↓
Validator / Adapter / Authoring UX
```

禁止为了尚未验证的外部资产需求，先冻结巨型 JSON Schema 再倒逼 Host 支持任意能力。

---

## 12. 跨阶段路线

### G2

固定三 Host placement、Narrative 主体验、responsive 与 readable typography。

### G3–G5

建立真正的 Game / World / Save / Knowledge / Agency / player-safe projection。

### G6 current

```text
MW-011 first real ViewModel consumer                DONE
MW-014 model-driven Character/milestone backend     ENGINEERING PASS / INTEGRATED
Character + Important Experiences fixed UI         CURRENT NEXT
People / mechanic-state / other grounded consumers LATER
Internal Declarative UI Host                        HOLD until repeated patterns
```

### G8

在 G6 capability 经真实 UAT 证明后，再外部化 World Pack / Mod UI declaration、Surface ownership、validator 与 authoring helper。

复杂脚本沙箱仍默认 Deferred。
