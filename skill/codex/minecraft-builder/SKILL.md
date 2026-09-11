# minecraft-builder v1.7

## 0. 定位：先形成设计，再施工，最后验证

Minecraft 建造任务的目标不是“尽快把方块写进去”，而是形成一个在玩家视角下成立、可以进入和使用、空间关系清楚、题材逻辑一致的三维环境。

本 Skill 采用三层结构：

1. **Thinking｜先想清楚为什么这样设计**；
2. **Design & Construction｜把设计翻译成 Minecraft 空间并分阶段建造**；
3. **Verification & Repair｜检查实际结果是否兑现设计，并对缺陷做有界修订**。

不要把 QA 当成设计本身。一个作品即使“可达、无孤立方块、与蓝图一致”，仍可能是糟糕的建筑或景观。反过来，设计意图写得漂亮，也不能代替最终世界中的真实结果。

`minecraft-builder` 的职责是完成空间骨架、建筑/景观系统、主要 Micro 与必要收口，使作品达到**空间上成立、工程上完整、题材上可信**的状态。展示级家具、杂物、雕塑、极细装饰、全面室内陈设等不作为默认无限扩张目标；这类工作可交给后续独立 finishing/detail 阶段。

大型 / 复杂任务只保留一个主要设计目标。不要边想到什么边往世界里堆什么。

---

## 1. 先判断任务类型与证据要求

施工前先归入最接近的一类：

- `BUILDING`：单体或少量建筑为主；
- `SETTLEMENT`：村庄、街区、城镇、聚落系统；
- `LANDSCAPE`：园林、森林、湿地、山水、自然景观；
- `MIXED_ENVIRONMENT`：建筑与地形、水体、道路、植被共同构成主体。

`LANDSCAPE` 与 `MIXED_ENVIRONMENT` 中，Terrain、Water、Rock、Structural Vegetation、Circulation、Architecture 都应作为一等系统，而不是最后补装饰。

### 现实 / 历史题材

先参考可靠的历史、考古、建筑、园林、聚落、地理或景观资料。研究不是为了复制照片，而是提取会改变空间设计的事实，例如：

- 选址与朝向；
- 使用者与功能；
- 空间关系；
- 交通与礼仪；
- 平面、剖面和建筑类型；
- 地形与水文；
- 材料、结构与建造技术；
- 植被、农业、防御、生产或宗教逻辑。

对重要 `BUILDING` 任务，不要只依赖“这种建筑有什么特征”的文字描述。条件允许时，至少取得一项可信的 **plan / section / elevation / measured typology** 参考，用于约束比例、跨间、层高、屋顶与结构关系。

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

# Layer A｜Architectural & Spatial Thinking

## 2. Architectural Thinking Kernel｜重要建筑先建立因果模型

对 `BUILDING`、`SETTLEMENT` 中的重要建筑、以及 `MIXED_ENVIRONMENT` 中承担主要空间作用的建筑，放置第一个建筑方块前先经过以下思维链。

### 2.1 Purpose & Users｜为什么建、给谁用

先回答：

- 为什么需要这座建筑？
- 谁建它、谁使用它？
- 使用者的身份、数量和主要活动是什么？
- 它在聚落、组织或文明中承担什么角色？
- 最重要的现实需求、礼仪需求或象征需求是什么？

建筑首先解决人的需求，不是先得到一个外观再往里塞功能。

### 2.2 Site & Context｜为什么建在这里

建筑必须属于它所在的地方。主动读取并利用：

- 地形、坡向、山脊、谷地、河流、水源；
- 道路、街道、港口、城墙、田地；
- 周边建筑、公共空间和聚落结构；
- 视线、景观、防御与礼仪关系；
- 日照、风雨、湿度、排水等环境条件。

优先 **fit architecture to context**。不要把建筑当成可以任意平移的独立模型，再靠场地去迁就它。

### 2.3 Program & Spatial Relationships｜先有活动与空间，再有墙

先识别实际活动，再形成空间：

- 核心空间；
- 次要空间；
- 服务 / 后勤空间；
- 公共 / 私密；
- 礼仪 / 日常；
- 生产 / 储藏；
- 洁净 / 污染；
- 安静 / 嘈杂；
- 人流 / 货流 / 防御流线。

