---
title: G8 Runtime-extensible UI 产品架构裁定
status: current
version: 1.1
date: 2026-08-18
project: 酒馆游戏新版主体
stage: G8 网页产品化
supersedes: 14_酒馆游戏_G8_RuntimeExtensibleUI产品架构裁定_v1.0_2026-08-16.md
---

# G8 Runtime-extensible UI 产品架构裁定 v1.1

> 本文件是第 14 号核心来源的新版本，**SUPERSEDES v1.0**，不增加核心来源数量。
>
> v1.1 保留 v1.0 的 Core Surface、九类 Host Capability、Owner / Contributor、pre-game incompatibility、玩家排序权、受控 secondary View / Section、Host layout/safety authority 等全部原则，并补齐 2026-08-18 审计发现的 Production Convergence、Creation Contribution 与动态数据投影边界。
>
> 本文件冻结的是 **Game Host 的真实能力边界**，不是 G9 `tavern-asset-spec vNext` 的最终 JSON / Schema / Creator authoring syntax。

---

## 1. 当前状态：Host 内核已成立，Final Convergence 仍 ACTIVE

当前已经由 typed internal contract、fixture、preview 与 Product UI 证明：

- Core 永久一级 World Surfaces；
- 9 类合法 Host Capability；
- Extension Surface Ownership / Contribution；
- duplicate Owner fail closed；
- explicit dependency；
- controlled secondary View / Section；
- safe component vocabulary；
- controlled Action Intent；
- Core + Extension Surface reorder；
- Restore 与 UI Preference 分离；
- Product UI 能渲染 Extension Surface、Contextual、Notice 等声明式结构。

但在进入 G9 前仍必须完成 **G8-WEB-04 Final Host Convergence**：

```text
handwritten typed Runtime UI Definition
→ Host Assembly
→ real Product Bootstrap / Server
→ player-safe Product Projection
→ Formal Product UI
```

默认正式游戏可以继续没有任何假 Expansion；但正式 Bootstrap 必须具备消费 game-specific internal UI Definition 的能力，而不能只靠浏览器 `?host=` preview 或测试 fixture 证明 Host。

---

## 2. Core 永久一级 World Surfaces

以下五个 Surface 永久具现：

1. 人物 `surface.people`
2. 地图 `surface.map`
3. 物品 `surface.items`
4. 信息 `surface.information`
5. 目标 `surface.objectives`

即使当前无内容，也保留入口与轻量 Empty State。

领域对象继续保持独立 authoritative owner；Product UI 允许按玩家心智模型聚合：

- Character / Relationship / Commitment → 人物；
- Region / Place / Scene → 地图；
- Item / Ownership / public Placement → 物品；
- Knowledge / Clue / player-visible Event → 信息；
- Task / Objective → 目标。

UI 聚合不得制造第二份业务事实源。

---

## 3. 九类正式 Host Capability

Game Host 至少支持：

1. Player Status；
2. Player Character Detail；
3. Entity Detail；
4. Core Surface Contribution；
5. Map Overlay；
6. Narrative Contextual；
7. Global Notice / Alert；
8. Game Creation / Settings Contribution；
9. Extension Surface。

这些是 G9 可以依赖的 **Game Host Capability Facts**。

G9 可以设计外部资产如何声明它们，但不得反向增加一个尚未由 Game Host 验证的新执行能力。

---

## 4. 声明式 UI 的正式定义

酒馆游戏的声明式 UI 不是“资产写前端代码”。

正式模型：

```text
Definition declares
- what UI capability is needed
- where it belongs
- what safe component kind it uses
- ownership / dependency / information architecture

Host owns
- rendering
- layout
- responsive behavior
- accessibility
- visual system
- overflow / navigation
- safe interaction dispatch
```

Definition 可以声明：

- Extension Surface；
- secondary View；
- Section / Card / Badge / Meter / Fact / List / Filter 等受控组件；
- controlled Action Intent；
- Creation / Settings field contribution。

Definition 不得声明或执行：

- React component；
- JavaScript callback；
- DOM access；
- `eval`；
- CSS injection；
- 任意 HTML/script；
- pixel-layout ownership；
- direct Game State mutation。

