# Task Identity & Lineage Governance v1.0

Status: **CURRENT / OWNER-APPROVED**  
Approved: 2026-09-04  
Scope: cross-project roadmap/task planning, Agent Task Packets, Independent Review correction, Owner-inserted work

## 1. Problem this rule solves

Roadmap position, executable work identity, and execution history are different facts.

Do not encode all three into one recursively growing identifier such as:

```text
G5-03M1R01C02...
```

That form makes current work hard to read, mixes planning hierarchy with review history, and encourages patch genealogy to become the task namespace.

## 2. Dual-coordinate model

Every executable work item should distinguish:

```text
Roadmap / Capability Anchor
!=
Executable Work Item ID
```

### 2.1 Roadmap / Capability Anchor

A stable product or stage coordinate, for example:

```text
G5-03
```

It answers: **where does this capability belong in the product/task axis?**

Anchors may have bounded planning decomposition, but they are not required to carry every execution/review event in their string.

### 2.2 Executable Work Item ID

A short, flat, project-local identifier, for example:

```text
MW-001
MW-002
MW-003
```

It answers: **which independently executable unit of work is this?**

Rules:

- use a stable project prefix plus flat serial number unless the project has another approved short namespace;
- do not encode arbitrary parent depth, correction rounds, review rounds, owner changes, or dependency chains into the ID;
- once assigned, the Work Item ID is immutable;
- display name remains human-readable and may evolve without changing identity.

## 3. Lineage belongs in metadata

Relationships must be explicit metadata rather than suffixes embedded in Task ID.

Use fields when relevant:

```text
Capability-Anchor:
Parent:
Depends-On:
Blocks:
Supersedes:
Fixes:
Triggered-By:
Inserted-By:
Revision:
Review-Round:
```

Not every task needs every field.

## 4. Revision vs new Work Item

### 4.1 Same Outcome, implementation defect or incomplete proof

If Independent Review finds a defect but the task's primary Outcome, architecture boundary, owner, and acceptance intent remain the same:

```text
same Work Item ID
+ Revision n+1
+ Review-Round n+1 when re-reviewed
```

Do **not** mint recursive IDs such as `C01`, `R02`, `PATCH2` solely to record the repair round.

The active repository-native Task Packet should be revised in place, with the review finding and focused correction scope recorded explicitly.

### 4.2 New independent Outcome / seam / Owner-inserted goal

Mint a new flat Work Item ID when the discovered or inserted work is independently meaningful, for example:

- a new architecture seam or contract is required;
- the Owner inserts a distinct implementation goal;
- the correction has a separate outcome/owner/gate;
- the original task can remain valid while the new work is tracked separately;
- a newly discovered prerequisite deserves its own acceptance gate.

Express the relationship with metadata such as `Triggered-By`, `Blocks`, `Depends-On`, or `Inserted-By`.

### 4.3 Canonical product semantics changed

If Owner/current canonical changes invalidate the task's previous Outcome or implementation premise:

```text
old Work Item → SUPERSEDED / STOPPED
new Work Item → new flat ID
```

Do not preserve a semantically obsolete task merely by appending another suffix.

## 5. Task-axis planning rule

Task hierarchy belongs to planning; execution identity stays flat.

A roadmap may describe:

```text
Stage
→ Capability
→ Milestone / slice
```

but once a slice becomes an executable Agent Task Packet, assign a short Work Item ID and keep the roadmap relationship in metadata.

Planning labels such as `M1`, `M2`, `A`, `B` may still be useful as human descriptions, but they must not become an indefinitely recursive executable namespace.

## 6. Independent Review notation

Independent Review history is written as metadata/evidence:

```text
Work Item: MW-001
Revision: 2
Review-Round: IR#2
```

not as:

```text
MW-001R02C01
```

A correction that does not change the Outcome remains the same Work Item.

## 7. Legacy / in-flight migration

Do not rename active in-flight tasks merely to make history aesthetically consistent.

For legacy IDs already in execution:

- preserve the existing Task ID/path;
- apply `Revision` / `Review-Round` metadata for subsequent correction rounds;
- do not append new recursive correction/review suffixes;
- assign the new flat Work Item namespace to the next genuinely new executable work item when practical.

Historical IDs remain valid references and are not rewritten solely for cleanup.

## 8. Task Packet identity block

Preferred form for new work:

```markdown
# TASK｜MW-001｜Stable Actor Registry Foundation
Type: implementation
Owner: Kimi
Capability-Anchor: G5-03
Revision: 1
Review-Round: 0
Depends-On: ...
Triggered-By: ...
Base: <repo / ref / HEAD>
```

Only include lineage fields that carry real information.

## 9. Decision rule summary

```text
Roadmap position changed?        → update Capability Anchor / Task Axis
Same outcome, defect found?      → same Work ID + Revision
Independent Review repeated?     → same Work ID + Review-Round
Distinct new outcome inserted?   → new flat Work ID + lineage metadata
Canonical premise superseded?    → supersede old + mint new Work ID
```

> **Hierarchy explains where work belongs. Identity says which work it is. Lineage explains what happened to it. Do not make one string carry all three.**