然后明确：

- 哪些必须相邻；
- 哪些应隔离；
- 谁从哪里进入；
- 一个活动结束后去哪里；
- 哪些空间共享庭院、廊道、楼梯、门厅或服务核心。

建筑的设计单位应优先理解为 **space / room / hall / courtyard / circulation / service zone**，而不是先调用 `box()` 再掏空。

复杂建筑建议先形成轻量 **Space Graph**：节点是重要空间，边是必须成立的连接关系。

### 2.4 Hierarchy & Sequence｜主次关系与人的体验顺序

判断：

- 什么是建筑核心；
- 哪些空间等级最高；
- 主要入口在哪里；
- 哪些空间服务于主空间；
- 玩家 / 使用者应该如何逐步接近核心。

可通过尺度、高度、位置、光线、开敞度、体量、轴线、门厅、前室、庭院和转折表达等级。

同时设计真实体验序列：

> approach → threshold → entry → transition → main space → secondary space → continuation / exit

“最终可达”只证明能走到；Sequence 解决为什么这样走、经过什么、何时压缩或释放。

### 2.5 Plan + Section + Environmental Logic｜水平与垂直同时设计

不要先画二维 footprint 再统一拉墙高。

从早期同时考虑：

- 空间需要多高、多宽；
- 不同功能为何高度不同；
- 楼层如何叠放；
- 楼梯和坡道如何穿越不同层；
- 屋顶覆盖什么跨度；
- 窗在什么高度才服务采光、视线、通风或防御；
- 地坪怎样与外部地面连接；
- 雨水、遮阳、排水、坡度和地形如何影响剖面。

复杂项目至少同时建立：

- **Plan**：水平关系；
- **Section**：垂直关系；
- **Sequence**：人的时间性体验。

### 2.6 Structure, Material & Technology｜先理解怎么建得出来

先判断题材合理的基本建造体系，例如：

- 承重墙；
- 木柱梁；
- 石柱与拱券；
- 肋拱；
- 穹顶；
- 框架；
- 扶壁；
- 木屋架；
- 其它与时代 / 文明相符的体系。

材料与技术不是换色皮肤。它们应反过来约束：

- 跨度；
- 墙厚；
- 柱距；
- 开口尺寸；
- 楼层高度；
- 屋顶形式；
- 建筑能建多高；
- 构件如何连接。

核心原则：

> **Structure generates architecture.**  
> **Material changes geometry.**

对于依赖结构体系的建筑，不得先完成通用 shell，再把尖拱、扶壁、梁柱、穹顶等风格构件覆盖上去。

### 2.7 Form as Consequence｜形态是因果链的结果

建筑的 Massing、Silhouette、入口、屋顶、开口与立面，应由以下条件共同产生：

> Purpose + Site + Program + Hierarchy + Section + Structure + Material + Culture

塔、侧翼、门楼、庭院、退台、穹顶、巨大窗、素墙、高低变化都需要设计理由。

**Form is consequence, not decoration.**

不要为了“反方盒”无理由制造复杂轮廓，也不要把所有不同功能都压缩成相似 box，再只靠门窗、材料和表面构件区分。

### 2.8 Minecraft Translation｜最后才把建筑转译成方块

Minecraft 不是现实建筑的低清晰度导出格式，而是需要重新设计的媒介。

优先保留：

- 比例关系；
- 空间等级；
- 结构可读性；
- 主要轮廓；
- 玩家视角下的体验；
- 重要构造关系。

主动判断：

- 哪些现实细节在方块分辨率下必须适当放大；
- 哪些现实距离过长需要压缩；
- stairs / slabs / walls / fences / trapdoors 等如何服务几何表达；
- 玩家眼高、移动速度、FOV 和视距下空间是否成立；
- 近景 / 中景 / 远景分别应该读出什么。

不要机械 1:1 复制现实绝对尺度。

### Architectural Intent｜重要建筑的轻量设计摘要

重要建筑正式施工前，简短记录以下八项，每项一两句话即可：

- Purpose；
- Users / Activities；
- Site / Context；
- Program / Space Graph；
- Hierarchy / Sequence；
- Plan + Section；
- Structure / Material / Technology；
- Form + Minecraft Translation。

