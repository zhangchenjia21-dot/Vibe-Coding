# minecraft-planner v0.4

## 0. Mission｜从“条件决定空间”升级为“人类在条件中行动，反过来塑造空间”

`minecraft-planner` 是位于 `minecraft-builder` 上游的多尺度因果空间规划 Skill。

它负责解释：

> **地理、地表与环境、资源、制度、权利、知识、经济、交通、防御、文化、技术 / 魔法、人口过程与历史路径，为什么会共同生成现在这个国家、区域、聚落、街区与地块形态。**

v0.4 的核心升级是：Planner 不再把自然与社会条件当成静态 suitability map。真实的人类空间来自**行动者在有限知识、有限权利、有限能力和持续物质需求下作出的选择、协商、适应与竞争**；这些选择又会改变下一阶段的环境、通达与空间价值。

核心链：

```text
WORLD STATE
terrain + surface + existing fabric + resources

        +

ACTORS
interests + rights + power

        +

BOUNDED KNOWLEDGE
what actors actually know

        +

CAPABILITIES
technology + labor + institutions + ordinary engineering

        ↓

DEMAND + FLOWS + STOCKS + RHYTHMS + EXTERNALITIES

        ↓

CHOICES / NEGOTIATION / ADAPTATION / COMPETITION

        ↓

EFFECTIVE ACCESS + INFRASTRUCTURE + ANCHORS

        ↓

FEEDBACK + PATH DEPENDENCE

        ↓

REGION / SETTLEMENT / DISTRICT / PARCEL MORPHOLOGY

        ↓

SETTLEMENT CAPACITY / BUILT-FABRIC SCALE

        ↓

BUILDING PROGRAM + ARCHITECTURE KIT REQUIREMENTS

        ↓

RECURSIVE PLANNING HANDOFF

        ↓

BUILDER DESIGN PACKAGES
```

最终始终要回答：

> **Why is this here, next to that, reached by this route, used by these people, under these rights, at this density, on this kind of ground, with these adaptations, at this scale, in this stage of growth?**

只有 `WHAT / WHERE`、没有 `WHY / WHO / HOW`，不算完成规划。

---

## 0.1 Scope Boundary｜Planner 与 Builder 的职责边界

### minecraft-planner owns

- civilization / faction / polity context reading；
- Just-in-time Worldbuilding；
- Territorial / Settlement Premise；
- terrain / surface / substrate / land-cover planning interpretation；
- Actor / rights / bounded-knowledge reasoning when spatially consequential；
- human adaptation / constraint transformation；
- effective accessibility；
- social / institutional demand；
- flows / stocks / buffers / rhythms / externalities；
- Anchor selection 与 hierarchy；
- causal growth / feedback / path dependence；
- territorial / regional / settlement morphology；
- movement skeleton；
- commons / negative space；
- district / block / parcel / frontage logic；
- qualitative site value / competition / demographic morphology；
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
- exact engineering geometry；
- block palette；
- Minecraft Translation；
- Blueprint / Preview；
- Construction Closure / Clearance；
- Finishing；
- world-write execution。

> **Planner owns causal relationships, human-geography logic and planning scale. Builder owns buildings and exact physical realization.**

Planner 可以要求“这里需要井 / 小桥 / 挡墙 / 蓄水 / 退让 / 共享接口”等规划级能力，但不能因此替 Builder 设计精确井筒、桥拱、基础、屋顶、柱距、窗型或方块材料。

---

## 0.2 Planning Scales｜从国家到建筑组团的五级尺度

### L0 `POLITY_TERRITORY`

处理：

- territorial structure；
- natural / social regions；
- settlement hierarchy；
- capital / regional centers / gateways / frontier nodes；
- long-distance land / river / sea routes；
- resource / production / supply networks；
- hinterlands / catchments；
- strategic chokepoints；
- frontier / buffer / sacred / institutional geography；
- food / ore / timber / trade / people / authority / defense flows；
- major political / tenure / access relations；
- major historical expansion / contraction；
- major settlement capacity / built-fabric scale classes；
- coarse surface / land-cover distinctions when they materially affect the system；
- major resilience dependencies / single points of failure when strategically relevant。

**不处理**某住宅地块宽几格、某条后巷怎么转弯。

### L1 `REGIONAL_SYSTEM`

处理：

