# 酒馆游戏｜Player-known Entity Directory 与长期资料表面信息架构裁定 v1.0

状态：`CURRENT DECISION`
日期：2026-08-18
scope:
- player-known-entity-directory
- people-surface
- long-lived-product-surfaces
- information-boundary
- g9-upstream

## 1. 核心裁定

正式区分：

```text
Current Scene Visible Characters
!=
Player-known Character Directory
```

【人物】Surface 的产品语义不是“当前场景里看得见谁”，而是：

> **玩家已经合法认识、见过或得知其明确身份的角色的长期 Player-known Directory。**

一旦角色合法进入该 Directory，角色记录默认跨场景长期保留；角色离开当前 Scene、远行、失联或死亡，都不应仅因当前不可见而从【人物】中消失。

当前 Scene 可见人物继续作为：
- 当前行动目标候选；
- Narrative interactable authority；
- current-scene Context；
- 即时 UI presence / “在场”标记。

两者不得再共用一个 DTO 集合表达。

---

## 2. Current Visible 与 Known Directory 的职责分离

### Current Visible Character Set

回答：

> “现在这个 Scene 里，玩家当前能直接感知并与之交互的人是谁？”

特点：
- 短期；
- 随移动即时变化；
- 用于 action / narrative grounding；
- 不自动保留历史人物索引语义。

### Player-known Character Directory

回答：

> “玩家到目前为止已经认识、见过或明确得知身份的角色有哪些？”

特点：
- 长期；
- 跨 Scene 保留；
- stable semantic identity；
- 只保存玩家有权知道的投影；
- 不因为 Runtime 实际知道 NPC 当前真实位置/秘密，就向玩家泄露。

因此正确模型类似：

```text
Game-local Character Canonical Truth
+
Player Knowledge / Encounter Evidence
↓
Player-known Character Directory Projection
```

而不是：

```text
all public Game-local Characters
→ People Surface
```

未被玩家认识的 public NPC 仍不得提前泄露。

---

## 3. Directory Entry 的最小语义

未来 Player-known Character entry 至少应能承载：

- stable `characterRef`；
- player-known display name / alias；
- player-known public description / dossier；
- relationship projection（若已知）；
- known affiliations / roles（若已知）；
- last interaction / last seen evidence（玩家安全）；
- current-presence flag（是否当前在场）；
- player-known status summary（若已知）；
- optional classification/facet metadata。

注意：

`last known` / `player-known` 不等于 Runtime live truth。

例如 NPC 已秘密前往另一座城市，但玩家不知道，则【人物】不得显示真实当前位置。

---

## 4. 进入 Directory 的合法条件

角色进入 People Directory 必须来自正式玩家知识来源，例如：

- 玩家实际见到并确认角色；
- NPC 正式自我介绍 / 他人可靠介绍；
- 玩家通过合法 Information / Knowledge 获得明确角色身份；
- Source / Creation 明确规定玩家开局已经认识某角色。

禁止：

```text
Game-local Character exists
→ automatically appears in People Surface
```

也禁止仅根据 display-name 相似度猜“玩家认识这个人”。

---

## 5. 永久保留与信息演化

“永久保留”指：

- 角色离场不删除；
- 当前不可见不删除；
- Save / Restore 按存档时知识状态恢复；
- Branch 可产生不同认识历史；
- 角色死亡后若玩家知道，可保留并显示已知状态；
- 后续发现真名、别名、身份、阵营、关系等时更新 Player-known dossier，而不是创建第二人物记录。

如果未来实现“遗忘/记忆受损”等特殊玩法，必须由明确 Domain / Runtime authority 修改 Player-known knowledge，不作为普通 UI 行为。

---

## 6. 与 Context Orchestration 的关系

正式冻结：

```text
Known Characters ↑↑↑
!=
ordinary Turn Prompt Characters ↑↑↑
```

玩家长期可能认识数十、数百角色；People Directory 可以很大，但 Context Orchestrator 只选择当前职责需要的 bounded subset。

典型：

