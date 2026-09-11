# minecraft-builder v1.6

## 1. 核心目标：先设计完整空间，再分阶段建造

Minecraft 建造任务的目标不是“尽快把方块写进去”，而是形成一个在玩家视角下成立、可以进入和使用、空间关系清楚、题材逻辑一致的三维环境。

大型 / 复杂任务只保留一个主要设计目标；先形成整体方案，再分阶段施工。不要边想到什么边往世界里堆什么。

## 2. 先判断任务类型

施工前先把任务归入最接近的一类：

- `BUILDING`：单体或少量建筑为主；
- `SETTLEMENT`：村庄、街区、城镇、聚落系统；
- `LANDSCAPE`：园林、森林、湿地、山水、自然景观；
- `MIXED_ENVIRONMENT`：建筑与地形、水体、道路、植被共同构成主体。

`LANDSCAPE` 与 `MIXED_ENVIRONMENT` 中，Terrain、Water、Rock、Structural Vegetation、Circulation、Architecture 都应作为一等系统，而不是最后补装饰。

## 3. 每个重要设计决定都要有因果逻辑

不要只问“这里还能放什么”，应问：

- 为什么这个空间在这里？
- 它服务谁？
- 它与地形、水、道路、建筑、植被有什么关系？
- 玩家如何到达、经过、停留、回望？
- 空白为什么存在？
- 高差为什么存在？

开放空间和留白可以很大，但必须有尺度、功能、生态、视线或构图原因。禁止为了“丰富”把每块空地都填满，也禁止把大面积无意义空白当作完成。

## 4. 现实 / 历史题材先做必要研究；幻想题材先定义世界规则

对于现实、历史、写实题材，先参考可靠的历史、考古、建筑、园林、聚落、地理或景观资料。研究不是为了复制照片，而是提取会改变空间设计的事实：

- 选址；
- 功能关系；
- 交通；
- 地形与水文；
- 建筑类型；
- 材料和结构；
- 植被 / 农业 / 防御 / 礼仪逻辑。

对于幻想题材，至少先定义：

- 使用者 / 文明；
- 环境；
- 材料与建造技术；
- 必要时的魔法规则；
- 这个社会真正需要的空间系统。

不必写长篇世界观，但不能无规则拼贴。

## 5. 避免无意识的现代规划

除非题材明确需要，否则不要默认使用：

- 直角道路网；
- 等间距建筑；
- 同尺寸地块；
- 统一退界；
- 镜像复制；
- 网格绿化；
- 郊区式“房子散在草坪上”。

规则性本身不是错误。礼仪轴线、果园、行道树、军营、正式庭院、防风林等本来就可以规则。禁止的是没有历史、功能或空间理由的规则性。

## 6. 尊重自然地形；测试平地也必须主动造地形

真实地形存在时，优先读取并利用现有山脊、坡面、谷地、河流、洼地、台地与视线。

在超平坦 / 测试画布中，如果原型依赖地形、水体、山体、台地、洼地或明显高差，必须主动创建这些条件。

“尊重地形”不等于保持平坦。

### Anti-Forced-Elevation｜禁止为了高差而制造高差

Anti-Flatness 不意味着必须人为抬高每个建筑，也不意味着把场地切成一系列互相独立的规则平台。

高差首先应来自：

- 原有或主动塑造的连续地形；
- 河谷、山脊、坡地、台地、洼地等自然地貌；
- 防洪、排水、防御、视线、等级、生产或交通功能；
- 建筑与地形真实的适应关系。

建筑应优先 **fit building to terrain before forcing terrain to building**。

局部找平、基座、挡土墙、台阶和填挖方可以成立，但必须有具体理由，尺度应与功能匹配，并与周围坡面形成可信过渡。不要先任意指定建筑标高，再用巨大石台或整块填方强迫地形服从建筑。

### Terrain-Conforming Circulation｜室外交通优先顺应地形

山路、坡道、台阶和其它主要用于克服自然高差的室外 circulation，默认应顺应、切入或依托地形，而不是作为独立高架构筑物悬在坡面之外。

优先考虑：

- 沿等高线或缓坡绕行；
- switchback / 折返；
- 局部 cut / fill；
- 短距离挡土墙；
- 贴坡台阶；
- 利用天然鞍部、沟谷、坡肩和台地作为路线节点。

