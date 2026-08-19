# TASK｜G9-02B Correction-01｜Player-known 边界修正

状态：`CURRENT CORRECTION PACKET`
执行者：Grok Build
任务分支：`agent/g9-02b`
Worktree：`D:\AI\Projects\.worktrees\sillytavern-agent`
Review 对象：`6368bd12a7ca84463fec112d21c8fb6a97ef75a5`
当前 main：`08f9dcf0a2f9e30b29d6d8391fa91701e57b6e23`

## 1. Outcome

不要重做 G9-02B 主体。

只修复 Independent Review 的四个 P1，并顺手收口一个 P2：

1. 异地 typed knowledge 不得自动获得 canonical dossier；
2. 离场人物不得读取 live relationship state；
3. 新 evidence 必须有 durable identity / revision / exactly-once；
4. pre-02B 已有游戏必须获得安全 directory foundation；
5. `lastInteractionTurn` 不得把“同场任意回合”伪装成互动。

Canonical Review：

`G9-02B_IndependentReview_玩家已知人物目录阻塞发现_v1.0_2026-08-19.md`

以该 Review 为本 correction 的完整验收依据。

---

## 2. Worktree / Git

**继续使用现有 worktree 和现有分支。不要新建分支。**

开始先确认：

```powershell
cd D:\AI\Projects\.worktrees\sillytavern-agent
git branch --show-current
git rev-parse HEAD
git status --short
git fetch origin
git rev-parse origin/main
git rev-parse origin/agent/g9-02b
```

必须满足：

```text
branch = agent/g9-02b
origin/main = 08f9dcf0a2f9e30b29d6d8391fa91701e57b6e23
origin/agent/g9-02b = 6368bd12a7ca84463fec112d21c8fb6a97ef75a5
```

如果 remote 已变化，先报告，不得自行 rebase/merge。

禁止：

```text
push origin main
git add .
git add -A
git reset --hard
git clean
git stash
git pull
git merge
git rebase
git commit --amend
force push
新建 fix branch
```

---

## 3. Required Corrections

### C01｜Evidence-bounded dossier

对 `introduction / knowledge / creation`：

- 不得因为已有 `characterRef` 就自动复制 Character canonical `displayName / publicDescription`；
- 玩家可见名称/别名、描述、状态、last-known location 必须来自 typed evidence / creation-known seed；
- evidence 未提供 description 时，不得回退 canonical description；
- `encounter` 且 NPC 当前确实 visible 时，可以 snapshot 当下 public fields。

必须新增 negative test：

```text
异地 knowledge 只给 identity/name
→ People 知道人名
→ 不知道 canonical publicDescription
```

### C02｜Off-scene relationship 不得 live refresh

撤销“known character 即可读取 current Runtime relationship state”的路径。

本轮最小安全行为：

```text
current visible character
→ 可显示当前 player-safe relationship

off-scene known character
+ 无独立 player-known relationship evidence
→ relationship 省略
```

不要在 correction 中扩成完整 Relationship Knowledge 系统。

新增 negative test：

```text
NPC 离场
→ Runtime relationship 私下/后台变化
→ People 不出现新 live value
```

### C03｜Evidence durable identity / revision

当前已有 entry 的第二条 evidence 不能只更新 `payload_json`。

必须满足：

```text
stable evidenceRef
→ durable persisted identity
→ retry same evidence = no-op
→ mutation metadata consistent
→ gameplay knowledge mutation belongs to formal revision ownership
→ Save / Restore / Branch / Recovery exactly-once
```

不要修改 G9-02BC shared `RuntimeDomainFormalChangePlan` cardinality。

可以使用当前 Domain persistence rails 建 module-owned evidence record，或等价 bounded internal evidence ledger；不得新增通用插件平台。

推荐职责：

- `creation`：bootstrap seed；
- `encounter`：Formal Turn transaction 内同步；
- gameplay `introduction / knowledge`：typed evidence 与产生该知识的 Formal Turn / revision 原子提交；
- 不保留一个可在正常 gameplay 中绕过 revision 的公共 player-visible mutation shortcut。

至少新增：

```text
evidence A
→ evidence B
→ retry B
→ B 不重复生效
```

以及 crash/recovery exactly-once。

### C04｜Existing-game compatibility

pre-02B 已有 DB/game 必须在升级后拥有：

- Player-known module binding；
- 升级时当前场景 public/visible NPC 的安全 encounter seed。

优先 append-only migration / deterministic compatibility upgrade。

禁止猜测无法从旧数据证明的历史离场联系人。

测试：

```text
pre-02B DB
→ 新代码打开
→ 当前 NPC 已在 known directory
→ 第一回合立即离场
→ NPC 仍保留 People
```

升级失败必须 atomic rollback。

### C05｜lastInteractionTurn

- 同场可见只能更新 `lastSeenTurn`；
- `lastInteractionTurn` 必须有明确 interaction evidence；
- 当前无法证明 interaction 时保持旧值。

---

## 4. Preserve What Already Passed

不得回滚：

- `visibleCharacters != knownCharacters`；
- People Surface 使用 known directory；
- stable `characterRef`；
- currentPresence 派生；
- unseen public NPC 不自动泄露；
- hidden Character fail closed；
- Save/Restore/Branch；
- 02BC Host rails；
- bounded player-known context projection；
- Product 不直接读取 Runtime state。

不要做 UI 大改。

---

## 5. Validation

至少：

```powershell
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

真实 Provider：`0`。

---

## 6. Deliver / Return

只 stage correction-owned exact files。

建议 commit：

`fix: close player-known directory review blockers`

然后：

```powershell
git push origin agent/g9-02b
```

不要合并 main，不要删除 worktree/branch。

Final Report 必须给：

- correction base SHA：`6368bd12...`
- new Final SHA
- branch / origin branch
- 四个 P1 各自的修改与 test evidence
- P2 处理
- regression results
- main untouched

最终状态只能写：

`G9-02B CORRECTION READY FOR INDEPENDENT REVIEW`
