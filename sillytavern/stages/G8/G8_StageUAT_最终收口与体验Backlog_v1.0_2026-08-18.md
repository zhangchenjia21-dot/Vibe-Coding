# G8｜Stage UAT 最终收口与体验 Backlog v1.0

状态：`CLOSED UAT EVIDENCE / NON-BLOCKING BACKLOG ROUTING`
日期：2026-08-18
最终 G8 代码对象：`sillytavern@cdbd9cd7ff0b5b9a5672156066478b57f732307c`

## 1. Project Owner Stage UAT 最终结论

```text
Project Owner Stage UAT   PASS WITH NON-BLOCKING UX FINDINGS
G8                        PASS / CLOSED
P0                        0
P1                        0
G9-01                     AUTHORIZED / NEXT
```

Owner 确认当前游戏已经可以持续运行；本轮剩余问题均属于体验、后续能力或产品管理，不再作为 G8 Stage blocker。

G8 的退出条件已经满足：真实 Creation 可形成具体开局世界；Runtime 可持续交互与 bounded local world growth；Narrative / Product / Runtime authority 已闭环；Save/Restore/Recovery 与 Dynamic Five 保持有效。

---

## 2. UX-01｜Item 资料的即时展示与“检查后资料成长”

### 观察

玩家对随身重要信件执行查看 / 打开等交互后，Narrative 已描述更多细节，但右侧【物品】中的随身 Item 没有同步表现这些细节；当 Item 被交给导师、从 `carried` 进入当前场景可见 Item 后，描述才明显出现在 UI。

### 当前代码事实

当前 Player Product UI 对 `carried` Item 卡片不展示 `publicDescription`；Item Detail 也只在该 Item 同时位于 `visibleItems` 时展示描述。因此已有 canonical `publicDescription` 在随身状态下存在 UI 展示遗漏。

另一方面，当前 `FormalDelta` 只拥有位置、Item placement、Runtime state、Knowledge / Commitment 等正式变化，不拥有 Game-local Item definition field patch。因此“玩家检查后，把新发现的外观 / 已知描述正式写回 Item dossier”尚不是 G8 Runtime capability。

### 裁定

拆成两层处理：

```text
UX-01A｜carried publicDescription 展示遗漏
Priority: P3 / Product polish
Route: G11 Alpha / Product UX cleanup

UX-01B｜inspection/discovery → Item known/public description evolution
Priority: P2 / architecture-use-case
Route: G9-02 acceptance input
```

G9-02 必须把该真实 UAT 例子纳入 Existing Game-local Asset Mutation proof：

```text
player inspects existing Item
→ Model proposes bounded Item known/public-description patch
→ Program validates
→ atomic Game-local Asset mutation
→ Product dossier refreshes from canonical value
→ Save / Restore reproduces same value
→ Source asset unchanged
```

这不是 G8 blocker；它正属于第 16 号裁定已经定义的 Game-local evolvable field mutation。

---

## 3. UX-02｜Objective / Goal authority 与“思考下一步”

### 观察

【目标】Surface 开局为空，会让部分玩家缺少方向感。Owner 希望玩家在“思考接下来做什么”等主动表达后，可以形成一条相对明确、可追求的目标，但不希望系统无条件替玩家决定目标。

### 当前设计事实

当前 Product Contract 明确冻结：Objective 必须来自未来 Runtime Task / Objective authority，Product 层不得从 Narrative 或 Commitment 自行伪造目标。因此空 Surface 不是 G8 接线 bug。

第 16 号裁定已经把 Objective 列为可长期存在的 Game-local Canonical record 类型之一。

### 裁定

```text
Priority: P2 Product Capability
G8 blocker: NO
G9-01: REQUIRED compatibility consideration
G9-02 implementation gate: NOT REQUIRED by default
Later implementation: dedicated Objective/Task vertical, no later than Alpha/G11
```

G9-01 必须确认 Source / Game-local / Runtime 三层和未来 asset-spec 不会把 Objective 排除在 canonical extensibility 之外，但本阶段不为了填空栏提前造一个不成熟的 Objective Engine。

未来正确链应是：

```text
Player reflection / explicit decision / formal commission
→ Semantic AI proposes Objective candidate
→ Program checks player agency / refs / scope
→ canonical Objective record
→ Product Objective Surface
```

不得：Narrative 说“你决定……” → Product 自动造目标。

---

## 4. UX-03｜Game Library 删除游戏

### 观察

【继续游戏 / 我的游戏】会积累大量测试世界，目前缺少删除入口，长期使用会显得杂乱。

### 裁定

```text
Priority: P3 / Alpha product management
G8 blocker: NO
G9 blocker: NO
Route: G11 Alpha / Game Library lifecycle management
```

