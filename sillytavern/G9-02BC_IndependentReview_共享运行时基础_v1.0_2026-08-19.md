# G9-02BC｜共享运行时基础 Independent Review v1.0

状态：`PASS / CLOSED`
日期：2026-08-19
审核对象：`zhangchenjia21-dot/sillytavern@5962e6f5933f245693e090cbdfd2f79791820ef1`
基线：`04603e1e4a3270e9f5740b5957cf545a2bd001d0`

## 1. 结论

```text
G9-02BC implementation        PASS
Independent Review            PASS / CLOSED
P0                            0
P1                            0
G9-02B breadth                AUTHORIZED / NEXT
G9-02C breadth                BLOCKED BY G9-02B closure
G9-03                         NOT AUTHORIZED
```

本审核确认：G9-02BC 已按 current spec 建立 02B / 02C 共用的 Program-owned Runtime rails，没有提前实现完整 Router、external schema、Creator 或 Opening Scenario。

## 2. 通过项

### 2.1 领域模块与所有权

- Program-built `RuntimeDomainModuleHost` 是模块代码注册与 owner resolution authority；
- `moduleRef` / `ownerNamespace` 唯一，unknown / duplicate fail closed；
- Asset / Definition 只能引用 Program 已注册 module identity，不携带可执行 JS / TS / callback / eval / query / state path；
- Package included / Feature enabled / Module enabled 三层独立；
- hard dependency 只控制 availability，不自动进入 Context。

### 2.2 Canonical Record / Runtime State

- 非 Core Domain Canonical Record 通过 `recordRef` 对接 G9-02A `GameLocalAssetMetadata`；
- 没有建立第二套 Game-local identity / lineage authority；
- Domain Runtime State 使用独立 `stateRef` 与 revision metadata；
- Core Character / Item / Scene / Relationship 专用表没有被强制迁入 generic domain storage；
- payload 仅允许 bounded scalar envelope，并必须通过 registered module validator。

### 2.3 Formal Turn / Persistence

- Domain change 在 SQLite Formal Turn transaction 内重新经 Host 校验；
- record mutation 使用 payload compare-and-swap + Game-local definition revision；
- runtime state 使用 payload compare-and-swap + last-modified revision；
- Domain Event / Handoff 与 Formal Turn、Narrative、time、durable checkpoint 同事务提交；
- Save snapshot 已覆盖 binding / canonical record / runtime state / event / handoff；
- Save-before / Save-after / Restore / Branch / restore fault rollback 有 focused evidence；
- semantic-ready crash 后 durable domain change 被复用，selector / candidate 不重复执行，Formal Turn / change / event / handoff exactly-once。

### 2.4 Context Foundation

固定轨道：

```text
最小路由目录
→ 选择模块
→ Program 校验
→ 仅对选中模块按需投影
→ 保留所有者身份的有界上下文
→ typed candidate
→ Program-validated domain change
```

- Selector 只能拿 routing directory，不直接拿 Domain records/states；
- JIT projection 只发生在 selected modules；
- joined context 不压平 owner identity；
- 100-module fixture 中只选择 / 投影 1 个 module；
- dependency 不递归扩展 Context；
- hidden reference state 未进入 joined context。

### 2.5 Migration / Regression

- SQLite migration `0012_g9_domain_module_foundation` append-only；
- migration fault 回滚，新 migration record/table 不残留，同时 0011 G9-02A lineage migration 保留；
- constructor migration failure 会关闭 SQLite handle，避免 Windows file lock；
- Core-only snapshot 无 Domain 数据时不错误要求 Domain Host；
- Codex 报告 focused 13/13、G9-02A 9/9、full suite 695/695，以及 G5–G8 / typecheck / lint / build / launcher / disclosure 全部 PASS；GitHub 当前无独立 CI workflow，因此这些本地 Gate 作为 executor evidence，与本次 exact-SHA architecture review 分开记录。

## 3. 非阻塞后续 Gate

以下不是 02BC blocker，而是 G9-02C 明确剩余工作，后续不得误报为已完成：

### C-01｜玩家语义 → 模块选择授权闭环

当前 foundation 证明“模块被选中后如何安全运行”，尚未冻结完整 Model-first Router。02C 必须让模型根据玩家输入做开放语义选择，同时保留 evidence / agency / Open Attempt，Program 不用关键词重复 NLP。

### C-02｜Routing Directory 进一步预筛

当前 large-registry proof 证明 selected-only JIT projection，不等于 Router 输入已与启用模块总数完全解耦。02C 应实现 enabled/pruned routing working set，避免普通 Turn 的 Router 输入随大量 enabled modules 无界增长。

### C-03｜Outcome-gated continuation

02BC 已建立 Event / Handoff envelope，但 02C 仍需证明正式上游 Outcome / Trigger 成立后才激活下游 continuation；Dependency 本身不得提前触发或扩大 Prompt。

### C-04｜People Directory stress

#17 的长期 Player-known Character Directory 仍由 G9-02B breadth 实现；完成后 02C 必须证明：

```text
Known Character Directory size ↑↑↑
ordinary unrelated Turn context ≈ bounded
```

## 4. 资源安排

若仍有约一次 Sol 深任务额度，不建议立即消耗在 02B 常规 breadth。

推荐顺序：

```text
G9-02BC PASS / CLOSED
↓
G9-02B breadth
由 Grok Build 在 frozen rails 上实现
↓
GPT exact-SHA review
↓
最后一次 Sol
优先用于 G9-02C Model-first Routing / Context Orchestration 核心
↓
其余 02C breadth 可继续由 Grok Build 扩展
```

原因：02B 的 Package/Feature/Module、Domain persistence、projection rails 已由 02BC 冻结，剩余主要是 bounded breadth；02C 仍涉及模型相关性判断、上下文成本和 authority boundary，是剩余返工半径最大的深水区。

## 5. Final

> **G9-02BC SHARED FOUNDATION PASS / CLOSED. G9-02B BREADTH AUTHORIZED / NEXT.**

不得据此宣布 G9-02B、G9-02C、G9-02 或 G9 整体关闭；G9-03 仍未授权。
