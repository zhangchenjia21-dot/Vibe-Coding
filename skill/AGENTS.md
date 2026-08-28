# Skill Subtree｜AI Governance

状态：current  
适用范围：`Vibe-Coding/skill/` 全部 Skill 与维护任务

## 1. Current Source

每个 Skill 的正式事实源是固定路径：

```text
skill/<runtime>/<skill-name>/SKILL.md
```

禁止在 active 目录并列 `SKILL_v2.md`、`SKILL_latest.md`、`SKILL_final.md` 等版本副本。历史默认由 Git history 承担；确需独立留档时进入仓库 `99_归档/`。

## 2. Authority

`skill/` 是跨项目默认方法层，不拥有具体项目的动态产品事实。

发生冲突时：

```text
用户当前明确指令
> 当前项目正式 Product / Architecture / Contract / Roadmap
> 可验证 implementation truth
> Vibe-Coding 跨项目治理
> skill/ 通用默认方法
> 历史 / 模型经验
```

不得因为 Skill 与项目现在位于同一仓库，就让 Skill 覆盖项目 current decision。

## 3. Freshness / Active-only

正式 Skill 工作开始时读取 `Vibe-Coding/main` current；写回前重新检查 HEAD。

```text
Task Base HEAD
!= Current HEAD ?
```

若变化影响目标 Skill、相关项目裁定或上游方法，先 audit / Decision Propagation，不能旧 Base 静默覆盖新 current。

## 4. Human-facing Version

人工 Skill 版本采用一位小版本：

```text
v1.0 → ... → v1.9 → v2.0 → v2.1
```

`vM.N` 中 `N ∈ [0,9]`；`vM.9` 后进入 `v(M+1).0`。重大变化由 Change Log / status / supersedes 表达，不使用多位 minor。

## 5. Formal Agent Instruction Routing

当任务需要生成、审核、缩短、拆分或交接正式 Agent Task Packet 时，读取：

`skill/gpt/agent-task-packet/SKILL.md`

涉及阶段规划、架构、迁移、重大重构或 Stage Gate 时，同时读取：

`skill/gpt/lifecycle-dev-process/SKILL.md`

```text
lifecycle-dev-process
→ 为什么做 / 哪个 Gate / 依赖谁

agent-task-packet
→ 指令如何组织 / 工作集 / 验收 / Git / 返回
```

## 6. Skill Quality Gate

新增或更新 Skill 至少检查：

- frontmatter `name` / `description` 准确；
- current 路径唯一；
- 与相邻 Skill 职责边界清楚；
- 触发条件和停止条件可理解；
- 示例不诱导绕过 Owner、测试、安全或正式 Owner mutation path；
- 不硬编码具体项目的当前 Task、账号、secret、本地临时状态或会快速过期的路线；
- 一次偶然 workaround 不直接抽象成跨项目 Skill。

## 7. Project-derived Skill Promotion

项目开发中形成的新方法按以下成熟路径处理：

```text
项目专属事实
→ 项目 current / supporting docs

跨项目可复用但仍偏经验
→ 项目经验/

具有明确触发条件、执行步骤、边界和成功标准的方法
→ skill/
```

规则：

1. **Reuse before creation**：先搜索现有 Skill；
2. **Evidence before abstraction**：真实实践后再抽象；
3. **Update instead of duplicate**：能力增量优先更新现有 current Skill；
4. **Do not promote project state**：动态项目状态不进入跨项目 Skill；
5. **Runtime placement follows host**：GPT / DSH 等运行环境差异明显时放在对应 runtime 目录；
6. **Promote useful learning promptly**：成熟方法形成后及时回写，不等项目结束才整理。

## 8. Repository-native Path

Skill 已与项目治理合并到 `zhangchenjia21-dot/Vibe-Coding`。旧独立 `zhangchenjia21-dot/Skill` 不再是 current authority。

正式路径从仓库根开始，例如：

```text
skill/gpt/lifecycle-dev-process/SKILL.md
skill/gpt/agent-task-packet/SKILL.md
skill/dsh/project-thread-handoff/SKILL.md
```

迁移 provenance 见 `skill/MIGRATION_PROVENANCE.md`。

> **项目产生经验，`项目经验/` 沉淀证据，`skill/` 沉淀可重复执行的方法。**
