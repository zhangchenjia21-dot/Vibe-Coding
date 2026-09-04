# G5-03 Agency Closeout & Faction Deferral v1.0 Decision

Status: **FROZEN / CURRENT CANONICAL**  
Date: 2026-09-04  
Capability: **G5-03 NPC / Faction Agency**  
Decision owner: GPT under roadmap-delegated post-consumer closure authority

## 1. Decision

**G5-03 is ENGINEERING PASS / CLOSED after MW-001.**

Do **not** add a separate Faction-agency implementation slice now.

This is a consumer-first deferral. It is not a claim that Faction agency is already implemented, and it is not a permanent ban on Faction semantics.

## 2. Why G5-03 can close

The capability now proves the first real independent-actor vertical:

```text
accepted ordinary player Narrative
→ post-Narrative semantic materialization
→ standalone Agency Selector on latest world
→ 0..4 current stable NPC actors
→ actor-scoped requests using own material + own Knowledge + own history
→ optional durable actor actions
→ foreground Player action always wins
```

The stable actor system now supports all required NPC ingress families:

```text
Guaranteed Source NPC
+ automatic Source-backed NPC
+ creation-authored Game-local NPC without Card
+ runtime Narrative-materialized NPC without Card
```

A Character Card is material, not identity authority. All stable actors use Program-owned Game-local identity.

This is enough to establish the G5-03 product thesis that non-player actors can independently create history without turning the game into a universal simulator.

## 3. Why a Faction slice is not justified now

The roadmap explicitly said Faction identity/agency must not be built merely for symmetry and delegated the post-consumer decision to GPT.

G5-02 also explicitly deferred Faction/shared knowledge.

At G5-03 closeout there is still no concrete consumer requiring all of:

```text
stable Faction identity
+ Faction-private/shared Knowledge
+ Faction-local actor material
+ Faction action history
+ independent Faction execution authority
```

Creating those abstractions now would be platform-first work without a first consumer.

Protected planning principle:

> **Vertical before platform. Consumer before infrastructure.**

Therefore no `Faction Actor` type, shared Faction Knowledge model, Faction scheduler, or generic relationship ontology is introduced merely to make the G5-03 label symmetrical.

## 4. What this decision does NOT mean

Do not interpret closure as any of the following:

- Factions are equivalent to NPCs;
- an NPC's private Knowledge is automatically Faction knowledge;
- registry membership implies Faction membership;
- multiple NPC actions equal a Faction action;
- Faction agency has been implemented;
- Faction identity may be inferred from display names or prose labels;
- future Faction work is prohibited.

## 5. Future pull rule

Faction capability may be introduced later only when a concrete product consumer proves that existing stable-NPC agency + durable world consequences are insufficient.

Likely pull points include:

- **G5-04 Event / Priority-driven World Evolution**, if a pressure/event must be owned by a persistent collective rather than represented as world pressure or individual actor action;
- a later Faction-facing product surface that needs durable collective identity/state;
- a real shared-knowledge consumer that cannot be represented safely as individual Knowledge Provenance.

When that happens:

1. identify the concrete consumer first;
2. define the smallest required Faction semantics;
3. do not retrofit a universal Faction simulation platform;
4. mint a distinct flat Work Item only if there is an independently executable implementation outcome.

## 6. Protected G5-03 semantics after closure

Do not reopen absent concrete regression or a new consumer:

- Model Freedom First / Visible Narrative First;
- Narrative acceptance independent of semantic/agency extraction;
- standalone Agency Selector after semantic terminal;
- ordinary accepted turn marks dirty; selector start consumes the opportunity once;
- foreground Player action invalidates uncommitted background work;
- committed actor actions remain durable;
- selector fan-out safety ceiling 0..4;
- separate actor-scoped execution requests;
- actor-private material / Knowledge / history;
- Program-owned stable actor identity;
- display name never authoritative identity;
- runtime Narrative actor origin bound to exact accepted turn/hash;
- stale runtime actors retained physically but excluded from current registry/Agency;
- no automatic Knowledge from registry membership/materialization;
- no runtime mutable Source lookup;
- Save/Restore preserves stable actor identity/history;
- no semantic `agency_candidates` selection authority.

## 7. Evidence

Implementation repository closeout/review:

- `docs/g5_03/G5-03M2A_INDEPENDENT_REVIEW_IR2.md`
- `docs/g5_03/MW-001_INDEPENDENT_REVIEW_IR1.md`
- `docs/g5_03/G5-03_CLOSEOUT.md`

Parent real G5-03 Provider proof remains honestly:

`PENDING / EXTERNAL PROVIDER UNAVAILABLE`

No Provider switch is authorized merely to manufacture evidence.

## 8. Next

Proceed to **G5-04 Event / Priority-driven World Evolution architecture shaping**.

Do not start G5-04 implementation until its smallest pressure/priority/event consumer and authority boundaries are frozen.