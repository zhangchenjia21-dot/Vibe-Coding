---
name: tavern-asset
description: Design, discuss, create, revise, migrate, audit, and maintain Tavern World Packs, Character Cards, Expansion Packs, asset families, and the generic asset library for The World on DeepSeek Harness (DSH) — with canonical single-source ownership, core-first architecture, source vs game-local separation, natural-language dependency expression, knowledge-provenance discipline, batch production, and repository-aware delivery. Content authority for how assets are written lives in the sillytavern-assets repository's 创作规范/ directory. Do not use for runtime implementation.
---

# tavern-asset v2.0

> [!abstract] 定位
> 用于 Tavern 资产的讨论、创作、修订、迁移、审核、收敛、资产族治理、通用资产库治理和资产仓库维护。
>
> v2.0 是方向性重写：目标宿主从旧第二版 SillyTavern Runtime 切换为 **The World on DeepSeek Harness (DSH)**。v1.0 中绑定旧机器架构的全部内容（Resolver / Bootstrap / Creator / asset-spec / Context Contract / Router / Feature-Module / Program Authority / G9 冻结边界）随 v2.0 废止。
>
> **资产怎么写的内容权威在资产仓库**：`sillytavern-assets/创作规范/`（总规范 + 三类资产模板）。本 Skill 管方法、流程、治理与仓库纪律，不复制创作规范正文。

---

# 0. 强制执行顺序

1. 识别任务：规划 / 讨论 / 创作 / 修订 / 审核 / 迁移 / 退役 / 仓库维护。
2. 先恢复 current：本 Skill、资产仓库 `创作规范/`、相关资产族 index 与目标资产、the-world 的 `docs/PRODUCT_SPEC_CURRENT.md` 与 `docs/ARCHITECTURE_CURRENT.md`。
3. 新资产族或大规模扩展先做 `Domain Survey → Genericity → Shared Foundation → Canonical Owner`。
4. 未获明确授权时先讨论；用户已授权的批次不重复确认。
5. 执行 Canonical Ownership / Namespace / Source-vs-game-local / Knowledge Provenance / Open Attempt / 依赖表达检查。
6. 创作、修订或迁移。
7. 单资产 Audit；批次 / 依赖簇形成后跨资产审核；阶段结束后 Family / Generic Library Convergence。
8. 旧资产被替代时执行 Supersession / Migration / Archive。
9. 同步受影响的 Asset Family / Generic Library Workspace 与 `创作规范/` 引用。
10. current 核心文件、Skill、创作规范变化时执行 Repository Sync Gate。

---

# 1. Repository Registry

| Owner | Repository | 职责 |
|---|---|---|
| Governance / Skill | `zhangchenjia21-dot/Vibe-Coding` | 跨项目治理与可复用 Skill；本 Skill current = `skill/gpt/tavern-asset/SKILL.md` |
| Game | `zhangchenjia21-dot/the-world` | The World（DSH-native RPG 宿主）：产品 / 架构 / World Core / 资产消费侧语义 |
| Assets | `zhangchenjia21-dot/sillytavern-assets` | World Pack / Character Card / Expansion / 资产族 / 通用资产库 / 创作规范 |

路由：

```text
Skill / cross-project governance → Vibe-Coding/skill + Vibe-Coding current governance
宿主产品 / 架构 / World Core → the-world
资产语义 / 创作规范 / 治理工作区 → sillytavern-assets
```

资产消费侧语义的权威来源是 the-world 的两份 CURRENT 文档；资产正文如何写作的内容权威是资产仓库的 `创作规范/`。两者冲突时先升级裁定，不得自行调和。通用 Skill 不能覆盖项目已经冻结的 current 决策。

## 1.1 Remote-current-first

正式任务前先读取目标 remote current。远端比本地 / 聊天稿更新时先合并差异，禁止旧稿覆盖新事实。正式写回前重新检查 HEAD。

## 1.2 Local Delivery + GitHub Sync

current 核心文件 / Skill / 创作规范变化时，同一任务默认同时：本地完整交付 + GitHub authoritative current 同步 + 回读验证 + commit SHA 报告。

## 1.3 Supersede-in-place

固定路径 Skill 直接 update；带版本 current 文件先建新 current 再移出旧 active；Git 历史承担普通留档；active 目录禁止多个"最新 / 最终"并列。

---

# 2. 目标宿主语义基线

资产创作必须对齐 the-world 已冻结语义（详见其 PRODUCT_SPEC / ARCHITECTURE，以下为资产侧推论）：

