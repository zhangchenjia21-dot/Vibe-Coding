# G9-02B｜Runtime Domain Breadth 与 Player-known Character Directory 规格 v1.0

状态：`CURRENT SPEC / ACTIVE NEXT`
日期：2026-08-19
阶段：G9-02B breadth
前置：
- `G9-02A PASS / CLOSED`
- `G9-02BC Shared Foundation PASS / CLOSED`
实现基线：`sillytavern@5962e6f5933f245693e090cbdfd2f79791820ef1`
默认执行者：Grok Build

## 1. Outcome

本任务在 G9-02BC 已冻结的内部轨道上完成 G9-02B 的真实 breadth，核心产品结果是：

> **【人物】Surface 从“当前场景可见人物”纠正为“玩家长期已知人物目录”，同时保留当前场景可见人物作为独立的即时互动集合。**

完成后至少形成：

```text
Game-local Character Canonical Truth
+
Program-owned Player Knowledge / Encounter Evidence
↓
Player-known Character Directory
↓
玩家安全长期投影
↓
Product People Surface
```

同时继续保持：

```text
Current Scene Visible Characters
= 当前可直接感知 / 互动

Player-known Character Directory
= 玩家已经合法认识 / 明确得知身份、可长期查阅

Canonical Character Truth
= 世界真实角色定义
```

三者不得共用同一生命周期或同一 DTO 集合。

## 2. 为什么现在做

当前实现仍是：

```text
projectPlayerSession()
→ visibleCharactersInCurrentScene()
→ PlayerSession.visibleCharacters
→ Product People Surface
```

导致角色一离开当前 Scene，就会从【人物】中消失。

G9-02BC 已提供：

- Program-owned built-in Domain Module Host；
- Package / Feature / Module activation；
- owner-scoped Domain Canonical Record / Runtime State；
- typed Domain Change / Event / Handoff；
- Routing Directory / JIT Projection boundary；
- Save / Restore / Branch / Recovery。

因此 02B 不再设计第二套平台，只需在这些 frozen rails 上实现真实长期人物目录并完成 Product 迁移。

## 3. 最高优先不变量

### INV-B01｜三种人物集合必须分离

```text
Canonical Character Truth
!= Player-known Character Directory
!= Current Scene Visible Character Set
```

- `visibleCharacters` 或等价集合继续服务当前 action / narrative / presence；
- People Surface 必须消费独立长期目录；
- 不得为了 People Surface 把 `visibleCharacters` 改成历史人物集合。

### INV-B02｜目录不是第二份 Character Truth

Player-known entry 表达的是：

> “玩家目前依法知道这个角色的哪些信息。”

不是复制一份独立 Character canonical object。

目录条目必须引用 stable `characterRef`，不能创建第二角色 identity。

目录里允许存在 player-known snapshot / dossier，但它的 authority 是 **Player Knowledge**，不是 Character Truth。

### INV-B03｜未认识 NPC 不得泄露

禁止：

```text
Game-local Character exists
→ automatically People Surface
```

也禁止：

- all public characters 自动进入目录；
- displayName / alias 文本相似度猜身份；
- 因 Runtime 知道 NPC 当前真实位置 / 状态而向玩家泄露。

### INV-B04｜合法进入来源

目录 membership / dossier mutation 必须来自显式 Player Knowledge / Encounter Evidence，至少支持以下来源类型：

1. `encounter`：玩家实际同场感知并获得可识别身份；
2. `introduction` / `knowledge`：正式介绍或合法 Knowledge 明确给出角色身份；
3. `creation`：创建游戏时明确规定玩家已认识该角色。

精确 TypeScript 枚举名可由实现决定，但 source kind 必须显式，不得仅靠文本猜测。

本任务不要求新建一套自然语言 Router 来判断“介绍”；可以用 existing formal knowledge / typed internal seam / deterministic reference fixture 证明该入口可被未来 02C 调用。

### INV-B05｜长期保留

一旦合法进入目录，默认：

- 离开 Scene 不删除；
- 远行不删除；
- 失联不删除；
- 死亡不删除；
- 当前不可见不删除。

只有未来明确的“遗忘 / 记忆修改”Domain authority 才能移除或改变长期知识；本任务不实现该玩法。

### INV-B06｜当前在场必须是派生值

`currentPresence` / “当前在场”只表示当前 Scene 可见性，不得决定目录 membership。

推荐语义：

```text
Directory membership = durable knowledge
Current presence = current runtime projection
```

不得把 presence 持久化成会过期的 live truth，除非字段明确是 `lastKnown...`。

### INV-B07｜Last-known 不等于 Live Truth

若存储：

- last seen scene；
- last interaction turn；
- last known status；
- known affiliation / role；

它们必须明确是玩家最后合法知道的值。

NPC 后续秘密移动或状态变化，玩家没有获得新 evidence 时，People 不得自动显示最新 Runtime truth。

### INV-B08｜玩家安全 dossier 可演化

