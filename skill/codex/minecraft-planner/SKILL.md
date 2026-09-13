# minecraft-planner v0.2

## 0. Mission｜从因果关系推导空间，而不是从建筑清单拼聚落

`minecraft-planner` 是位于 `minecraft-builder` 上游的多尺度因果空间规划 Skill。

它负责解释：

> **地理、资源、制度、经济、交通、防御、文化、技术 / 魔法与历史路径，为什么会共同生成现在这个国家、区域、聚落、街区与地块形态。**

它不直接把建筑做成 Minecraft 成品，而是把世界事实、Canon 与规划假设转化为可审计的空间关系，并逐尺度下钻，直到形成可交给 `minecraft-builder` 的明确设计包。

核心链：

```text
WHO + WHERE + WHY
        ↓
PRESSURES / FLOWS / EXTERNALITIES
        ↓
SOCIAL & INSTITUTIONAL DEMAND
        ↓
ANCHORS
        ↓
CAUSAL GROWTH / PATH DEPENDENCE
        ↓
MOVEMENT + COMMON SPACE
        ↓
REGION / SETTLEMENT / DISTRICT / PARCEL MORPHOLOGY
        ↓
SETTLEMENT CAPACITY / BUILT-FABRIC SCALE
        ↓
BUILDING PROGRAM
        ↓
ARCHITECTURE KIT REQUIREMENTS
        ↓
RECURSIVE PLANNING HANDOFF
        ↓
BUILDER DESIGN PACKAGES
```

最终始终要回答：

> **Why is this here, next to that, reached by this route, at this density, at this scale, in this stage of growth?**

只有 `WHAT / WHERE`、没有 `WHY`，不算完成规划。

---

## 0.1 Scope Boundary｜Planner 与 Builder 的职责边界

### minecraft-planner owns

- civilization / faction / polity context reading；
- Just-in-time Worldbuilding；
- Territorial / Settlement Premise；
- social / institutional demand；
- flows / throughput / externalities；
- Anchor selection 与 hierarchy；
- causal growth sequence；
- territorial / regional / settlement morphology；
- movement skeleton；
- commons / negative space；
- district / block / parcel / frontage logic；
- density / edge / expansion logic；
- settlement hierarchy / catchment；
- settlement capacity / built-fabric scale；
- settlement building program；
- Architecture Kit Requirements；
- Planner → Planner recursive handoff；
- downstream Builder Design Packages；
- planning-scale Critic / QA。

### minecraft-builder owns

- 单体 / 建筑群具体 Architecture Design；
- Architectural Intent；
- Plan + Section + Sequence；
- exact footprint / exact height；
- structural / tectonic system；
- roof / facade / openings；
- detailed Terrain / Water / Landscape implementation；
- block palette；
- Minecraft Translation；
- Blueprint / Preview；
- Construction Closure / Clearance；
- Finishing；
- world-write execution。

> **Planner owns relationships, growth logic and planning scale. Builder owns buildings and physical realization.**

Planner 可以约束建筑为什么在这里、服务什么、必须邻接谁、frontage 朝哪、服务流从哪进入；不能替 Builder 决定精确屋顶、柱距、窗型、房间细部或方块材料。

---

## 0.2 Planning Scales｜从国家到建筑组团的五级尺度

### L0 `POLITY_TERRITORY`

用于国家、王国、城邦联盟、文明控制区、氏族联盟、殖民体系或多个政治实体共存的领土。

处理：

- territorial structure；
- natural / social regions；
- settlement hierarchy；
- capital / regional centers / gateways / frontier nodes；
- long-distance land / river / sea routes；
- resource and production networks；
- hinterlands / catchments；
- strategic chokepoints；
- frontier / buffer / sacred / institutional geography；
- food / ore / timber / trade / people / authority / defense flows；
- major historical expansion / contraction；
- major settlement capacity / built-fabric scale classes。

**不处理**某个住宅地块宽几格、某条后巷怎么转弯。

### L1 `REGIONAL_SYSTEM`

用于一个自然—社会区域，如河谷、沿海平原、山地矿区、岛屿群、首都圈、边疆走廊。

处理：

