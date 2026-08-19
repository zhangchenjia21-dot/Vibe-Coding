# G9-02B｜Player-known Character Directory Independent Review 阻塞发现 v1.0

状态：`CURRENT REVIEW / FAIL / CORRECTION REQUIRED`
日期：2026-08-19
审核对象：`sillytavern@6368bd12a7ca84463fec112d21c8fb6a97ef75a5`
任务分支：`agent/g9-02b`
当前 main：`08f9dcf0a2f9e30b29d6d8391fa91701e57b6e23`
任务原始基线：`5962e6f5933f245693e090cbdfd2f79791820ef1`

## 1. 结论

```text
G9-02B Implementation Review   FAIL
P0                             0
P1                             4
Main Integration               NOT AUTHORIZED
Task Branch                    KEEP / CORRECTION
Worktree                       KEEP / CORRECTION
G9-02C                         BLOCKED
G9-03                          NOT AUTHORIZED
```

本轮主体方向正确：

- `visibleCharacters` 与长期 `knownCharacters` 已拆分；
- People Surface 已改为消费长期目录；
- 目录条目引用稳定 `characterRef`，没有复制第二 Character Truth；
- `currentPresence` 从当前场景派生；
- 离场人物的公开状态在 Product People 中默认不继续读取 current Character state；
- 开局 encounter、离场保留、再次相遇、异地 typed knowledge、未认识 NPC、Save/Restore/Branch/Recovery 等已有 focused coverage；
- G9-02BC Host 继续被复用，没有另建第二插件系统。

但以下 P1 会破坏 #17 的 Player-known / live truth 分离或 G6/G7 revision/recovery 语义，必须先修正。

---

## 2. P1-01｜异地知识会在缺省字段时回填 Canonical Character 公开资料

当前 `recognizePlayerKnownCharacter()` 对一个 public Character，在调用方未提供 `knownDisplayName` / `knownDescription` 时回退到：

```text
character.displayName
character.publicDescription
```

这意味着 typed evidence 若只证明“玩家知道这个角色身份”，仍可能顺带把完整公开描述放进 Player-known dossier。

违反：

```text
Game-local Character public truth
!=
Player-known dossier
```

以及 G9-02B CASE D：异地身份知识只能显示本次 evidence 合法提供的 dossier。

### Required correction

- `encounter` 且角色当前确实 player-visible 时，可以从当下可感知的 canonical public fields 形成 encounter snapshot；
- `introduction / knowledge / creation` 不得自动使用未由 evidence 提供的 canonical dossier；
- 新建异地已知条目时，玩家可见名称/别名必须由 typed evidence 明确提供或由明确 creation-known seed 提供；
- description 没有 evidence 时应为空/省略/安全未知，而不是 canonical fallback；
- 增加 negative test：只提供 identity/name evidence 时，People 不得出现 canonical `publicDescription`。

---

## 3. P1-02｜离场已知人物会泄露当前实时 Relationship state

当前 `projectPlayerSession()` 将 Relationship 投影从：

```text
current visible character
```

扩大到：

```text
current visible OR player-known character
```

但 Relationship `publicState` 仍直接来自 Runtime 当前 canonical state。

因此已知 NPC 离场后，如果关系状态在玩家不知情的情况下变化，People 可能直接看到 live truth。

违反：

```text
Player-known / last-known
!= Runtime live truth
```

### Required correction

本轮不要求建立完整 Player-known Relationship history。

最小安全方案：

- 保持现有 `PlayerSession.relationships` 的 current-visible 安全语义；
- 离场人物如果没有独立 typed Player-known Relationship evidence/snapshot，People 中省略 relationship；
- 不得为了让离场 People 看起来信息更全而读取 live relationship state；
- 增加 negative test：NPC 离场后秘密改变 relationship runtime state，People 不得显示新值。

未来若要长期显示已知关系，应由独立 Player Knowledge evidence 演化，不由当前 live state 自动刷新。

---

## 4. P1-03｜已有目录条目的新 evidence 没有 durable identity / revision 语义

当前 `recognizePlayerKnownCharacter()`：

1. duplicate 检查只查 `domainCanonicalRecords.payload.evidenceRef`；目录 Entry 只保存首次 evidence；
2. 对已有人物的新 evidence 不创建独立 evidence record；
3. 更新 `knownStatus / lastKnownLocation` 时只改 `domain_runtime_state.payload_json`；
4. 不更新 `last_modified_turn / last_modified_revision`；
5. 不增加 game revision，也不属于 Formal Turn durable artifact。

因此：

- 第二条 evidence 的相同 retry 无法依靠 evidence identity 被可靠识别；
- 同一 evidence 在不同 payload 下可再次改变玩家知识；
- player-visible state 可以在 game revision 不变时发生变化；
- state mutation metadata 与实际 payload history 脱节；
- gameplay introduction/knowledge 尚未具备 G7 exactly-once 所需的 durable boundary。