同一个 `characterRef` 后续可更新：

- known name / alias；
- player-known description；
- known role / affiliation；
- known relationship；
- player-known status summary；
- last interaction / last seen evidence。

更新同一目录条目，不建立第二人物记录。

### INV-B09｜不冻结分类 taxonomy

可以保留未来安全分类所需 metadata seam，例如：

- current presence；
- last interaction turn；
- known status；
- known affiliation；
- generic facet/status metadata。

但不得在 G9 提前冻结：

- 同行 / 熟人 / 关键人物等最终分类；
- 完整筛选 / 搜索 / 排序 UI；
- 收藏 / 标签产品规则。

这些属于 G11 Product maturity。

### INV-B10｜必须复用 02BC rails

如果目录采用 Domain Canonical Record / Domain Runtime State：

- 使用 Program 注册的 built-in module；
- explicit module/owner identity；
- 复用 G9-02A Game-local metadata；
- module-owned typed validation；
- Formal Turn / Save / Restore / Recovery boundary。

不得新增第二个插件系统、第二个通用 owner registry 或 arbitrary JSON mutation path。

如果某一小部分使用 specialized table 更符合语义，也必须仍由同一个 Runtime Owner / projection seam 负责，且不能绕过 02BC Host 形成平行扩展平台。

### INV-B11｜Product 只消费玩家安全目录 DTO

Runtime L3 必须提供独立于 `visibleCharacters` 的 player-known directory projection，例如 `knownCharacters` 或语义等价字段。

Product People Surface 必须改用该独立字段。

浏览器 / Product 不得读取 Game State 自己推导目录。

### INV-B12｜即时互动不回归

`visibleCharacters` 继续：

- 当前行动目标；
- Narrative grounding；
- current-scene presence；
- 当前场景必须上下文。

02B 不得为了修 People，把所有历史人物混进 action / Narrative current-scene collection。

### INV-B13｜Relationship / Knowledge 只显示玩家已知部分

People entry 可以关联 relationship / commitments / knowledge，但：

- 只有已有玩家安全证据的内容才能显示；
- 不得因为 Character canonical truth 或 hidden relationship 存在就自动展示；
- 当前系统若缺 subject linkage，不得把全局 knownInformation 复制到每一个人物条目伪装为“相关信息”。

允许本轮只提供有可靠 refs 的最小关联视图。

### INV-B14｜目录增长不等于 Prompt 增长

本任务只提供未来 02C 可按需查询 / 投影的 owner seam。

禁止：

```text
Player-known Directory
→ every ordinary Turn prompt includes all known characters
```

完整 relevance routing / scale stress 属于 02C，但 02B 数据结构不得强迫全量注入。

## 4. 推荐内部模型

精确命名由实现者决定，但语义建议如下。

### 4.1 Built-in Player Knowledge Domain Module

使用一个 Program-owned built-in module 作为 Player-known Character Directory owner，避免把目录逻辑散落在 Product/UI。

至少拥有：

- stable module identity；
- stable owner namespace；
- typed player-known character entry validator；
- typed evidence / update candidate；
- player-safe directory projection；
- Save / Restore / Recovery-compatible mutation path。

### 4.2 Player-known Character Entry

至少能够表达：

- `characterRef`；
- known display name / alias；
- known public dossier / description；
- `knownSinceTurn` 或等价证据时间；
- `lastInteractionTurn` / `lastSeenTurn`（若已知）；
- optional last-known location/status；
- evidence source kind/ref；
- 可选安全 facet seam。

当前 presence 应在投影时根据 current visible set 派生，不作为 membership source。

### 4.3 Evidence

Evidence 至少要有 stable identity / idempotency 语义，避免 retry/recovery 重复创建目录条目。

同一角色重复 encounter：

- 不创建第二 entry；
- 可以更新 last-seen / last-interaction；
- definition/knowledge revision 只在真实知识变化时增加。

## 5. Production Integration

至少覆盖以下真实路径。

### CASE A｜开局当前场景人物

玩家进入一个已有公开、可识别 NPC 的开局 Scene。

Expected：

- NPC 属于 current visible；
- NPC 合法进入 player-known directory；
- People 显示该 NPC；
- 不需要额外模型调用。

### CASE B｜离场后长期保留

玩家离开 NPC 所在 Scene。

Expected：

- NPC 不再属于 current visible；
- People 仍保留该 NPC；
- `currentPresence=false`；
- 不泄露 NPC 后续秘密位置。

### CASE C｜再次相遇

玩家再次见到该 NPC。

Expected：

- 同一个 `characterRef`；
- 目录不重复；
- last interaction / seen evidence 可更新；
- current presence 恢复 true。

### CASE D｜合法得知异地 NPC 身份

通过 typed formal knowledge / introduction evidence 得知一个当前不在 Scene 的角色身份。

Expected：

- 可以进入 Directory；
- current presence=false；
- 只显示本次 evidence 合法提供的 dossier；
- 不泄露 Runtime 当前真实位置/状态。