目的是强迫设计因果显式化，不是制造长篇文档。普通小建筑可简化，不必过度工程化。

---

# Layer B｜Design & Construction

## 3. 每个重要设计决定都要有因果逻辑

不要只问“这里还能放什么”，应问：

- 为什么这个空间在这里？
- 它服务谁？
- 它与地形、水、道路、建筑、植被有什么关系？
- 玩家如何到达、经过、停留、回望？
- 空白为什么存在？
- 高差为什么存在？

开放空间和留白可以很大，但必须有尺度、功能、生态、视线或构图原因。禁止为了“丰富”把每块空地都填满，也禁止把大面积无意义空白当作完成。

## 4. 避免无意识的现代规划

除非题材明确需要，否则不要默认使用：

- 直角道路网；
- 等间距建筑；
- 同尺寸地块；
- 统一退界；
- 镜像复制；
- 网格绿化；
- 郊区式“房子散在草坪上”。

规则性本身不是错误。礼仪轴线、果园、行道树、军营、正式庭院、防风林等本来就可以规则。禁止的是没有历史、功能或空间理由的规则性。

## 5. Terrain｜尊重自然地形；测试平地也必须主动造地形

真实地形存在时，优先读取和利用现有山脊、坡面、谷地、河流、洼地、台地与视线。

在超平坦 / 测试画布中，如果原型依赖地形、水体、山体、台地、洼地或明显高差，必须主动创建这些条件。“尊重地形”不等于保持平坦。

### Anti-Forced-Elevation

Anti-Flatness 不意味着必须人为抬高每个建筑，也不意味着把场地切成一系列独立规则平台。

高差应优先来自：

- 连续地形；
- 河谷、山脊、坡地、台地、洼地；
- 防洪、排水、防御、视线、等级、生产或交通功能；
- 建筑与地形真实的适应关系。

建筑应优先 **fit building to terrain before forcing terrain to building**。

局部找平、基座、挡土墙、台阶和填挖方可以成立，但必须有具体理由并与周围坡面形成可信过渡。不要先任意指定建筑标高，再用巨大石台或整块填方强迫地形服从。

### Terrain-Conforming Circulation

山路、坡道、台阶和其它主要用于克服自然高差的室外 circulation，默认应顺应、切入或依托地形。

优先考虑：

- 沿等高线或缓坡绕行；
- switchback；
- 局部 cut / fill；
- 短距离挡土墙；
- 贴坡台阶；
- 利用鞍部、沟谷、坡肩和台地。

不要先定义理想三维路径，再从地面向上填成长墙、长石台或实体坡道。

桥梁、城防墙梯、栈道、架空连廊、码头等有明确工程理由时可例外，但结构身份、起讫接口和支承逻辑必须清楚。

### Natural Terrain Morphology

自然地形不能只是连续平行的 contour terrace / 蛋糕式台阶。

主动造地形时，应通过有因果的地貌特征形成变化，例如：

- ridge；
- swale；
- gully / erosion；
- shoulder；
- cliff；
- talus；
- rock exposure；
- soil / vegetation pocket；
- 局部缓坡与陡坡转换。

目标不是给高度场加入随机噪声，而是让地形读起来像由地貌过程形成。

### 现有环境处理

现有随机植被可为新的整体设计清理后重植；不要因为“原本就在这里”而保留破坏空间结构的随机树草。

## 6. Water｜真实水体必须符合 Minecraft 流体语义

Minecraft 水不是静态蓝色体素。只要使用真实 `water` 方块，就必须把 Vanilla fluid behavior 作为设计约束。

同时考虑：

- channel bed / pool bottom；
- bank / wall containment；
- source 与 flowing water；
- lateral spill；
- downstream drop；
- inlet / outlet / overflow；
- 桥墩、岸脚、建筑和水轮等接口；
- 邻接更新后是否保持预期形态。

禁止把多层 `water[level=0]` 静态写成阶梯水带，然后因为几何连通就认为河道成立。

自然河溪默认先塑造稳定河床与岸线，再放水。

如果工具允许，应触发真实 fluid update / 邻接更新并重新读回；如果工具不允许：