- **Source ≠ game-local**：资产定义开局之前的世界；局内变化永不回写 Source；`game-local reality > source default trajectory`；
- **不预写未来**：未来的死亡、战役、背叛、统一、结局不得以任何形式预写；
- **Game Composition 由玩家确认**：Optional 拓展包默认关闭，资产不得静默自激活；
- **知识来源边界**：`GM / Source / System knows X ≠ NPC knows X`；资产负责标注信息暴露层级，不负责执行传播控制；
- **Unlimited Attempt**：`Player owns Attempt；World owns Consequence`；技能 / 机制清单不是玩家输入白名单；
- **双循环**：World Loop 与 Life Loop 并存；资产要为日常与非功能性互动提供素材；
- **重要性决定注意力，不决定存在**：高稀有度资源保持稀缺，不写成廉价供货；
- **资产不写清单**：开局向导、组合确认、durable maintenance、存储路径、节奏状态机、存档策略、Schema / Manifest / Validator 均属宿主职责，资产正文一律不涉及。

---

# 3. Repository-aware Asset Architecture

```text
sillytavern-assets/
├─ 世界包/        # Canonical World Pack Store
├─ 人物卡/        # Canonical Character Card Store
├─ 拓展包/        # Canonical Expansion Pack Store
├─ 资产族/        # 按作品聚合的索引 / 蓝图 / 版本锁 / 审核
├─ 通用资产库/    # 跨世界通用 Expansion 的治理工作区
└─ 创作规范/      # 创作纪律与三类资产模板（内容权威）
```

正式资产正文只保存一份，位于按类型目录；资产族与通用资产库只保存索引、蓝图、版本锁与审核资料，禁止复制第二份正文。

---

# 4. Core-first / Shared Foundation

新资产族生产前建立 `Domain → Candidate Owner → Consumers → Generic/World-specific → Status`。

多个世界共享的长期机制优先抽 Generic Core（`拓展包/通用拓展包/`）；发现第二个消费者时重新评估是否上提。禁止先复制多份临时专用机制再事后反向抽取。

---

# 5. Canonical Ownership

每类长期事实只有一个 Canonical Owner。区分：事实的 Source 定义者、局内状态承担者、读者、派生投影。

- 派生摘要 / 综合评分可重算，不反写 Canonical 事实；
- `定义 ≠ 实例`：资产更新不静默覆盖已存在局内实例；局内变化不回写资产；
- 跨域链按 `Cause → Process → Consequence` 分层，每层只修改自己拥有的事实；
- 大型关系图（亲缘、政治）只持久化语义主关系 / 源事实，可确定重算的派生关系不镜像成第二事实源。

---

# 6. 依赖与协同表达

不使用任何机器依赖字段。资产间关系用自然语言三类写法（细则见创作规范总规范第 4 章）：

1. **可选性声明**：建议只是建议，未启用时按世界常识自然叙述；
2. **协同 / 降级双写**：每条关系同时写"启用时获得什么"与"未启用时怎么办"；
3. **归属指引**："X 属于《Y》"，划界不建依赖。

世界包可用自然语言给出 Required / Recommended / Optional 分级建议（对齐 Game Composition 语义），但只写段落，不发明机器元数据。

---

# 7. 知识边界纪律

- 世界包按可见层级组织：玩家公开事实 / 仅 GM 掌握 / 永不预写；
- 人物卡按"凭什么知道"组织：以人物的身份、经历、开局年份回答每条信息的来源；
- 角色隐藏信息、未来史实、系统 / 超自然私有信息、GM 后台计划必须显式标注暴露层级。

---

# 8. Character Card Gate

重要人物卡额外检查：价值排序、欲望、恐惧、判断方式、核心矛盾、偏见 / 盲点、风险偏好、压力反应、改变条件、语言 / 情感表达、制度立场。

- 一人一卡；时间字段最小化；未知优先于虚构；
- 人格写生成式决策结构，不写标签；后世文学标签不是人格真相；
- 支持玩家接管的卡写清 T0 前锁定 / T0 后归玩家；不预设重大承诺、阵营、忠诚与恋爱结果；
- 每张卡提供 Life Loop 素材（日常面 + 非功能性互动面）。

结构细则见 `创作规范/人物卡_创作模板`。

---

# 9. 机制资产纪律

- 机制不拍平成 lore：保留真正改善游玩的区分；也不写成数值契约或伪代码；
- 语义阶梯优先于数值；裁定写成 GM 的思考顺序；写明"何时才值得正式裁定"；
- 长期状态只声明"哪些事实值得记住"，存储位置与格式由宿主决定；
- 后果移交对应领域，写清对方未启用时的降级处理；本包独立完整可用。

