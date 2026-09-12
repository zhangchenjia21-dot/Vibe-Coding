# minecraft-builder v1.9

## 0. Mission｜从设计到完成品的一条主流程

Minecraft 建造任务的目标不是“尽快把方块写进去”，也不是靠后期堆细节制造完成感，而是让一个空间从**设计成立 → 三维建成 → 玩家尺度完成 → 验证交付**。

本 Skill 采用统一但隔离的五层结构：

1. **Thinking｜先形成空间与建筑因果**；
2. **Builder Core｜建立不可替代的空间、结构与环境骨架**；
3. **Spatial Completion Gate｜确认核心已经成立，才允许精修**；
4. **Finishing｜完成使用、构造收口、材质、光、生活痕迹与玩家尺度质量，并通过 Finishing Completion Gate**；
5. **Verification / Delivery｜验证结果、处理回退并交付**。

核心原则：

> **Design first. Build core second. Finish only after spatial completion. A finished scene must be perceptibly finished at player scale. Verify the real world result last.**

QA 不能代替设计；Finishing 也不能拯救失败的建筑。作品即使“可达、无孤立方块、与 Blueprint 一致”，仍可能是糟糕空间；反过来，设计文档写得漂亮，也不能代替 Minecraft 世界里的真实结果。

同样，**有精修动作不等于完成精修**。如果玩家正常游览时几乎感受不到从 `SPATIAL_COMPLETE` 到 `FINISHED` 的变化，不能因为“已经加过家具、做旧、灯光或执行过 Restraint”就宣告 `FINISHED`。

---

## 0.1 Work Scope｜状态属于当前施工范围，而不是整个世界

大型项目可以包含多个 Work Scope，例如：

- 单栋建筑；
- 一组相关建筑；
- 一个庭院；
- 一段街区；
- 一处园林 / 景观区域；
- 一个独立基础设施系统。

每个 Scope 独立维护状态：

> `DESIGN_READY → CORE_BUILDING → SPATIAL_COMPLETE → FINISHING → FINISHED → VERIFIED`

规则：

- Scope 没有达到 `SPATIAL_COMPLETE`，不得进入该 Scope 的 Finishing；
- Scope 没有通过 **Finishing Completion Gate**，不得从 `FINISHING` 升为 `FINISHED`；
- 如果 Scope 依赖的共享 Terrain / Water / Circulation / Structural Vegetation 尚未稳定，也不得提前精修；
- 大型聚落不要求全世界同时进入 Finishing。已稳定的重点 Scope 可以先完成，外围普通 Scope 可停在 `SPATIAL_COMPLETE`；
- 若用户只要求空间骨架或背景建筑，`SPATIAL_COMPLETE` 可以是合法交付状态；
- 对边界明确的重要单体 / 核心场景，如果用户没有要求停止在空间完成阶段，默认继续到 `FINISHED`。

**Spatial role overrides object size.** 模块职责由对象对空间的作用决定，不由对象大小决定。小花海可能属于 Builder Core；巨大雕塑也可能只是 Finishing。

---

## 0.2 Task Type｜先判断任务类型

将任务归入最接近的一类：

- `BUILDING`：单体或少量建筑；
- `SETTLEMENT`：村庄、街区、城镇、聚落系统；
- `LANDSCAPE`：园林、森林、湿地、山水、自然景观；
- `MIXED_ENVIRONMENT`：建筑、地形、水体、道路、植被共同构成主体。

`LANDSCAPE` 与 `MIXED_ENVIRONMENT` 中，Terrain、Water、Rock、Structural Vegetation、Circulation、Architecture 都是可能的一等系统，不得默认降为“后期装饰”。

---

# Layer A｜Thinking

## 1. Research / World Rules｜现实题材研究，幻想题材先定规则

### 现实 / 历史 / 写实题材

先参考可靠的历史、考古、建筑、园林、聚落、地理或景观资料。研究不是为了复制图片，而是提取会改变设计的事实：

- 选址与朝向；
- 使用者与功能；
- 空间关系与交通 / 礼仪；
- 平面、剖面、建筑类型；
- 地形、水文与环境条件；
- 材料、结构与建造技术；
- 植被、农业、防御、生产或宗教逻辑。

重要 `BUILDING` 条件允许时，至少取得一项可信的 **plan / section / elevation / measured typology** 参考，用于约束比例、跨间、层高、屋顶和结构关系；不要只读“这种建筑有什么风格特征”的文字摘要。

### 幻想题材

至少先定义：

- 使用者 / 文明；
- 环境；
- 社会真正需要的空间系统；
- 材料与建造技术；
- 必要时的魔法规则；
- 哪些现实规律仍成立，哪些被世界规则改变。

不必写长篇世界观，但不能无规则拼贴。

---

## 2. Shared Spatial Principles｜所有类型共用的空间原则

每个重要设计决定都应有因果。不要只问“这里还能放什么”，而应问：

- 为什么这个空间 / 对象在这里？
- 它服务谁、服务什么活动或生态过程？
- 它与 Terrain / Water / Circulation / Architecture / Vegetation 有什么关系？
- 玩家如何到达、经过、停留、回望？
- 空白为什么存在？
- 高差为什么存在？
- 规则性或不规则性为什么存在？

开放空间可以很大，但必须有功能、生态、视线、礼仪、生产、交通或构图原因。禁止为了“丰富”把每块空地填满，也禁止把无意义空白当成完成。

### Anti-Unconscious Modern Planning

除非题材明确需要，不要默认使用直角道路网、等间距建筑、同尺寸地块、统一退界、镜像复制、网格绿化、郊区式“房子散在草坪上”。