1. 使用保守、明显有床岸约束的几何；
2. 标记 `fluid stability unverified`；
3. 不得用 connected component、写入成功或静态截图代替流体稳定性。

水体 QA 必须区分：

- geometry connectivity；
- water-level logic；
- bank / bed containment；
- fluid-update stability。

## 7. Vegetation & Ground｜植被和地表是空间系统

### Structural Vegetation

大型乔木、树群、林带、竹林、树林、绿篱等可承担：

- 围合；
- 遮挡；
- 框景；
- 背景 / 前景；
- 路径引导；
- 水岸软化；
- 高低过渡；
- canopy / skyline；
- 视觉焦点。

因此 Structural Vegetation 属于 Macro / Meso，应与地形、水体、建筑共同规划。

### Detail Vegetation

花、草、小灌木、荷叶、藤蔓、地被等属于 Micro，但不是可有可无。

适合题材时主动检查目标运行时可用的植物 palette，如短草 / 高草、蕨、苔藓、蒲公英、罂粟、郁金香、矢车菊、滨菊、铃兰、绒球葱、兰花等。它们只是语言示例，不要求每个场景都使用。

### Layered Vegetation

乡野、森林、湿地、河岸、园林等自然环境应按语境形成适当层级：

- canopy；
- understory；
- herbaceous；
- groundcover；
- seasonal / flowering。

不要求全部同时存在，但自然环境通常不应只是大片 `grass_block` + 少量孤立乔木或零星花。

密度随职责变化：

- forest edge 较密且不规则；
- riverbank 连续但有开口；
- meadow 以草本和地被为主；
- settlement core 可因踩踏、放牧而稀疏；
- farmland / orchard / flowerbed 受人工管理。

### Flowering Community

允许花卉出现时，不要只放极少量单一花种象征“有花”。

选择少数相容 species / colors，形成：

- substantial patch；
- ribbon；
- clearing carpet；
- 林缘 / 草甸 / 道路 / 田埂 / 水边 / 庭院边缘群落；
- 与草本、蕨、地被共同出现的混合层。

### Flower Abundance Bias

对 lush forest / enchanted forest / sacred grove / ornamental garden / spring-summer meadow 等强调繁盛、花季、仙境感的题材，如果生态 / 世界规则没有反对理由，花层应在玩家尺度上明显可见。

默认允许多个有视觉分量的花群区域，宁可第一版明显丰富再局部削减，也不要长期停留在象征性下限。

“更多”仍不等于：

- 全图均匀撒花；
- 集齐所有颜色；
- 覆盖全部道路和林下；
- 用花替代草、蕨、灌丛、苔藓和 Ground Plane。

采用 **dense patch + sparse transition + open gap**。

荒漠、严寒高地、裸岩、深暗闭合林下、重度踩踏 / 放牧、城防净空等可保持低花量。

### Ground Plane

`grass_block` 不能自动被视为完成地面。

根据空间职责判断这里是：

- 草坪 / 草坡；
- 林下地表；
- 苔藓；
- 裸土 / 粗土；
- 碎石 / 砾石；
- 铺装；
- 农田 / 花圃；
- 水岸过渡；
- 其它题材合理地表。

大面积纯草地可以成立，但必须是有尺度、功能、生态或构图理由的设计结果。

### 自然式植被 = 受约束的不规则

自然式环境默认禁止无理由的等间距、网格、镜像、统一株高 / 冠幅、同一树型高频复制、直线切林缘。

但纯随机撒点同样不合格。

优先通过：

- cluster；
- density gradient；
- irregular edge；
- opening；
- hierarchy；
- overlap；
- asymmetry；
- terrain response；
- sightline response。

果园、行道树、宫殿礼仪轴线、规则庭院等有明确人工秩序时可规则排列。

### 植被尺度

Minecraft 原生小树不是默认景观树答案。

承担遮挡、背景、框景、canopy 或 skyline 的乔木应根据建筑和地形主动决定高度、冠幅、主干、分枝、倾斜和不对称程度。

尺度至少检查：

```text
player → building → terrain / landscape → canopy / skyline
```

## 8. Architecture｜空间与构造系统先于外壳

### Space Graph → Section / Structure → Envelope

