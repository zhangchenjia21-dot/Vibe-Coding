---
title: G8 Runtime-extensible UI 产品架构裁定
status: current
version: 1.0
date: 2026-08-16
project: 酒馆游戏新版主体
stage: G8 网页产品化
---

# G8 Runtime-extensible UI 产品架构裁定 v1.0

> 本文件冻结 G8 阶段 Runtime-extensible UI 的产品语义与 Host 能力边界。
> 不冻结 G9 `tavern-asset-spec vNext` 的最终 JSON / Schema / 字段命名。

## 1. Core 一级 World Surfaces

以下五个 Core Surface 永久具现：

1. 人物
2. 地图
3. 物品
4. 信息
5. 目标

即使当前没有内容，也保留入口并使用轻量 Empty State。

这些 Surface 是玩家理解世界的稳定入口，不是后台数据库对象的一一映射。

### 人物

聚合玩家安全的 Character、Relationship、人物相关 Commitment、最近互动、玩家已知人物信息，以及 Expansion 对人物详情的安全 Contribution。

### 地图

聚合 Region、Place、Scene、玩家已知空间关系与安全 Map Overlay。

### 物品

聚合 Item、Ownership、player-safe Placement、已知物品状态与 Item Detail Contribution。

### 信息

“信息”取代工程阶段较窄的“已知 / 记录”表达。

可聚合 Knowledge、Clue、玩家已知 Fact、Rumor、玩家可见的重要 Event 摘要、调查信息，以及来源、可信度、时效等玩家安全信息。

后台 Knowledge / Clue / Event 等对象继续职责分离。

### 目标

Task / Objective 是 World OS Core 正式能力，不是传统 RPG 强制 Quest Log。

可显示目标、阶段、状态、最近进展、期限，以及关联人物 / 地点 / 物品 / 信息。

任务不能因模糊剧情钩子滥建；玩家明确接受、主动决定、正式委托或规则触发后才形成。

---

## 2. 领域对象独立，UI 可以聚合

必须保持：

> 领域对象独立 ≠ UI 必须一对象一栏目。

Relationship、Commitment、Event、Knowledge、Clue、Task 等继续拥有独立权威语义。

Product UI 可以关联聚合，例如：

- Commitment → 人物详情中的“约定”；
- 与目标有关的 Commitment → 目标详情引用；
- Event / Knowledge / Clue → 信息；
- Relationship → 人物；
- Task → 目标。

同一事实只存在一个正式 Owner。多个 UI 位置可以引用同一 player-safe projection，但不得复制第二份业务事实源。

---

## 3. 正式合法 UI Contribution 落点

根据现有汉末三国与诸界余辉资产族的实际机制需求，Game UI Host 至少支持以下产品层落点。

### 3.1 Player Status Surface

高频玩家状态与资源，例如疲劳、饥饿、伤势摘要、Mana、Magic Strain、Divine Channel Strain 等。

不得固定假设 HP / MP / 等级 / 金币。

### 3.2 Player Character Detail Surface

玩家长期详细资料，例如技能、经历、专长、训练、魔法掌握、神术契约、详细康复状态。

左栏只承担摘要。

### 3.3 Entity Detail Surface

至少支持：

- Person Detail
- Item Detail
- Place / Region Detail

Expansion 可以向实体详情贡献安全 Section。

### 3.4 Core Surface Contribution

Expansion 可以向人物、地图、物品、信息、目标贡献安全 Section / Card / Filter / Badge / Action Intent。

### 3.5 Map Overlay

支持政治控制、战线、位面连接、魔法地脉、圣所、系统探测、资源分布等玩家安全 Overlay。

Overlay 不得泄露后台 World Truth。

### 3.6 Narrative Contextual Surface

临时机制优先进入中央上下文 UI，例如 Combat Context、施法、Divine Audience、治疗、交易、制作、抽奖等。机制结束后可收起或消失。

### 3.7 Global Notice / Alert Surface

用于高重要但非永久栏目状态，例如“历史已经改变”、危险状态、系统模块解锁、Recovery、重要世界警告。

### 3.8 Game Creation / Settings Contribution

Expansion 可以声明创建游戏或当前实例设置需求，但设置只能形成受控配置，不得直接绕过 Runtime 修改正式世界。

### 3.9 Extension Surface

真正形成独立长期玩家工作空间的机制可以获得一级 Extension Surface，例如系统、魔法、神术 / 信仰、势力 / 政务等。

---

## 4. Surface Ownership 与 Surface Contribution 分离

### Surface Ownership

资产声明：

> 我创建并拥有一个一级 Extension Surface。

### Surface Contribution

另一个资产声明：

> 我依赖该 Surface 的 Owner，并向它贡献内容。

例如：

基础魔法包 owns `surface.magic`。

主题魔法包 depends on 基础魔法包，并 contributes to `surface.magic`。

不得因为显示名相同就隐式合并互不相关的资产。

---

## 5. 创作者明确要求一级 Surface

如果创作者在拓展包创作中明确表达：

> 游戏本体应为本拓展包增加独立一级栏目 / Surface。

或语义等价的明确要求，Creator 应将其识别为：

> Extension Surface Ownership Request

最终正式资产必须把这项意图编译成结构化声明并经过 Validator。

正式 Runtime 不应在每次启动时重新用 LLM 猜说明文字。

声明合法后，Host 必须提供该一级 Surface；Host 不能因为认为内容可以塞入其他栏目而擅自取消创作者明确要求的一级工作空间。

---

## 6. Surface Owner 冲突 = 创建游戏前不兼容