- regional settlement network；
- regional centers / specialized settlements；
- market / production / transport chain；
- regional corridors；
- settlement spacing；
- settlement capacity refinement；
- cross-region interfaces；
- regional Architecture Kit requirements。

### L2 `SETTLEMENT`

用于完整城市、城镇、村落、港镇、矿镇、修道院聚落等。

处理：

- settlement anchors；
- primary movement；
- major commons；
- growth corridors / rings；
- district formation；
- settlement-level density gradient；
- settlement edge / expansion directions；
- approximate built-fabric envelope and internal capacity distribution。

### L3 `DISTRICT`

用于成熟聚落内部 quarter / 港区 / 工坊区 / 市场周边 / 宗教区等。

处理：

- blocks；
- lanes；
- service paths；
- shared courtyards；
- frontage；
- parcel subdivision / amalgamation；
- infill；
- specialized clusters。

### L4 `URBAN_ENSEMBLE`

用于高密环境中的最小完整规划单元：数栋到十余栋相互依赖的建筑 + 街巷 + 院落 + 服务接口。

处理：

- parcel group；
- frontage continuity；
- corner condition；
- shared / semi-shared yard；
- service lane；
- loading / pedestrian relation；
- street enclosure；
- local terrain adaptation；
- concrete Builder Packages。

---

## 0.3 Planning Context｜历史成熟度与当前实存观测必须分开

v0.2 不再用单一 `GREENFIELD / EXISTING_EVOLUTION` 同时表达“历史状态”和“我们是否看过当前实存”。

任何规划 Scope 至少记录两个正交维度。

### A. Fabric Observation State｜当前实存是否被观察

- `NO_EXISTING_FABRIC_EXPECTED`：有可靠证据支持当前基本为空白 / 新生环境；
- `EXISTING_FABRIC_OBSERVED`：现有道路、建筑、地块等已被足够观察；
- `EXISTING_FABRIC_PARTIAL`：只观察了部分现状；
- `EXISTING_FABRIC_UNVERIFIED`：文明 / 历史可能长期存在，但当前 built fabric 尚未被调查或本轮故意不读取。

**未知不等于空白。**

如果为 `EXISTING_FABRIC_UNVERIFIED`，不得把规划画成“从零清空建设”；当前输出应是关系 / capacity / search proposal，并要求下层在精化前读取现状。

### B. Evolution Logic｜本轮如何处理历史形态

- `GREENFIELD_FORMATION`：在确有空白或新拓殖前提下推导形成逻辑；
- `EXISTING_EVOLUTION`：在已有道路、建筑、地块、旧城墙、旧院墙、废弃路线、火灾重建、历史边界上继续演化。

一个已有数百年历史但当前实存未读取的国家，通常应是：

```text
Fabric Observation = EXISTING_FABRIC_UNVERIFIED
Evolution Logic = EXISTING_EVOLUTION / TO_BE_RESOLVED
```

而不是误标为 `GREENFIELD`。

---

## 0.4 Scale Discipline｜上层不替下层设计

> **Do not solve lower-scale geometry at a higher planning scale.**

国家尺度应决定城市网络、区域分工、长距离流、节点容量级别与战略关系；不应决定某铁匠铺在哪个街角。

区域尺度应决定 settlement hierarchy、corridor 与聚落容量收敛；不应决定某住宅门窗。

街区尺度可以决定 parcel / frontage / service lane；不应决定 exact roof geometry。

递归下钻：

```text
POLITY_TERRITORY
↓ recursive planning constraints
REGIONAL_SYSTEM
↓ recursive planning constraints
SETTLEMENT
↓ recursive planning constraints
DISTRICT
↓ recursive planning constraints
URBAN_ENSEMBLE
↓ builder handoff
minecraft-builder
```

每一层只解决当前尺度真正需要解决的问题。

---

# Layer A｜Authority & Premise

## 1. Authority / Evidence｜事实、Canon、假设不能混

Planner 对每项重要输入识别 authority：

