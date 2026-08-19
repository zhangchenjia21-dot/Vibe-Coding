# G9-02B｜Player-known Character Directory 最终 Independent Review v1.0

状态：`PASS / CLOSED`
日期：2026-08-19

## 1. Review 对象

实现仓库：`zhangchenjia21-dot/sillytavern`

最终实现 SHA：

```text
0ee847e1173ae8d17e643d5b838d238cf889031e
```

提交：

```text
fix: close remaining player-known consistency blockers
```

集成前 `origin/main`：

```text
08f9dcf0a2f9e30b29d6d8391fa91701e57b6e23
```

Review 期间任务分支：

```text
agent/g9-02b
```

## 2. Final Verdict

```text
G9-02B Implementation             PASS
Correction-01                     PASS after correction
Correction-02                     PASS
Independent Review                PASS / CLOSED
P0                                0
P1                                0
Main Integration                  FAST-FORWARD PASS
```

`0ee847e1173ae8d17e643d5b838d238cf889031e` 已验证为 `agent/g9-02b` 当前顶点，并且是 `08f9dcf0...` 的纯 fast-forward descendant；最终 `main` 已非强制快进到该精确 SHA，无 merge commit、squash 或 rebase。

## 3. 已冻结的 Player-known Directory 语义

```text
Canonical Character Truth
!= Player-known Character Directory
!= Current Scene Visible Character Set
```

已证明：

- `visibleCharacters` 继续只表示当前场景可感知 / 可互动人物；
- `knownCharacters` 是长期玩家已知人物目录；
- People Surface 消费长期目录，不再使用当前场景人物作为历史目录；
- 每个目录条目只引用稳定 `characterRef`，不创建第二 Character Truth；
- 未认识 public NPC 不进入目录；hidden Character fail closed；
- 角色离场后仍保留长期 membership；
- `currentPresence` 是当前 Runtime 派生值，不决定 membership；
- 离场人物不读取实时 Character state 或实时 Relationship state；
- last-known 只表达玩家已经合法知道的信息。

## 4. Evidence / Dossier Boundary

已证明：

- `encounter` 且 NPC 当前确实 visible 时，可 snapshot 当下公开资料；
- `introduction / knowledge / creation` 不得因为持有 `characterRef` 自动复制 canonical dossier；
- evidence 未授权 description 时，People 不获得 canonical publicDescription；
- Player-known evidence 使用稳定 `evidenceRef` 持久化；
- 相同 evidenceRef 重试是 no-op；
- evidence A → evidence B → retry B 不重复生效；
- crash / recovery 后 evidence exactly-once。

## 5. Revision Consistency

当长期 dossier 的 `knownDisplayName / knownDescription` 真实变化时：

```text
domain canonical payload change
+
Game-local definitionRevision +1
+
lastModifiedTurn / lastModifiedRevision = 当前 Formal Turn / revision
```

只改变 `knownStatus / lastKnownLocation / lastSeen / lastInteraction` 等 Runtime state 时，不推进 definitionRevision。

重复相同 evidenceRef 不重复推进 revision；Save / Restore 可恢复对应 dossier revision。

## 6. Seen / Interaction Semantics

正式冻结：

```text
lastSeenTurn
= 玩家确实看到人物

lastInteractionTurn
= 玩家与该人物存在明确 interaction evidence
```

不得因为：

- knowledge；
- introduction；
- creation；
- 首次进入目录；
- 单纯同场；

自动推进 `lastInteractionTurn`。

当前 number 字段继续以 `0` 表示尚无已记录互动。

## 7. Existing-game Compatibility

新游戏与老游戏升级 seed 已明确分离。

### New Game

```text
knownSinceTurn = 0
createdRevision = 0
```

### Existing Game Upgrade

0013 compatibility migration 只能证明升级当时当前场景可见的人物，因此使用升级时真实：

```text
turnNumber
revision
current scene
```

作为 knownSince / evidence / metadata / state 时间与修订来源。

不猜测旧历史中已经离场的人物，不伪造 turn 0 认识史，不伪造 lastInteraction。

migration failure 保持 atomic rollback。

## 8. Persistence / Recovery

已覆盖：

- Save-before / Save-after；
- Restore；
- Branch divergence；
- evidence exactly-once；
- crash / retry / recovery；
- Game-local metadata revision restore；
- 人物目录 membership 与 last-known state 恢复。

## 9. Context / Scale Guard

继续保持：

```text
Player-known
!= Runtime Relevant
!= Model Visible
```

当前 Player-known 模块 projection 仍为 bounded owner-scoped seam；完整 Model-first Router、routing working-set pruning、outcome-gated continuation 与 People Directory scale stress 保留给 G9-02C。

G9-02B 不授权把整个 known directory 自动注入普通 Turn prompt。

## 10. Regression Evidence

执行者报告：

```text
g9:02b:test           23 PASS
g9:02bc:test          13 PASS
g9:02a:test            9 PASS
g8:ui-host:test       18 PASS
g8:product-e2e         6 PASS
g5:test              207 PASS
g6:test               17 PASS
g7:test               20 PASS
g8:test              208 PASS
npm test              82 files / 718 tests PASS
typecheck             PASS
lint                  PASS
product:build         PASS
launcher:smoke        PASS
g2:disclosure         PASS
Real Provider calls    0
```

GitHub 当前无独立 CI status；以上本地 Gate 作为 supplemental execution evidence，最终结论以 exact-SHA code review 与不变量审计为主。

## 11. Scope Guard

本 Review 没有授权或宣称：

- G9-02C 已完成；
- G9-02 Integrated Closure 已完成；
- G9-03 已授权；
- external asset-spec 已冻结；
- Opening Scenario Runtime 已实现。

## 12. Next

```text
G9-02B Player-known Directory       PASS / CLOSED
↓
G9-02C Context Orchestration        ACTIVE / NEXT
```

G9-02C 继续处理：

1. 玩家语义 → 相关模块选择的模型优先路由；
2. 大量 enabled modules 下的 routing working-set pruning；
3. state-mandatory augmentation；
4. selected-only JIT context projection；
5. bounded owner-preserving join；
6. outcome-gated downstream continuation；
7. People Directory scale stress；
8. deterministic background zero-model-call。

最终结论：

```text
G9-02B PASS / CLOSED
G9-02C ACTIVE / NEXT
```