规则性本身不是错误。礼仪轴线、果园、行道树、军营、正式庭院、防风林等可以高度规则；禁止的是没有历史、功能或空间原因的规则性。

---

## 3. Architectural Thinking Kernel｜重要建筑先建立因果模型

对 `BUILDING`、`SETTLEMENT` 中的重要建筑，以及 `MIXED_ENVIRONMENT` 中承担主要空间作用的建筑，在放置第一个建筑方块前先经过以下思维链。

### 3.1 Purpose & Users｜为什么建、给谁用

先确定建筑存在理由、建设者与使用者、人数 / 身份 / 活动，以及它在社会、组织或文明中的角色。建筑首先解决需求，不是先得到外观再往里塞功能。

### 3.2 Site & Context｜为什么建在这里

主动读取并利用地形、水源、道路、港口、城墙、田地、周边建筑、公共空间、视线、防御、礼仪、日照、风雨、湿度与排水。

优先 **fit architecture to context**。不要把建筑当成可以任意平移的独立模型，再让场地去迁就它。

### 3.3 Program & Spatial Relationships｜先有活动与空间，再有墙

识别核心空间、次要空间、服务 / 后勤、公共 / 私密、礼仪 / 日常、生产 / 储藏、洁净 / 污染、安静 / 嘈杂、人流 / 货流 / 防御流线。

明确哪些空间必须相邻、哪些应隔离、谁从哪里进入、活动之后去哪里、哪些空间共享庭院 / 廊道 / 楼梯 / 门厅 / 服务核心。

复杂建筑先形成轻量 **Space Graph**：节点是重要空间，边是必须成立的连接。

设计单位优先理解为 `space / room / hall / courtyard / circulation / service zone`，而不是先调用 `box()` 再掏空。

### 3.4 Hierarchy & Sequence｜主次关系与人的体验顺序

判断核心空间、主要入口、主次等级和服务关系，并通过尺度、高度、位置、光线、开敞度、体量、轴线、门厅、前室、庭院与转折表达。

同时设计：

> `approach → threshold → entry → transition → main space → secondary space → continuation / exit`

“最终可达”只证明能走到；Sequence 解释为什么这样走、经过什么、何时压缩或释放。

### 3.5 Plan + Section + Environmental Logic｜水平与垂直同时设计

不要先画二维 footprint 再统一拉墙高。早期同时考虑：

- 空间宽高；
- 功能导致的层高差；
- 楼层叠放；
- 楼梯 / 坡道；
- 屋顶跨度；
- 窗的采光、视线、通风或防御作用；
- 地坪与室外接口；
- 雨水、遮阳、排水、坡度与地形。

复杂项目至少同时建立 **Plan + Section + Sequence**。

### 3.6 Structure, Material & Technology｜先理解怎么建得出来

先判断题材合理的结构体系，例如承重墙、木柱梁、石柱与拱券、肋拱、穹顶、框架、扶壁、木屋架等。

材料与技术应反向约束跨度、墙厚、柱距、开口、层高、屋顶、建筑高度和构件连接。

> **Structure generates architecture.**  
> **Material changes geometry.**

对依赖结构体系的建筑，不得先完成通用 shell，再把尖拱、扶壁、梁柱、穹顶等“风格件”覆盖上去。

### 3.7 Form as Consequence｜形态是因果链的结果

Massing、Silhouette、入口、屋顶、开口与立面应由：

> `Purpose + Site + Program + Hierarchy + Section + Structure + Material + Culture`

共同产生。

塔、侧翼、门楼、庭院、退台、穹顶、巨大窗、素墙与高低变化都要有理由。

> **Form is consequence, not decoration.**

不要为了反方盒制造无理由复杂轮廓，也不要把所有功能压缩成相似 box，再只靠门窗和材料区分。

### 3.8 Minecraft Translation｜最后才转译成方块

Minecraft 不是现实建筑的低清晰度导出格式，而是独立媒介。优先保留：

- 比例；
- 空间等级；
- 结构可读性；
- 主要轮廓；
- 玩家体验；
- 重要构造关系。

主动判断哪些现实细节需要放大、哪些距离需要压缩，以及 stairs / slabs / walls / fences / trapdoors 等如何服务几何表达。检查玩家眼高、移动速度、FOV 与视距下的近 / 中 / 远景，不机械 1:1 复制现实绝对尺度。

### Architectural Intent｜同一份意图贯穿 Builder 与 Finishing

重要建筑正式施工前，简短记录：

- Purpose；
- Users / Activities；
- Site / Context；
- Program / Space Graph；
- Hierarchy / Sequence；
- Plan + Section；
- Structure / Material / Technology；
- Form + Minecraft Translation。

每项一两句话即可。它是后续 Core 与 Finishing 共用的设计状态，不是交接文档，也不是为了制造长篇报告。普通小建筑可简化。

---

# Layer B｜Builder Core

## 4. Module Contract｜One rule, one owner

每个主要模块只定义四件事：

- **Responsibility**：它负责什么；
- **Inputs**：它依赖哪些已成立关系；
- **Output / Invariants**：完成后什么必须成立；
- **Non-responsibility**：什么不是它的工作。

Verification 只验证这些 invariants，不重新复述完整设计理论。

---

## 5. Terrain Module

**Responsibility**：地形骨架、连续高差、建筑与地形接口、自然地貌逻辑，以及依赖自然高差的室外交通。

### Anti-Forced-Elevation

高差优先来自连续地形、河谷、山脊、坡地、台地、洼地，以及防洪、排水、防御、视线、等级、生产或交通需要。

> **fit building to terrain before forcing terrain to building**

局部找平、基座、挡土墙、台阶和填挖方可以成立，但必须有具体理由并与周围坡面可信过渡。不要先任意指定标高，再用巨大石台或整块填方强迫地形服从建筑。