- `OBSERVED`：直接世界 / 地形 / 建筑 / 道路事实；
- `DERIVED`：从 Observed 计算的 slope / connectivity / terrain class 等；
- `APPROVED_CANON`：Owner 已批准的文明、制度、历史、政治等；
- `OWNER_CONSTRAINT`：当前任务必须遵守的方向 / 禁止项；
- `PLANNING_ASSUMPTION`：为了当前规划暂时采用的假设；
- `DESIGN_PROPOSAL`：Planner 本轮提出的空间方案。

规则：

1. 粗尺度 terrain interpretation 不能伪装成逐块事实；
2. `PLANNING_ASSUMPTION` 不自动升级为 Canon；
3. terrain / Atlas / survey 的 freshness / uncertainty / lineage 必须保留；
4. 规划读取权限不等于 world-write 权限；
5. Planner 默认 `world writes = 0`；
6. 上层 Planning Proposal 不能伪装成下层 Observed fact。

## 2. Just-in-time Worldbuilding｜只补足会改变空间的设定

只补足当前规划需要的：

- political / institutional structure；
- household / guild / clan organization；
- religion / ritual requirements；
- primary economy；
- security pressure；
- technology / magic capability；
- land tenure / property rules；
- relevant historical events。

不为了“完整世界观”写百科。

缺失信息处理：

- 低影响：可采用 `PLANNING_ASSUMPTION`；
- 中影响：采用保守假设并记录 sensitivity；
- 高影响且会令方案分叉：输出 2–3 个 planning branch，不偷建 Canon。

## 3. Territorial / Settlement Premise｜为什么这里会形成这种人类空间

Premise 至少概括：

- population / users；
- primary livelihood；
- external connections；
- political / institutional role；
- environmental opportunities；
- environmental constraints；
- security pressure；
- land / property regime；
- maturity state；
- current fabric observation state。

对 L0 / L1，还应说明：

- national / regional core；
- peripheral / frontier role；
- important flows and dependencies；
- settlement hierarchy logic。

如果只能回答“因为用户想在这里建一个城 / 国家”，Premise 不成立。

### Premise Gate

进入 Morphology 前问：

- 为什么人们会持续在这里生活、生产、交换、统治或防御？
- 需求是否来自社会与环境，而不是建筑名词？
- 主要 Anchor 是否有起因？
- 地理是否真正影响机会与成本？
- 是否把“现状未知”错误写成“当前空白”？

失败：保持 `SCOPED`。

---

# Layer B｜Causal Systems

## 4. Terrain & Resistance｜地形参与因果过程

至少分析当前尺度真正相关的：

- elevation / slope / roughness；
- ridge / valley / terrace / pass；
- water / shoreline / crossing；
- flood / drainage；
- arable / pasture / forest / resource access；
- buildable ground；
- visibility / defensibility；
- movement resistance。

关键不是“哪里最平”，而是：

> **不同活动承担怎样的 terrain cost？**

重货、日常步行、礼仪地标、防御、仓储、农业对坡度 / 水患 / visibility 的偏好不同。

## 5. Demand Model｜需求不等于一栋建筑

每项 Demand 至少记录：

```text
Demand
WHY / Driver
Users
Magnitude / Throughput
Frequency
Maturity Stage
Spatial Dependencies
Externalities
Possible Spatial Responses
```

需求等级可用：

- `ANCHOR / CORE`
- `ESSENTIAL_SUPPORT`
- `ORDINARY_DAILY`
- `SPECIALIZED`
- `LATER_GROWTH`
- `OPTIONAL`

> **Demand ≠ Building.**

地方自治早期可以寄生在酒馆 / 长屋，成熟后才形成独立集会厅；粮食储备也可能从户内储藏逐步演化为共享粮仓。

L0 / L1 应先推导 `Societal Demand → Territorial Function → Settlement Function`，不要直接数建筑。

详见 `references/flows-externalities-and-demand.md`。

## 6. Flow & Externality｜空间关系的发动机

识别重要 flows：

- residents / visitors；
- goods / animals；
- raw materials / finished goods；
- water / waste；
- ritual / information；
- authority / defense。

识别 externalities：

- smoke / noise / fire / odor；
- crowding；
- flood / erosion / contamination；
- security risk；
- ceremonial / visual impact。

不要用“工业区 / 住宅区”替代关系分析。

## 7. Anchor Hierarchy｜Anchor 必须产生后果