不要先指定一条理想化的三维路径，再为了达到目标标高从地面向上填成连续高墙、长石台或巨型实体坡道。

桥梁、城防墙梯、栈道、架空连廊、码头等如果确有明确工程或空间理由，可以脱离自然坡面，但应让其结构身份、起讫接口和支承逻辑清楚。

### Natural Terrain Morphology｜自然地形不能只是连续等高台阶

主动造地形时，不要把“有高差”简单实现为一层层连续、平行、长距离延伸的 contour terrace / 蛋糕式台阶。

自然山体、河谷和坡面应通过有因果的地貌特征形成变化，例如：

- ridge / 凸脊；
- swale / 凹谷；
- gully / 沟槽与冲蚀；
- shoulder / 坡肩；
- cliff / 局部陡坎；
- talus / 坡脚碎石或堆积；
- rock exposure / 裸岩带；
- soil pocket / 植被与土壤口袋；
- 局部缓坡与陡坡转换。

目标不是给高度场加入随机噪声，而是让地形读起来像由地貌过程形成，而不是数学函数的可视化等高线。

### Minecraft Water / Fluid Semantics & Stability｜水体必须符合实际游戏流体规则

Minecraft 中的水不是静态蓝色体素。只要使用真实 `water` 方块，就必须把 Vanilla fluid behavior 作为设计约束，而不能只保证视觉几何或六邻接连通。

设计河流、溪流、水池、渠道、瀑布、跌水和人工水景时，应同时考虑：

- channel bed / 河床或池底是否连续；
- bank / 岸壁是否能约束目标水位；
- source water 与 flowing water 的实际行为；
- lateral spill / 水是否会从侧边开放位置向外蔓延；
- downstream drop / 降水位处是否存在可信跌水、瀑布或收水结构；
- inlet / outlet / overflow 是否有明确去向；
- 水面相邻方块更新后是否仍保持预期形态；
- 桥墩、岸脚、建筑、水轮等与水体交界是否会意外堵流或制造小水袋。

禁止把多层 `water[level=0]` 静态写成阶梯水带，然后只因为它们彼此连通就认为“河道成立”。如果水侧边没有河岸、槽壁、地形或其它合理约束，正常 fluid update 后会横向扩散，就必须重新设计河道截面。

对于自然河溪，默认优先让河床与岸线塑造出稳定水体，再放置水；不要先画一条理想化水带，再让周围地形去迁就它。

如果工具允许，应在水体施工后执行真实客户端 / 服务端 fluid updates、邻接更新或等价模拟，并在更新后重新读回检查。若当前工具无法安全触发流体更新，则应：

1. 采用保守、明显有岸壁 / 河床约束的水体几何；
2. 单独标记 `fluid stability unverified`；
3. 不得以 `water connected components = 1`、写入成功或静态截图代替 Vanilla 流体稳定性验证。

水体 QA 至少区分：

- `geometry connectivity`；
- `water-level logic`；
- `bank / bed containment`；
- `fluid-update stability`。

### 现有环境处理

现有随机植被可以为新的整体设计清理后重植；不要因为“原本就在这里”而保留破坏空间结构的随机树草。

## 7. 复杂设计至少同时考虑 Plan + Section + Sequence

只做平面图不够。

### Plan

确定：

- 主要空间和次要空间；
- 建筑 / 山水 / 道路 / 植被体块；
- 开放空间；
- 视线；
- 功能关系。

### Section

主动检查垂直关系：

- 水面；
- 地面与坡度；
- 建筑基座 / 楼层；
- 屋顶；
- 山峰 / 岩壁；
- 树冠；
- skyline。

### Sequence

玩家体验需要有顺序：

- 到达；
- 转折；
- 遮挡；
- 释放；
- 进入；
- 登高 / 下行；
- 框景 / 回望；
- 主空间与次空间转换。

复杂园林、聚落、山地建筑尤其不能只靠顶视图“看起来合理”。

## 8. 可达性和功能优先于表面装饰

必须避免：

- 楼梯尽头是墙；
- 门被堵住；
- 楼层无法到达；
- 道路无意义断头；
- 建筑相互穿插；
- 景观点无法进入；
- 主路线在关键位置失去净空。