复杂建筑优先按以下顺序生成：

> Program / Space Graph  
> → Plan + Section  
> → Structural / Tectonic System  
> → Circulation  
> → Massing / Roofline  
> → Envelope / Openings  
> → Facade / Material  
> → Detail

不要把“完整矩形 shell → 掏空 → 后贴风格构件”作为重要建筑的默认生成策略。

矩形体、盒体和参数化函数可以作为实现原语，但必须服务于已经成立的空间、剖面与结构逻辑，不能反过来决定建筑。

### Massing & Silhouette

建筑可以矩形、方正、封闭或高度正交。问题不是“方”，而是不同功能全部退化为相似 box，再靠表面装饰制造差异。

体量应由功能、结构、剖面、场地与等级产生。

主动判断：

- primary / secondary mass；
- entrance mass；
- vertical hierarchy；
- recess / projection；
- courtyard / wing / tower / service mass；
- roofline / skyline；
- section logic；
- terrain / street / courtyard / water interface。

### Massing Gate｜进入 Meso 前验证的不只是“看得出来”

对于重要建筑，Macro 后进入 Meso 前执行 Massing Gate。

Gate 不能仅以“能辨认中殿、侧翼、塔”等 typological legibility 作为通过依据。至少检查：

- proportion / 比例是否成熟；
- volumetric composition / 主次体量组合是否成立；
- structure / section 是否真正驱动外形；
- major mass junctions 是否解决；
- roof transition 是否合理；
- entrance hierarchy 是否清楚；
- negative space / courtyard / gap 是否有意；
- secondary masses 是否也具有与功能相符的质量；
- 暂时忽略材质与小构件后，整体 silhouette 是否仍成立。

如果失败，先重做体量 / 剖面 / 构造，不进入 facade / material。

### Reusable Generator ≠ Repeated Morphology

代码复用、模板和蓝图是实现手段，不是设计理由。

功能、等级、地形关系或空间职责明显不同的对象，不应只通过同一 generator 改长宽高 / 换材料制造差异。应检查：

- 体量组合；
- roofline；
- 开口节奏；
- 结构方式；
- 入口关系；
- 地形 / 道路 / 庭院接口；
- skyline 权重。

回廊、柱列、军营、行列住宅等原型本来依赖重复时可有意识重复。

### Controlled Material Language

“材质丰富”不等于随机混很多方块。

形成清楚的材料层级：

- primary material；
- secondary material；
- structural accent；
- transition / weathering。

目标运行时提供合适 stairs / slabs / walls / fences / trapdoors 等时，可服务屋顶、檐口、墙脚、开口、岩体和铺地的几何层次，但不要无意义堆零件。

### Material Placement Logic

材料变化应尽量对应构造、使用、年代或环境：

- foundation / wall base；
- corner / opening surround；
- primary wall field；
- repair / weathering / damp zone；
- roof ridge / eave / edge；
- paving transition。

不要因为生成方便把一种材料整面铺满，也不要用均匀概率 noise 随机混材伪造丰富度。

### Facade & Surface Articulation

大型建筑完成主次体量后，在玩家近中距离检查是否只剩“巨大平面 + 方洞”。

根据题材和构造逻辑形成适度 depth / shadow / rhythm，例如：

- base / plinth；
- corner treatment；
- door / window surround；
- buttress / pilaster / beams；
- floor line；
- recess / projection；
- eave / cornice；
- roof ridge；
- parapet / battlement。

不是每面墙都必须复杂。防御素墙、仓库、极简建筑都可克制；关键是几何层次有构造与比例理由。

## 9. Assets｜优先复用，但候选不得自动入库

可以积极复用 Litematica、Blueprint 或已有资产库，但资产必须满足：

- 功能；
- 尺度；
- 地形接口；
- 风格；
- 空间职责；
- 视觉权重。

大型乔木、古树、竹丛、林带、假山、小型构筑物等高价值资产：

1. 先查已有蓝图库；
2. 有合适 variant 时优先复用；
3. 没有时可为当前任务设计 candidate；
4. candidate 可用于当前场景验证；
5. 不得自动写入正式资产库。

最终输出可增加：

> **推荐入库资产候选清单**