### Required correction

不得修改 G9-02BC `RuntimeDomainFormalChangePlan` 的 shared cardinality 来绕过此问题。

应建立一个 bounded、Program-owned 的 Player Knowledge Evidence commit seam，满足：

```text
stable evidenceRef
+ explicit source kind
+ evidence-bounded known fields
+ atomic mutation
+ revision ownership
+ Save / Restore / Branch
+ retry/recovery exactly-once
```

推荐方向：

- `creation` 可在 bootstrap/revision 0 seed；
- current-scene `encounter` 继续在 Formal Turn transaction 内同步；
- gameplay `introduction / knowledge` 应作为 typed evidence 与产生它的 Formal Turn / revision 原子提交，不能依赖一个独立的无 revision store mutation；
- evidence identity 必须持久化，不能只把第一条 evidenceRef 存在 Directory Entry 上；可以复用当前 Domain persistence rails 建立 module-owned evidence record，或采用等价 bounded internal evidence ledger；不得新增通用插件系统；
- state `lastModifiedTurn / lastModifiedRevision` 必须与真实 mutation 一致。

必须新增：

- 同一第二条 evidence 连续 retry 两次只生效一次；
- evidence A → evidence B → retry B，B 不得再次改变状态；
- crash/recovery 后 evidence/entry/status exactly-once；
- player-visible knowledge mutation 不得在正常 gameplay 中绕过 formal revision ownership。

---

## 5. P1-04｜已有存档/游戏不会自动获得 Player-known foundation

当前 Player-known binding + opening seeds 只在 `store.bootstrap()` 中执行。

但正常已有游戏的启动路径是：

```text
readGame(gameId) exists
→ skip bootstrap
```

因此 pre-02B 游戏可能：

- 没有 Player-known module binding；
- 没有当前场景人物 seed；
- 打开 People 时为空；
- 如果升级后的第一回合直接移动，旧场景 NPC 可能在离场前从未进入 Directory。

### Required correction

需要 append-only 的已有游戏升级路径，优先使用 migration / deterministic compatibility upgrade：

- 为已有 game 补 Player-known module binding；
- 对升级时“当前场景内、public、可识别”的 NPC 建立安全 encounter seed；
- 不伪造已经无法从旧数据证明的历史离场人物；无法重建的旧历史联系人可以作为明确 legacy limitation，不得猜测；
- 升级不得改变现有 Game-local Character Truth / G9-02A lineage；
- migration/upgrade failure atomic rollback；
- 测试：pre-02B DB → 新代码打开 → 当前可见 NPC 已进入 known directory；第一动作立即离场后仍保留。

---

## 6. P2｜lastInteractionTurn 当前实际是“同场任意回合”

`syncPlayerKnownEncounters()` 对当前同场人物每次 Formal Turn 同时更新：

```text
lastSeenTurn
lastInteractionTurn
```

即使玩家只是等待、观察别处或执行与该 NPC 无关的动作，也会把它记录成“最近互动”。

这是非阻塞但应在本次 correction 顺手收口的语义偏差。

建议：

- `lastSeenTurn` 可由同场可见更新；
- `lastInteractionTurn` 只有明确 interaction evidence 时更新；
- 若当前无法可靠证明 interaction，宁可保留旧值，不要伪造互动。

---

## 7. Main / Worktree 状态

当前 `main` 已由 Project Owner 的人工提交推进到：

`08f9dcf0a2f9e30b29d6d8391fa91701e57b6e23`

该提交包含原先受保护的 README / `.gitignore` / 旧路线文件本地状态，早于 Grok 功能提交。

当前 Grok 功能提交：

`6368bd12a7ca84463fec112d21c8fb6a97ef75a5`

是当前 main 的直接 fast-forward 后代，因此 correction 继续在同一：

`agent/g9-02b`

完成即可。

Review FAIL 期间：

- 不更新 main；
- 不删除 worktree；
- 不删除 task branch；
- 不新建 fix branch；
- correction 追加 commit 到原分支。

---

## 8. Gate

Correction 后至少重新运行：

```text
npm run g9:02b:test
npm run g9:02bc:test
npm run g9:02a:test
npm run g8:ui-host:test
npm run g8:product-e2e
npm run g5:test
npm run g6:test
npm run g7:test
npm run g8:test
npm test
npm run typecheck
npm run lint
npm run product:build
npm run launcher:smoke
npm run g2:disclosure
git diff --check
```

真实 Provider 调用仍为 `0`。

Correction 完成只能返回：

`G9-02B CORRECTION READY FOR INDEPENDENT REVIEW`