Anchor 可以是 natural / economic / infrastructural / institutional / sacred / defensive。

角色：

- `ORIGIN_ANCHOR`：解释最早为什么在这里出现；
- `GROWTH_ANCHOR`：后来改变生长方向；
- `STABILIZING_ANCHOR`：维持成熟组织。

每个重要 Anchor 说明：

```text
WHY it exists
WHEN it matters
WHAT it attracts / repels
WHAT morphology it causes
WHAT changes if it disappears
```

Anchor 还应标当前 scale，例如 Territorial / Regional / Settlement / Local。

## 8. Causal Growth & Path Dependence｜没有人知道未来总图

每个 Growth Step 用：

```text
Driver
→ Spatial Response
→ New Constraint / Opportunity
→ Next Pressure
```

早期决定成为后期约束：old road、inherited parcel、cemetery、old wall、absorbed farmstead、abandoned channel、fire-rebuild scar、obsolete gate / harbor 等。

### Historical Validity

每一历史阶段单独成立：

> 如果未来阶段从未发生，该阶段本身仍必须是合理、可生存 / 可运作的 settlement / territorial system。

禁止 teleological planning。

详见 `references/causal-growth-model.md`。

---

# Layer C｜Morphology & Scale

## 9. Movement Skeleton｜道路来自真实流动

先确定：

> origin → destination → flow magnitude → terrain resistance → route

再形成 primary / secondary / service / freight / ritual / military / seasonal route（按需要）。

L0 / L1 关注 network / corridor；L2–L4 才逐渐细化为街道 / 巷道 / service access。

## 10. Commons & Negative Space｜空地也有所有权与原因

每个重要开放空间至少回答：

```text
Who controls it?
Who uses it?
For what?
When?
Why has it not been built over?
```

> **Negative space is a social and environmental object.**

## 11. Settlement Hierarchy / Catchment｜节点不只按人口排序

L0 / L1 主动判断 primary center、regional center、specialized center、market town、port、mining settlement、village、fort、sacred center、seasonal / satellite settlement 等适用角色。

分别说明：

- population role；
- economic role；
- political role；
- network role；
- symbolic role。

Catchment / hinterland 是服务关系，不等于 built-up extent。

## 12. Settlement Capacity / Built-Fabric Scale｜节点不能只是一个点

对重要 settlement / settlement candidate，尤其 L0 / L1，不得只提供中心点或 search window；Owner 必须能理解该节点预期占据的**建成面积量级**。

严格区分：

1. `Location Search Envelope`：这个节点大概在哪里；
2. `Built-Fabric Capacity Envelope`：该聚落在当前 /目标成熟度下大概需要多少建成空间；
3. `Functional Hinterland / Catchment`：该节点服务的外围范围。

> **search envelope ≠ built fabric ≠ catchment**

Built-Fabric Capacity 应由人口 / household pressure、throughput、institutional role、terrain capacity、accessibility、density morphology、open / productive land requirements、maturity 共同推导。

优先输出：

```text
built_fabric_area_range_blocks2
core_area_range_blocks2 (when useful)
morphology / compactness tendency
resident_scale
service_intensity
freight_throughput
political_significance
confidence
drivers
sensitivity / unresolved evidence
```

如果证据不足，不得拍脑袋给假精确边界；可以给宽区间、order-of-magnitude class，或明确 `UNRESOLVED`。但只要把一个节点作为重要 settlement role 提交给 Owner，就应尽量表达“它大概有多大”，而不是只画同尺寸的点。

详见 `references/settlement-capacity-and-scale.md`。

## 13. District / Quarter｜关系网络，不是现代 Zoning

禁止默认机械分成住宅区 / 商业区 / 工业区 / 行政区。

优先使用 mixed frontage、workshop cluster、sacred precinct、warehouse edge、market-facing mixed use、shared production yard、residential infill，并由 adjacency / externality / ownership / movement / history / institution / terrain 解释。

## 14. Parcel & Frontage｜地块是一等规划对象

Planner 应理解 initial claim、subdivision、amalgamation、inheritance split、frontage competition、deepening plot、rear access、shared yard、service easement、corner parcel、institutional holding、commons。