### Terrain-Conforming Circulation

山路、坡道、台阶等主要用于克服自然高差的室外 circulation，默认应顺应、切入或依托坡面。优先使用沿等高线绕行、switchback、局部 cut/fill、短挡土墙、贴坡台阶和天然鞍部 / 沟谷 / 坡肩 / 台地。

桥梁、城防墙梯、栈道、架空连廊、码头等有明确工程理由时可脱离坡面，但结构身份、支承和起讫接口必须清楚。

### Natural Terrain Morphology

自然地形不能只是连续平行 contour terrace / 蛋糕式台阶。通过有因果的 ridge、swale、gully、shoulder、cliff、talus、rock exposure、soil / vegetation pocket 与缓陡坡转换形成地貌。

目标不是随机噪声，而是让地形像由地貌过程形成。

**Invariant**：主要高差与地貌可解释；建筑与地形有可信接口；依赖地形的交通不成为无理由高架实体。

**Non-responsibility**：墙脚小草、碎石点缀、局部生活磨损等 Finishing Micro。

---

## 6. Water Module

**Responsibility**：水体几何、床岸、水位、source / flow 语义、跌水、inlet / outlet / overflow，以及水体与建筑 / 地形的工程接口。

Minecraft 水不是静态蓝色体素。真实 `water` 必须考虑：

- channel bed / pool bottom；
- bank / wall containment；
- source 与 flowing water；
- lateral spill；
- downstream drop；
- inlet / outlet / overflow；
- 桥墩、岸脚、建筑、水轮等接口；
- 邻接更新后的稳定性。

禁止把多层 `water[level=0]` 静态写成阶梯水带，然后因为几何连通就认为河道成立。自然河溪默认先塑造稳定床岸，再放水。

如果工具允许，触发真实 fluid update / 邻接更新并重新读回；如果不能：

1. 使用保守且明确受床岸约束的几何；
2. 标记 `fluid stability unverified`；
3. 不得用 connected component、写入成功或静态截图代替流体稳定性。

水体验证区分：

- `geometry connectivity`；
- `water-level logic`；
- `bank / bed containment`；
- `fluid-update stability`。

**Non-responsibility**：水边小花、灯笼、杂物等 Finishing Micro。

---

## 7. Architecture / Spatial System Module

**Responsibility**：Program、Space Graph、Plan、Section、Structure / Tectonics、Massing、Roofline、Envelope、主要 openings 和建筑内部主要空间关系。

默认生成顺序：

> `Program / Space Graph → Plan + Section → Structural / Tectonic System → Circulation → Massing / Roofline → Envelope / Openings → Core Facade`

不要把“完整矩形 shell → 掏空 → 后贴风格构件”作为重要建筑默认策略。矩形体、盒体和参数函数可以是实现原语，但必须服务于已经成立的空间、剖面和结构逻辑。

### Massing & Silhouette

体量由功能、结构、剖面、场地与等级产生。主动判断：

- primary / secondary mass；
- entrance mass；
- vertical hierarchy；
- recess / projection；
- courtyard / wing / tower / service mass；
- roofline / skyline；
- terrain / street / courtyard / water interface。

### Reusable Generator ≠ Repeated Morphology

代码复用、模板与蓝图是实现手段，不是设计理由。功能、等级、场地或空间职责不同的对象，不应只通过同一 generator 改长宽高 / 换材料制造差异。回廊、柱列、军营、行列住宅等原型本来依赖重复时可以有意识重复。

### Core Facade / Envelope

决定建筑身份和构造的立面关系属于 Builder Core，例如扶壁、梁柱体系、主要开口、墙脚、重要檐口、主要退进、主屋顶收边。它们不能等 Finishing 来“救”。

**Invariant**：忽略小构件和表面装饰后，建筑仍具有可读的三维身份；内部空间、剖面、结构与外部形态相互对应。

**Non-responsibility**：展示级窗框深化、局部装饰收边、生活痕迹和表面做旧。

---

## 8. Circulation Module

**Responsibility**：道路、路径、楼梯、坡道、走廊、桥、门 / portal，以及这些连接与 Terrain / Architecture / Settlement / Landscape 的接口。

重要建筑的关键 Space Graph edge 必须有实际对应的入口 / 廊道 / 楼梯 / route，而不是只让两个坐标在几何上相邻。

主要 circulation 应在 Builder Core 阶段成立；不得等待 Finishing 用家具摆放或铺地来“提示路线”。

**Invariant**：关键空间连接真实成立，门槛、落脚、净空和主要竖向交通可用。

---

## 9. Vegetation & Ground Structure Module

**Responsibility**：承担空间组织、生态骨架或主要视觉角色的植被与地表。

### Structural Vegetation

大型乔木、树群、林带、竹林、树林、绿篱等可承担围合、遮挡、框景、背景 / 前景、路径引导、水岸软化、高低过渡、canopy / skyline 与视觉焦点，因此属于 Macro / Meso。

Minecraft 原生小树不是默认景观树答案。承担结构作用的树应根据建筑和地形主动确定高度、冠幅、主干、分枝、倾斜与不对称程度。尺度至少检查：

> `player → building → terrain / landscape → canopy / skyline`

### Layered Vegetation

乡野、森林、湿地、河岸、园林等自然环境应按语境形成 canopy、understory、herbaceous、groundcover、seasonal / flowering 等适当层级。

自然式环境优先通过 cluster、density gradient、irregular edge、opening、hierarchy、overlap、asymmetry、terrain response、sightline response 形成受约束的不规则；随机撒点同样不合格。

### Flowering Community / Flower Abundance Bias