至少说明临时名称 / ID、类型、尺寸、用途、当前位置 / 预览、推荐理由、类似现有资产、建议类别。

只有 Owner 明确批准后，才能进入正式提取 / 转换 / 注册任务。

## 10. Macro → Gate → Meso → Micro

### Macro

先解决：

- 场地与地形骨架；
- 大水体；
- 主次空间；
- 总体高度关系；
- 大型 Structural Vegetation；
- 重要建筑的 Program / Space Graph；
- 建筑 Plan + Section + Structural System；
- 主次体量与主要 roofline；
- 主要视觉焦点。

### Gate

重要建筑执行 Massing / Section / Tectonic Gate。景观项目执行与阶段目标相应的 Terrain / Water / Vegetation 检查。

失败就先返工，不得靠后续细节掩盖。

### Meso

再解决：

- 道路 / 游廊 / 桥；
- 次级山水；
- 建筑之间关系；
- 林带、竹林、树群；
- 建筑 bay / support / roof junction；
- 入口、主要门洞和垂直交通；
- 立面 / 梁柱；
- 视线与空间开合。

### Micro

最后处理：

- 门窗收边；
- 栏杆；
- 活板门；
- 花草；
- 小灌木；
- 地被与 Ground Plane 细化；
- 材料 placement；
- 铺地变化；
- 小型点景；
- 基础陈设与照明。

不得在 Macro / Meso 尚未成立时，用 Micro 掩盖平板、空旷、网格化、比例失衡、体量失败或构造问题。

## 11. 小修补与整体重构采用不同策略

门窗、材料、局部屋顶、单株植物等小修改可以原地修补。

若城堡内院、村庄核心、园林山水骨架、主水体、空间序列、植被围合关系、建筑主剖面或主要体量发生根本变化，应优先有边界地清空后重构：

1. 明确保留范围；
2. 明确拆除范围；
3. 基于清空后的状态重新规划；
4. 施工前检查新旧结构和新对象之间的碰撞。

不要持续在根本不合适的旧布局上叠加补丁。

---

# Layer C｜Verification & Repair

## 12. 分阶段自检，但自检不能只看施工成功

“方块成功写入 / samples 全通过 / 与 Blueprint 一致”只证明执行成功，不等于设计成功。

复杂项目可根据工具选择：

- top view；
- height / contour；
- section / profile；
- oblique / perspective；
- 玩家关键视点；
- 主要游览路线；
- skyline；
- collision / reachability。

不得只用顶视图判断三维空间。

如果能取得真实 Minecraft 客户端截图 / 玩家视点，应优先用于最终感知检查。软件体素渲染只能作为补充，不能替代真实纹理、光照、方块模型、FOV 与尺度体验。

## 13. Architecture Integrity｜验证建筑关系，而不只是全局可达

### Space Graph / Circulation Edge Validation

全局 `target reachable = true` 不能证明每一条设计连接都成立。

对重要建筑，验证 Architectural Intent / Space Graph 中的关键边：

- 指定空间 A 是否能通过预期入口 / 廊道 / 楼梯到达空间 B；
- 是否必须绕行其它非预期路线；
- 垂直交通是否真实落到目标楼层；
- 路径是否需要异常跳跃、擦边或穿越错误空间。

### Portal / Threshold Integrity

重要门、门洞、拱廊入口、楼梯口、庭院接口等，检查：

- from-space / to-space 是否明确；
- 两侧 floor continuity 是否成立；
- threshold / landing / step / ramp 是否连续；
- headroom / width 是否合理；
- 门外是否立即撞柱、墙、栏杆、深坑或其它构件；
- 门洞是否真正穿透所需墙体层；
- 室外接口是否与地面、道路、排水和建筑基座合理衔接。

**Coordinate overlap is not architectural connection.**

### Opening Grammar

建筑 envelope 中足以被玩家读成开口的 void，应能解释其角色，例如：

- door / portal；
- window；
- arcade；
- stair void；
- service opening；
- intentional ruin / breach；
- 其它明确用途。

不要求建立繁琐注册表，但不得让随机 `air` carving 产生无意义洞口。

如果声称是 arcade / colonnade，应从玩家视角读出连续的 column / pier + arch / lintel + bay rhythm，而不是只在代码注释里叫“尖拱列”。