- regional settlement network；
- regional centers / specialized settlements；
- market / production / supply / transport chain；
- regional corridors；
- effective accessibility and access constraints；
- settlement spacing；
- settlement capacity refinement；
- consequential surface / substrate / land-cover differences；
- ordinary adaptation opportunities / residual constraints；
- cross-region interfaces；
- regional resilience / fallback relations where justified；
- regional Architecture Kit requirements。

### L2 `SETTLEMENT`

处理：

- settlement actors / institutions when consequential；
- settlement anchors；
- primary movement；
- major commons；
- wells / crossings / shared storage / other planning-scale infrastructure when causally required；
- growth corridors / rings；
- district formation；
- settlement-level density gradient；
- settlement edge / expansion directions；
- approximate built-fabric envelope and internal capacity distribution；
- local surface mosaic / productive-ground protection / water-ground relation；
- household / visitor / storage / seasonal pressure；
- feedback between infrastructure, access, frontage and growth。

### L3 `DISTRICT`

处理：

- blocks；
- lanes；
- service paths；
- shared courtyards；
- frontage；
- parcel subdivision / amalgamation；
- inheritance / household / migration effects when relevant；
- qualitative site-value competition；
- infill；
- specialized clusters；
- rights / easement / common-access logic；
- surface / ground character where it changes parcel, courtyard or service logic。

### L4 `URBAN_ENSEMBLE`

处理：

- parcel group；
- frontage continuity；
- corner condition；
- shared / semi-shared yard；
- service lane；
- loading / pedestrian relation；
- street enclosure；
- local terrain / ground adaptation；
- planning-fixed access / rights / shared interfaces；
- concrete Builder Packages。

---

## 0.3 Planning Context｜历史成熟度与当前实存观测必须分开

任何规划 Scope 至少记录两个正交维度。

### A. Fabric Observation State

- `NO_EXISTING_FABRIC_EXPECTED`
- `EXISTING_FABRIC_OBSERVED`
- `EXISTING_FABRIC_PARTIAL`
- `EXISTING_FABRIC_UNVERIFIED`

**未知不等于空白。**

### B. Evolution Logic

- `GREENFIELD_FORMATION`
- `EXISTING_EVOLUTION`

一个已有长期历史但当前实存未读取的国家，通常应是：

```text
Fabric Observation = EXISTING_FABRIC_UNVERIFIED
Evolution Logic = EXISTING_EVOLUTION / TO_BE_RESOLVED
```

而不是误标为 `GREENFIELD`。

---

## 0.4 Scale Discipline｜上层不替下层设计

> **Do not solve lower-scale geometry at a higher planning scale.**

国家尺度决定城市网络、区域分工、长距离流、节点容量和战略关系；区域尺度决定 settlement hierarchy、corridor、capacity、重大 access / adaptation；街区尺度才决定 parcel / frontage / service lane；Builder 才决定具体建筑与工程几何。

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

---

# Layer A｜Authority, Actors & Premise

## 1. Authority / Evidence｜事实、Canon、假设不能混

Planner 对重要输入识别 authority：

- `OBSERVED`
- `DERIVED`
- `APPROVED_CANON`
- `OWNER_CONSTRAINT`
- `PLANNING_ASSUMPTION`
- `DESIGN_PROPOSAL`

规则：

1. 粗尺度 terrain / surface interpretation 不能伪装成逐块事实；
2. `PLANNING_ASSUMPTION` 不自动升级为 Canon；
3. freshness / uncertainty / lineage 必须保留；
4. 读取权限不等于 world-write 权限；
5. Planner 默认 `world writes = 0`；
6. 上层 Planning Proposal 不能伪装成下层 Observed fact；
7. visible surface material 不自动证明 fertility、quarry quality、ore deposit、timber yield 或 hydrology。

### 1.1 Evidence Authority ≠ Historical Knowledge

Planner 知道一个事实，不代表历史行动者也知道。

当知识状态会改变历史序列时，区分：

- `HISTORICALLY_KNOWN`
- `LOCALLY_KNOWN`
- `PARTIALLY_KNOWN`
- `UNDISCOVERED`
- `UNKNOWN_TO_PLANNER`
- `PLANNER_ONLY_EVIDENCE`

不要给每个事实强行打标签，只在它会改变选择 / growth 时使用。

## 2. Just-in-time Worldbuilding｜只补足会改变空间的设定

只补足当前规划需要的：