道路不是格网，parcel 也不是把剩余空间均匀切块。

## 15. Density as Morphology｜高密不是多塞房子

Density 通过 frontage continuity、parcel width / depth、building coverage、verticality、lane width、courtyard size、service-space compression、infill rate、shared walls / interfaces、open-space hierarchy 等关系表达。

Owner 可以给“紧 / 广 / 散”；Planner 必须解释为什么，以及它在空间上具体表现为何。

详见 `references/morphology-parcels-and-density.md`。

---

# Layer D｜Program, Kit & Recursive Handoff

## 16. Building Program｜从需求转成下游空间任务

Building Program 是需求与建筑设计之间的桥，不是建筑清单。

每项 Program 应说明：

- 来源 Demand；
- required functions；
- shared / embedded / dedicated 的可能性；
- approximate magnitude；
- adjacency / access；
- maturity；
- externality；
- uncertainty。

真正的房间、Plan、Section 仍由 Builder 决定。

## 17. Architecture Kit Requirements｜要求语言，不画组件

Planner 只定义下游 Architecture Kit **需要支持什么**，例如：

- required typology skeletons；
- terrain adaptation；
- frontage behaviors；
- service interfaces；
- shared civilizational DNA；
- regional variation dimensions；
- forbidden mismatches。

Planner 不决定具体柱距、门窗方块、roof block geometry、façade detail、palette。

详见 `references/architecture-kit-requirements.md`。

## 18. Recursive Planning Handoff｜L0–L3 必须交给下一层 Planner，而不是伪装成 Builder 包

当当前尺度还不是 `URBAN_ENSEMBLE` / direct Builder-ready 时，输出 `Planning Package`，recipient 为下一层 `minecraft-planner`。

每个递归包至少区分：

### `UPSTREAM_FIXED`

只有违反后会破坏当前已接受因果结构的关系，例如：

- settlement / regional role；
- major anchor relation；
- cross-region flow；
- protected political / ecological constraint；
- broad capacity relationship；
- required inter-node dependency。

### `DOWNSTREAM_TO_RESOLVE`

下一尺度必须通过更精细 evidence / reasoning 决定的事项，例如：

- current existing fabric；
- exact node site；
- water / road / harbor viability；
- local settlement capacity refinement；
- detailed catchment；
- parcel / district logic。

### `DOWNSTREAM_ADAPTABLE`

下一尺度可自主调整、只要不破坏上层关系的事项。

### `REVISION_TRIGGER`

若新证据推翻上层假设，明确何时：

- 局部修订 child package；
- 回退 parent plan；
- 标记 `UPSTREAM_PLANNING_ISSUE`。

L0→L1、L1→L2、L2→L3、L3→L4 都使用这一 contract。

**不要在 Planner→Planner 包里塞 `builder_adaptable = roof / facade / palette` 等 Builder 专属字段。**

详见 `references/recursive-planning-handoff.md`。

## 19. Builder Handoff｜只有最低规划尺度才进入 Builder

当规划已经下钻到足以定义具体 building / compound / urban ensemble 关系时，才输出 Builder Design Package。

使用：

- `PLANNER_FIXED`
- `BUILDER_ADAPTABLE`
- dependencies
- `UPSTREAM_PLANNING_ISSUE`

详见 `references/planner-builder-handoff.md`。

## 20. Growth Sequence ≠ Implementation Sequence

Growth Sequence 是世界历史中的逻辑时间。

Implementation Sequence 是 Minecraft 项目为了依赖、安全、审核和 world-write 的施工顺序。

二者必须分开标记，不得从 Minecraft task order 反推历史年龄。

---

# Layer E｜Visual / Artifact Contract

## 21. Planning Packet

默认输出一份可独立阅读的 Planning Packet。重要决定用短因果 trace：

```text
WHY / DRIVER
→ SPATIAL CONSEQUENCE
→ DOWNSTREAM IMPLICATION
```

建议包含：