### Roof / Section / Junction Integrity

重要体量交接必须在 Gate / Meso 阶段解决，而不是拖到 Micro 才发现：

- main volume ↔ side aisle；
- main block ↔ transept / wing；
- tower ↔ crossing / roof；
- building ↔ cloister / attached mass；
- roof ↔ internal volume；
- roof ridge / valley / eave / flashing-like junction；
- floor / stair / landing；
- wall thickness ↔ opening。

检查是否有屋面穿进室内、半格缝隙、悬空屋面、错误封顶或大面积不合理重叠。

## 14. Construction Integrity Sweep｜施工完整性

阶段完成和最终交付前，主动检查非设计意图造成的：

- floating / 悬空残片；
- isolated / disconnected 几何；
- 岩体、挡墙、台地边缘或建筑构件意外断开；
- 浮空植被或施工残留；
- 地形 / 建筑交界缺少应有支承或接口。

不是要求模拟现实物理，也不是要求每个方块直接接触下方。桥、拱、梁、挑檐、悬挑平台、题材允许的魔法悬浮都可成立。

目标是区分：

> intentional cantilever / suspension  
> vs  
> unintended orphan geometry

## 15. System Interface Integrity｜后施工系统不得破坏前序系统

重点检查：

- circulation ↔ architecture；
- terrain ↔ architecture；
- water ↔ architecture；
- vegetation ↔ circulation / architecture；
- structure ↔ open space；
- roof / facade ↔ internal volume。

道路、地形整形、水体、植被或后期装饰需要修改既有建筑、墙体、屋顶、楼板、入口、桥梁等关键结构时，必须确认是明确设计意图。

道路只有在门洞、城门、拱廊、内部街道等明确关系下才可进入建筑 envelope；不得为了获得净空直接清除住宅墙体。

使用 `fill / clear / replace / carve` 等大范围写入时，尤其检查包络是否跨入已完成对象。

## 16. Building Envelope Integrity

主要建筑最终重新检查：

- roof；
- exterior walls；
- floor / foundation interface；
- intended openings；
- entrances；
- vertical circulation。

区分 intentional openness 与 accidental destruction。谷仓、敞廊、门洞可以开放，但不能把后续施工造成的缺口误判成设计。

## 17. Final Spatial Review｜最终检查真实空间质量

大型 / 复杂 / Landscape / Mixed 项目交付前至少检查：

### Anti-Flatness

- 是否因为方便把大部分空间压在同一标高；
- 原型要求的山、水、高差和树冠层次是否真实存在；
- 是否为了“有高差”反而制造无因果平台或强制抬高。

### Terrain Morphology & Circulation

- 是否存在长距离连续平行等高台阶或数学 heightfield 痕迹；
- 是否有可读 ridge / valley / gully / shoulder / cliff / talus；
- 室外山路、坡道、台阶是否依托坡面；
- 脱离地形的桥、栈道、城防梯是否有明确工程理由。

### Water / Fluid Stability

- 水体是否不仅几何连通，而且床岸、水位和流体规则成立；
- 是否存在开放 source water 静态水带；
- 多级水位是否有真实跌水 / overflow；
- inlet / outlet 是否有去向；
- 能执行 fluid update 时更新后是否稳定；
- 不能验证时是否明确标记未验证。

### Anti-Grid & Space Use

- 道路、地块、自然式植被是否无理由方正、等距、镜像；
- 是否存在意义不明的大面积空白；
- 是否为了“丰富”反而过度填充。

### Vegetation Structure

- Structural Vegetation 是否真正参与空间组织；
- 是否形成 canopy / understory / herbaceous / groundcover 层次；
- 是否只是孤立大树 + 少数花草；
- lush / enchanted / sacred / ornamental 等题材花层是否在玩家尺度上足够可见。

### Scale & Minecraft Translation

- player / building / terrain / canopy 比例是否匹配；
- 现实尺度是否机械 1:1 转译导致过空或过密；
- 方块分辨率下关键构造是否仍可读。

### Sequence

- 玩家移动中是否有合理 approach、threshold、转折、开合、遮挡、释放、框景、回望与主次转换。

### Architectural Thinking Consistency