建筑内部不是重点时也必须保证基本入口、楼层接口和核心交通成立。

## 9. Structural Vegetation 与 Detail Vegetation 必须分开设计

### Structural Vegetation｜结构性植被

大型乔木、树群、林带、竹林、树林、绿篱等可以承担：

- 空间围合；
- 遮挡；
- 框景；
- 借景关系；
- 背景 / 前景；
- 路径引导；
- 水岸软化；
- 高低过渡；
- canopy / skyline；
- 视觉焦点。

因此 Structural Vegetation 属于 Macro / Meso 阶段，应与山体、水体和建筑共同规划。

### Detail Vegetation｜细节植被

花、草、小灌木、荷叶、藤蔓、地被、小型点景植物等可在后期 Micro 阶段补充。

但“后期补充”不等于“可有可无”。对于本来应具有丰富近地层植物的园林、林地、乡野、河岸、庭院等环境，应主动检查目标 Minecraft 运行时可用的原版 / 已安装植物 palette，而不是只使用树叶方块模拟所有林下层。

适合题材时，可以主动使用例如：

- 短草 / 高草；
- 蕨类；
- 苔藓与贴地植物；
- 蒲公英、罂粟；
- 郁金香；
- 矢车菊；
- 滨菊；
- 铃兰、绒球葱、兰花；
- 其它与当地生态、季节、园艺或文化语境相符的小型植物。

这些只是可用语言示例，不是要求每个场景都使用，也不得为了“丰富”而把不同颜色花朵随机撒满地面。

### Layered Vegetation｜植被应形成层级，而不只是植物散点

在乡野、森林、湿地、河岸、园林等自然环境中，植被质量不能只通过“有没有树、草、花”判断。

应根据场景建立适当的垂直与平面层级，例如：

- canopy / upper tree layer；
- understory / small tree or shrub layer；
- herbaceous layer；
- groundcover layer；
- seasonal / flowering layer。

不要求所有场景同时具备全部层级，但自然环境通常不应只是大片裸露 `grass_block` 加少数孤立乔木或零星花朵。

植被密度与覆盖应随环境职责变化，而不是全图采用同一稀疏度：

- forest edge 可形成较密且不规则的林缘；
- riverbank 可形成连续但有开口的带状植被；
- meadow 应以草本和地被覆盖为主；
- settlement core 可因踩踏、放牧和高频使用而明显稀疏；
- farmland、果园、花圃与道路周围受人工管理逻辑控制。

### Flowering Community｜花卉应形成符合环境的可见群落

当题材、生态、季节和人为管理条件允许野花或观赏花卉出现时，不要只放极少量单一花种作为“已经有花”的象征。

应从目标 Minecraft 运行时可用 palette 中选择少数彼此相容的 species / colors，并根据环境形成：

- substantial patch / 明显花丛；
- ribbon / 带状花群；
- clearing carpet / 林间开口中的成片花层；
- 林缘、草甸、道路、田埂、水边或庭院边缘的群落；
- 与草本、蕨类、地被共同出现的混合层。

### Flower Abundance Bias｜繁茂题材默认采用更强的花卉正向偏置

对于以下类型或具有类似视觉语义的场景：

- lush forest / 繁茂森林；
- enchanted forest / 魔法森林；
- sacred grove / 圣林、森林圣所；
- ornamental garden / 观赏园林；
- spring / summer meadow；
- 明确强调生命力、繁盛、花季、仙境感的环境；

如果生态 / 世界规则没有反对理由，**花卉层应在玩家尺度上明显可见，而不是只在俯视图或统计里“存在”。**

默认应有多个视觉上有分量的花群区域，而不是只有少数几处很小的 patch。允许先做较强的正向矫正：宁可让第一版花层明显丰富，再通过玩家透视检查局部削减，也不要长期维持“象征性几朵花”的保守下限。

这里的“更多”仍然不等于：

- 全图均匀撒花；
- 每种颜色都必须出现；
- 花覆盖所有道路、建筑边缘和林下空间；
- 用花取代草、蕨、灌丛、苔藓和 Ground Plane。

应通过 **dense patch + sparse transition + open gap** 形成层次。主要花群可以高密度，向外逐渐稀疏，并保留无花的林下、草地和通行空间作为对比。