原则：

> **允许声明信息架构与安全组件，不允许声明层执行前端。**

---

## 5. Declarative Structure ≠ Live Data

v1.1 新增正式边界：

> **声明式结构与当前运行值必须分离。**

Definition 负责表达：

> “我需要一个 Mana Meter / 伤势摘要 / 已学法术列表 / 势力栏目。”

但 Definition / Asset 不得拥有任意 Runtime 读取能力。

禁止把以下机制引入资产协议或浏览器：

```text
game.player.stats.mana
state.player.xxx
${state.xxx}
state => state.xxx
arbitrary selector
arbitrary query / expression DSL
```

正式数据流必须保持：

```text
Authoritative Runtime State
→ player-safe domain projection
→ bounded Host contribution projection / materialization
→ safe Product DTO
→ Host rendering
→ Browser
```

因此：

- Runtime / Adapter / Projection 拥有“当前值从哪里来”的服务端知识；
- Host Definition 只声明结构需求和合法 capability；
- Browser 只收到已经 materialized 的 player-safe DTO；
- Derived UI data 不反向成为 authoritative world state。

G8 只需冻结最小 typed bridge，不建设通用 state-path / expression / query engine。

---

## 6. Surface Ownership 与 Contribution

必须继续区分：

```text
owns surface.X
```

与：

```text
contributes to surface.X
```

一个唯一 Extension Surface 只能有一个 Owner。

两个独立 Definition 同时 owns 同一 Surface：

```text
A owns surface.X
+
B owns surface.X
→ SURFACE_OWNERSHIP_CONFLICT
→ INCOMPATIBLE before Game Instance creation
```

不得通过加载顺序、覆盖或隐式合并解决。

Contributor 向其他 Owner 的 Surface 贡献内容时必须有明确 dependency。

---

## 7. Contribution Source Identity 必须保留

v1.1 新增：Host Assembly 不得在 materialize Contribution 时丢失其来源 identity。

需要形成内部 typed bridge：

```text
sourceDefinitionId / internal owner identity
→ contribution source
→ gameplay / creation owner identity
```

禁止：

- 根据 displayName 推断 Owner；
- 根据字段主题推断来源；
- 根据相同 label 隐式合并；
- 让浏览器自己猜 Contribution 属于哪个玩法。

最终外部 Asset ID / gameplay ID 的 machine naming 由 G9 冻结；G8 只要求来源身份在内部链中可追踪、不可歧义。

---

## 8. Creation Contribution 必须使用 Creation Project 当前语言

旧 Five Sections Creation Contribution 语言正式废弃。

Host 的 Game Creation Contribution 必须与当前 Creation Project 对齐，placement 至少为：

```text
expansion_settings
character
opening
```

声明字段至少包含当前 Host/Creation 真正需要的结构信息：

- fieldId；
- label；
- component；
- aiFillAllowed；
- deferredAllowed；
- bounded options（若组件需要）。

Creation Project 继续拥有：

- value；
- lifecycle；
- ownership；
- active / dormant；
- autosave；
- Final Create compilation。

正式桥接：

```text
Runtime UI Definition declares safe creation contribution
→ Host validates placement/component
→ Creation adapter converts to current CreationContributionDefinition
→ Creation Project owns field state
```

必须支持 gameplay enable/disable：

```text
enabled  → contribution active
disabled → contribution dormant, value preserved
re-enable → prior value/lifecycle/ownership restored
```

Host 不推断字段，AI 只 Fill values，永远不 invent schema。

---

## 9. Host Action Intent 边界

受控 Intent 可以表达 UI 意图，例如：

- prefill composer；
- open surface。

Intent 不是 Formal Outcome，也不是 direct mutation capability。

资产/Definition 不能通过 UI Action 绕过 Runtime 权限、Program Judge 或正式 mutation path。

未来 G9 若增加外部声明，只能编译为 Host 已验证的受控 Intent vocabulary。

---

## 10. Player Surface Order 是 Durable Product Preference

玩家拥有 Core + Extension 一级 Surface 的最终排序权。

