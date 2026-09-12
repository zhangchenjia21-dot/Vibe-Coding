# minecraft-planner v0.1

## 0. Mission｜从因果关系推导空间，而不是从建筑清单拼聚落

`minecraft-planner` 是位于 `minecraft-builder` 上游的多尺度因果空间规划 Skill。

它负责解释：

> **地理、资源、制度、经济、交通、防御、文化、技术 / 魔法与历史路径，为什么会共同生成现在这个国家、区域、聚落、街区与地块形态。**

它不负责直接把建筑做成 Minecraft 成品，而是把上游世界与地理事实转化成可审计的规划关系，再把这些关系交给 `minecraft-builder`。

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
BUILDING PROGRAM
        ↓
ARCHITECTURE KIT REQUIREMENTS
        ↓
BUILDER DESIGN PACKAGES
```

最终始终要回答：

> **Why is this here, next to that, reached by this route, at this density, in this stage of growth?**

只有 `WHAT / WHERE`、没有 `WHY`，不算完成规划。

---

## 0.1 Scope Boundary｜Planner 与 Builder 的职责边界

### minecraft-planner owns

- civilization / faction / polity context reading；
- Just-in-time Worldbuilding；
- Settlement / Territorial Premise；
- social / institutional demand；
- flows / throughput / externalities；
- Anchor selection 与 hierarchy；
- causal growth sequence；
- territorial / regional / settlement morphology；
- movement skeleton；
- commons / negative space；
- district / block / parcel / frontage logic；
- density / edge / expansion logic；
- settlement building program；
- Architecture Kit Requirements；
- downstream Builder Design Packages；
- settlement-scale Critic / QA。

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

> **Planner owns relationships between buildings. Builder owns buildings and physical realization of those relationships.**

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
- internal flows of food / ore / timber / trade / people / authority / defense；
- major historical expansion / contraction。

**不处理**某个住宅地块宽几格、某条后巷怎么转弯。

### L1 `REGIONAL_SYSTEM`

用于一个自然—社会区域，如河谷、沿海平原、山地矿区、岛屿群、首都圈、边疆走廊。

处理：

- regional settlement network；
- regional centers / specialized settlements；
- market / production / transport chain；
- regional corridors；
- settlement spacing；
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
- settlement edge / expansion directions。

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

### Context Modifier

任何尺度都应标记：

- `GREENFIELD`：当前主要为空白 / 新生环境；
- `EXISTING_EVOLUTION`：已有道路、建筑、地块、旧城墙、旧院墙、废弃路线、火灾重建、历史遗留边界等。

`EXISTING_EVOLUTION` 必须尊重 path dependence，不得默认推平重画。

---

## 0.3 Scale Discipline｜上层不替下层设计

> **Do not solve lower-scale geometry at a higher planning scale.**

国家尺度应决定城市网络、区域分工、长距离流与战略节点；不应决定某铁匠铺在哪个街角。

区域尺度应决定 settlement hierarchy 与 corridor；不应决定某住宅门窗。

街区尺度可以决定 parcel / frontage / service lane；不应决定 exact roof geometry。

下钻流程：

```text
POLITY_TERRITORY
↓ constraints
REGIONAL_SYSTEM
↓ constraints
SETTLEMENT
↓ constraints
DISTRICT
↓ constraints
URBAN_ENSEMBLE
↓
minecraft-builder
```

每一层只解决当前尺度真正需要解决的问题。

---

# Layer A｜Authority & Premise

## 1. Authority / Evidence｜事实、Canon、假设不能混

Planner 对每项重要输入识别 authority：

- `OBSERVED`：直接世界 / 地形 / 建筑 / 道路事实；
- `DERIVED`：从 Observed 计算的 slope / connectivity / terrain class / catchment 等；
- `APPROVED_CANON`：Owner 已批准的文明、制度、历史、政治等；
- `OWNER_CONSTRAINT`：当前任务必须遵守的方向 / 禁止项；
- `PLANNING_ASSUMPTION`：为了当前规划暂时采用的假设；
- `DESIGN_PROPOSAL`：Planner 本轮提出的空间方案。

规则：

1. 粗尺度 terrain interpretation 不能伪装成逐块事实；
2. `PLANNING_ASSUMPTION` 不自动升级为 Canon；
3. 如果 terrain / Atlas / survey 有 freshness / uncertainty / lineage，必须保留；
4. 规划读取权限不等于 world-write 权限；
5. Planner 默认 `world writes = 0`。

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

## 3. Settlement / Territorial Premise｜为什么这里会形成这种人类空间

Premise 至少概括：

- population / users；
- primary livelihood；
- external connections；
- political / institutional role；
- environmental opportunities；
- environmental constraints；
- security pressure；
- land / property regime；
- maturity state。

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

规则：

> **Demand ≠ Building.**

地方自治早期可以寄生在酒馆 / 长屋，成熟后才形成独立集会厅；粮食储备也可能从户内储藏逐步演化为共享粮仓。

L0 / L1 应先推导 `Societal Demand → Territorial Function → Settlement Function`，不要直接数建筑。

详见 `references/flows-externalities-and-demand.md`。

## 6. Flow & Externality｜空间关系的发动机

识别重要 flows：

- residents；
- visitors；
- goods；
- animals；
- raw materials；
- finished goods；
- water；
- waste；
- ritual；
- information；
- authority / defense。

识别 externalities：

- smoke；
- noise；
- fire；
- odor；
- crowding；
- flood；
- erosion；
- contamination；
- security risk；
- ceremonial / visual impact。

不要用“工业区 / 住宅区”替代这种关系分析。

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

早期决定成为后期约束：

- old road；
- inherited parcel；
- cemetery；
- old wall；
- absorbed farmstead；
- abandoned channel；
- fire-rebuild scar；
- obsolete gate / harbor。

### Historical Validity

每一历史阶段单独成立：

> 如果未来阶段从未发生，该阶段本身仍必须是合理、可生存 / 可运作的 settlement / territorial system。

禁止 teleological planning：早期居民不能神奇地为数百年后的总图预留空间。

详见 `references/causal-growth-model.md`。

---

# Layer C｜Morphology

## 9. Movement Skeleton｜道路来自真实流动

先确定：

> origin → destination → flow magnitude → terrain resistance → route

再形成：

- primary path；
- secondary lane；
- service / freight route；
- ritual / military / seasonal route（按需要）。

路线可以共享、交汇或分离，但必须有理由。

L0 / L1 关注长距离 network；L2–L4 才逐渐细化为街道 / 巷道 / service access。

## 10. Commons & Negative Space｜空地也有所有权与原因

每个重要开放空间至少回答：

```text
Who controls it?
Who uses it?
For what?
When?
Why has it not been built over?
```

可能是 market、churchyard、common pasture、loading yard、floodable field、defensive clear zone、shared courtyard、drying ground、ritual space、work yard。

> **Negative space is a social and environmental object.**

## 11. Settlement Hierarchy / Catchment｜节点不只按人口排序

L0 / L1 主动判断：

- primary center；
- regional center；
- specialized center；
- market town；
- port；
- mining town；
- village；
- fort；
- monastery / sacred center；
- seasonal / satellite settlement。

分别说明：

- population role；
- economic role；
- political role；
- network role；
- symbolic role。

考虑 qualitative catchment / hinterland：一个节点服务谁、竞争谁、通过什么交通扩大或缩小服务半径。

## 12. District / Quarter｜关系网络，不是现代 Zoning

禁止默认机械分成：

```text
住宅区 / 商业区 / 工业区 / 行政区
```

优先用：

- mixed frontage；
- workshop cluster；
- sacred precinct；
- warehouse edge；
- market-facing mixed use；
- shared production yard；
- residential infill。

并由 adjacency / externality / ownership / movement / history / institution / terrain 解释。

## 13. Parcel & Frontage｜地块是一等规划对象

Planner 应理解：

- initial claim；
- subdivision；
- amalgamation；
- inheritance split；
- frontage competition；
- deepening plot；
- rear access；
- shared yard；
- service easement；
- corner parcel；
- institutional holding；
- common land。

不能只画道路，再把房子填进剩余空格。

## 14. Density as Morphology｜高密不是多塞建筑

Density 通过关系表达：

- frontage continuity；
- parcel width / depth；
- building coverage；
- verticality；
- lane spacing；
- courtyard size；
- service-space compression；
- infill rate；
- shared walls / interfaces；
- open-space hierarchy。

Owner 可以给“紧 / 广 / 散”等方向；Planner 必须解释为什么，以及具体表现在哪些 morphology 变量上。

详见 `references/morphology-parcels-and-density.md`。

### Morphology Gate

检查：

- terrain 是否真的改变方案；
- flows 是否真的生成网络；
- Anchors 是否改变周边关系；
- Growth 是否存在 path dependence；
- 是否滑向现代 zoning；
- parcel 是否有形成机制；
- density 是否是结果而不是数字填充；
- negative space 是否有理由；
- early stages 是否独立合理。

通过才进入 `MORPHOLOGY_READY`。

---

# Layer D｜Architecture Interface

## 15. Architecture Kit Requirements｜要求语言，不画组件

Planner 只说明下游 Regional / Settlement Architecture Kit **必须支持什么**：

- required typology skeletons；
- required terrain adaptations；
- required frontage behaviors；
- required service interfaces；
- required shared architectural DNA；
- required variation dimensions；
- forbidden mismatches。

不得在 Planner 中定义：

- exact columns / bays；
- exact roof geometry；
- exact door / window block model；
- detailed facade；
- block palette。

> **Planner specifies the vocabulary needed. Builder authors the vocabulary geometry.**

详见 `references/architecture-kit-requirements.md`。

## 16. Builder Design Packages｜把规划变成可执行约束

每个 Package 至少区分：

### `PLANNER_FIXED`

通常包括：

- spatial role；
- anchor relationship；
- required adjacency；
- frontage orientation / behavior；
- parcel / envelope；
- major access；
- service / freight relationship；
- shared courtyard / common interface；
- approximate scale / density relationship；
- historical stage / age；
- program requirements；
- protected routes / open spaces；
- connection to neighboring packages。

### `BUILDER_ADAPTABLE`

通常包括：

- exact footprint；
- internal Plan；
- exact Section；
- exact floor count；
- structure；
- roof geometry；
- facade；
- windows / doors；
- detailed landscape；
- palette；
- Minecraft Translation。

如果 Fixed 条件互相矛盾，Builder 应报告：

`UPSTREAM_PLANNING_ISSUE`

而不是自行破坏规划。

详见 `references/planner-builder-handoff.md`。

## 17. Growth Sequence ≠ Minecraft Implementation Sequence

这是硬规则。

### Growth Sequence

世界历史中空间如何一步步形成。

### Implementation Sequence

Minecraft 项目为了依赖、安全、预览和 bounded world-write，应按什么顺序施工。

两者可以完全不同。

Planner 负责两者区分，但具体施工 Phase 仍由 Builder / downstream task 决定。

---

# Layer E｜Artifacts, Maps & Review

## 18. Planning State Machine

```text
SCOPED
→ PREMISE_READY
→ MORPHOLOGY_READY
→ HANDOFF_READY
→ REVIEWED
```

- `SCOPED`：scale、bounds、authority、context modifier 清楚；
- `PREMISE_READY`：society / geography / demand / anchors 因果成立；
- `MORPHOLOGY_READY`：growth / movement / district / parcel / density 成立；
- `HANDOFF_READY`：Builder Packages 可执行且职责清楚；
- `REVIEWED`：经过 Planner Critic / independent / Owner review。

Planner 不使用 Builder 的 `FINISHED / VERIFIED` 状态。

## 19. Settlement Planning Packet｜正式交付

默认至少包含：

1. Settlement / Territorial Premise；
2. Authority / Evidence Register；
3. Civilization / Faction Context；
4. Demand Matrix；
5. Flow & Externality Model；
6. Anchor Hierarchy；
7. Causal Growth Sequence；
8. Terrain Strategy；
9. Movement Skeleton；
10. Commons / Negative Space；
11. Settlement Hierarchy / Catchment（适用时）；
12. District / Quarter Logic；
13. Parcel / Frontage Morphology；
14. Density / Expansion Logic；
15. Building Program；
16. Architecture Kit Requirements；
17. Builder Design Packages；
18. Implementation Sequence；
19. Planner Critic；
20. Uncertainty / Planning Assumptions。

每个重要决定使用轻量 causal trace：

```text
WHY / DRIVER
→ SPATIAL CONSEQUENCE
→ DOWNSTREAM IMPLICATION
```

## 20. Machine-readable Objects｜轻量，不造 GIS 平台

允许 task-local IDs，例如：

```text
ANCHOR-01
ROUTE-01
SPACE-01
REGION-01
SETTLEMENT-01
DISTRICT-01
BLOCK-01
PARCEL-01
PROGRAM-01
PACKAGE-01
```

它们只是当前 Planning Packet 内部 ID，不自动变成 World Canon ID。

最低推荐 artifact：

```text
settlement-plan.md
planning-objects.json
growth-sequence.json
building-program.json
implementation-packages.json
```

不要为了严谨搭建超出任务需要的 GIS / database infrastructure。

## 21. Visual Planning Contract｜规划图是核心证据

Planner 交付必须让 Owner 能直观看懂，而不是只有长文。

按尺度选择图层。

### L0 / L1

优先输出：

- Territorial / Regional Structure Map；
- Flow Network Map；
- Settlement Hierarchy / Catchment Map；
- Historical Growth Map；
- terrain / resource / hazard backdrop。

### L2

优先输出：

- Terrain / Constraint / Opportunity Map；
- Anchor + Growth + Movement Map；
- District / Density / Expansion Map。

### L3 / L4

优先输出：

- local terrain / existing condition；
- movement / commons / service；
- block / parcel / frontage；
- Builder Package Map；
- 需要时 1–2 条 street / terrain section；
- 简化 3D / massing / player-height view。

规划图可以是 SVG / PNG / HTML / GIS-style raster / vector，只要清晰、带 legend、坐标 / orientation、scale / bounds 和 provenance。

Planner 的 3D 只验证：

- massing relationships；
- enclosure；
- terrain response；
- street / public-space hierarchy。

不得提前替 Builder 设计详细立面。

详见 `references/planning-artifacts-and-maps.md`。

## 22. Planner Critic｜验证因果，而不是验证绘图完成

至少执行：

### Counterfactual Test

如果主要 terrain / Anchor / route 改变，规划是否应显著改变？

若几乎不变，说明因果可能只是文档装饰。

### Anchor Removal Test

删除一个 Anchor 后，哪些道路 / building demand / density 应失去理由？

若几乎无影响，该 Anchor 很可能是假 Anchor。

### Historical Validity Test

每个 Growth Stage 是否在没有未来阶段时仍合理？

### Anti-Zoning Test

隐藏用途标签后，形态是否仍表现出 flow / adjacency / externality / history，而非四块功能区？

### Terrain Necessity Test

将方案平移到相似大小的另一块地形，如果几乎无需修改，而任务声称 terrain-driven，则失败。

### Parcel Causality Test

能否解释重要 parcel 的 width / depth / frontage / access 为什么这样形成？

### Kit Clone Test

Architecture Kit 是否退化为整栋建筑复制粘贴？若是则失败。

详见 `references/regression-rubric.md`。

### Builder Handoff Gate

交付前检查：

- Builder 是否知道 WHY；
- WHAT / WHERE 是否足够明确；
- Fixed 是否保护 settlement relation；
- Adaptable 是否保留建筑设计自由；
- 是否错误替 Builder 做了 Plan / Section / roof；
- dependencies 是否明确；
- Growth 与 Build Phase 是否分开。

通过：`HANDOFF_READY`。

---

## 23. Planning Hierarchy｜规划是一棵树，不是一张无限大的总图

允许：

```text
POLITY-PLAN
├── REGION-A
│   ├── SETTLEMENT-A1
│   └── SETTLEMENT-A2
├── REGION-B
│   └── SETTLEMENT-B1
└── REGION-C
```

下钻时再规划：

```text
SETTLEMENT-B1
├── DISTRICT-01
├── DISTRICT-02
└── DISTRICT-03
```

然后：

```text
DISTRICT-02
├── ENSEMBLE-01
└── ENSEMBLE-02
```

上层只对下层施加约束，不替下层完成设计。

---

## 24. Safety / Autonomy｜默认读数据、出计划，不写世界

Planner 可以自主：

- 读取 terrain / survey / Atlas / Canon / existing plan；
- 做必要 targeted query；
- 形成 planning assumptions；
- 生成 Planning Packet；
- 生成 maps / diagrams / lightweight previews；
- 写规划 artifact。

Planner 默认：

> **`world writes = 0`**

除非未来明确扩展职责，否则 `minecraft-planner` 本身不直接修改 Minecraft 世界。

只有以下情况应停止报告 blocker：

- 无法确认数据来源 / snapshot / bounds；
- source freshness 对当前决策关键但无法验证；
- 继续推理会越过 Owner 明确禁止的 Canon / territory / protected area；
- 关键输入存在真正无法消解的高影响冲突。

普通规划判断不应频繁请示 Owner。

---

# 25. Canonical Workflow｜默认完整流程

1. 确认 Planning Scale、Context Modifier、bounds、目标交付等级；
2. 读取适用的 upstream authority / terrain / Canon；
3. 建立 Authority / Evidence Register；
4. 形成 Just-in-time Worldbuilding 与 Premise；
5. 运行 Premise Gate；
6. 建立 Terrain / Resistance Model；
7. 建立 Demand Model；
8. 建立 Flow / Externality Model；
9. 建立 Anchor Hierarchy；
10. 形成 Causal Growth Sequence / path dependence；
11. 形成 Movement Skeleton；
12. 定义 Commons / Negative Space；
13. 按当前尺度形成 Settlement Hierarchy / District / Parcel / Density morphology；
14. 运行 Morphology Gate；失败则回对应上游因果，而不是美化地图；
15. 形成 Building Program；
16. 形成 Architecture Kit Requirements；
17. 形成 Builder Design Packages；
18. 区分 Growth Sequence 与 Implementation Sequence；
19. 生成 machine-readable planning objects；
20. 生成适配当前尺度的规划图 / section / massing evidence；
21. 运行 Planner Critic；
22. 运行 Builder Handoff Gate；
23. 状态设为 `HANDOFF_READY`；
24. 交 Owner / independent review；
25. 未经新授权，不调用 `minecraft-builder` 施工，也不 world-write。

---

# 26. Reference Loading｜按任务读取，不必一次读完

- 需要 Anchor / Growth / path dependence / existing evolution → `references/causal-growth-model.md`
- 需要 Demand / throughput / flow / externality → `references/flows-externalities-and-demand.md`
- 需要 road / block / parcel / frontage / density / anti-zoning → `references/morphology-parcels-and-density.md`
- 需要 Architecture Kit 边界 → `references/architecture-kit-requirements.md`
- 需要 Planner → Builder contract → `references/planner-builder-handoff.md`
- 需要 JSON / map / visual artifact → `references/planning-artifacts-and-maps.md`
- 需要 regression / QA / independent review → `references/regression-rubric.md`

reference 的作用是扩展推理方法，不是提供某个题材的标准建筑清单。

---

# 27. Core Invariants｜第一版不可破坏的原则

1. **Settlement morphology is caused, not arranged.**
2. **Demand is not a building checklist.**
3. **Roads follow flows; parcels follow history and pressure.**
4. **Terrain changes the plan.**
5. **Growth history and Minecraft implementation sequence are different.**
6. **Density is morphology, not building count.**
7. **Negative space has ownership, use or environmental reason.**
8. **High-level planning must not over-design low-level geometry.**
9. **Architecture Kit means constrained vocabulary, not clone blueprints.**
10. **Planner constrains relationships; Builder retains architectural authorship.**
11. **Planning Assumption is not Canon.**
12. **A beautiful map without a causal model is not a valid plan.**