只有城防净空、重度踩踏 / 放牧区、荒漠、严寒高地、裸岩、深暗闭合林下等具有明确抑制因素的场景，才默认保持低花量。

## 10. Ground Plane｜地表本身也必须被设计

在经过主动设计的场地中，`grass_block` 不能自动被视为“已经完成的地面”。应根据空间职责判断这里究竟是：

- 有意识保留的草坪 / 草坡；
- 林下地表；
- 苔藓地；
- 裸土 / 粗土；
- 碎石 / 砾石；
- 铺装；
- 农田 / 花圃；
- 水岸过渡；
- 或其它符合题材的地表。

大面积纯草地完全可以成立，但必须是有明确尺度、功能、生态或构图理由的设计结果，不能只是因为超平坦或地形生成默认留下了草方块。

## 11. 自然式植被必须是“受约束的不规则”，不是网格，也不是纯随机

自然式园林、森林、河岸、山地植被默认禁止无理由的：

- 等间距种植；
- 行列网格；
- 对称复制；
- 统一株高；
- 统一冠幅；
- 同一树型高频重复；
- 直线切割式林缘。

但“随机撒点”同样不是合格设计。

自然式植被应优先通过以下关系形成：

- cluster / 丛聚；
- density gradient / 疏密渐变；
- irregular edge / 不规则林缘；
- opening / 局部开口；
- hierarchy / 主树、副树、林下层级；
- overlap / 树冠叠合；
- asymmetry / 偏心与不对称；
- terrain response / 顺应坡地、水岸和建筑；
- sightline response / 为遮挡、框景或留出视线服务。

只有果园、行道树、宫殿礼仪轴线、规则庭院等题材有明确人工秩序时，才主动使用规则排列。

## 12. 景观植被的尺度必须与空间职责匹配

Minecraft 原生小树不是默认景观树答案。

如果一棵树承担遮挡、背景、框景、树冠天际线或空间围合作用，应根据周边建筑和地形主动确定需要的高度、冠幅、主干、分枝、倾斜和树冠不对称程度。

对于重要景观场景，优先使用或设计足够大的 custom vegetation asset；原生树仅在其尺度和形态确实合适时使用。

尺度检查至少考虑：

```text
player scale
→ building scale
→ terrain / landscape scale
→ canopy / skyline scale
```

不得因为“Minecraft 默认树长这样”就接受明显失衡的景观比例。

## 13. 植被与其它高价值资产优先复用蓝图库；不足时先生成候选，不自动入库

可以积极复用 Litematica、Blueprint 或已有资产库，但“能放进去”不等于“应该放进去”。资产必须满足当前项目的：

- 功能；
- 尺度；
- 地形接口；
- 风格；
- 空间职责；
- 视觉权重。

对于大型乔木、古树、竹丛、林带、林缘组合等高价值景观资产：

1. 先查询已有蓝图库 / 参考库；
2. 有合适资产时选择合适 variant，而不是高频复制同一对象；
3. 没有合适资产时，可以为当前任务自行设计新的候选资产；
4. 候选资产可保存在**本任务临时 / candidate 产物**中用于当前场景验证；
5. **不得因为模型自行设计完成，就自动写入正式资产库。**

### Owner Approval Gate｜资产入库必须真人批准

完成存档编辑任务后，最终输出中应增加：

> **推荐入库资产候选清单**

仅列出本轮确实具有重复使用价值的候选，例如：

- 大型景观树；
- 古树；
- 特殊竹林 / 林带组合；
- 假山模块；
- 亭 / 桥 / 小型构筑物；
- 其它高质量可复用资产。

候选至少说明：

- 临时名称 / ID；
- 类型；
- 尺寸；
- spatial role / 用途；
- 当前世界位置或预览；
- 推荐复用理由；
- 类似现有资产（若已知）；
- 建议资产类别。

只有 Owner 明确批准后，才能进入独立的正式提取、转换和注册任务。

## 14. Macro → Meso → Micro

### Macro

先解决：

- 场地与地形骨架；
- 大水体；
- 主次空间；
- 总体高度关系；
- 大型 Structural Vegetation；
- 建筑主次体量与总体 silhouette；
- 主要视觉焦点。