允许花卉出现时，不要只放象征性几朵。选择少数相容 species / colors，形成 patch、ribbon、clearing carpet、林缘 / 草甸 / 道路 / 水边群落。

对 lush forest / enchanted forest / sacred grove / ornamental garden / spring-summer meadow 等强调繁盛或花季的场景，在生态 / 世界规则没有反对理由时，花层应在玩家尺度上明显可见。采用：

> **dense patch + sparse transition + open gap**

但不等于全图撒花、集齐颜色、覆盖道路，或用花替代草 / 蕨 / 灌丛 / 苔藓。

### Ground Plane

`grass_block` 不能自动被视为完成地面。根据空间职责决定草坪、林下、苔藓、裸土、碎石、铺装、农田、花圃、水岸过渡等。

**Spatial role overrides object size**：如果花海、地被、矮篱或低墙本身决定空间骨架，它属于 Builder Core；若只是墙脚几株植物，则属于 Finishing。

---

## 10. Construction Material Module

**Responsibility**：材料与构造的基本对应关系，而不是展示级做旧。

形成受控材料语言：

- primary material；
- secondary material；
- structural accent；
- construction transition。

材料变化应尽量对应 foundation / wall base、corner / opening surround、primary wall field、roof ridge / eave / edge、paving transition 等构造位置。

不要因为生成方便整面铺同一种材料，也不要用均匀概率 noise 随机混材伪造丰富度。

Builder Core 应建立“这座建筑为什么用这些材料、它们如何构造”的基本逻辑；weathering / soot / damp / repair / wear 等时间层交给 Finishing。

---

## 11. Assets｜复用是实现策略，不是设计替代

可以复用 Litematica、Blueprint 或已有资产库，但资产必须满足当前 Scope 的：

- 功能；
- 尺度；
- 地形接口；
- 风格；
- 空间职责；
- 视觉权重。

高价值植被、假山、小型构筑物等：先查已有资产；合适则选 variant；没有则可在当前任务设计 candidate。Candidate 可以用于本轮，但不得自动写入正式资产库。

---

## 12. Builder Phases｜Macro → Design Gate → Meso → Base Micro

### Macro

先解决：

- 场地 / Terrain 骨架；
- 大水体；
- 主次空间；
- 总体高度；
- Structural Vegetation；
- 重要建筑 Program / Space Graph；
- Plan + Section + Structural System；
- 主次体量与主要 roofline；
- 主要视觉焦点。

### Design / Massing / Section / Tectonic Gate

重要建筑 Macro 后进入 Meso 前检查：

- proportion；
- volumetric composition；
- structure / section 是否真正驱动外形；
- major mass junction；
- roof transition；
- entrance hierarchy；
- negative space；
- secondary masses；
- 暂时忽略材质和小构件后 silhouette 是否成立。

不能仅因为“看得出这是中殿 / 塔 / 侧翼”就通过。失败时先重做体量 / 剖面 / 构造，不进入 Meso。

景观 Scope 则执行与当前 Macro 对应的 Terrain / Water / Vegetation Gate。

### Meso

完成道路、游廊、桥、次级山水、建筑间关系、林带 / 树群、building bay / support / roof junction、主要 opening、垂直交通、Core Facade、视线与空间开合。

### Base Micro

只完成让空间“成立”的必要 Micro：

- 必要门窗收边；
- 栏杆；
- 主要台阶；
- 功能性铺地；
- 关键地表过渡；
- 空间角色需要的花草 / 地被；
- Construction Material placement；
- 必要照明与基础陈设。

> **Base Micro makes the space complete, not richly finished.**

不得在 Macro / Meso 尚未成立时，用 Micro 掩盖空旷、网格化、比例失衡、体量失败或构造问题。

---

## 13. Spatial Completion Gate｜Finishing 的唯一入口

Finishing 之前，对当前 Scope 执行一次 Spatial Completion Gate。它不是美化检查，而是判断“核心空间是否已经完整”。

至少确认：

### Spatial Logic

- 功能与主要空间关系完整；
- 不存在明显缺失空间、错误连接或无意义核心空腔；
- 主要活动可以真实发生。

### Circulation

- 主入口、主路线、主要楼层、门、楼梯、坡道、桥和关键连接成立；
- 不依赖异常跳跃、绕行、擦边或错误空间。

### Massing & Section

- 体量、层高、主要 roofline 与 section 成立；
- 不需要靠后续家具 / 装饰才能“看起来像建筑”。

### Structural / Environmental Systems

- Structure / Terrain / Water / Circulation / Structural Vegetation 等一等系统稳定且没有互相破坏。

### Minecraft Usability

- 玩家尺度基本成立；
- 没有大量穿墙、卡头、断层、悬空、门槛错误等 Core blocker。

### Upstream Defect Test

问：

> **如果现在完全不增加家具、props、做旧、装饰和展示级 Micro，这个 Scope 是否已经是一个完整、合理、可使用的空间？**

如果答案是 NO：保持 `CORE_BUILDING`，回 Builder Core 修复。

如果答案是 YES：状态设为 `SPATIAL_COMPLETE`，冻结上游核心语义，才允许进入 Finishing。

---

# Layer C｜Finishing

## 14. Finishing Boundary｜继承 Intent，但不继承上游设计权

进入 `FINISHING` 后，默认权限分三档。

### Allowed

可自由在既有意图内调整：

- furniture；
- props；
- lighting；
- surface / aging；
- Detail Vegetation；
- 局部窗框 / 檐口 / 墙脚等 refinement；
- 小型铺地与近景收口。

### Restricted

只有不改变 Core semantic 时可局部调整：

- opening depth；
- 栏杆；
- 小型台阶 / landing；
- 小范围 ground interface；
- 非结构性隔断；
- 局部屋面 / 立面收边。