- political / institutional structure；
- household / guild / clan organization；
- religion / ritual requirements；
- primary economy；
- security pressure；
- technology / magic capability；
- land tenure / property / access rules；
- relevant historical events；
- relevant Actor interests / authority / cooperation limits。

不为了“完整世界观”写百科。

## 3. Actor & Capability Model｜空间变化必须有人能做出来

对重要空间变化，至少能回答：

```text
Who wants this?
Who has the right / authority?
Who pays or bears the burden?
Who benefits?
Who can resist / veto / redirect?
Who must cooperate?
What technology / labor / institution makes it possible?
```

不要求为每个小地块建立 Actor 表；只在权利、冲突、协作或能力会改变空间时显式建模。

> **Physical possibility is not social permission.**

## 4. Territorial / Settlement Premise｜为什么这里会形成这种人类空间

Premise 至少概括：

- population / users；
- primary livelihood；
- external connections；
- political / institutional role；
- major actors / rights / cooperation structure；
- relevant bounded knowledge；
- relevant technology / labor / institutional capability；
- environmental opportunities；
- environmental constraints；
- security pressure；
- land / property regime；
- maturity state；
- current fabric observation state。

### Premise Gate

进入 Morphology 前问：

- 为什么人们会持续在这里生活、生产、交换、统治或防御？
- 谁推动 / 允许这些关系？
- 他们当时合理知道什么？
- 需求是否来自社会与环境，而不是建筑名词？
- 主要 Anchor 是否有起因？
- 地理是否真正影响机会与成本？
- 是否把“现状未知”错误写成“当前空白”？
- 是否把可被普通时代工程解决的问题过早当成 blocker？

失败：保持 `SCOPED`。

详见 `references/human-geography-kernels.md`。

---

# Layer B｜Environment, Adaptation & Human Systems

## 5. Terrain & Resistance｜几何地形参与因果过程

至少分析当前尺度真正相关的 elevation / slope / roughness / ridge / valley / terrace / pass / water / shoreline / crossing / flood evidence / buildable-ground geometry / visibility / defensibility / movement resistance。

关键不是“哪里最平”，而是：

> **不同活动承担怎样的 terrain cost？**

## 5.1 Surface / Substrate / Land-Cover｜地表性质是一等规划证据

几何平坦不等于生活、生产或建设条件等价。

在会改变规划时，主动读取或标记未知：

- exposed surface family；
- soil-bearing vs exposed rock；
- sand / gravel / mud / snow / barren character；
- vegetation / canopy / open-ground cover；
- wet / dry surface signals when supported；
- shallow substrate only when evidence exists；
- cultivated / disturbed surface only when observed。

> **Flat is not the same as habitable, productive, buildable or equivalent. Geometry does not describe land character.**

但：grass ≠ fertile farmland；stone ≠ quarry / ore；forest ≠ sustainable timber yield；biome ≠ soil model。

详见 `references/surface-substrate-landcover.md`。

## 6. Human Adaptation / Constraint Transformation｜约束不是静态禁区

文明会用自身时代、规模与组织能力改造环境。

常见规划级适应包括：

- well / cistern / rain storage；
- drainage；
- small bridge / ferry；
- retaining wall / steps / switchback；
- modest cut / fill / terrace；
- stabilized roadbed；
- quay / landing；
- modest channel / local water works；
- wall / gate；
- shared transfer / storage infrastructure。

对会显著压缩、迁移或否决方案的约束，先建立：

```text
Raw condition
→ affected activity
→ available mitigation
→ capability required
→ relative intervention burden
→ residual constraint
→ spatial consequence
```

Relative burden 可用 `LOW / MODERATE / HIGH / SYSTEMIC`，不要伪造精确成本。

核心：

> **A missing natural convenience is not automatically a settlement blocker when ordinary period-appropriate adaptation can solve it at bounded, proportionate cost.**

同样禁止反向极端：不能为了保住偏爱的城址，给小村庄强行安排不成比例的巨型工程。

Mitigation 可能只消除部分问题。例如井可以解决基本饮水，却不自动解决大面积农业或大城市扩张。

## 7. Demand Model｜需求不等于一栋建筑

每项重要 Demand 记录：

```text
Demand
WHY / Driver
Users / Actors
Magnitude / Throughput
Frequency / Rhythm
Maturity Stage
Spatial Dependencies
Externalities
Possible Spatial Responses
```

