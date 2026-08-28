# Skill Repository Consolidation｜Migration Provenance

Status: completed snapshot migration  
Migration date: 2026-08-28  
Source repository: `zhangchenjia21-dot/Skill`  
Source branch: `main`  
Source HEAD: `f3051774be80dce9509aca3bac9c1a457c6b9794`

## 1. Purpose

原独立 Skill 仓库的 current 内容并入 `zhangchenjia21-dot/Vibe-Coding/skill/`，使项目治理、跨项目经验和可复用执行 Skill 共享同一个 governance repository，同时保持不同 Authority。

迁移后：

```text
Vibe-Coding/skill/
= current reusable Skill authority

old zhanchengjia21-dot/Skill
= historical source only; no new authoritative writes
```

## 2. Path Mapping

```text
old Skill/AGENTS.md
→ Vibe-Coding/skill/AGENTS.md  (rewritten for subtree governance)

old Skill/README.md
→ Vibe-Coding/skill/README.md  (rewritten for merged navigation)

old Skill/skill/<runtime>/<name>/SKILL.md
→ Vibe-Coding/skill/<runtime>/<name>/SKILL.md
```

## 3. Imported Current Skill Snapshot

The following source blobs were recreated in Vibe-Coding before publication:

```text
GPT
agent-task-packet              8096b3c1f59cff14677e991e475b93a595fc5059
grill-me                       7197718afda1aadaaa7f5ffd82cfec01447ff850
lifecycle-dev-process          220b8640457b2223bbd4762e1aa37a18f511cb89
lifecycle-templates            6aaf26ffff4375a46ac85bed564e14bf5f880393
tavern-asset                   ffc32a34d917042c7cf9d523d4175dfc21300243
tavern-creator-import-draft    7da43ea29e52bcb1ae9de05ae2199ae457a10ab8
长上下文交接                    cf0ff981223bece4487ca70b4935ec1ee789368c

DSH
lifecycle-dev-process          c98349d06e1871dd23d206134c1e2f0a39bd785a
lifecycle-templates            a7ab0aa216820e2faa9b9fceac1a76729bec7c8c
project-thread-handoff         97752e7e07af246c394cfa1a703f0fdb1555b601
tavern-asset                   40c972d41a294d5fc15c4bfdd5511a21ef868e28
tavern-creator-import-draft    d6601cda9f84482fc468d4f53285f7ac1f2d560e
user-profile                   1a744466ba1d99857de945a80029c0eb46e2ca19
```

The matching blob SHA demonstrates byte-for-byte equality for the imported snapshot before any intentional navigation/routing edits made after consolidation.

## 4. History Boundary

This consolidation preserves the **current source snapshot and exact source HEAD provenance**, but it does **not graft the old repository's entire standalone commit graph into the Vibe-Coding Git history**.

Therefore, if the old `zhangchenjia21-dot/Skill` repository is permanently deleted, its repository-specific historical commits will no longer be available from GitHub. If that history matters, archive the old repository instead of permanently deleting it, or keep an external mirror/bundle.

Current operation does not depend on the old repository after the consolidated paths and stale references are updated.

## 5. Authority After Migration

```text
User current instruction
> project current Product / Architecture / Contract / Roadmap
> verified implementation truth
> Vibe-Coding cross-project governance
> Vibe-Coding/skill reusable defaults
> historical evidence
```

Same repository does not imply same semantic ownership.

## 6. Completion Checks

Migration is complete only after:

- all current Skill files exist under `Vibe-Coding/skill/`;
- root `AGENTS.md` and `README.md` point to the new subtree;
- `skill/AGENTS.md` and `skill/README.md` define subtree authority;
- active project / implementation navigation no longer depends on the old Skill repository;
- stale-reference audit is completed;
- old repository receives no further authoritative writes.
