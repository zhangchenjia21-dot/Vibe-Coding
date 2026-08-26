# 代码 Agent｜Worktree 隔离与 Main 合并治理 v1.0

状态：`CURRENT EXECUTION GOVERNANCE`
日期：2026-08-19
适用：SillyTavern 代码 Agent（Grok Build / Codex / 后续 Coding Agent）

## 1. 目标

在不增加长期分支管理负担的前提下，隔离 Agent 施工风险。

正式采用：

```text
main
= 受保护的正式集成主线

当前唯一临时任务分支
= 当前 Agent 的施工线

独立 Git worktree
= 当前 Agent 的物理施工目录
```

核心目标：

1. Agent 失误不能直接污染 `main`；
2. Project Owner 不负责 Git 技术诊断；
3. GPT 在精确提交审核通过前不允许代码进入 `main`；
4. 任何时刻原则上只保留 `main + 1 条当前任务分支`；
5. 审核通过后使用快进集成，不制造无意义 Merge Commit；
6. 任务结束立即清理 worktree 与临时分支。

---

## 2. Main 的新语义

```text
main
= Protected Integration Line
```

默认规则：

- Coding Agent 不直接在 `main` 工作区编辑代码；
- Coding Agent 不直接 `push origin main`；
- `main` 只接收已经经过 GPT exact-SHA Independent Review 的提交；
- Project Owner 可继续在原 main worktree 保留自己的本地未提交内容，Agent worktree 与其隔离。

允许 Agent 在创建 worktree 时对主仓库执行只读 Git 检查，以及 `git worktree add/remove` 所需的仓库元数据操作；不得因此编辑原 main worktree 文件。

---

## 3. 单临时分支原则

默认不采用长期：

- `develop`；
- `staging`；
- 长期 `grok`；
- 多层 feature branch；
- 每次 correction 再开一条 fix branch。

正式规则：

```text
任一时刻
≈ main + 当前唯一临时任务分支
```

分支命名：

```text
agent/<task-id>
```

示例：

```text
agent/g9-02b
agent/g9-02c-router-core
agent/g9-04-adapter
```

同一任务出现 Independent Review correction：继续提交到原任务分支，不另开分支。

只有确有并行、多 Owner 或长期 PR 审核需求时，才允许例外增加分支；必须先由 GPT 明确授权。

---

## 4. 固定 Worktree 路径

SillyTavern 默认复用一个固定 Agent worktree 目录：

```text
D:\AI\Projects\.worktrees\sillytavern-agent
```

每项任务结束后删除该 worktree；下一项任务在同一路径重新创建。

这样本地目录也不会不断增加。

原正式工作区继续是：

```text
D:\AI\Projects\sillytavern
```

Coding Agent 的代码编辑、测试、构建和 commit 必须发生在 Agent worktree。

---

## 5. 任务启动协议

以任务 `<task-id>`、指定 Base SHA `<base-sha>` 为例。

先在主仓库执行只读/管理命令：

```powershell
git -C D:\AI\Projects\sillytavern fetch origin
git -C D:\AI\Projects\sillytavern rev-parse origin/main
git -C D:\AI\Projects\sillytavern worktree list
git -C D:\AI\Projects\sillytavern branch --list agent/<task-id>
```

必须确认：

```text
origin/main == Task Base SHA
```

并确认固定 Agent worktree 路径未被另一任务占用。

然后创建：

```powershell
git -C D:\AI\Projects\sillytavern worktree add `
  D:\AI\Projects\.worktrees\sillytavern-agent `
  -b agent/<task-id> `
  origin/main