### Frozen

默认不得在 Finishing 阶段改变：

- Program / Space Graph；
- 主体量；
- 主要 Plan / Section；
- 主屋顶体系；
- 主入口与主要 Circulation；
- 大地形；
- 主水体；
- Structural Vegetation；
- 核心 skyline；
- 主要空间序列与功能分区。

若 Finishing 发现必须修改 Frozen 内容：

> `FINISHING → UPSTREAM_BUILDER_ISSUE → CORE_BUILDING → Repair → Spatial Completion Gate`

不得偷偷越权大改。

---

## 15. Finishing Thinking Kernel｜精修不是“加更多东西”

### 15.1 Finish From Use

不要先问“这里还能放什么”，先问“谁在这里做什么”。家具、工具、储物、光源、磨损、污渍和工作痕迹应从活动产生。

例如厨房应从 `烹饪 → 火源 → 备餐 → 储藏 → 洗涤 → 行走净空` 生成细节，而不是因为房间空就塞桌子。

### 15.2 Finish From Construction

门框、柱脚、檐口、屋脊、墙脚、梁端、转角、排水、扶手、台阶等应说明建筑怎么被建出来、怎样收边和落地，而不是作为随机装饰件。

### 15.3 Finish From Material & Time

表面变化来自接地、受水、烟熏、踩踏、维修、风化、潮湿、阳光、植物侵入和人为维护。

> **Aging is causal, not noisy.**

不同使用强度、财富、管理水平和环境条件应产生不同程度的时间层。

### 15.4 Finish From Attention｜密度有主次，但主次不能成为漏做借口

精修密度必须有主次：

- **Focal**：主要入口、祭坛、炉火、工作核心、主窗景等；
- **Supporting**：帮助理解空间和使用；
- **Quiet**：允许安静、留白和背景。

> **Uniform detail density = failure.**

但 `Focal / Supporting / Quiet` 描述的是**信息密度与视觉权重**，不是“是否需要完成”。

> **Quiet ≠ Untouched. Sparse ≠ Unfinished.**

Quiet 区可以没有大量家具和 props，但仍应通过适合其角色的构造收口、材质关系、地面 / 墙脚 / 边缘处理、光暗、维护状态或其它低密度手段表现为**有意完成的安静空间**，而不是没有处理的毛坯。

### 15.5 Finish From Light

光是空间材料，不是按固定间距插火把。考虑自然光入口、功能照度、重点、路线、夜间识别和题材合理的光源。

### 15.6 Finish From Life

通过储物、工具、材料、货物、生活用品、工作状态、清洁程度、维护水平、财富和习惯表达“这里真的被使用”。

生活痕迹不是到处撒杂物。

### 15.7 Finish For Minecraft Perception

按三种距离思考：

> `Far → silhouette / rhythm / contrast`  
> `Mid → facade / material / structure / larger props`  
> `Near → furniture / surface / small objects / wear`

考虑玩家眼高、FOV、移动速度、block resolution 和真实方块模型。现实中合理但 Minecraft 中不可读的细节可以适度放大或简化。

Finishing 的价值必须在玩家相关视角中可读，而不是只能靠坐标账本、局部放大、代码注释或“告诉玩家去看哪里”才能发现。

### 15.8 Finish Across Experience｜Coverage 与 Density 是两件事

不要用几个精修得很好的点代替整个 Scope 的完成。

先识别玩家真正会经历的：

- approach / entrance；
- 主要 route；
- 核心空间；
- 次要但高频使用空间；
- 关键 threshold / junction；
- 代表性的 outdoor / courtyard / service / quiet zone；
- 主要近景、中景观察面。

这些体验区域不要求同样复杂，也不要求每个区域都必须新增方块；但应逐一判断它是否已经以符合角色的方式达到完成状态。

> **Detail Density ≠ Finishing Coverage.**

如果一个空间本来已经完成，可以明确保持；如果它仍显毛坯、空洞、接口粗糙或玩家感知上与 Core 阶段几乎没有区别，不能仅因为它被定义为 Quiet 就跳过。

---

## 16. Finishing Systems｜六个独立精修系统

### 16.1 Architectural Refinement

处理墙脚、门窗深度、拱券收边、柱脚 / 柱头、檐口、屋脊、局部排水、栏杆、台阶 landing、转角与接口。

它只能完成既有建筑，不得重新决定“为什么这里是一扇门 / 为什么这里是一座塔”。

### 16.2 Interior / Activities / Props

先识别 Activity Zone，再形成：

> `Activity → Furniture → Storage → Props → Clearance`

家具和道具服从房间用途、使用者、身份和题材。不得因为空间空就机械放桌椅箱子。

### 16.3 Surface / Aging

处理 wear、damp、soot、repair、dirt、erosion、moss、maintenance difference 等时间层。

用“原因 + 位置 + 强度”决定分布，不采用均匀随机噪声。

### 16.4 Lighting

处理 natural light、artificial light、focal lighting、route readability、night experience 与 atmosphere。不同题材 / 活动需要不同照明，不按固定格距布灯。

### 16.5 Micro Landscape

处理墙脚植物、庭院小植被、地被、小灌木、道路边缘、水岸微过渡、盆栽、小型园艺、踩踏地面、局部泥土与碎石。

它不得重做 Structural Vegetation 或改变景观骨架。

### 16.6 Composition / Restraint

统一调节 Focal / Supporting / Quiet 信息层级，检查太空、太满、重点不清、重复过度、Micro 抢远景等问题。

最后执行正式 **Restraint Pass**：

> 不只问“还缺什么”，也问“哪些东西应该删除”。

