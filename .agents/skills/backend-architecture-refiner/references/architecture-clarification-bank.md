# Architecture Clarification Bank

Only ask questions that materially affect:

- architecture
- runtime semantics
- ownership
- consistency
- contracts
- operational behavior

The goal is uncertainty reduction.

Not conversational completeness.

---

# Phase 0 — Input & Context Acquisition

## Source Material

- Do you already have a PRD or requirement document?
- Do you already have workflow descriptions?
- Do you already have sequence diagrams?
- Do you already have API/event/topic definitions?
- Do you already have schemas?
- Do you already have repository or code context?
- Are runtime or operational expectations already known?

---

## System Shape Discovery

Is the system primarily:

- workflow-oriented
- event-driven
- orchestration-oriented
- integration-oriented
- domain-oriented
- pipeline-oriented

?

---

## Constraint Discovery

- Are consistency requirements already known?
- Are scaling expectations already known?
- Are deployment/runtime constraints already known?
- Are retry/order guarantees already known?

---

## Ownership Discovery

- Which systems already exist around this service?
- Which system is authoritative?
- Is this service a source of truth or a projection/cache?

---

# Phase 1 — Context & Constraints

## Runtime Constraints

- What traffic shape is expected?
- What throughput expectations exist?
- What latency expectations exist?
- Which flows are burst-sensitive?
- Which flows are retry-heavy?

---

## Consistency

- Which operations require strong consistency?
- Which operations tolerate eventual consistency?
- Is read-after-write required?
- Are stale reads acceptable?

---

## Failure Sensitivity

- Do downstream failures affect user-visible behavior?
- How should partial success be handled?
- Which failures are catastrophic?

---

## Design Drivers

- Which constraints dominate the architecture?
- Which runtime concerns matter most?
- Which assumptions are highest risk?
- Which unknowns could invalidate the design?

---

# Phase 2 — Runtime Semantics & Data Modeling

## Lifecycle & State

- Which states are terminal?
- Which states are retryable?
- Can terminal states be reopened?
- Which transitions are invalid?

---

## Retry & Idempotency

- How should duplicate requests be handled?
- How should duplicate events be handled?
- Which operations must be idempotent?
- Which retries are dangerous?

---

## Ordering & Replay

- Can events arrive out of order?
- What ordering guarantees exist?
- Is replay allowed or forbidden?
- Which operations depend on ordering?

---

## Failure Semantics

- How should partial downstream success be handled?
- How should late callbacks/events be handled?
- Is compensation required?
- What is the deadletter strategy?

---

## Persistence & Ownership

- Which data is authoritative?
- Which data is cached or derived?
- Which service owns persistence?
- Are cross-service writes allowed?

---

# Phase 3 — Contracts & Interfaces

## Contract Semantics

- Which guarantees does the contract provide?
- Which guarantees does the contract explicitly NOT provide?
- What retry behavior is caller-visible?
- What timeout semantics are caller-visible?
- What idempotency guarantees exist?

---

## Error Semantics

- Which failures are retryable?
- Which failures are terminal?
- Which failures are caller-visible?

---

## Auth & Permissions

- Which callers are trusted?
- Which operations require authorization?
- Which operations require auditability?

---

## Contract Risks

- Which assumptions remain unresolved?
- Which contracts are highest risk?
- Which runtime semantics remain unclear?
- Which guarantees may change later?