建筑类任务在 Macro 阶段就应建立建筑的主次体量、入口权重、竖向层级与主要 roofline；不要把建筑先压成几个简单 box，再把“建筑设计”全部推迟到立面阶段。

### Massing Gate｜进入 Meso 前先验证建筑体量

对于包含重要建筑的任务，进入立面、材料和 Micro 之前，应先完成一次 Massing Gate。

默认设计顺序：

> **Function / Typology → Massing → Section / Roofline → Facade → Material → Detail**

至少检查：

- primary / secondary mass 是否清楚；
- 主要入口是否通过体量关系成立，而不只是墙上开洞；
- 不同功能是否需要不同的高低、进退、庭院、侧翼、塔体、廊道或附属体量；
- section 与 roofline 是否响应内部层高、结构和功能；
- 建筑与地形、街道、庭院、水体之间的体量接口是否合理；
- 从远 / 中距离看，轮廓是否已经具有可读身份。

如果这个阶段失败，应先重做体量；不得指望窗户、材料、柱子、屋檐或其它 facade detail 在后期救一个失败的主轮廓。

### Meso

再解决：

- 道路 / 游廊 / 桥；
- 次级山水；
- 建筑之间关系；
- 林带、竹林、树群；
- 屋顶 / 立面 / 梁柱；
- 视线与空间开合。

### Micro

最后处理：

- 门窗细节；
- 栏杆；
- 活板门；
- 花草；
- 小灌木；
- 地被与 Ground Plane 的细化；
- 铺地变化；
- 小型点景。

不得在 Macro / Meso 尚未成立时，用大量 Micro 装饰掩盖平板、空旷、网格化、比例失衡或建筑体量失败等问题。

## 15. 统一设计语言，同时避免复制感

同一项目中的建筑、景观和资产应有家族相似性：共享时代 / 文化、材料、结构、比例与环境逻辑；不同用途和等级再产生合理差异。

不要把不同作者、不同体系下只因为都标为“medieval”“fantasy”“tree”或“garden”的资产机械混用。

也不要连续复制同一建筑或同一树蓝图。优先使用真实 variant，并通过朝向、地形关系、组合方式和上下文产生变化。不得依赖简单重复制造规模感。

### Reusable Generator ≠ Repeated Morphology

代码复用、参数化函数、模板和蓝图是实现手段，不是设计理由。

如果两个对象的功能、等级、地形关系或空间职责明显不同，不应只通过“同一个 generator 改长宽高 / 换材料”制造差异。应检查它们是否需要不同的：

- 体量组合；
- 屋顶 / 顶部轮廓；
- 开口节奏；
- 结构方式；
- 入口关系；
- 与地形、道路和庭院的接口；
- skyline 权重。

回廊、柱列、军营、行列住宅等原型本来依赖重复时，可以有意识地重复；禁止的是**无意识的参数化复制感**。

### Architectural Massing & Silhouette｜建筑不能只靠盒体加立面细节完成

建筑可以是矩形、方正、封闭或高度正交；仓库、堡垒、商队驿站、传统院落、现代建筑等题材本来就可能如此。问题不是“方”，而是把所有功能都压缩成相似 box，再只靠开窗、换材料、加柱子和表面装饰制造差异。

在进入 Facade / Material / Micro 之前，应先从 Macro / Meso 层建立符合题材、功能、结构与环境的三维体量。应主动判断是否需要：

- primary / secondary mass / 主体量与次级体量；
- entrance mass / 门楼、门廊、凹入入口、前室或其它入口体量；
- vertical hierarchy / 高低层级；
- recess / projection / 庭院、侧翼、廊道、塔体、后勤体量等进退关系；
- roofline / skyline / 坡顶、穹顶、拱顶、塔、女儿墙、露台等与原型相符的顶部轮廓；
- section logic / 外部轮廓与内部层高、跨度和结构的对应；
- terrain / street / courtyard / water interface / 与地形、街道、庭院和水体的体量接口。

建筑体量应由功能与构造产生，而不是为了“看起来不方”无理由制造奇形怪状。

一个实用检查是：**暂时忽略材质、门窗和小构件，只看主要实体轮廓。** 如果不同建筑在这个层级仍只是几个尺寸不同的长方体，或主要建筑的入口、主次、顶部轮廓与空间等级都无法读出，就说明 massing 尚未完成。