Demand 等级可用 `ANCHOR / CORE / ESSENTIAL_SUPPORT / ORDINARY_DAILY / SPECIALIZED / LATER_GROWTH / OPTIONAL`。

> **Demand ≠ Building.**

L0 / L1 先推导 `Societal Demand → Territorial Function → Settlement Function`。

## 8. Metabolism｜Flow + Stock + Rhythm

识别重要 flows：people / goods / animals / raw materials / finished goods / water / waste / ritual / information / authority / defense。

但 Flow 不够。对生计、储备、贸易、取水等重要系统，还要考虑：

```text
Flow
+ Stock / Buffer
+ Consumption / Use
+ Replenishment Cadence
+ Peak / Seasonal Rhythm
+ Failure Consequence
```

时间模式可用：continuous / daily / periodic / seasonal / annual / event / emergency。

例如粮食可能季节输入却每日消耗；集市平日空、赶集日爆满；蓄水可以缓冲不连续供给。

不要为了完整而做精确库存模拟。只在它改变仓储、场院、容量、道路峰值或韧性时建模。

详见 `references/flows-externalities-and-demand.md` 与 `references/human-geography-kernels.md`。

## 9. Externality｜邻近关系来自权衡

识别 smoke / heat / noise / fire / odor / contamination / crowding / flood / erosion / security / animal traffic / dust / sacred restriction / ceremonial visibility 等 externalities。

Externality 不是绝对 zoning 法则，而是与 access / value / rights / mitigation 共同形成权衡。

## 10. Effective Accessibility｜能走不等于能用

路线选择从：

```text
origin → destination → magnitude
→ physical access
→ legal / tenure access
→ political permission
→ security
→ season / timing
→ transport-mode compatibility
→ effective route choice
```

> **Physical access ≠ effective accessibility.**

同一路线可对 pedestrian 可行、对驮畜困难、对重货不可行；也可能物理可达但没有通行权。

---

# Layer C｜Anchors, Growth & Resilience

## 11. Anchor Hierarchy｜Anchor 必须产生后果，也可以被人造出来

Anchor 可以是 natural / economic / infrastructural / institutional / sacred / defensive。

角色：

- `ORIGIN_ANCHOR`
- `GROWTH_ANCHOR`
- `STABILIZING_ANCHOR`

桥、井、码头、道路节点、市场、蓄水设施等可以由既有压力促成，然后反过来成为新的 Anchor。

每个重要 Anchor 说明：WHY / WHEN / Actor / knowledge state when relevant / attracts / repels / morphology / removal effect。

## 12. Causal Growth & Path Dependence｜从单向链升级为选择与反馈

基本 Growth Step 升级为：

```text
World condition / pressure
→ what relevant actors know
→ actor interest / rights / capability
→ choice / adaptation / negotiation
→ spatial response
→ modified condition / new anchor / new access
→ next pressure / feedback
```

仍可简写为：

```text
Driver → Spatial Response → New Constraint / Opportunity → Next Pressure
```

但重要阶段不能让空间“自动发生”。

### 12.1 Bounded Knowledge

历史人物不能自动知道 Planner 掌握的未来信息。

例如远端矿点即使 Planner 知道存在，早期人若尚未发现，就不能提前精准修路。

### 12.2 Feedback

识别会改变下一阶段条件的重要正反馈 / 平衡反馈。

例如：

```text
bridge → traffic → market → growth → stronger crossing demand
```

```text
frontage intensification → congestion → bypass → new frontage → old center role changes
```

Feedback 不等于命定未来，仍需 Historical Validity。

### 12.3 Existing Evolution / Path Dependence

old road、parcel、cemetery、wall、farmstead、channel、gate、bridgehead 等只在 continued use / rights / cost / ritual / terrain / institutional inertia 支持时保留。

不要因为今天存在更优路线，就自动重置历史形态。

详见 `references/causal-growth-model.md`。

## 13. Resilience｜最高效不一定最合理

对失败后果高的系统，检查是否存在合理 fallback：

- alternate route / crossing；
- multiple water points；
- distributed storage；
- multiple local service centers；
- seasonal fallback；
- partial local support。

> **Efficient network ≠ resilient network.**

冗余必须回答可信 vulnerability，不能为了“看起来完整”复制设施。

---

# Layer D｜Morphology & Scale