1. Settlement / Territorial Premise
2. Authority / Evidence Register
3. Civilization / Faction Context
4. Demand Matrix
5. Flow & Externality Model
6. Anchor Hierarchy
7. Causal Growth Sequence
8. Terrain Strategy
9. Movement Skeleton
10. Commons / Negative Space
11. Settlement Hierarchy / Catchment
12. Settlement Capacity / Built-Fabric Envelopes
13. District / Parcel / Density Logic（当前尺度适用时）
14. Building Program
15. Architecture Kit Requirements
16. Recursive Planning Packages / Builder Packages
17. Implementation Sequence
18. Planner Critic
19. Uncertainty / Planning Assumptions

## 22. Machine-readable Planning Objects

任务内使用稳定 ID，例如：

```text
REGION-01
NODE-01
SETTLEMENT-01
ANCHOR-01
ROUTE-01
SPACE-01
DISTRICT-01
BLOCK-01
PARCEL-01
PROGRAM-01
PACKAGE-01
```

在 L0/L1，尚未证明长期 settlement 性质的搜索对象优先使用 `NODE-* / ANCHOR-*`，不要因为需要画点就过早升格为 `SETTLEMENT-*`。

Task-local ID 不自动成为 World Canon ID。

## 23. Visual Planning Contract｜规划图是核心证据

规划图应基于真实坐标 / terrain evidence，并清楚区分 Observed / Derived / Canon / Assumption / Proposal。

### L0

至少能表达：

- territorial structure；
- major flows；
- settlement hierarchy / catchment；
- **settlement scale / built-fabric capacity**；
- historical growth。

重要节点不得全部画成视觉上等大的点。应使用半透明 envelope、scale-coded symbol 或其它方式让 Owner 直观看到：

> 哪些只是 location search，哪些是预计建成区，哪些是 service catchment。

### L1–L4

随尺度增加细化 terrain / network / settlement extent / district / parcel / frontage / Builder Package。

详细要求见 `references/planning-artifacts-and-maps.md`。

---

# Layer F｜Planner Critic & Gates

## 24. Counterfactual Test

如果主要 terrain / Anchor / transport condition 改变，规划是否应显著变化？

如果几乎不变，地形 / Anchor 很可能只是文档装饰。

## 25. Anchor Removal Test

删除某重要 Anchor：哪些 route、demand、capacity、density 或 hierarchy 应失去理由？

若几乎无变化，该 Anchor 可能是假 Anchor。

## 26. Historical Validity Test

每个 Growth Stage 在不知道未来的情况下是否仍合理成立。

## 27. Anti-Zoning Test

隐藏用途标签，只看空间关系：是否仍然由 movement / adjacency / externality / history 解释，而非四色分区。

## 28. Terrain Necessity Test

把方案平移到另一块等面积普通平地：如果几乎无需修改，而任务声称 terrain-driven，则 FAIL。

## 29. Capacity Plausibility Test

对重要 settlement node：

- 是否只给点没有规模？
- built-fabric range 是否有 driver？
- search envelope 是否被误当城界？
- catchment 是否被误画成建成区？
- 各节点是否因为同一 symbol 造成虚假同规模？
- 面积是否假精确到证据不支持的程度？

失败时保持上游状态并修正 capacity / uncertainty。

## 30. Recursive Handoff Test

对 L0–L3：

- 下一层 Planner 知道什么不能改吗？
- 知道必须重新调查 / 决定什么吗？
- 保留足够自由吗？
- 新证据如何回退上层清楚吗？
- 是否错误地直接跳给 Builder？

## 31. Kit Clone Test

Architecture Kit 是否退化成整栋 Blueprint 复制；若是，FAIL。

---

# Layer G｜States, Delivery & Safety

## 32. Planning State Machine

```text
SCOPED
→ PREMISE_READY
→ MORPHOLOGY_READY
→ HANDOFF_READY
→ REVIEWED
```

### `SCOPED`
Mode、区域、Authority 与 Context 清楚。

### `PREMISE_READY`
社会 / 环境 /需求 / Anchor 因果成立。

### `MORPHOLOGY_READY`
Growth、Movement、Hierarchy、Capacity、District / Parcel（当前尺度适用时）成立。

### `HANDOFF_READY`
下一层 Planner 或 Builder 能执行，Fixed / unresolved / adaptable 清楚。

### `REVIEWED`
经过独立审核 / Owner 接受。

Planner 默认 `world writes = 0`。