Facade articulation 用于解决近中距离的墙面深度；Massing / Silhouette 负责更早、更大的远中距离体量。两者不能互相替代。

### Controlled Material Language｜受控材料语言

“材质丰富”不等于随机混很多方块。设计应先形成清楚的材料层级，例如：

- primary material / 主体材料；
- secondary material / 次要材料；
- structural accent / 构造强调；
- transition / weathering / 边界、风化或地形过渡材料。

大面积墙面、屋面、岩壁、铺地或地表如果长期只有单一纹理，应检查这是不是题材和构造的真实结果，而不是生成方便造成的平板化表面。

目标运行时若提供合适的 stairs / slabs / walls / fences / trapdoors 或其它形状、状态与材料变体，可用于改善屋顶坡面、檐口、墙脚、开口、边缘、岩体和铺地的几何层次；但不得为了“细节”无意义堆零件。

### Material Placement Logic｜材料变化必须跟随构造与环境原因

建立了 palette 不等于已经完成材料设计。不同材料应尽量对应真实的构造、使用、年代或环境关系，例如：

- foundation / wall base：承重、潮湿、接地或易磨损位置；
- corner / opening surround：转角、门窗框、拱券等需要更整齐或更坚固的构造位置；
- primary wall field：主体墙面；
- repair / weathering / damp zone：修补、风化、受水、苔生或年代差异；
- roof ridge / eave / edge：屋脊、檐口、收边；
- paving transition：道路、院落和门口的边界变化。

不要把一种材料整面铺满只因为生成方便，也不要用均匀概率噪声把第二、第三种材料随机撒进墙面来伪造“丰富度”。

材料变化应让玩家能读出结构、使用和时间，而不是只看到颜色噪声。

### Facade & Surface Articulation｜大型表面必须有可信的几何层次

大型建筑完成主次体量后，应在玩家近中距离检查墙面、屋面和主要构件是否只剩下“巨大平面 + 方洞”。

必要时根据题材和构造逻辑，通过以下方式形成适度 depth / shadow / rhythm：

- base / plinth / 墙脚；
- corner treatment / 转角；
- door / window surround / 门窗框与拱券；
- buttress / pilaster / 梁柱；
- floor line / 腰线或楼层结构；
- recess / projection / 局部退进与突出；
- eave / cornice / 檐口；
- roof ridge / 屋脊与收边；
- parapet / battlement / 栏墙等与题材相符的顶部构造。

不是每面墙都必须复杂，也不是要求平均添加装饰。防御性素墙、仓库、极简建筑都可以克制；关键是大型表面应体现其构造与比例，而不是仅靠更换材质掩盖平板几何。

## 16. 小修补与整体重构采用不同策略

门窗、材料、局部屋顶、单株植物等小修改可以原地修补。

若城堡内院、村庄核心、园林山水骨架、主水体、空间序列或植被围合关系发生根本变化，应优先有边界地清空后重构：

1. 明确保留范围；
2. 明确拆除范围；
3. 基于清空后的状态重新规划；
4. 施工前检查新旧结构和新对象彼此碰撞。

不要持续在根本不合适的旧布局上叠加补丁。

## 17. 逐阶段施工必须逐阶段自检，但自检不能只看施工成功

用户要求分阶段建造时，每一阶段完成后应检查与该阶段目标相关的设计质量。

“方块成功写入 / samples 全通过”只证明施工执行成功，不等于空间设计成功。

复杂项目的阶段检查可根据现有工具选择：

- top view；
- height / contour view；
- section / profile；
- oblique / perspective view；
- 玩家关键视点；
- 主要游览路线；
- skyline；
- 碰撞 / 可达性。

不得只用顶视图判断三维景观是否成功。

### Construction Integrity Sweep｜施工完整性检查

阶段完成和最终交付前，应主动检查是否存在**非设计意图**造成的：

- floating / 悬空残片；
- isolated / disconnected 小型几何；
- 石块、岩体、挡墙、台地边缘或建筑构件与主体意外断开；
- 浮空植被或施工残留；
- 地形 / 建筑交界处明显缺少应有支承或接口。

这不是要求模拟现实物理，也不是要求每个 Minecraft 方块都直接接触下方方块。桥、拱、梁、挑檐、悬挑平台，以及题材明确允许的魔法悬浮都可以成立。