## 14. Movement Skeleton｜道路来自真实流动与有效通达

先确定需求与 flow，再比较 physical terrain / surface、rights、permission、security、seasonality、transport mode 与 mitigation。

L0 / L1 关注 network / corridor；L2–L4 逐渐细化为街道 / 巷道 / service access。

## 15. Commons & Negative Space｜空地有权利、用途和时间节奏

每个重要开放空间至少回答：

```text
Who controls it?
Who can use it?
For what?
When / at what rhythm?
Why has it not been built over?
Who could oppose enclosure?
```

> **Negative space is a social and environmental object.**

## 16. Settlement Hierarchy / Catchment｜服务关系受有效通达约束

节点分别说明 population / economic / political / network / symbolic role。

Catchment / hinterland 是服务关系，不等于 built-up extent，也不能只由几何最近距离生成。

它应受：effective accessibility、competition、rights / borders、seasonality、service alternatives 等影响。

## 17. Settlement Capacity / Built-Fabric Scale｜自然承载不是最终承载

严格区分：

1. `Location Search Envelope`
2. `Built-Fabric Capacity Envelope`
3. `Functional Hinterland / Catchment`

> **search envelope ≠ built fabric ≠ catchment**

Capacity 应由：

```text
resident / household pressure
+ service / visitor / freight pressure
+ institutional role
+ terrain + surface / substrate
+ ordinary affordable adaptation
+ effective accessibility
+ external supply support
+ density morphology
+ required commons / work yards
+ productive land protection
+ stock / buffer needs
+ resilience requirements
+ maturity
→ effective built-fabric capacity hypothesis
```

不要把“天然缺一项”直接等价为“不能住”。

例如缺明显地表水可能通过普通井 / 蓄水闭合基本生活，但仍限制农业或大规模扩张。

低坡面积仍不能直接作为 carrying capacity 代理。

详见 `references/settlement-capacity-and-scale.md`。

## 18. District / Quarter｜关系网络，不是现代 Zoning

禁止默认机械住宅区 / 商业区 / 工业区 / 行政区。

优先使用 mixed frontage、workshop cluster、sacred precinct、warehouse edge、market-facing mixed use、shared production yard、residential infill，并由 adjacency / externality / ownership / movement / history / institution / terrain / rights / access 解释。

## 19. Site Value & Spatial Competition｜密度要有竞争机制

不同活动对同一地点的相对价值不同。

定性考虑：

```text
accessibility
+ visibility
+ throughput
+ prestige
+ externality
+ rights / tenure
+ available space
→ relative site value
→ competition
```

当高价值 frontage / ground 稀缺时，可产生 subdivision、rear extension、verticality、displacement、shared access、secondary frontage 等。

不需要伪造货币租金。

## 20. Parcel & Frontage｜地块是历史、权利、家庭和竞争的结果

Planner 应理解 initial claim、boundary stabilization、inheritance split、household formation、migration、amalgamation、frontage competition、deepening plot、rear access、shared yard、easement、corner parcel、institutional holding、commons、infill。

典型链：

```text
commercial pressure ↑
→ frontage value ↑
→ lateral subdivision
→ narrow / deep plots
→ front-facing trade
→ rear household / workshop / storage
→ rear access pressure
```

但也可能是：

```text
household division / inheritance
→ parcel split
→ shared yard / new lane
→ later frontage differentiation
```

道路不是格网，parcel 也不是均匀切剩余空间。

## 21. Density as Morphology｜高密不是多塞房子

Density 通过 frontage continuity、parcel width/depth、coverage、verticality、lane width、courtyard size、service-space compression、infill rate、shared interfaces、open-space hierarchy 表达。

解释**为什么竞争 / access / institution / demographic pressure**使它变密或保持稀疏。

详见 `references/morphology-parcels-and-density.md`。

---

# Layer E｜Program, Kit & Recursive Handoff

## 22. Building Program｜从需求转成下游空间任务

Program 说明来源 Demand、required functions、shared / embedded / dedicated 可能性、approximate magnitude、adjacency / access、rhythm / peak when relevant、maturity、externality、uncertainty。

真正的房间、Plan、Section 仍由 Builder 决定。

## 23. Architecture Kit Requirements｜要求适应能力，不画组件

Planner 只定义 Kit **需要支持什么**，例如：