```text
Definition recommends
→ Host initializes
→ Player reorders
→ Player preference wins
```

Surface Order：

- 不属于 Game State；
- 不属于 canonical Save Snapshot；
- Restore 不回滚；
- 不形成 Formal Turn；
- 不推进 worldTime；
- 不生成普通世界 Event。

v1.1 进一步冻结：

> 正式 Product UI Preference 必须能够跨应用/服务重启持久化，而不仅是同一进程内存。

可以与游戏 SQLite 同文件，但必须是独立 Product Preference owner/table，不得塞入 canonical world state。

新增 Surface 时：

1. 保留已有玩家排序的相对顺序；
2. 新 Surface 按 Host 推荐位置确定性插入；
3. 无可靠推荐时追加；
4. 玩家仍可重新排序。

---

## 11. Extension Surface 内部结构

复杂 Extension Surface 可以声明受控：

- secondary View；
- Section；
- Host-owned sub-navigation；
- List / Card / Meter / Fact / Action 等安全组件。

Host 必须验证 secondary View 的 Owner 声明；Contributor 不能向不存在的 View 写入内容。

资产决定“需要表达什么”；Host 决定“如何安全呈现”。

---

## 12. Dynamic UI 的 G8 Definition of Done

在 G8 CLOSED 前，至少需要一组 **handwritten internal definitions** 通过真实 Product vertical proof：

```text
handwritten UI Definition
→ Host Assembly
→ Real Product Server
→ Product Session DTO
→ Formal Product UI
```

并证明：

1. Extension Surface / secondary View 根据声明出现；
2. Core / Status / Detail / Overlay / Contextual / Notice / Creation Contribution 可进入对应安全落点；
3. 至少一个 live contribution 的 player-safe 值能在 Formal Turn 后由 Runtime Projection 更新，而不替换 fixture；
4. duplicate Owner / invalid dependency / unsupported component / arbitrary code fail closed；
5. hidden/private Runtime data 不进入 Browser；
6. Surface Order 跨产品重启保持；
7. Restore 不回滚 UI Preference；
8. Creation Contribution 使用当前 Creation Project 语言并支持 active ↔ dormant。

完成这些后，G8 才能宣称：

> **声明式动态 UI Host 已作为正式 Game Capability 成立。**

---

## 13. G8 / G9 边界

### G8

负责冻结并真实验证：

- Host capability vocabulary；
- ownership / dependency；
- safe component vocabulary；
- declarative structure vs live data projection boundary；
- current Creation Contribution；
- source identity；
- durable UI preference；
- real Product vertical proof。

### G9

负责：

- External Asset machine schema；
- Surface/Contribution/Dependency 的最终 asset-spec naming；
- Validator；
- Asset Adapter；
- Creator compiler / authoring UX；
- external asset → Runtime UI Definition 的正式编译。

禁止：

> G9 asset-spec 为了支持某资产需求，反向发明一个 G8 Host 尚未验证的任意 UI execution / data-binding 能力。

如果确有新增 Host Capability 需求，应先回到 Host capability review，而不是在 Schema 中暗加前端执行权。

---

## 14. 当前阶段裁定

`G8-WEB-04` 当前状态：

> **FINAL CONVERGENCE ACTIVE**

当前 Codex 任务应完成：

- 正式 Bootstrap handwritten-definition path；
- declarative structure / live data typed bridge；
- Creation Contribution 对齐；
- source identity；
- durable UI Preference；
- rich Host real Product vertical proof。

在独立审核 PASS 前：

- 不开始正式 asset-spec vNext machine schema；
- 不开始 Creator 重建；
- 不把 fixture/preview 证明冒充 Production Host Closure。

---

## 15. 核心摘要

```text
Asset / Definition
= declares safe UI intent and information architecture

Runtime / Projection
= owns authoritative state and player-safe live values

Host
= validates + assembles + renders

Player
= owns final surface order preference
```

```text
Declarative UI
!= arbitrary frontend code
!= arbitrary state query
!= second game-state owner
```

```text
G8 validates Host capability first
→ G9 defines external asset protocol second
```

> **先把 Game Host 证明成真实能力，再让资产协议去声明这个能力。**