回看 Architectural Intent：

- 最终建筑是否仍服务最初 Purpose / Users；
- Site / Program / Hierarchy 是否真实进入平面和剖面；
- 结构与材料是否真的生成建筑，而不是后贴风格；
- Form 是否能从前述因果链解释；
- 是否出现“文档说一套、几何做另一套”。

### Architectural Massing & Silhouette

暂时忽略材质、窗户和小构件：

- primary / secondary mass 是否清楚；
- 比例、组合和主要 junction 是否成熟；
- entrance hierarchy 是否成立；
- roofline / skyline 是否与 section / structure 对应；
- secondary masses 是否也有功能对应的质量；
- 是否仍只是多个 primitive box；
- 是否为了反 box 无理由复杂化。

### Architectural Surface Quality

- 大墙面是否只有单一材料平面和方洞；
- palette 是否通过构造、环境、风化逻辑真正落到表面；
- 近中距离是否有题材合理的墙脚、转角、门窗、梁柱、扶壁、退进、檐口、屋脊；
- 是否反过来随机堆构件或混材。

### Construction / System / Envelope

- 是否有非设计意图的悬空、孤立、断裂；
- 后施工系统是否破坏前序系统；
- 门、楼梯、屋顶、墙、地坪和主要 opening 是否完整。

如果 Macro / Meso 层面失败，应优先重建相关区域，不得仅靠 Micro 装饰掩盖。

## 18. 自主完成，但不要过度工程化

模型应自主完成研究、设计、施工、自检、必要返工和最终交付，不需要在每个普通设计选择上停下来向用户请示。

只有以下情况应停止并报告 blocker：

- 无法确认真实写入存档；
- 继续操作可能伤害用户真实世界；
- 工具不可用；
- 存档格式 / 版本无法安全处理；
- 任务存在真正无法自行消解的重大歧义。

检测和自动化应服务设计质量，而不是取代设计。

## 19. 默认工作顺序

除非任务明确需要其它顺序，默认：

1. 理解任务并判断 BUILDING / SETTLEMENT / LANDSCAPE / MIXED_ENVIRONMENT；
2. 做必要现实研究或定义幻想世界规则；
3. 读取场地 / 当前世界；
4. 对重要建筑先建立 Architectural Intent：Purpose → Users → Site → Program / Space Graph → Hierarchy / Sequence → Plan + Section → Structure / Material / Technology → Form / Minecraft Translation；
5. 找出整个项目不可缺少的一等空间系统；
6. 形成整体 Plan + Section + Sequence；
7. Macro：先完成 Terrain / Water / Structural Vegetation / 主要空间，以及重要建筑的 structural-spatial system、主次体量和主要 roofline；
8. 重要建筑执行 Massing / Section / Tectonic Gate；检查比例、组合、主要 junction、roof transition、结构因果和 secondary masses；失败则先返工；
9. 检查 Terrain Morphology、Anti-Forced-Elevation 与 Terrain-Conforming Circulation；
10. 有真实水体时检查 bed / bank / water level / inlet-outlet / fluid stability；
11. 检查 player / building / terrain / canopy 与 Minecraft 尺度转译；
12. 进入 Meso：完成道路、桥、游廊、建筑 bay/support、主要 opening、垂直交通、roof junction、次级植被与空间开合；
13. 做 Space Graph / Portal / Threshold / Roof-Junction 等建筑关系检查；
14. 每个后续阶段复核 System Interface Integrity；
15. Micro：完成 Ground Plane、Layered / Flowering Vegetation、材料 placement、立面收口、铺地、基础陈设与其它小细节；
16. 做 Top + Section + Perspective / Route 审核；能取得真实客户端视点时优先使用；
17. 做 Construction Integrity + Building Envelope + System Interface Sweep；
18. 做 Final Spatial Review：Terrain / Water / Vegetation / Scale / Sequence / Architectural Thinking Consistency / Massing / Surface / Portal & Circulation；
19. 对 Macro / Meso 缺陷做有界重建，而不是装饰掩盖；
20. 完成世界；
21. 输出推荐入库资产候选清单；
22. 等待 Owner 实机检查；
23. 只有 Owner 明确批准的候选才能正式提取入库。
