# Architecture Review Checklist

The purpose of this checklist is architecture validation.

Not document completeness.

Use this checklist before asking the user to confirm each phase.

---

# Global Review Rules

- [ ] The current output belongs to only one phase.
- [ ] Later phases have not been prematurely generated.
- [ ] Architecture-impacting unknowns are explicitly visible.
- [ ] Risks and assumptions are separated.
- [ ] Ownership semantics are explicit.
- [ ] Source-of-truth semantics are explicit.
- [ ] Runtime semantics are treated as first-class concerns.
- [ ] The output avoids fake completeness.
- [ ] Architectural uncertainty is decreasing phase-by-phase.

---

# Phase 0 — Input & Context Acquisition

- [ ] Source material has been requested when missing.
- [ ] PRD/workflow/runtime context has been identified.
- [ ] Upstream/downstream systems have been identified when relevant.
- [ ] Runtime expectations have been identified when relevant.
- [ ] Ownership-sensitive areas have been identified.
- [ ] Missing architecture-critical inputs are visible.
- [ ] Premature architecture generation has not started.
- [ ] The system shape is roughly understood before Phase 1.

---

# Phase 1 — Context & Constraints

- [ ] System objectives are clear.
- [ ] Scope and non-scope are explicit.
- [ ] Upstream/downstream systems are identified.
- [ ] Communication patterns are explicit.
- [ ] Sync vs async boundaries are explicit.
- [ ] Ownership boundaries are explicit.
- [ ] Source-of-truth mapping is explicit.
- [ ] Runtime assumptions are explicit.
- [ ] Scalability expectations are explicit.
- [ ] Consistency expectations are explicit.
- [ ] Design drivers are explicitly listed.
- [ ] Architecture-impacting unknowns are visible.
- [ ] Risks are visible.
- [ ] Retry-sensitive interactions are identified.
- [ ] Ordering-sensitive interactions are identified.
- [ ] Failure-sensitive areas are identified.
- [ ] The user was explicitly asked to confirm Phase 1.

---

# Phase 2 — Runtime Semantics & Data Modeling

- [ ] Lifecycle/state transitions are explicit.
- [ ] Retry behavior is explicit.
- [ ] Duplicate-delivery behavior is explicit.
- [ ] Ordering assumptions are explicit.
- [ ] Replay semantics are explicit.
- [ ] Timeout behavior is explicit.
- [ ] Partial-failure behavior is modeled.
- [ ] Compensation behavior is defined when necessary.
- [ ] Failure amplification risks are identified.
- [ ] Persistence ownership is explicit.
- [ ] Consistency semantics are explicit.
- [ ] Strong vs eventual consistency expectations are explicit.
- [ ] Async/event flows are modeled.
- [ ] Failure flows are modeled.
- [ ] Operational risks are visible.
- [ ] Architecture-impacting unresolved items remain visible.
- [ ] The user was explicitly asked to confirm Phase 2.

---

# Phase 3 — Contracts & Interfaces

- [ ] Contracts reflect runtime semantics.
- [ ] All required implementation interfaces are listed, not just examples.
- [ ] Interface inventory covers APIs, RPCs, events, async jobs/tasks, webhooks/callbacks, batch paths, and internal service/module methods when relevant.
- [ ] Interface inventory is traced back to use cases, lifecycle transitions, mutations, queries, events, permission-sensitive operations, and operator/admin actions.
- [ ] Every requirement/flow/state transition has either an interface ID or an explicit explanation for why no backend interface is needed.
- [ ] Deferred or rejected interfaces are listed with reasons.
- [ ] Retry behavior visible to callers is explicit.
- [ ] Idempotency semantics are explicit.
- [ ] Ordering guarantees are explicit.
- [ ] Timeout semantics are explicit.
- [ ] Async semantics are explicit.
- [ ] Auth and permission boundaries are explicit.
- [ ] Contract-level risks are visible.
- [ ] Contract-level test points are listed.
- [ ] Remaining unresolved architecture-impacting items are collected in one place.
- [ ] The user was explicitly asked to confirm Phase 3.

---

# Final Readiness

- [ ] Runtime semantics are understandable.
- [ ] Ownership semantics are understandable.
- [ ] Failure semantics are understandable.
- [ ] Consistency semantics are understandable.
- [ ] Remaining unresolved architecture-impacting questions are visible.
- [ ] No fake certainty has been introduced.