检查目标是区分：

> **intentional cantilever / suspension** 与 **unintended orphan geometry**。

如果工具允许，应结合连通分量、局部支承关系、目标对象包围盒、剖面 / 透视和玩家视点检查；不要只检查建筑楼板，也应覆盖岩石、挡墙、地形构件、植被和其它明显重型几何。

### System Interface Integrity｜系统接口完整性

不同空间系统分阶段施工时，后施工系统不得无意破坏已经成立的前序系统。

重点检查：

- circulation ↔ architecture；
- terrain ↔ architecture；
- water ↔ architecture；
- vegetation ↔ circulation / architecture；
- structure ↔ open space。

道路、地形整形、水体、植被或后期装饰如果需要修改既有建筑、墙体、屋顶、楼板、入口、桥梁或其它关键结构，应确认这种穿越、切割、覆盖或清空是明确设计意图。

道路只有在门洞、城门、拱廊、内部街道等明确空间关系下才可进入建筑 envelope；不得为了获得道路净空而直接清除住宅墙体或其它无关结构。

每个较大的后续阶段完成后，应重新抽查前序关键系统是否仍保持完整。不要只验证“当前阶段自身可达 / 无阻挡”，还要验证它没有让先前已经成立的系统退化。

当施工工具采用 `fill / clear / replace / carve` 等大范围写入时，尤其要检查其包络是否跨入其它已完成对象。

## 18. Final Spatial Review｜最终必须检查玩家实际体验

大型 / 复杂 / Landscape / Mixed 项目在交付前至少检查以下问题：

### Anti-Flatness

- 是否因为方便而把大部分空间压在同一标高？
- 原型要求的山、水、高差和树冠层次是否真实存在？
- 是否反过来为了“有高差”而制造无因果的巨大平台或强制抬高？

### Terrain Morphology & Circulation

- 自然地形是否出现长距离连续、平行的等高台阶或其它明显“数学高度场”痕迹？
- 是否存在可读的 ridge / valley / gully / shoulder / cliff / talus 等地貌组织？
- 室外山路、坡道、台阶是否优先依托坡面，还是被做成与地形脱节的高架实体？
- 如存在桥梁、栈道、城防墙梯等脱离地形的结构，其工程理由和接口是否明确？

### Water / Fluid Stability

- 水体几何是否不仅连通，而且符合目标 Minecraft 版本的真实流体规则？
- 河床、岸壁和池岸是否约束了目标水位？
- 是否存在 source water 侧边开放、正常更新后会无意外溢的静态水带？
- 多级水位之间是否有真实跌水 / 瀑布 / 溢流接口，而不是悬空的水平水片？
- inlet / outlet / overflow 是否有去向？
- 如果工具能执行 fluid update，更新后世界是否仍保持设计形态？
- 如果不能验证，是否明确标记未验证，而不是用连通分量代替？

### Anti-Grid

- 道路、地块或自然式植被是否出现无理由的方正、等距、对称和机械重复？

### Space Use

- 是否存在大面积意义不明、空旷但没有构图或功能价值的留白？
- 是否也存在为了“丰富”而过度填充的问题？

### Vegetation Structure

- Structural Vegetation 是否真的参与空间组织？
- 自然环境是否形成合理的 canopy / understory / herbaceous / groundcover 层次与疏密变化？
- 是否只是孤立大树 + 少数花草散点？
- 允许花卉出现的环境里，是否只有极少量单一花种作为象征性点缀？
- 对 lush / enchanted / sacred / ornamental 等题材，花层是否在玩家尺度上具有足够视觉存在感，并形成多个 substantial patch / ribbon / clearing community？

### Scale

- player / building / terrain / tree / canopy 尺度是否相互匹配？

### Sequence

- 玩家移动中是否有转折、开合、遮挡、释放、框景、回望或层次变化？

### Architectural Massing & Silhouette