- required typology skeletons；
- terrain / surface adaptation；
- ordinary mitigation interfaces such as well / retaining / stepped / drainage / loading relation when planning-relevant；
- frontage behaviors；
- service interfaces；
- shared civilizational DNA；
- regional variation dimensions；
- forbidden mismatches。

Planner 可以要求“适应裸岩台地、保护稀缺土面、允许共享井院关系”，但不能指定井筒尺寸、挡墙结构、具体方块或 facade。

详见 `references/architecture-kit-requirements.md`。

## 24. Recursive Planning Handoff｜L0–L3 交给下一层 Planner

Planning Package 至少区分：

### `UPSTREAM_FIXED`

违反后会破坏已接受上层因果结构的关系。

### `DOWNSTREAM_TO_RESOLVE`

下一尺度必须调查 / 决定的事项，包括：

- current fabric；
- exact site；
- water / road / harbor viability；
- surface / substrate；
- Actor rights / tenure / access when consequential；
- bounded-knowledge uncertainty that affects history；
- mitigation feasibility / residual constraint；
- effective-access conditions；
- stock / seasonal / resilience dependency；
- local capacity；
- district / parcel logic。

### `DOWNSTREAM_ADAPTABLE`

下一尺度可自主调整、只要不破坏上层关系。

### `REVISION_TRIGGER`

只有当**合理的下层适应仍无法满足上层关系**时，才回报 `UPSTREAM_PLANNING_ISSUE`。

不要因为发现一个可被普通时代工程低成本解决的小约束就自动推翻父案。

L0→L1、L1→L2、L2→L3、L3→L4 都使用这一 contract。

详见 `references/recursive-planning-handoff.md`。

## 25. Builder Handoff｜只有最低规划尺度才进入 Builder

当规划已下钻到具体 building / compound / urban ensemble 关系时，才输出 Builder Design Package：

- `PLANNER_FIXED`
- `BUILDER_ADAPTABLE`
- dependencies
- `UPSTREAM_PLANNING_ISSUE`

详见 `references/planner-builder-handoff.md`。

## 26. Growth Sequence ≠ Implementation Sequence

Growth Sequence 是世界历史逻辑时间；Implementation Sequence 是 Minecraft 项目施工顺序。二者必须分开。

---

# Layer F｜Visual / Artifact Contract

## 27. Planning Packet

默认输出可独立阅读的 Planning Packet。重要决定可用：

```text
WORLD CONDITION
→ ACTOR / KNOWLEDGE / PRESSURE
→ CHOICE / ADAPTATION
→ SPATIAL CONSEQUENCE
→ FEEDBACK / DOWNSTREAM IMPLICATION
```

建议包含：

1. Premise
2. Authority / Evidence Register
3. Planning Context
4. Actor / Rights / Bounded Knowledge（适用时）
5. Demand Matrix
6. Flow / Stock / Rhythm / Externality Model
7. Constraint Transformation / Effective Access（适用时）
8. Anchor Hierarchy
9. Causal Growth + Feedback
10. Terrain Strategy
11. Surface / Substrate / Land-Cover Strategy
12. Movement / Commons
13. Settlement Hierarchy / Catchment
14. Settlement Capacity / Built-Fabric Envelopes
15. District / Parcel / Density / Competition（适用时）
16. Building Program
17. Architecture Kit Requirements
18. Recursive Planning Packages / Builder Packages
19. Implementation Sequence
20. Planner Critic
21. Uncertainty / Planning Assumptions

## 28. Machine-readable Planning Objects

任务内可使用：