Restraint 的目标是删除噪声，而不是把必要的完成度一起删掉。Restraint 后必须重新检查 Coverage 和 Perceptual Completion。

---

## 17. Finishing Passes｜先建立 Coverage，再逐层完成

正式精修前建立一个轻量 **Finishing Coverage Map**。不需要长表格，只需把当前 Scope 的主要玩家体验区域分为 Focal / Supporting / Quiet，并注明最可能需要的系统，例如：

- Use / Interior；
- Architectural Refinement；
- Surface / Aging；
- Lighting；
- Micro Landscape；
- deliberate no-change / already complete。

Coverage Map 不是配额，不要求每区使用所有系统，也不要求最低 block count；它只防止模型因为只盯几个局部节点而漏掉整个玩家体验。

默认 Pass：

1. **Functional Finish**：家具、活动区、储物、必要 props、功能照明与净空；
2. **Architectural Finish**：门窗、墙脚、柱、檐口、屋脊、转角和接口收口；
3. **Material / Environmental Finish**：材料时间层、风化、湿度、烟熏、Detail Vegetation、Micro Ground；
4. **Composition / Atmosphere**：视觉主次、光暗、节奏、空间气氛；
5. **Restraint Pass**：删除冗余、重复、抢戏或破坏空间关系的细节。

每个 Pass 后可以局部复核；Restraint 后必须回看 Coverage Map。

不要求所有 Scope 使用同一密度。核心建筑可以深入；普通背景 Scope 可以停在 `SPATIAL_COMPLETE` 或轻量 Finish。

---

## 18. Finishing Completion Gate｜只有玩家可感知地完成，才叫 FINISHED

这是从 `FINISHING` 升为 `FINISHED` 的硬门。它检查的不是“是否执行过五个 Pass”，而是**最终玩家体验是否真的从空间完成提升到成品完成**。

### 18.1 Coverage

检查 Coverage Map 中的重要体验区域：

- Focal 是否真正完成，而不只是放了几个 props；
- Supporting 是否帮助空间被读懂；
- Quiet 是否是有意的安静，而不是未处理；
- 主要 route、入口、核心空间、关键接口与代表性 outdoor / service 区是否存在明显 unfinished pocket。

不要求所有区域发生修改；但“未修改”必须是因为该区域已经完成或有意保持，而不是因为模型漏看。

### 18.2 Perceptual Delta

将进入 Finishing 前的 `SPATIAL_COMPLETE` baseline 与当前结果，用相同或可比较的玩家相关视点复核：

- approach / entrance；
- 一到两个核心空间；
- 主要 route；
- 代表性 Supporting / Quiet 区；
- 必要的 Mid / Near 视角。

问：

> **如果不给坐标提示、不标注“这里改了什么”、不放大到单个方块，玩家正常游览时能否自然感到这个 Scope 已经从“建筑完成”进入“成品完成”？**

不要求每个视角都发生巨大变化，也不要求追求戏剧化 before / after；但如果主要变化只存在于极少局部点，正常游览几乎看不出来，就不能通过。

### 18.3 Role-Appropriate Completion

完成度必须符合空间角色，而不是统一堆满：

- Focal 可以高信息量；
- Supporting 提供可读性与使用证据；
- Quiet 通过低密度构造 / 材质 / 光 / 边缘 / 维护状态保持完整。

> **A finished scene may be sparse, but it cannot feel unfinished.**

### 18.4 Mid / Near Resolution

至少检查：

- 中景是否能读出 facade / material / structure / lighting rhythm；
- 近景是否有可信 threshold、interface、furniture / props、surface 或其它与用途相关的完成信息；
- 关键区域是否仍停留在大平面 + 少量点状 detail 的状态。

### 18.5 Restraint Balance

Restraint 后重新问：

- 删除是否减少了噪声；
- 是否误删了支持空间身份与完成感的必要信息；
- 是否因为害怕 Over-detail 而系统性落入 Under-finish。

### 18.6 Evidence Standard

不能用以下证据单独通过此 Gate：

- 修改方块数量；
- Pass 已执行；
- 修改账本完整；
- Blueprint diff 正确；
- 几个局部 close-up 有变化。

优先使用真实 Minecraft 客户端玩家视角。如果没有客户端控制工具，使用当前工具能提供的最佳同机位前后透视、主要 route 视图与 Mid / Near 证据。

如果现有证据仍明显不足以证明玩家尺度完成，不得为了流程闭环自行宣告 `FINISHED`；保持 `FINISHING`，继续有针对性的 Finish Pass，或明确报告 `perceptual completion unverified`。

### Gate Result

- **PASS**：状态可从 `FINISHING` 升为 `FINISHED`；
- **FAIL — UNDER_FINISH**：保持 `FINISHING`，针对覆盖不足区域继续精修，再重跑本 Gate；
- **FAIL — UPSTREAM_BUILDER_ISSUE**：如果真正问题属于 Frozen Core，则回 Builder Core，修复后重新 Spatial Completion Gate。

此 Gate 不设最低 block count、最低家具数或“每面墙都必须变化”的配额。

---

# Layer D｜Verification & Repair

## 19. Verification Framework｜验证 invariants，不重复设计教材

“方块成功写入 / samples 全通过 / Blueprint 一致”只证明执行成功，不等于设计成功。

复杂 Scope 根据工具选择 top、height / contour、section、oblique / perspective、玩家关键视点、主要 route、skyline、collision / reachability。

如果能取得真实 Minecraft 客户端截图 / 玩家视点，应优先用于最终感知检查。软件体素渲染只能补充，不能替代真实纹理、光照、模型、FOV 与移动尺度体验。

---

## 20. Geometry / Construction Integrity

检查非设计意图造成的：

