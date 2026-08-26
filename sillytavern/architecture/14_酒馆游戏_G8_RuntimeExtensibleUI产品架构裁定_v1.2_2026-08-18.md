---
title: G8 Runtime-extensible UI 产品架构裁定
status: current
version: 1.2
date: 2026-08-18
project: 酒馆游戏新版主体
stage: G8 网页产品化
supersedes: 14_酒馆游戏_G8_RuntimeExtensibleUI产品架构裁定_v1.1_2026-08-18.md
---

# G8 Runtime-extensible UI 产品架构裁定 v1.2

> 本文件是第 14 号核心来源的新版本，**SUPERSEDES v1.1**，不增加核心来源数量。
>
> v1.2 不改变 v1.1 冻结的产品语义，而是记录 `G8-WEB-04｜Final Host Convergence` 已通过正式实现与独立审核，Host 能力边界现在可以作为 G9 的真实上游。

## 1. 状态

`G8-WEB-04｜Final Host Convergence`：**PASS / CLOSED（2026-08-18）**。

正式代码基线：

`zhangchenjia21-dot/sillytavern@5c76f4302152a7598b54d7f9d5774616b1fd618d`

提交：

`feat: complete runtime-extensible ui host convergence`

## 2. 已冻结并真实证明的 Host 能力

Core 永久 World Surfaces：

```text
人物
地图
物品
信息
目标
```

合法 Host Capability 继续为九类：

1. Player Status；
2. Player Character Detail；
3. Entity Detail；
4. Core Surface Contribution；
5. Map Overlay；
6. Narrative Contextual；
7. Global Notice / Alert；
8. Game Creation / Settings Contribution；
9. Extension Surface。

继续冻结：

- Surface Owner / Contributor 分离；
- duplicate Definition / duplicate unique Surface Owner fail closed；
- Contributor 向其它 Owner Surface 贡献必须显式依赖；
- Extension Surface 可声明受控 secondary View / Section；
- Host 拥有布局、安全组件、响应式与最终渲染权；
- 禁止 arbitrary JS / React / DOM / CSS / eval / direct Game State mutation；
- 玩家拥有 Core + Extension Surface 最终排序权。

## 3. Production Bootstrap 已收口

正式 `startG2LocalServer` 已支持注入进程内 typed handwritten `PlayerProductRuntimeUiDefinition[]` 与当前 Creation gameplay catalog。

正式纵向链已证明：

```text
handwritten typed Runtime UI Definition
→ Host Assembly
→ Real Product Bootstrap / Server
→ Player-safe Product DTO
→ Formal Product UI consumer
```

无 definition 时保持 Core-only Host；默认正式游戏没有伪造 Magic / Politics 等 Expansion。

浏览器 query、Markdown、外部 manifest、asset repository 不是当前 Bootstrap truth。

## 4. Declarative Structure 与 Live Data 边界已冻结

正式原则：

```text
Declarative Definition
= 声明安全 UI 结构与受控数据需求

Runtime / Player-safe Projection
= 产生当前公开值

Host Materializer
= 把已校验声明 + player-safe snapshot 物化成浏览器 DTO
```

当前 live binding 只允许从已经通过 Runtime L3 disclosure 的公开状态槽位中选择受控 semanticRef / slotRef。

明确禁止：

- arbitrary `statePath`；
- JS selector；
- callback；
- expression language；
- eval；
- 通用 query DSL；
- 声明层直接读取 Runtime Game State。

`dataSource` 只存在于服务端 prepared declaration；浏览器只得到 materialized contribution DTO。

Formal Turn 动态纵切已证明：同一 declarative Contribution 在 Runtime public state 提交后，通过新的 Product Projection 自动显示新值，不靠替换 fixture 或手工改浏览器 DTO。

## 5. Creation Contribution 已与 Creation Project 对齐

Host `game_creation_settings` 不再使用旧 Five Sections 的：

- `sectionId`；
- `required`；
- `playerEditable`；
- `GAME_CREATION_SECTIONS` / `CORE_GAME_CREATION_FIELDS` adapter。

当前正式 Creation placement：

```text
expansion_settings
character
opening
```

链路：

```text
Runtime UI Definition declares safe creation contribution
→ Host validates
→ sourceDefinitionId / ownerGameplayId preserved
→ Creation L3 adapter
→ CreationContributionDefinition
→ Creation Project owns value / lifecycle / ownership
```

玩法 disabled → Contribution dormant 且保留值；重新 enabled → 恢复原值、lifecycle 与 player ownership。

## 6. Source / Owner Identity

Host Assembly 自动写入稳定 `sourceDefinitionId`；Creation bridge 使用显式 `ownerGameplayId`。

禁止从：

- displayName；
- title；
- field label；
- topic semantics；

推断 Owner。

duplicate definition identity / duplicate gameplay owner fail closed。

最终外部 Asset ID、Schema 字段命名仍属于 G9。

## 7. Product UI Preference 已成为独立 durable owner

Surface Order 使用独立 SQLite Product UI Preference 表持久化。

它：

- 不属于 Runtime Game State；
- 不属于 canonical Save Snapshot；
- 不形成 Formal Turn / Event / worldTime；
- 产品完整重启后可恢复；
- Restore 不回滚；
- 新增 Surface 时保留玩家已有相对顺序，并按 Host 推荐位置确定性插入。

## 8. Fail-closed 与 Disclosure

当前已覆盖：

- duplicate definition；
- duplicate gameplay owner；
- duplicate Surface Owner；
- missing Surface dependency；
- missing secondary view；
- invalid component；
- arbitrary binding / arbitrary frontend code；
- partial Host 不暴露。

Product Host live-data snapshot 只从已经 player-safe 的 Session projection 裁剪最小公开集合，不包含 hidden/private/provider/reasoning 数据。

## 9. G8 / G9 边界

G8 Host 已完成真实能力证明。

G9 可以把外部 Asset / Creator 声明编译到这些已验证能力，但不得反向新增：

- arbitrary frontend execution；
- arbitrary state path / expression/query engine；
- direct Game State mutation；
- 未经 Host capability review 的新 UI execution capability。

如果真实资产提出 Host 当前不存在的能力，应先回到 Host capability review，再扩协议。

## 10. 自动化与审核说明

Codex 报告本地 Gate：

- `g8:test` 183 PASS；
- `g8:creation-project:test` 18 PASS；
- `g8:product-e2e` 6 PASS；
- `g8:ui-host:test` 18 PASS；
- full tests 643 PASS；
- typecheck / lint / product build / launcher smoke / disclosure / diff-check PASS；
- automated real Provider calls = 0。

GitHub 当前未返回该 commit 的 CI status，因此上述数字属于实现者本地 supplemental evidence。

独立审核通过 exact-SHA 代码与测试验证了：production Bootstrap、player-safe materialization、Creation bridge、source identity、dormant lifecycle、durable preference、Restore boundary、live Turn dynamic projection 与 fail-closed contract。

结论：

> **G8-WEB-04 PASS / CLOSED。**

下一 G8 正式任务：

> `G8-WEB-05｜Technical Migration Closure` —— retire legacy One Draft / Five Sections production authority，使正式 New Game authority 收敛为 Creation Project only。
