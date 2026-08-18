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

## 6. 对 G9 的传播

这 4 项不会新增 G9-01 前置任务。

但传播两个真实约束：

### G9-01 Compatibility Audit

新增检查：

1. Objective / Task 类长期记录未来能够成为 Game-local Canonical Asset / Extension，而不被当前协议结构封死；
2. Item / Character / Place 等已存在 Game-local asset 的“已知描述 / public definition evolution”属于正式 mutable-field compatibility 范围。

### G9-02 Runtime Asset Binding + Game-local Layer

把本次信件 UAT 固定为 Existing Asset Mutation Reference Case：

```text
existing Item
→ inspection reveals durable public/known detail
→ typed local asset patch
→ Program commit
→ Product refresh
→ Save/Restore
→ source unchanged
```

不要求在 G9-02 同时实现完整 Objective Engine、Game Delete 或 Provider model selector。

---

## 7. 最终阶段状态

```text
G1–G8      PASS / CLOSED
G9         ACTIVE
Current    G9-01 Compatibility Audit
G9-02      BLOCKED BY G9-01
G9-03+     BLOCKED BY UPSTREAM DAG
```

Owner UAT 的非阻塞体验问题已正式进入后续路由，不再反复 reopen G8。