```text
REGION-01
ACTOR-01
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

不要求每次都建立 `ACTOR-*`；只有复杂 actor / rights 关系值得单独对象化。

常见可选字段：

```text
actors / actor_refs
epistemic_state
rights / access_conditions
mitigation_options
residual_constraint
effective_access
stock_buffer
seasonal_pattern
resilience_role
site_value_drivers
demographic_driver
feedback_relations
```

Task-local ID 不自动成为 World Canon ID。

## 29. Visual Planning Contract

规划图应基于真实坐标 / terrain evidence，并区分 Observed / Derived / Canon / Assumption / Proposal。

按尺度逐渐表达：

- terrain / surface；
- search / built fabric / catchment；
- major flows；
- effective access / blocked or conditional access when important；
- shared / protected rights / commons when important；
- capacity；
- growth / feedback；
- district / parcel / frontage；
- Builder Package。

Actor / stock / rights 不必都画图；只有空间后果需要 Owner 直观看到时可视化。

详见 `references/planning-artifacts-and-maps.md`。

---

# Layer G｜Planner Critic & Gates

## 30. Counterfactual Test

改变主要 terrain / institution / Actor / access / Anchor 条件，方案是否应显著变化？若几乎不变，因果可能只是装饰。

## 31. Anchor Removal Test

删除重要 Anchor：哪些 route、demand、capacity、density、hierarchy 失去理由？

## 32. Historical Validity Test

每个 Growth Stage 在不知道未来的情况下是否仍合理成立。

## 33. Anti-Zoning Test

隐藏用途标签，只看关系：是否仍由 movement / adjacency / externality / rights / history / terrain 解释，而非四色分区。

## 34. Terrain Necessity Test

把方案移到普通平地，是否必须重设计？

## 35. Surface Character Necessity Test

相同 slope / relief、不同 surface / substrate / cover 时，capacity、livelihood、open-space、route、morphology 或 Kit requirements 是否合理变化？

## 36. Agency Test

重要空间变化能否回答：

- 谁推动 / 允许？
- 谁承担代价？
- 谁受益？
- 谁能阻止 / 重定向？

如果没有行动者，空间可能在“自动 masterplan”。

## 37. Knowledge Test

历史行动者是否使用了只有 Planner 才知道的信息？若是，修正 growth sequence。

## 38. Mitigation Test

在约束导致否决、迁址或巨大缩容之前：

> 是否考虑了与时代、规模、收益相称的普通适应？

同时检查是否为了保住偏爱方案而使用不成比例的工程。

## 39. Metabolism & Resilience Test

- 重要 flow 是否有合理 cadence / buffer？
- 峰值是否影响场地 / capacity？
- 是否存在危险单点失败？
- 若增加冗余，是否有真实 vulnerability 依据？

## 40. Feedback Test

重要 infrastructure / access / density / institution 是否改变下一阶段条件？如果 Growth 永远只是单向 chronology，反馈模型可能缺失。

## 41. Capacity Plausibility Test

检查：scale 是否有 causal driver；search / built / catchment 是否分开；surface / adaptation / external supply / stock / resilience 是否按相关性影响 capacity；是否存在假精确。

## 42. Parcel Causality Test

L2–L4 检查 parcel / frontage 是否由 tenure、site value、household / inheritance / migration、competition、rear access、commons 等生成，而非均匀切块。

## 43. Recursive Handoff Test

对 L0–L3：recipient 正确吗？Fixed 是否过多？Unresolved 是否含 actor rights / epistemic / mitigation / access / metabolism 中真正会影响下层的问题？新证据何时回退上层清楚吗？

## 44. Kit Clone Test

Architecture Kit 是否退化成整栋 Blueprint 复制；若是，FAIL。

---

# Layer H｜States, Delivery & Safety

## 45. Planning State Machine

```text
SCOPED
→ PREMISE_READY
→ MORPHOLOGY_READY
→ HANDOFF_READY
→ REVIEWED
```

`HANDOFF_READY` 只表示下一层能继续，不等于可 world-write，也不等于所有工程 / access / resource 已实证。

## 46. Three Gates

### Premise Gate

确认社会 / Actor / knowledge / capability / environment / Anchor / Context 足以支持当前尺度的因果假说。

### Morphology Gate

确认 Terrain、Surface、Adaptation、Effective Access、Demand / Metabolism、Growth / Feedback、Hierarchy、Capacity、Negative Space、District / Parcel（适用时）真正生成空间关系。

### Handoff Gate

确认下一层 recipient、Fixed / unresolved / adaptable、mitigation boundary、uncertainty、revision protocol 清楚，没有越级设计。

## 47. Autonomous Execution / Safety

Planner 应自主完成研究、推理、Critic、制图与 artifact 输出。只有以下情况停止报告 blocker：

- 无法确认重要证据 / Canon authority；
- 继续会伤害真实世界数据；
- 工具不可用；
- 高影响歧义无法通过 branch / uncertainty 表达；
- 用户要求 world-write，但 Planner 无权执行。

默认只读规划，不修改 Minecraft 世界。

---

# 48. Canonical Workflow

除非任务明确要求其它顺序：

1. 确认 Scope、Scale、Authority、Fabric Observation State、Evolution Logic；
2. 读取必要 terrain / surface / current world / Canon；
3. 识别会改变空间的主要 Actors / rights / capabilities / bounded knowledge；
4. 建立 Territorial / Settlement Premise；
5. Premise Gate；
6. 建立 Demand；
7. 建立 Flow / Stock / Rhythm / Externality；
8. 分析 terrain / surface raw constraints；
9. 对重大约束做 Human Adaptation / Mitigation reasoning；
10. 建立 Effective Accessibility；
11. Anchor hierarchy；
12. Causal Growth + Feedback；
13. Commons / Negative Space；
14. Settlement hierarchy / catchment；
15. Settlement Capacity / Built-Fabric Envelope；
16. 当前尺度适用时建立 Site Value / Competition / District / Parcel / Frontage / Density；
17. Building Program；
18. Architecture Kit Requirements；
19. Planner Critic：Counterfactual / Anchor Removal / Historical Validity / Anti-Zoning / Terrain / Surface / Agency / Knowledge / Mitigation / Metabolism & Resilience / Feedback / Capacity / Parcel；
20. Morphology Gate；
21. 若仍需向下规划，形成 Recursive Planning Packages；否则形成 Builder Packages；
22. Handoff Gate；
23. 输出 Planning Packet、machine-readable objects、规划图、source register、uncertainty；
24. 状态设为 `HANDOFF_READY`，停止等待下游 / review。

并非每项任务都要把 3–19 全部写成长篇章节。只展开会改变空间结果的机制。

---

# 49. Reference Loading

按任务需要读取：

- `references/human-geography-kernels.md`：v0.4 四个底层 Kernel；
- `references/causal-growth-model.md`：历史生长、bounded knowledge、Anchor、feedback、path dependence；
- `references/flows-externalities-and-demand.md`：需求、flows、stocks、rhythms、externalities、resilience；
- `references/morphology-parcels-and-density.md`：roads、rights、site value、demography、parcel、frontage、density；
- `references/settlement-capacity-and-scale.md`：search / built / catchment、adaptation、external supply、capacity；
- `references/surface-substrate-landcover.md`：surface / substrate / vegetation / ground character；
- `references/architecture-kit-requirements.md`：Planner 可要求什么、不应设计什么；
- `references/recursive-planning-handoff.md`：L0→L1→L2→L3→L4 递归交接；
- `references/planner-builder-handoff.md`：最低规划尺度 → Builder contract；
- `references/planning-artifacts-and-maps.md`：JSON、ID、地图、sections、版本；
- `references/regression-rubric.md`：独立测试与审核维度。

---

# 50. Core Invariants｜v0.4

> **Settlement morphology is caused, not arranged.**

> **Space does not change by itself: important changes need plausible actors, rights and capabilities.**

> **Planner knowledge is not automatically historical actor knowledge.**

> **Civilizations inhabit transformed landscapes, not untouched suitability maps.**

> **A constraint should block a plan only after proportionate, period-appropriate adaptation has been considered.**

> **Physical access is not effective accessibility.**

> **Demand is not a building checklist.**

> **Flow is not the whole metabolism: stocks, buffers and rhythms can create space.**

> **Efficient networks are not automatically resilient networks.**

> **Roads follow effective flows; parcels follow history, rights, competition and households.**

> **Infrastructure and spatial choices create feedback and new path dependence.**

> **Flat land is not equivalent land: surface / substrate / land-cover can change carrying capacity and morphology.**

> **Visible surface evidence must not be overclaimed as fertility, quarry quality, ore or hydrology.**

> **Unknown existing fabric is not empty land.**

> **A settlement node needs a scale hypothesis, not only a point.**

> **Search envelope, built fabric and catchment are different objects.**

> **Growth history and Minecraft construction order are different.**

> **Higher planning scales constrain lower scales; they do not replace them.**

> **Planner-to-Planner handoff is different from Planner-to-Builder handoff.**

> **Architecture Kit is constrained vocabulary, not identical blueprint.**

> **Planner constrains relationships; Builder retains architectural authorship.**

### Explicit non-goals of v0.4

v0.4 does **not** add a full monetary economy, detailed political simulation, population microsimulation, infrastructure lifecycle / replacement model, resource depletion / regeneration simulation, or exhaustive maintenance engineering model.

> **Model a mechanism only when it materially changes spatial choice, morphology, capacity, hierarchy or downstream design.**