```

创建后，Agent 必须切换工作目录到：

```text
D:\AI\Projects\.worktrees\sillytavern-agent
```

随后再次记录：

```powershell
git branch --show-current
git rev-parse HEAD
git status --short
```

Expected：

```text
branch = agent/<task-id>
HEAD = Task Base SHA
worktree = clean
```

如果固定 worktree 已存在、任务分支已存在但来源不明、Base 不匹配，返回 `BLOCKED`，不得自行 reset/clean/rebase。

---

## 6. 施工期间规则

Agent 只能在任务 worktree 修改任务允许的文件。

禁止：

```text
push origin main
checkout main 后施工
修改原 main worktree 文件
git add .
git add -A
git reset --hard
git clean
git stash
git pull
git merge
git rebase
git commit --amend
force push
```

允许：

- focused commit；
- correction commit；
- push 当前 `agent/<task-id>`；
- fetch origin；
- 只读检查 main/remote 状态。

默认一个任务尽量形成一个主要实现 commit；Independent Review 后需要修正时允许继续追加少量 correction commit。不要为了追求“一个提交”使用 amend/rebase 破坏已审核历史。

---

## 7. Agent 交付协议

离线 Gate 全部通过后：

```powershell
git push -u origin agent/<task-id>
```

Agent Final Report 必须提供：

```text
Task Base SHA
Task Branch
Task Final SHA
origin/task-branch SHA
changed files
validation results
main untouched
```

此时：

- 不删除 worktree；
- 不删除临时分支；
- 不合并 main；
- 不宣布阶段 CLOSED。

任务状态只能到：

```text
READY FOR INDEPENDENT REVIEW
```

---

## 8. GPT 独立审核门禁

GPT 只审核：

```text
Task Base SHA
→ Task Final SHA
```

并重新检查当前：

```text
origin/main
```

审核至少包含：

- 规格/不变量；
- exact diff；
- authority / persistence / disclosure；
- regression evidence；
- 是否越过 Allowed / Prohibited；
- 是否出现第二 truth / legacy dual path；
- Task Final SHA 是否为 Task Base 的可快进后代。

### Review FAIL

```text
main 不变
↓
原 task branch / worktree 继续修
↓
追加 correction commit
↓
重新 push
↓
GPT 对新的 Final SHA 重新审核
```

禁止为 correction 新开分支。

### Review PASS

进入集成门禁。

---

## 9. Main 集成门禁

集成前必须重新证明：

```text
Current origin/main == reviewed Task Base SHA
```

并证明：

```text
reviewed Task Final SHA
是 Current origin/main 的 fast-forward descendant
```

若成立，默认由 GPT 通过 GitHub 将 `main` **快进到已审核的 Task Final SHA**。

等价语义：

```text
main ref
Base SHA
↓ fast-forward
Reviewed Final SHA
```

不得：

- 新建 Merge Commit；
- squash 成一个未被审核的新 SHA；
- rebase 产生一组未审核的新 SHA；
- 在集成时顺手改代码。

因此：

> **进入 main 的就是 GPT 刚刚审核过的精确提交。**

### Main 已漂移

如果审核期间 `origin/main` 已经变化：

- 不自动 merge；
- 不让 Agent 自行 rebase；
- GPT 做 Freshness / Decision Propagation；
- 若变化无关且可安全重放，再明确生成 integration/correction 指令；
- 若触及同一 Owner/Contract，旧 Task 返回需要重新整合。

---

## 10. 合并后清理

只有确认：

```text
origin/main == Reviewed Final SHA
```

之后才清理。

由当前 Coding Agent 执行机械清理：

```powershell
git -C D:\AI\Projects\sillytavern worktree remove `
  D:\AI\Projects\.worktrees\sillytavern-agent

git -C D:\AI\Projects\sillytavern branch -d agent/<task-id>

git -C D:\AI\Projects\sillytavern push origin --delete agent/<task-id>

git -C D:\AI\Projects\sillytavern worktree prune
```

清理后验证：

```powershell
git -C D:\AI\Projects\sillytavern worktree list
git -C D:\AI\Projects\sillytavern branch --list "agent/*"
```

目标恢复为：

```text
正式 main worktree
+
无活动 Agent worktree
+
无活动 agent/* 临时分支
```

Project Owner 不需要亲自执行上述 Git 清理。

---

## 11. 不使用 PR 的默认理由

当前项目采用“单写入 Agent + GPT exact-SHA Review”，默认无需 PR：

- 不需要多人同时 code review；
- 不需要长期审查线程；
- 不希望积累 PR/branch 噪音；
- GPT 已直接审核 base→head 精确差异。

因此默认：

```text
临时 branch
→ GPT exact-SHA Review
→ fast-forward main
→ delete branch
```

只有以下情况才考虑 PR：

- 多 Agent 并行；
- 需要多人长期审查；
- main 已有并行推进；
- GitHub CI/required checks 必须绑定 PR；
- 任务跨度大到需要阶段性 discussion thread。

---

## 12. 与现有 Writer Serialization 的关系

旧规则：

```text
同一 repo 同时只有一个主要代码写入 Agent
```

继续成立。

Worktree 不是为了默认并行，而是为了**隔离风险**。

```text
Worktree isolation
!= permission for parallel overlapping writers
```

若未来真的并行，必须另行冻结 branch ownership 和 integration owner。

---

## 13. 对 Project Owner 的体验

Project Owner 默认只需要做两件事：

1. 把 GPT 给出的正式任务发送给 Coding Agent；
2. 需要时做产品体验/UAT。

以下事务默认由 GPT + Coding Agent 负责：

- 分支创建；
- worktree 创建；
- Git 状态检查；
- push task branch；
- exact-SHA review；
- main fast-forward gate；
- worktree / branch cleanup。

Project Owner 不承担 Git 冲突诊断和手工合并责任。

---

## 14. 最终原则

```text
Agent 可以把自己的施工现场弄坏
!=
Agent 可以把 main 弄坏
```

以及：

> **一个任务，一个临时分支，一个隔离 worktree；审核通过才进入 main，进入后立即清理。**