- floating / orphan geometry；
- disconnected 重型构件；
- 断裂 roof / wall / floor；
- 浮空植被；
- 地形 / 建筑交界缺支承或接口；
- 施工残留。

桥、拱、梁、悬挑、题材允许的魔法悬浮可以成立。目标是区分 intentional cantilever / suspension 与 unintended orphan geometry。

---

## 21. Interface & Circulation Integrity

### System Interface

检查：

- circulation ↔ architecture；
- terrain ↔ architecture；
- water ↔ architecture；
- vegetation ↔ circulation / architecture；
- structure ↔ open space；
- roof / facade ↔ internal volume；
- finishing ↔ frozen core。

后施工系统不得无意破坏前序系统。使用 `fill / clear / replace / carve` 等大范围写入时，尤其检查其包络是否跨入已完成对象。

### Space Graph / Local Expected Edge Validation

全局 `target reachable = true` 不能证明指定设计连接成立。

对重要 edge 检查：

- A 是否通过预期 portal / corridor / stair 到达 B；
- 是否必须绕行其它非预期路线；
- 垂直交通是否落到目标楼层；
- 是否需要异常跳跃、擦边或穿越错误空间。

优先采用受局部范围约束的 expected-edge reachability，不接受全局绕路代替指定连接。

### Portal / Threshold Integrity

重要门、门洞、拱廊入口、楼梯口与庭院接口检查：

- from-space / to-space；
- 两侧 floor continuity；
- threshold / landing / step / ramp；
- headroom / width；
- 门外是否立即撞柱、墙、栏杆、深坑；
- 是否真正贯穿所需 wall depth；
- 室外接口与地面、道路、排水、建筑基座是否成立。

> **Coordinate overlap is not architectural connection.**

### Roof / Section / Junction

重要主屋面、侧翼、塔、回廊、附属体量和高差接口检查：

- roof 是否覆盖对应内部空间；
- junction 是否完整；
- 不同 roofline 是否有结构 / section 原因；
- 是否存在屋顶切进室内、悬空壳、漏缝、错误穿插。

---

## 22. Semantic Integrity｜标签、几何和用途必须一致

足以被玩家读成建筑 / 景观语义的对象，应同时在几何和使用逻辑上成立。

例如：

- `pointed arcade` 不能只是方洞；
- “门”不能实际不可通行；
- “储物区”不能完全无法使用；
- “排水沟”应有合理去向；
- “结构柱”不能只是表面贴块；
- “入口”不能没有真正 threshold / route；
- “水池”不能只有蓝色平面而没有床岸逻辑。

> **Semantic label ≠ generated geometry.**

无需建立繁琐注册表，但明显对象必须能解释其角色。

---

## 23. Finishing Quality & Phase Protection

### Functional Obstruction

家具、props、植物和灯光不得堵门、楼梯、道路、关键视线或玩家净空。

### Semantic / Stylistic Fit

家具、材料老化、光源、props 和植物应符合空间用途、文化、时代、环境与使用者。

### Over-detail / Under-finish

检查：

- 是否所有墙面都同样复杂；
- 是否 clutter 过多、Focal 消失；
- 是否 Micro 破坏远景与空间层级；
- 关键空间是否反而仍像毛坯；
- 主要接口是否缺乏完成感；
- 是否只有几个局部精修点，而主要 route / 核心空间在玩家视角下几乎仍与 `SPATIAL_COMPLETE` baseline 相同；
- 是否把 Quiet 错误理解为 Untouched；
- 是否为了避免 Over-detail 而整体精修幅度低到玩家难以察觉。

### Repetition

避免同样桌椅、箱子、花盆、灯、墙面做旧纹样或家具组合高频机械复制。重复如果来自真实制度 / 模块化 / 队列逻辑，可以成立。

### Phase Protection

Finishing 不得未经回退流程修改 Frozen Core。条件允许时，在进入 Finishing 时记录当前 Scope 的关键边界 / Space Graph edges / roofline / major route / water / terrain / Structural Vegetation 等 baseline，并在结束后复核。

发现未经授权的核心损坏：标记 `UPSTREAM_DAMAGE`，回 Builder Core 修复并重新通过 Spatial Completion Gate。

---

## 24. Perceptual Review｜最终服务玩家体验

最终至少从以下层级复核当前 Scope，并在 Finishing 任务中尽量与 `SPATIAL_COMPLETE` baseline 使用同机位或可比较视角：

### Far

- silhouette；
- skyline；
- 主要体量 / canopy；
- 与环境关系；
- Micro 是否制造视觉噪声。

### Mid

- facade / material rhythm；
- openings；
- structure readability；
- Structural Vegetation；
- 主要 lighting / props / path edge；
- Finishing 是否对普通游览距离产生可读提升，而不是只能近贴单块观察。

### Near

- threshold；
- stairs；
- furniture；
- surface；
- construction interface；
- wear / props；
- Detail Vegetation。

### Route

沿主要 approach / entry / sequence / circulation 检查转折、开合、遮挡、释放、框景、回望、净空与空间身份。

Finishing 后还应问：如果不提供提示，沿这条 route 是否自然感到更多完成度、使用证据、材质深度和空间气氛？

### Thinking Consistency

回看 Architectural Intent / World Rules：最终作品是否仍服务 Purpose / Users，Site / Program / Hierarchy 是否真实进入空间，Structure / Material 是否生成形态，Finishing 是否放大而不是覆盖这些逻辑。

---

## 25. Repair Strategy｜小修、重构和阶段回退必须区分

门窗、材料、局部屋顶、家具、单株植物等小问题可以有界原地修补。

若发现以下问题，应回相应上游模块，而不是继续叠补丁：