```text
Current Scene Characters
→ current action / Narrative mandatory context

Known Directory
→ only retrieve intent-relevant / relationship-relevant / referenced characters
```

因此：

```text
Player-known
!= Runtime Relevant
!= Model Visible
```

本裁定扩展 #15 的集合分离原则。

---

## 7. 长期资料 Surface 的可扩展信息架构

对于会随长期游戏持续增长的资料 Surface，正式要求产品合同不得假定“永远是一张短平列表”。

当前明确包括：
- 人物；
- 信息 / Knowledge；
- 目标 / Objective；
- 未来可能持续增长的其它 dossier / extension surfaces。

未来 Product maturity 应支持按需：

- 分类 / Grouping；
- Filter；
- Sort；
- Search；
- Active / Historical 分层；
- status facets；
- pagination / virtualized presentation（规模需要时）。

本裁定不冻结具体 UI 分类名称，也不要求 G9-02A 立即实现上述交互。

---

## 8. People Surface 的最小未来分类方向

分类能力必须建立在 player-safe metadata 上，而不是浏览器猜语义。

候选维度可包括：
- 当前在场 / 不在场；
- 最近互动；
- 同行 / 熟人 / 关键人物；
- 已知关系；
- 已知组织 / 势力；
- 存活状态（仅玩家已知）；
- 自定义收藏 / 标记。

最终分类 taxonomy 延后到 Product / Alpha 设计，不在 G9 asset-spec 提前冻结。

---

## 9. 对 Objective / Information 等 Surface 的传播

### Objective

Objective 已有 `active / paused / completed / failed` 状态语义。未来数量增长时，至少应支持状态分类与历史收纳；完整 Objective Engine 仍按既定路线后置。

### Information

Knowledge 会随长期游戏增长。未来应允许按 subject / source / time / domain 等玩家安全 metadata 分类，但不能把 raw event journal 混回 Information Surface。

### Items

Items 当前主要是 current inventory / current visible scene objects，不自动变成“所有历史物品目录”。若玩家 inventory 长期膨胀，可在 Product maturity 阶段增加分类/搜索；不改变当前 canonical holder / placement 语义。

---

## 10. G9 Task Propagation

### G9-02A

`NO CHANGE / DO NOT INTERRUPT`。

当前已发 Codex 任务只做 Source Binding + Game-local Revision，不要求插入 Player-known Character Directory 实现。

### G9-02B

必须建立或保留 Player-known Entity / Character Directory 的正式 Runtime owner / projection seam，使 People Surface 不依赖 current scene visibility。

同时 Domain / Expansion Host 不得把“当前在场”与“长期已知”混成一个 record lifecycle。

### G9-02C

Context Orchestrator 必须证明：

```text
Known Character Directory size ↑↑↑
ordinary unrelated Turn context ≈ bounded
```

Current-scene characters 与 referenced/relevant known characters 分开选择。

### G9-03

external asset-spec 不应把 Player-known Directory 误建成 Source Character Card 的字段，也不冻结具体 People UI 分类 taxonomy。

### G11 / Product Maturity

实现 People / Information / Objective 等长期 Surface 的分类、筛选、搜索、历史分层等完整信息架构交互。

---

## 11. Current Code Deviation

截至 `sillytavern@cdbd9cd7ff0b5b9a5672156066478b57f732307c`：

```text
projectPlayerSession()
→ visibleCharactersInCurrentScene()
→ PlayerSession.visibleCharacters
→ Product People Surface
```

因此当前实现把 People Surface 与 current-scene visibility 混在一起。

这是已知 Product / projection deviation，但不阻塞正在执行的 G9-02A。

在 G9-02B / Product projection 收口时必须纠正。

---

## 12. 最终原则

```text
Current Visible
= 现在能直接互动的人

Player-known Directory
= 玩家已经认识、可长期查阅的人

Canonical Character Truth
= 世界真实角色定义
```

三者必须分离。

以及：

> **Long-lived dossier surfaces must scale by information architecture, not by silently dropping history.**
