---
title: my world｜G4-04 Multi-Game Storage Topology Decision
status: current-canonical-architecture-decision
version: 1.0
created: 2026-08-29
updated: 2026-08-29
task: G4-04 Multi-Game Lifecycle / Game Library Foundation
supersedes: MY_WORLD_架构_CURRENT.md section 11 unresolved per-Game-vs-shared choice
implementation_base: b227ff9043a25b3ebf7581eb340f3e2f9006a919
---

# G4-04｜Multi-Game Storage Topology Decision

## 1. Decision

G4-04 第一代正式采用：

> **One Game = One SQLite database.**

不采用：

> shared SQLite + `game_id` multi-tenant storage。

这不是长期禁止共享数据库，而是第一代在已有 G3 evidence 上的最小可靠选择。

---

## 2. Why this is the current canonical choice

当前 G3/G4-01 production seam 已经天然支持 per-Game physical isolation：

```text
Application
→ explicit database path
→ CurrentGameRuntime.open_current_game(path)
→ writer ownership derived from that path
→ SQLite connection for that path
→ backup/recovery root derived from that path
→ close/release that Game Session
```

当前 Runtime 对 existing DB 仍坚持 one-Game semantics：一个数据库中出现多个 Game identity 会被视为 ambiguous，而不是自动选择。

当前 database safety 同样按 database path 派生：

- writer lock；
- recovery root；
- physical backup；
- corruption classification；
- staged replacement / quarantine。

因此 per-Game SQLite 可以直接复用 G3 已通过的 persistence、single-writer、backup/corruption recovery 与 G4-01 Application/Game Session seam。

---

## 3. Why shared SQLite is rejected for G4-04

若现在改成 shared SQLite + `game_id`，至少需要重新定义：

- G3 one-current-Game verification；
- `CurrentGameRuntime` 的 zero/one/multi identity contract；
- 所有 persistence query 的 Game scoping；
- writer-lock blast radius；
- backup / corruption recovery 粒度；
- one corrupt physical DB 对全部 Games 的影响；
- migration / restore / quarantine semantics；
- G3 已验证证据的可复用程度。

这会把“需要多个 Game”升级成 persistence ownership redesign，与当前原则冲突：

> **不要从未来可能有很多 Game，直接推导出数据库服务层。**

---

## 4. First-generation physical model

正式方向：

```text
Game Library metadata
→ selects exact Game record
→ record resolves exact database path
→ open one Game Session
→ one SQLite database
```

新建 Game 的目标布局原则：

```text
user://my-world/games/<game_id>/game.sqlite
```

具体 Game Library metadata 文件布局由 G4-04 Task Packet 的 pre-implementation matrix 在不改变本决策的前提下冻结。

Game Library metadata 不得成为 gameplay truth。它只拥有 Application 需要的 index/projection，例如：

- stable Game identity reference；
- player-safe display metadata；
- exact database path/location；
- current/latest selection intent；
- legacy/adoption bookkeeping。

打开 Game 时必须用数据库内部 Game identity 交叉验证 Library record；不能只信 metadata 文件。

---

## 5. Legacy G3 Game adoption

现有：

```text
user://my-world/current-game.sqlite
```

G4-04 默认优先 **adopt in place**，而不是为了目录整齐立即搬迁数据库。

理由：

- 已有 DB 可能同时拥有 `.recovery` backup/quarantine evidence；
- 物理移动会额外制造 migration/crash boundary；
- Runtime 已能打开 explicit database path；
- Game Library record 可以安全记录这个 legacy exact path。

因此第一代允许：

```text
legacy Game Library record
→ database_path = existing current-game.sqlite
```

同时新 Games 使用 per-Game managed directory。

以后如果确有价值，可单独做 verified physical relocation；G4-04 不为目录统一制造一次高风险迁移。

---

## 6. Application lifecycle consequences

G4-01 seam 继续成立：

```text
Application Lifetime != Game Session Lifetime
```

G4-04 只把：

```text
Continue
→ fixed current-game.sqlite
```

升级为：

```text
Continue / Select Game
→ Game Library resolves exact existing Game record/path
→ open one Game Session
```

切换 Game 必须：

```text
close current Session completely
→ release SQLite/writer/provider/view ownership
→ resolve selected Game
→ open selected Session
```

不得同时保持两个 writable Game Sessions。

---

## 7. Creation boundary

G4-04 建立 Game Library 与多 Game coexistence，但不实现正式资产建局。

正式 New Game 的创建仍属于 G4-05/06：

```text
Wizard Composition
→ Final Review
→ Atomic Final Create
→ allocate independent Game identity/path
→ create per-Game SQLite
→ register Game Library record
```

G4-04 测试可以创建 task-owned multi-Game fixtures，但不能把测试 helper 变成玩家 New Game product path。

---

## 8. Required invariants for G4-04 implementation

```text
Game Library != Source Library
Game Library metadata != gameplay truth
One Game = one SQLite
one open writable Game Session at a time
Game record path must be safe / managed or explicitly adopted legacy path
Game Library record game_id must match DB internal identity when opening
missing/corrupt Game fails loud; never auto-create replacement on Continue/Switch
legacy G3 Game adoption is non-destructive
new Game must never overwrite another Game
G3 Save/Restore/Recovery semantics remain per Game
```

No production SQLite schema migration is authorized by this decision.

---

## 9. Decision status

```text
Design Gate: PASS
Chosen topology: per-Game SQLite
Shared SQLite + game_id: REJECTED for G4 first generation
Owner product decision required: NO
Implementation may proceed: YES, after current Task Packet issuance
```
