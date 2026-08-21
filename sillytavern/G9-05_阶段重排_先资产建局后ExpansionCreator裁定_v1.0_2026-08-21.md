---
title: G9-05 阶段重排｜先资产建局后 Expansion Creator
status: current-decision-frozen
version: 1.0
date: 2026-08-21
stage: G9-05
---

# G9-05｜阶段重排｜先资产建局后 Expansion Creator 裁定 v1.0

## 1. 决策

在 G9-05D Character Creator 通过 exact-SHA Independent Review 并达到 `P0=0 / P1=0` 后，下一优先级不再是 Expansion Creator，而是：

```text
G9-05E｜Use My Assets Game Creation Vertical
```

即正式打通产品中已存在但当前仍 unavailable 的：

```text
创建新游戏
→ 使用我的资产库
→ 选择已发布 Source Assets
→ exact Game Asset Manifest
→ compatibility / binding / creation
→ 创建游戏实例
→ 进入可游玩 Session
```

Expansion Creator 顺延为：

```text
G9-05F｜Expansion Creator Vertical
```

并在 G9-05E PASS/CLOSED 前保持 `NOT AUTHORIZED`。

## 2. 为什么现在重排

当前代码早已预留 `use_my_assets` 创建方式和资产建局步骤，但仍因历史阶段理由保持 unavailable。现有顺序为：

```text
world_pack
→ expansion_packs
→ player_character
→ other_characters
→ opening_parameters
→ compatibility_validation
→ create_game_instance
```

G9-03 已冻结 `TavernGameAssetManifestV1`；G9-04 已证明 exact Manifest → Source Binding / lineage / Save-Restore；G9-05B 已建立正式 Source Asset Library；G9-05C 已完成 World Creator；G9-05D 正在完成 Character Creator。因此，World + Character 两类玩家可创建资产一旦闭环，即具备优先验证“资产是否真的能用来开游戏”的产品价值。

Expansion Creator 不是这一 Gate 的前置条件。`TavernGameAssetManifestV1.expansions` 可以为空；G9-05E 首个闭环允许：

```text
1 exact published World
+
0..N exact published Characters
+
0 published Expansions
```

后续若 Source Asset Library 中已有正式 Expansion，G9-05E 可以选择并验证；但不得为了本阶段要求先实现 Expansion Creator。

## 3. Draft / Source 边界

【使用我的资产库】只能消费**已发布 Source Asset**，不得直接使用 Creator Draft。

```text
Creator Draft
→ validate
→ explicit user Publish
→ Saved Source Asset
→ selectable by Use My Assets Game Creation
```

禁止：

- Draft 直接进入 `TavernGameAssetManifestV1`；
- Draft 自动发布；
- 建局时偷偷把 Draft 编译成临时 Source；
- 因“快速试玩”破坏 `Creator Draft != Saved Source Asset`。

允许产品层显示未发布 Draft 的提示和“返回 Creator 完成发布”入口，但 Draft 不得成为 selectable game asset。

## 4. G9-05E 首轮目标

G9-05E 详细产品/内部合同在 G9-05D 收口后另行冻结，但当前顺序和最低 Gate 先冻结：

1. 【使用我的资产库】从 unavailable 变为真实产品路径；
2. 从正式 Source Asset Library 列出可选 exact snapshots；
3. World：exactly one required；
4. Character：允许选择玩家角色候选与其他角色，身份必须 exact；
5. Expansion：0..N，可为空；若有正式 Source 才可选；
6. 创建时生成 exact `TavernGameAssetManifestV1`，禁止 stable-ref-only / latest-version 隐式解析；
7. 调用现有 G9-03 catalog / manifest validation；
8. 调用现有 G9-04 binding semantics / G9-02 lineage rails，不复制第二套 binding；
9. 明确区分 Character Source Binding 与 Character materialization；选为玩家角色不等于 Source binding 本身自动生成 Runtime state；
10. 创建出的游戏必须进入现有 Save / Continue / Restore / Recovery 轨道；
11. Source 新版本发布不得静默改变已经创建的游戏；
12. 无 Provider 时，纯已发布资产建局路径原则上仍应可完成，除非开场参数的已冻结 Runtime 合同明确需要模型；任何模型依赖必须在 G9-05E 规格中单独说明。

## 5. 后续 DAG

```text
G9-05C World Creator                 PASS / CLOSED
↓
G9-05D Character Creator             IMPLEMENTATION ACTIVE
↓ exact-SHA PASS only
G9-05E Use My Assets Game Creation   AUTHORIZED / NEXT AFTER D
↓
G9-05F Expansion Creator             DEFERRED / NOT AUTHORIZED
↓
三类主资产完整组合建局与游玩闭环
↓
Primary Asset End-to-End Closure Gate
```

## 6. 当前执行影响

当前正在执行的 `agent/g9-05d-character-creator` 任务**不变更、不重发、不追加新实现范围**。Grok 只完成已发出的 G9-05D。

G9-05D 通过 GPT exact-SHA Independent Review 后，GPT 才冻结 G9-05E 详细规格并生成新的独立 task branch / Task Packet。

Expansion Creator 不得由任何 Agent 在 G9-05E PASS/CLOSED 前自行启动。
