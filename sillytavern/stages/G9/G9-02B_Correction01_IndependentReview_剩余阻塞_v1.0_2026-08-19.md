# G9-02B｜Correction-01 Independent Review｜剩余阻塞 v1.0

状态：`CURRENT REVIEW / FAIL / CORRECTION-02 REQUIRED`
日期：2026-08-19

## 1. Review 对象

实现仓库：`zhangchenjia21-dot/sillytavern`

```text
main                08f9dcf0a2f9e30b29d6d8391fa91701e57b6e23
reviewed branch     agent/g9-02b
correction head     76c7440e38a7ccdffd5c2dae4cf24012435e995c
correction message  fix: close player-known directory review blockers
```

Correction-01 已修复：

- C01 异地 knowledge 不再自动回填 canonical publicDescription；
- C02 离场人物不再读取 live relationship state；
- typed evidence 已进入 Formal Turn transaction；
- evidenceRef 已有 durable record identity；
- 0013 compatibility migration 已建立；
- 普通同场回合不再自动推进 lastInteractionTurn。

但仍有 3 个 P1，不能进入 main。

```text
P0  0
P1  3
```

---

## 2. P1-01｜目录 Definition payload 更新未同步 Game-local revision metadata

当前 `commitPlayerKnownEvidence()` 在已有人物 entry 上更新：

```text
domain_canonical_record.payload_json
```

但没有同步该 entry 对应 `game_local_asset` 的：

- `definition_revision`；
- `last_modified_turn`；
- `last_modified_revision`。

这会导致：

```text
Player-known dossier definition 已变化
!=
Game-local metadata revision 仍表示未变化
```

违反：

- G9-02A Game-local revision authority；
- Correction-01 C03 的 `mutation metadata consistent`；
- Save / Restore / Branch 对 definition revision 的一致性要求。

### Required

已有目录 entry 的 knownDisplayName / knownDescription 发生真实变化时：

```text
payload mutation
+
GameLocalAssetMetadata definition revision CAS/update
```

必须在同一 Formal Turn transaction 内完成。

如果新 evidence 只改变 Runtime last-known state，而不改变 dossier definition，则不得伪增 definitionRevision。

增加测试：

```text
evidence A 建立 dossier
→ evidence B 改 knownDescription
→ definitionRevision +1
→ lastModifiedTurn/Revision = B 所属 Formal Turn
→ retry B 不再增加 revision
```

---

## 3. P1-02｜lastInteractionTurn 仍在非互动 evidence 路径被错误推进

Correction-01 已修复：同场 observation 不再自动更新 lastInteractionTurn。

但当前仍有两个错误入口：

### 新 entry

`directoryState(...)` 默认：

```text
lastInteractionTurn = turn
```

因此玩家在第 12 回合只是通过传闻第一次知道某 NPC，系统会错误记录：

```text
lastInteractionTurn = 12
```

### 已有 entry

`commitPlayerKnownEvidence()` 对已有 entry 仍无条件：

```text
payload.lastInteractionTurn = turn
```

因此“听说守卫失踪”也会被记成“本回合和守卫互动”。

### Required

正式语义：

```text
lastSeenTurn
= 本回合玩家确实看见该角色

lastInteractionTurn
= 本回合存在明确 player↔character interaction evidence
```

`knowledge / introduction / creation` 本身不能自动推进 lastInteractionTurn。

首次 encounter 也只证明 seen；如果同一回合同时有明确 interaction target，才可写 interaction turn。

建议内部 API 明确传入 `interactionOccurred` / `lastInteractionTurn` 或等价 typed evidence；不得靠 sourceKind 猜。

增加测试：

1. 第 N 回合异地 knowledge 首次认识 → lastInteractionTurn 不等于 N；
2. 已知人物收到第二条 knowledge → lastInteractionTurn 保持旧值；
3. 第一次见面但没交互 → only lastSeen；
4. 第一次见面并明确 dialogue → lastSeen + lastInteraction 同回合。

---

## 4. P1-03｜0013 老存档升级伪造“第 0 回合已认识”历史

`0013_g9_player_known_directory_compatibility` 调用 `ensurePlayerKnownDirectoryFoundation()`，后者复用 `openingPlayerKnownSeeds()`。

但 `openingPlayerKnownSeeds()` 对 seed 固定写：

```text
knownSinceTurn = 0
createdTurn = 0
createdRevision = 0
lastSeenTurn = 0
```

这只适用于真正的新游戏 opening，不适用于已经玩到第 N 回合的 pre-02B game。

例如旧局当前在第 37 回合首次升级：

```text
升级时当前看见 NPC
```

只能证明：

```text
在第 37 回合升级时玩家现在看见他
```

不能证明：

```text
玩家第 0 回合就认识他
```

### Required

必须区分：

```text
new-game opening seed
vs
existing-game compatibility seed
```

Existing-game compatibility seed 应使用升级时当前权威：

- current `turnNumber`；
- current `revision`；
- current visible scene；

作为最早可证明的目录 evidence time。

不得猜测升级前已经离场的历史联系人，也不得回填第 0 回合。

增加测试：

```text
pre-02B game 先推进到 turn >= 2
→ 删除/不存在 02B directory foundation
→ 新代码执行 0013
→ 当前 visible NPC knownSinceTurn == upgrade current turn
→ createdRevision == upgrade current revision
→ 立即离场后仍保留
```

migration failure rollback 继续必须通过。

---

## 5. 已通过、不得回滚

- `visibleCharacters != knownCharacters`；
- People Surface 使用长期目录；
- stable `characterRef`；
- currentPresence 派生；
- 异地 knowledge dossier evidence-bounded；
- off-scene relationship 不 live refresh；
- unseen public NPC 不自动泄露；
- hidden Character fail closed；
- evidenceRef durable record；
- duplicate evidence 不重复修改 dossier/state；
- 02BC Host rails；
- bounded context projection；
- Product 不直接读取 Runtime truth。

---

## 6. 非阻塞后续 Guard

完整 Model-first Router 尚未实现，因此正常玩家语义如何生成 `PlayerKnownEvidenceDraft` 仍属于 G9-02C。

G9-02C 接入时必须继续遵守 G7 durable execution：如果 typed player-known evidence 在 semantic/domain 阶段形成，Crash/Recovery 必须复用 durable artifact，不得恢复时重新让模型判断。

这不是本次 Correction-02 的新增实现范围。

---

## 7. Review Verdict

```text
G9-02B Correction-01 Implementation   PARTIAL PASS
Independent Review                    FAIL
P0                                    0
P1                                    3
Main Integration                      NOT AUTHORIZED
Correction-02                         REQUIRED
```

Correction-02 只修上述 3 个 P1，不扩展 G9-02C，不重新设计 02BC Host。