- 主体量 / 主剖面错误；
- Space Graph 根本错误；
- Terrain / Water 骨架失败；
- 主 circulation 错误；
- Structural Vegetation 空间关系错误；
- Finishing 必须拆核心才能继续。

若只是 `UNDER_FINISH`，仍留在 Finishing：根据 Coverage Map 找到覆盖不足区域，优先补完成关系与玩家可读性，不得为了提高“幅度”盲目增加 clutter。

重构时：

1. 明确保留范围；
2. 明确拆除范围；
3. 基于清空状态重新规划；
4. 施工前检查新旧对象碰撞；
5. 重新执行对应 Gate。

不要在根本不合适的旧布局上无限叠加补丁。

---

# Layer E｜Delivery

## 26. Completion Levels｜交付时明确当前真实状态

合法完成状态：

### `SPATIAL_COMPLETE`

空间、结构、地形 / 水 / circulation 等 Core 已成立，可使用、可继续精修，但不声称展示级完成。

### `FINISHED`

已完成与 Scope 重要性匹配的玩家尺度精修，包括用途、建筑收口、材质时间层、光、Micro 与 Restraint，并且已经通过 **Finishing Completion Gate**：主要玩家体验范围具有足够 Coverage，正常游览能够感知从 Core 到成品的完成度提升，而不依赖提示去寻找零散修改点。

### `VERIFIED`

在目标状态基础上完成当前工具能够提供的 Integrity / Semantic / Perceptual Verification，并明确未验证边界。

不得用“写入成功”“蓝图一致”“全局可达”“执行过五个 Finishing Pass”“修改了很多方块”冒充更高完成状态。

---

## 27. Asset Candidate Gate｜复用资产必须真人批准

最终输出中，如本轮出现确有重复使用价值的原创资产，可列：

> **推荐入库资产候选清单**

至少说明临时名称 / ID、类型、尺寸、spatial role、当前位置 / 预览、推荐理由、类似现有资产、建议类别。

只有 Owner 明确 `批准入库` 后，才能进入正式提取、转换和注册任务。不得因为模型自行生成成功就自动写入正式资产库。

---

## 28. Autonomous Execution / Safety

模型应自主完成研究、设计、Builder Core、Gate、自检、Finishing、必要返工与交付，不需要在每个普通设计选择上停下来请示。

只有以下情况应停止并报告 blocker：

- 无法确认真实写入的目标世界 / 存档；
- 继续操作可能伤害用户真实世界；
- 工具不可用；
- 存档格式 / 版本无法安全处理；
- 任务存在真正无法自行消解的重大歧义。

检测和自动化应服务设计质量，而不是取代设计。不要为了“严谨”搭建远超任务需要的基础设施。

---

## 29. Canonical Workflow｜默认完整流程

除非任务明确要求其它顺序：

1. 确认世界、Scope、任务类型和目标完成等级；
2. 现实题材研究或幻想题材定义 World Rules；
3. 读取场地与现有系统；
4. 对重要建筑建立 Architectural Intent；
5. 找出当前 Scope 的一等系统和依赖；
6. 建立整体 Plan + Section + Sequence / Landscape spatial logic；
7. `CORE_BUILDING`：Macro 完成 Terrain / Water / Structural Vegetation / 主要空间 / 建筑 structural-spatial system 与主次体量；
8. 执行 Design / Massing / Section / Tectonic Gate；失败则先返工；
9. 完成 Terrain Morphology、Water semantics、Circulation 与 Minecraft scale 检查；
10. Meso 完成道路、桥、游廊、building bay/support、主要 openings、vertical circulation、roof junction、次级植被与 Core Facade；
11. 执行 Space Graph / Portal / Threshold / Roof-Junction / System Interface 检查；
12. Base Micro 只完成让空间成立所需的地表、材料、门窗收口、必要植被、照明和基础陈设；
13. 执行 Spatial Completion Gate；未通过则保持 Core，禁止进入 Finishing；
14. 通过后状态设为 `SPATIAL_COMPLETE`，冻结上游核心语义，并保存可比较的 pre-Finishing baseline；
15. 如果目标只需 `SPATIAL_COMPLETE`，进入 Verification / Delivery；否则进入 `FINISHING`；
16. 建立轻量 Finishing Coverage Map，识别 Focal / Supporting / Quiet 与主要玩家体验区域；
17. Finishing Pass 1：Functional Finish；
18. Pass 2：Architectural Finish；
19. Pass 3：Material / Environmental Finish；
20. Pass 4：Composition / Atmosphere；
21. Pass 5：Restraint；
22. 回看 Coverage Map，检查 Phase Protection，确认 Finishing 未破坏 Frozen Core；
23. 执行 Finishing Completion Gate；若 `UNDER_FINISH`，保持 `FINISHING` 并针对覆盖不足区域继续精修，再重跑 Gate；若发现 Frozen Core 问题，则回 Builder Core；
24. Gate 通过后，执行 Geometry / Interface / Circulation / Semantic / Water / Terrain / Vegetation invariants；
25. 做 Far + Mid + Near + Route 的 Perceptual Review，并尽量与 pre-Finishing baseline 做相同视点比较；能取得真实客户端视点时优先使用；
26. 对问题执行有界 Repair；如触及上游则回退相应状态并重新过 Gate；如只是精修不足则留在 Finishing；
27. 只有 Finishing Completion Gate 与必要 Integrity 检查都支持时，才标记 `FINISHED`；完成当前工具能够提供的最终验证后可标记 `VERIFIED`；
28. 明确仍未验证的客户端、流体、真实碰撞、夜间照明或其它边界；
29. 输出推荐入库资产候选；
30. 等待 Owner 实机检查；
31. 只有 Owner 明确批准的候选才能正式入库。
