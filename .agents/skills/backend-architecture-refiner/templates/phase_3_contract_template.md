# Phase 3 — Contracts & Interfaces

# 1. Contract Overview

Brief summary of exposed contracts.

State the contract boundary:

- external caller-facing interfaces
- internal service/module interfaces
- async/event/task interfaces
- interfaces intentionally out of scope

---

# 2. Required Implementation Interface Inventory

List every interface that must be implemented. This is a completeness pass, not
a representative sample.

## 2.1 Interface Discovery Sources

| Source | Required Interface Implication | Covered By |
|---|---|---|
| User-visible use case |  |  |
| Lifecycle/state transition |  |  |
| Owned data mutation |  |  |
| Required read/query |  |  |
| Event producer |  |  |
| Event consumer |  |  |
| Scheduled/async job |  |  |
| Webhook/callback |  |  |
| Import/export/batch path |  |  |
| Permission-sensitive operation |  |  |
| Operator/admin action |  |  |

## 2.2 Interface Inventory

| Interface ID | Name | Type | Sync/Async | Caller/Producer | Callee/Consumer | Purpose | Must Implement Now? | Notes |
|---|---|---|
| IF-001 |  | API/RPC/Event/Task/Webhook/Batch/Internal Method |  |  |  |  | Yes/Deferred/Rejected |  |

## 2.3 Coverage Check

| Requirement / Flow / State Transition | Interface IDs | Gap |
|---|---|---|
|  |  |  |

If a requirement has no interface, explain why no backend interface is needed.

---

# 3. Interface Definition

Repeat this section for every `Must Implement Now = Yes` interface.

For deferred interfaces, keep a short deferred contract note in section 4.

## Name

### Interface ID

```text
IF-001
```

---

### Interface Type

```text
API / RPC / Event / Task / Webhook / Batch / Internal Method
```

---

### Endpoint / Topic / RPC

```text
POST /example
```

---

### Purpose

- ...

---

### Caller / Producer

- ...

---

### Callee / Consumer

- ...

---

### Request Semantics

| Field | Meaning |
|---|---|

---

### Response Semantics

| Field | Meaning |
|---|---|

---

### Retry Semantics

- retryable:
- duplicate-safe:
- replay-safe:

---

### Ordering Semantics

- ordering required:
- ordering guarantee:

---

### Timeout Semantics

- timeout behavior:
- timeout ownership:

---

### Consistency Semantics

- strong/eventual:
- read-after-write expectation:

---

### Error Categories

| Error | Retryable | Notes |
|---|---|---|

---

### Auth & Permission

| Requirement | Notes |
|---|---|

---

### Compatibility & Versioning

| Concern | Decision |
|---|---|

---

### Observability

| Signal | Purpose |
|---|---|

---

# 4. Deferred / Rejected Interfaces

| Interface | Status | Reason | Revisit Trigger |
|---|---|---|---|
|  | Deferred/Rejected |  |  |

---

# 5. Contract-Level Risks

| Risk | Why It Matters |
|---|---|

---

# 6. Contract-Level Test Points

| Scenario | Expected Behavior |
|---|---|

---

# 7. Remaining Unresolved Questions

| Question | Impact |
|---|---|

---

# 8. Preferred Artifact Output

Generate when stable:

- OpenAPI
- protobuf
- AsyncAPI
- SQL schema draft