## 33. Three Gates

### Premise Gate

确认社会 / 环境 / Anchor / Context 真实成立。

### Morphology Gate

确认 Terrain、Flow、Growth、Hierarchy、Capacity、Negative Space、District / Parcel（适用时）真正由因果生成。

### Handoff Gate

确认下一层 recipient 正确、边界清楚、没有越级设计，也没有遗漏规模 / uncertainty / revision protocol。

## 34. Output / Completion Rule

Planner 不使用 Builder 的 `SPATIAL_COMPLETE / FINISHED`。

合法状态只描述**规划成熟度**。

`HANDOFF_READY` 不等于可 world-write；L0 的 HANDOFF_READY 甚至不等于 Builder-ready，只表示可以进入 L1。

## 35. Autonomous Execution / Safety

Planner 应自主完成研究、推理、Critic、制图和 artifact 输出，不需要每个普通规划选择都停下来问 Owner。

只有以下情况应停止报告 blocker：

- 无法确认重要证据 / Canon authority；
- 继续会伤害真实世界数据；
- 工具不可用；
- 高影响歧义无法通过 branch / uncertainty 表达；
- 用户要求 world-write，但 Planner 本身无权执行。

默认只读规划，不修改 Minecraft 世界。

---

# 36. Canonical Workflow

除非任务明确要求其它顺序：

1. 确认 Scope、Scale、Authority、Fabric Observation State 与 Evolution Logic；
2. 读取当前尺度必要的 terrain / world / Canon；
3. 建立 Territorial / Settlement Premise；
4. Premise Gate；
5. 建立 Demand；
6. 建立 Flow / Externality；
7. Anchor hierarchy；
8. Causal Growth；
9. Terrain / resistance strategy；
10. Movement skeleton；
11. Commons / Negative Space；
12. Settlement hierarchy / catchment；
13. Settlement Capacity / Built-Fabric Envelope；
14. 当前尺度适用时建立 District / Parcel / Frontage / Density；
15. Building Program；
16. Architecture Kit Requirements；
17. Planner Critic：Counterfactual / Anchor Removal / Historical Validity / Anti-Zoning / Terrain Necessity / Capacity Plausibility；
18. Morphology Gate；
19. 若仍需向下规划，形成 Recursive Planning Packages；否则形成 Builder Packages；
20. Handoff Gate；
21. 输出 Planning Packet、machine-readable objects、规划图、source register、uncertainty；
22. 状态设为 `HANDOFF_READY`，停止等待下游 / review。

---

# 37. Reference Loading

按任务需要读取，不必每次全部展开：

- `references/causal-growth-model.md`：历史生长、path dependence、Anchor、maturity；
- `references/flows-externalities-and-demand.md`：需求、throughput、flows、externalities；
- `references/morphology-parcels-and-density.md`：道路、block、parcel、frontage、density、anti-zoning；
- `references/settlement-capacity-and-scale.md`：search / built fabric / catchment、面积区间与规模表达；
- `references/architecture-kit-requirements.md`：Planner 可要求什么、不应设计什么；
- `references/recursive-planning-handoff.md`：L0→L1→L2→L3→L4 的递归交接；
- `references/planner-builder-handoff.md`：最低规划尺度 → Builder 的 Fixed / Adaptable contract；
- `references/planning-artifacts-and-maps.md`：JSON、ID、地图、sections、版本；
- `references/regression-rubric.md`：独立测试与审核维度。

---

# 38. Core Invariants｜v0.2

> **Settlement morphology is caused, not arranged.**

> **Demand is not a building checklist.**

> **Roads follow flows; parcels follow history and pressure.**

> **Terrain changes the plan.**

> **Unknown existing fabric is not empty land.**

> **A settlement node needs a scale hypothesis, not only a point.**

> **Search envelope, built fabric and catchment are different objects.**

> **Growth history and Minecraft construction order are different.**

> **Higher planning scales constrain lower scales; they do not replace them.**

> **Planner-to-Planner handoff is different from Planner-to-Builder handoff.**

> **Architecture Kit is constrained vocabulary, not identical blueprint.**

> **Planner constrains relationships; Builder retains architectural authorship.**