一个唯一 Surface 只能有一个 Owner。

如果两个资产同时声明拥有同一个 Surface：

```text
Asset A owns surface.X
+
Asset B owns surface.X
→ Surface Ownership Conflict
→ INCOMPATIBLE
```

系统必须在创建 Game Instance 之前阻止该资产组合。

不得拖到运行时通过抢占、覆盖、加载顺序或隐式合并解决。

这条思路可泛化到其他“唯一 Owner 型 Host 能力”，例如：

- unique Rule System ownership；
- exclusive Resolution replacement；
- exclusive State / Resource namespace ownership；
- exclusive Host Capability provider。

总原则：

> 能够在组装游戏前证明组合不成立的冲突，应优先判定为资产不兼容，而不是建设运行时仲裁。

---

## 7. Host Layout Authority

资产决定“需要表达什么”，Host 决定“怎样安全呈现”。

资产不得指定任意绝对坐标、固定像素布局、任意 React、JS、DOM、eval 或 CSS 注入。

Game Host 拥有：

- Desktop / narrow 响应式；
- Tab / Drawer / overflow 表现；
- 视觉主题；
- Accessibility；
- 安全组件；
- 具体间距；
- overflow / scroll；
- UI Action → Action Intent。

---

## 8. 玩家拥有一级 Surface 最终排序权

玩家可以自行调整 Core Surface 与 Extension Surface 的显示顺序。

资产作者只能提供初始推荐顺序、推荐分组或推荐重要度。

一旦玩家调整：

> 玩家 UI Preference 优先于资产推荐。

Surface Order：

- 不形成 Formal Turn；
- 不推进 worldTime；
- 不生成世界 Event；
- 不属于 canonical Game State；
- 不属于 Save Snapshot 的世界恢复 authority；
- Restore 不回滚玩家当前 UI 排序。

如果后来出现新 Surface：

1. 保留已有玩家排序相对顺序；
2. 新 Surface 按推荐位置插入；
3. 无可靠推荐时追加到 Extension 区域；
4. 玩家之后可以重新排序。

---

## 9. 一级 Surface 过多时

通过“优先扩展已有 Surface”主动控制一级栏目数量。

例如：

- 恋爱 → 人物；
- 生存 → 玩家状态；
- 伤势 → 状态 / 人物详情；
- Theme Magic → 基础魔法 Surface；
- 战斗 → Contextual Surface；
- 线索机制 → 信息；
- 系统子功能 → 系统 Surface 内部。

空间不足时允许横向滚动或轻量 overflow / “更多”。

当前选中的 Surface 必须自动进入可见区域。

Host 不得因为空间不足，把正式一级 Surface 静默降级成另一个栏目内部内容。

---

## 10. Extension Surface 内允许受控二级结构

复杂 Extension Surface 可以拥有：

- 二级 View；
- Section；
- Host-owned sub-navigation；
- List / Card / Meter / Fact / Action 等安全组件。

例如：

```text
系统
├─ 概览
├─ 任务
├─ 商城
├─ 抽奖
├─ 仓库
└─ 强化
```

或：

```text
魔法
├─ 已学法术
├─ 法术书
├─ 仪式
└─ 研究
```

但资产仍不得构造任意页面 DOM、注入 React/CSS/JS 或自行决定像素布局。

原则：

> 允许资产描述信息架构，不允许资产执行前端。

---

## 11. G8 / G9 边界

### G8

先冻结并用手写 Runtime Definition 验证 Host Capability：

- Core 五 Surface；
- Player Status；
- Entity Detail；
- Core Surface Contribution；
- Map Overlay；
- Contextual Surface；
- Global Notice；
- Settings Contribution；
- Extension Surface；
- Surface reorder；
- Owner conflict semantics；
- controlled secondary View / Section。

### G9

再设计 `tavern-asset-spec vNext` 的最终机器协议，包括 Surface ID、Ownership、Contribution、Dependency、Conflict、Host Requirement、Validator 与 Creator authoring compiler。

不得让 asset-spec 反向决定 Game Host 尚未验证的能力。

---

## 12. 当前冻结摘要

```text
Core Permanent World Surfaces
=
人物
地图
物品
信息
目标
```

```text
Asset UI Extension
=
优先扩展已有 Surface
+
按机制需要贡献到玩家状态 / 实体详情 / 地图 / 中央临时 UI / Alert / Settings
+
创作者明确声明时可创建一级 Extension Surface
```

```text
same Surface owner
+
another same Surface owner
=
INCOMPATIBLE before game creation
```

```text
Asset recommends order
→ Player can reorder
→ Player preference wins
```

```text
Extension Surface
→ may declare controlled secondary Views / Sections
→ Host renders
→ no arbitrary frontend code
```

> **资产决定需要表达什么；Game Host 决定怎样安全呈现；玩家决定自己最想先看到什么。**

---

## 13. 生效说明

本裁定已由项目所有者在 G8 阶段内正式确认，用作：

- `G8-WEB-04｜Runtime-extensible UI Host Foundation` 的产品上游；
- 后续正式右栏 IA / Surface 排序 / Extension Surface 表现层的产品依据；
- G9 `tavern-asset-spec vNext` 设计时的 Game Host 能力事实源。

本文件冻结的是**产品语义和 Host Capability**，不是最终机器 Schema。

自本版本起，工程阶段临时栏目：

```text
人物｜地图｜物品｜已知｜承诺｜记录
```

不再视为正式最终 IA。

正式 Core World Surfaces 为：

```text
人物｜地图｜物品｜信息｜目标
```