删除是 destructive lifecycle operation，需要同时定义确认、SQLite/Game-local data 清理、存档/分支处理与错误恢复；不应为了清测试数据在 G8→G9 关口临时塞一个危险 DELETE 按钮。

---

## 5. UX-04｜DeepSeek 模型选择

### 观察

当前 API 设置展示固定模型，Owner 希望未来可以在支持的 DeepSeek 模型之间选择，例如 Pro / Flash 类模型。

### 当前代码事实

当前 API Settings Contract 明确为 `modelEditable: false`；本地 DeepSeek configuration 由启动时注入的固定 `model` 提供当前模型。

### 裁定

```text
Priority: P2 / Provider UX
G8 blocker: NO
G9 blocker: NO
Route: G10 Provider Expansion
```

G10 再建立受支持模型 registry / selector / connection-probe / restart semantics；届时应根据 DeepSeek 当时的官方 model IDs 与实际 strict-tool 兼容性确认可选项，不在 G8/G9 现在冻结具体 Flash 标识。

---

## 6. UX-05｜【人物】应是长期已知角色目录，而不是当前场景可见人物

### 观察

Owner 指出当前【人物】Surface 只显示当前 Scene 中可见角色，这与【物品】的“当前可见”语义混淆。正确产品语义应是：玩家已经见过、认识或明确得知身份的角色，应长期保留在【人物】资料中，离开当前场景后也不消失。

### 当前代码事实

截至 G8 final code：

```text
projectPlayerSession()
→ visibleCharactersInCurrentScene()
→ PlayerSession.visibleCharacters
→ Product People Surface
```

因此当前实现确实把 `Current Visible Characters` 与 `Player-known Character Directory` 合并了。

### 裁定

```text
Priority: P2 / Product-architecture semantics
G8 blocker: NO
Current G9-02A impact: NO / DO NOT INTERRUPT
G9-02B: REQUIRED Runtime owner/projection seam
G9-02C: REQUIRED bounded-context proof
G11: classification/search/filter/pagination product maturity
```

正式产品/架构裁定：

`17_酒馆游戏_PlayerKnownEntityDirectory与长期资料表面信息架构裁定_v1.0_2026-08-18.md`

核心：

```text
Current Scene Visible Characters
!=
Player-known Character Directory
```

People Surface 只显示玩家合法已知角色，而不是所有 Game-local Character；未认识 NPC 不得泄露。当前在场只作为 Directory entry 的 presence 状态之一。

长期玩家可能认识数十/数百角色，因此：

```text
Known Characters ↑↑↑
ordinary Turn Prompt Characters ≈ bounded
```

不得因为 People Directory 变大而线性扩大模型上下文。

---

## 7. 长期资料 Surface 分类整理规则

Owner 同时提出：如果【人物】【目标】及其它资料 Surface 长期堆叠信息，应提供分类整理能力。

正式路由：

```text
G9
→ 保留 stable ID / status / source / player-safe facet metadata seam
→ 不冻结具体 UI taxonomy

G11 Product maturity
→ grouping / filter / sort / search / active-history separation
→ scale needed 时 pagination / virtualization
```

当前明确重点：
- People：长期 Character Directory，未来分类；
- Objective：按 active/paused/completed/failed 等状态组织；
- Information：长期 Knowledge 分类，但仍禁止混入 generic event journal；
- Items：继续是 inventory/current visible semantics，不自动做历史物品目录；库存量大时再做分类/搜索。

---

## 8. 对 G9 的传播

这些 UAT 项不 reopen G8。

### G9-02A

继续原已发任务，不插入 People Directory 实现。

### G9-02B

新增：
1. Player-known Entity / Character Directory Runtime owner / projection seam；
2. current scene presence 与长期 player-known lifecycle 分离；
3. 未知/hidden Character 不得因 canonical existence 出现在 People Surface。

### G9-02C

新增压力验证：

```text
Known Character Directory size ↑↑↑
ordinary unrelated Turn context ≈ bounded
```

current-scene characters 与 referenced/relevant known characters 分开选取。

### G9-03

禁止：
- 把 Player-known Directory 做成 Source Character Card 字段；
- 把 current visibility 与 long-term acquaintance lifecycle 混合；
- 提前冻结 People/Objective/Information UI 分类 taxonomy。

---

## 9. 最终阶段状态

```text
G1–G8      PASS / CLOSED
G9         ACTIVE
Current    G9-02A
G9-02B     BLOCKED BY G9-02A
G9-02C     BLOCKED BY G9-02B
G9-03+     BLOCKED BY UPSTREAM DAG
```

Owner UAT 的非阻塞体验问题已正式进入后续路由，不再反复 reopen G8。