- 暂时忽略材质、窗户和小构件时，主要建筑是否仍有清楚的三维身份？
- primary / secondary mass 是否可读，还是只剩几个尺寸不同的 box？
- 主要入口是否通过门楼、凹入、前室、廊道、体量突出或其它题材合理方式被强调，而不只是墙上开洞？
- 不同功能建筑是否真的拥有不同 volume composition，而不仅是尺寸和材料不同？
- roofline / skyline 是否与建筑类型、内部 section 和空间等级对应？
- 是否存在有理由的高低、进退、庭院、侧翼、塔体、拱顶、穹顶、露台或女儿墙等层级；又是否反过来为了“反方盒”无理由制造复杂轮廓？
- 从远 / 中距离看，建筑主次与整体 silhouette 是否已经成立，还是必须依赖 facade detail 才能被识别？

### Architectural Surface Quality

- 大型墙面是否只有单一材料平面和方洞？
- material palette 是否真正通过构造、环境和风化逻辑落到表面，而不是只存在于代码 / palette 列表？
- 近中距离是否存在与题材相符的墙脚、转角、门窗、梁柱、扶壁、退进、檐口、屋脊等几何层次？
- 是否反过来为了细节而无意义堆砌构件或随机混材？

### Building Envelope Integrity

主要建筑最终应重新检查：

- roof；
- exterior walls；
- floor / foundation interface；
- intended openings；
- entrances；
- vertical circulation。

应区分 intentional openness 与 accidental destruction。谷仓、敞廊、门洞等可以开放，但不能把道路、水体、地形整形或其它后续施工造成的墙体缺口误判成设计开口。

### Construction / System Integrity

- 是否存在非设计意图的悬空、孤立或断裂几何？
- 后施工系统是否破坏前序建筑、道路、水体、地形或植被关系？

如果 Macro / Meso 层面失败，应优先重建相关区域，不得仅靠 Micro 装饰掩盖。

## 19. 自主完成，但不要过度工程化

模型应自主完成研究、设计、施工、自检、必要返工和最终交付，不需要在每个普通设计选择上停下来向用户请示。

只有以下情况应停止并报告 blocker：

- 无法确认真实写入存档；
- 继续操作可能伤害用户真实世界；
- 工具不可用；
- 存档格式 / 版本存在无法安全处理的问题；
- 任务存在真正无法自行消解的重大歧义。

不要为了证明“严谨”而搭建远超任务需要的基础设施。检测和自动化应该服务于设计质量，而不是取代设计。

## 20. 默认工作顺序

除非任务明确需要其它顺序，默认：

1. 理解任务；
2. 判断 BUILDING / SETTLEMENT / LANDSCAPE / MIXED_ENVIRONMENT；
3. 做必要现实研究或定义世界规则；
4. 读取场地 / 当前世界；
5. 找出不可缺少的空间系统；
6. 做 Plan + Section + Sequence；
7. 先解决 Macro Terrain / Water / Structural Vegetation / Architecture，并建立建筑主次体量与 silhouette；
8. 对重要建筑执行 Massing Gate：Function / Typology → Massing → Section / Roofline；体量失败时先返工，不进入 facade / material；
9. 检查高差是否由真实空间因果驱动，并检查 Terrain Morphology 与 Terrain-Conforming Circulation，避免 Forced Elevation / 高架式假山路；
10. 如果存在真实水体，先检查河床 / 岸壁 / 水位 / 跌水 / inlet-outlet，再检查 Minecraft fluid-update stability；几何连通不能替代流体有效性；
11. 检查尺度；
12. 进入 Meso，完成建筑之间关系、roofline / facade / structure 与其它中尺度关系；
13. 分阶段施工并做与阶段目标对应的自检；
14. 每个后续阶段复核 System Interface Integrity，避免破坏前序系统；
15. Micro 完成 Ground Plane、Layered / Flowering Vegetation、材料 placement、立面 / 表面构造和其它细节；对 lush / enchanted / sacred / ornamental 场景采用明显的 Flower Abundance Bias；
16. 做 Top + Section + Perspective / Route 审核；
17. 做 Construction Integrity + Building Envelope + System Interface Sweep；
18. 做 Water / Fluid + Terrain / Circulation + Vegetation + Architectural Massing / Silhouette + Architectural Surface 最终质量复核；
19. 对 Macro / Meso 缺陷做有界重建，而不是装饰掩盖；
20. 完成世界；
21. 输出推荐入库资产候选清单；
22. 等待 Owner 实机检查；
23. 只有 Owner 明确批准的候选才能正式提取入库。