结构细则见 `创作规范/拓展包_创作模板`。

---

# 10. 生产与审核

## 10.1 批次生产（人物卡）

批次是生产次序，与优先级（S/A/B/C）、卡片复杂度三者分离；每批 10–15 张；按对照关系交错生产；批次完成后做跨卡审核再开下一批。

## 10.2 审核 Gate

- 单资产：按创作规范各模板的检查表逐项过；
- 批次 / 整族：共享历史不重复、关系对称、声线收敛、名人标签核查、未来无污染、跨资产引用有效、索引 / 版本锁 / 蓝图同步。

## 10.3 Convergence

依赖簇形成后做簇级收敛（Ownership / 术语 / 引用 / 版本）；阶段收口做族级 / 通用库级收敛。重点风险：同一事实多处重复、互相矛盾的人物身份、静默激活、旧 Runtime 语言回潮、规范漂移。

---

# 11. Supersession / Migration / Archive

新规范 / 新架构完整取代旧件时：

```text
retire old
→ preserve evidence（99_归档/ 或 Git history）
→ classify unique deltas
→ migrate / rebind
→ 同 delivery 同步索引与引用
```

active 目录同一文档族只保留一个 current；未发布且无兼容义务时，完整 supersession 优先于长期兼容层。

---

# 12. Architecture Reflection

在 Shared Foundation、高耦合模块成簇、阶段收口或大迁移前，以及用户主动质疑方向时执行：

- 规模扩大 5–10 倍是否仍成立？
- 是否出现 duplicate Owner / 同一事实多处维护？
- 抽象是否有真实消费者？
- 资产是否在替宿主承担责任？
- 是否为了"完整"虚构低史料人物？

涉及真实产品体验或不可逆权衡时，先给事实 / 风险 / 推荐，再邀请用户反思。

---

# 13. Stage Boundary

- 资产可以对齐 the-world **已冻结**的产品 / 架构语义；不得假设尚未建成的宿主能力（World Core 未实现的职责，资产不得预先为其写格式）；
- 不新建 manifest / compiler / importer / validator，除非真实资产已因缺少它而受阻并经正式授权；
- 资产仓库不反向定义宿主能力；宿主需求以 the-world 正式文档为准。

---

# 14. Formal Deliverables

- 正式语义成果为实际 `.md`，frontmatter 仅人工治理元数据，不冒充机器协议；
- current 核心 / Skill / 创作规范变化必须 Local Delivery + GitHub Sync；
- 写后回读文件头、关键段、尾，确认版本、状态、乱码、截断与 supersession；
- 正式任务 Final Report 至少包含：Result / 变更的 canonical assets / 同步的治理工件 / 证据 / Git（base HEAD、final HEAD、commit 清单）/ Remaining。

---

# 15. Recommended Gate Set v2.0

```text
G0  Current Source / Freshness
G1  Discussion / Authorization
G2  Domain Architecture Survey
G3  Genericity / Core-first
G4  Canonical Ownership
G5  Semantic Namespace
G6  依赖表达（自然语言三类写法 / 分级声明）
G7  Source vs game-local / 不预写未来
G8  Knowledge Provenance / 暴露层级标注
G9  Open Attempt / Unlimited Attempt
G10 机制完整（不拍平 / 无数值契约 / 长期状态与存储分离）
G11 宿主职责边界（资产不写清单）
G12 Life Loop / 稀缺性
G13 批次生产 / 跨卡审核
G14 Convergence / 规范漂移
G15 Supersession / Archive
G16 Repository Sync / 单事实源
G17 Deliverable / Readback
G18 Architecture Reflection
```

---

# 16. v2.0 Change Log

相对 v1.0：

- 目标宿主切换为 The World on DSH；旧 SillyTavern Runtime（Resolver / Bootstrap / Creator / asset-spec / G9）相关内容全部废止；
- 资产写作内容权威移交 `sillytavern-assets/创作规范/`，本 Skill 收敛为方法、流程与仓库治理；
- Context Contract / Router / Feature-Module / Program Authority / Surface Ownership / Registry Projection 等旧机器上下文体系删除，保留其语义内核（Full Asset ≠ Prompt、知识边界、不拍平机制）并并入宿主语义基线与机制纪律；
- 依赖表达改为自然语言三类写法 + Required / Recommended / Optional 段落式分级；
- Repository Registry 更新：`Vibe-Coding/skill/` 成为 Skill current authority，`the-world` 成为 Game 仓库；
- Gate Set 由 26 项旧机器门重构为 18 项语义门；
- v1.0 全文留档于 Git history。