本任务不要求开发新的 NLP Router 来生成该 evidence；重点是 production owner seam 可正确承接 typed evidence。

### CASE E｜未认识 public NPC

Game-local 中存在公开 NPC，但玩家从未 encounter / introduction / knowledge。

Expected：People 完全不可见。

### CASE F｜角色死亡 / 失踪

如果玩家已认识角色：

- membership 不删除；
- 只有玩家合法得知死亡 / 失踪时才更新 known status；
- Runtime hidden live status 不自动泄露。

## 6. Save / Restore / Branch / Recovery

必须证明：

### Save-before-knowing

```text
S1
→ 认识 NPC
→ Restore S1
```

Expected：该 Directory entry / evidence 消失。

### Save-after-knowing

```text
认识 NPC
→ S2
→ 离场 / 更新 dossier
→ Restore S2
```

Expected：恢复 S2 的 known dossier / evidence。

### Branch

Restore 旧存档后可以形成不同的认识历史，不污染 archived future。

### Retry / Recovery

同一 evidence / directory mutation：

- no duplicate entry；
- no duplicate evidence；
- no duplicate Formal Turn / Domain Event；
- exactly-once。

## 7. Product Contract 迁移

必须新增独立 Player-safe DTO，不能继续让 People Surface 的类型来源是：

`PlayerSessionView['visibleCharacters']`

建议：

```text
PlayerSession.visibleCharacters
= current scene only

PlayerSession.knownCharacters
= long-lived directory
```

具体字段名可调整，但语义必须明确。

Product People Surface：

```text
surface.people
→ known directory
```

即时 scene / action / Narrative：

```text
visibleCharacters
→ unchanged current presence semantics
```

若存在 deprecated `right.people` compatibility view，也必须与新 People 语义一致，不能继续引用 current visible。

Player UI 只消费 Product Contract，不直接读取 Runtime。

## 8. UI Host / Contribution Boundary

02B 只要求：

- People Surface 继续通过正式 Product Host 投影；
- Extension contribution 仍为声明式 / player-safe；
- source / owner identity 保留；
- 不新增 callback / arbitrary query / state path；
- 不做视觉大改版。

若现有 Player UI 能自动消费新的 People Surface 数据，只做必要 contract / rendering compatibility 修改。

## 9. Tests / Acceptance

至少：

AC-B01：`visibleCharacters` 与 `knownCharacters` / equivalent 是两个独立 DTO collections。

AC-B02：开局可见 public NPC 合法进入 Directory。

AC-B03：移动离场后 NPC 从 visible 消失，但仍在 People。

AC-B04：再次相遇不重复创建 entry。

AC-B05：typed introduction / knowledge evidence 可让 off-scene NPC 进入 Directory。

AC-B06：unseen public NPC 不进入 Directory。

AC-B07：hidden NPC / hidden identity 不泄露。

AC-B08：last-known location/status 不自动跟随 hidden live truth。

AC-B09：角色死亡/失踪不会仅因 current visibility 从 Directory 删除。

AC-B10：Save-before / Save-after / Restore 精确恢复 known history。

AC-B11：Restore 后不同 branch 可以拥有不同认识历史。

AC-B12：retry/recovery 不重复 entry/evidence/change/event。

AC-B13：Product `surface.people` 消费 long-lived Directory，不再消费 current visible collection。

AC-B14：current action / Narrative 继续使用 current visible，不被历史目录污染。

AC-B15：People 不从 browser 直接读 Runtime state。

AC-B16：现有 02BC module/activation/persistence rails 回归 PASS。

AC-B17：G9-02A source lineage / revision / mutation 回归 PASS。

AC-B18：G5/G6/G7/G8 regressions PASS。

AC-B19：目录 membership 不自动等于 model-visible context。

AC-B20：没有冻结 People 分类 / 搜索 taxonomy。

## 10. Focused Commands

实现者应新增例如：

```powershell
npm run g9:02b:test
```

并至少运行：

```powershell
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

真实 Provider Gate 默认 `0` 次；本任务不需要新增模型行为。

## 11. Non-scope

禁止扩张为：

- 完整 Model-first Context Router；
- routing working-set pruning 的最终算法；
- outcome-gated continuation 完整实现；
- People 分类 / 搜索 / 筛选 / 排序 UI；
- Objective Engine；
- external asset-spec；
- Markdown/YAML parser；
- Adapter / Compiler；
- Creator；
- Opening Scenario Runtime；
- Game Delete；
- Provider model selector；
- UI redesign。

## 12. Closure Gate

执行 Agent 完成后只能宣布：

```text
G9-02B IMPLEMENTATION READY FOR INDEPENDENT REVIEW
```

不得宣布：

- G9-02B CLOSED；
- G9-02C ACTIVE/CLOSED；
- G9-02 CLOSED；
- G9-03 AUTHORIZED；
- G9 COMPLETE。
