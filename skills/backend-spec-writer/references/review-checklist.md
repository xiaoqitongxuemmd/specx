# Backend Spec Review Checklist

Use this checklist before finalizing a backend spec.

## Requirement Quality

- [ ] The source PRD or notes are summarized.
- [ ] Conflicts are listed, not hidden.
- [ ] Ambiguities and missing information are listed.
- [ ] Confirmed decisions are recorded.
- [ ] Remaining assumptions are explicit.
- [ ] Scope and non-scope are clear.

## Backend Design

- [ ] Service/module responsibility is clear.
- [ ] Upstream and downstream dependencies are identified.
- [ ] Data model is organized with DDD concepts where useful: bounded context, aggregate root, entity, value object, domain service, domain event.
- [ ] Domain splitting is justified; if one domain is enough, the spec says so instead of forcing unnecessary boundaries.
- [ ] ER diagrams are split into small domain-scoped diagrams when multiple domains exist; no single oversized ER diagram hides domain boundaries.
- [ ] API/RPC/event/task contracts are defined.
- [ ] Request, response, and error behavior are clear.
- [ ] Data ownership and aggregate ownership are clear.
- [ ] Tables/entities include primary keys, unique constraints, indexes, and audit fields where needed.
- [ ] State transitions are explicit when lifecycle state exists.
- [ ] Idempotency, retry, and duplicate handling are defined.
- [ ] Concurrency and transaction boundaries are considered.

## Microservice Readiness

- [ ] The reason for creating a separate service is justified.
- [ ] Runtime shape is described: API, worker, consumer, cron, or mixed.
- [ ] Configuration and secrets are listed.
- [ ] Deployment dependencies and startup assumptions are listed.
- [ ] Health checks, metrics, logs, and alerts are defined.
- [ ] Failure handling, degradation, and recovery are described.

## Compatibility And Operations

- [ ] Existing APIs/data/users are not broken, or breakage is clearly called out.
- [ ] Migration and rollback strategy are described.
- [ ] Permission and audit requirements are described.
- [ ] Sensitive data handling is described.
- [ ] Observability is sufficient for debugging.

## Testing

- [ ] Unit test scenarios are tied to domain rules.
- [ ] Integration tests cover database and external contracts.
- [ ] Error paths and edge cases are covered.
- [ ] Migration tests are included if schema changes are required.
- [ ] Acceptance criteria are testable.

## Final Output

- [ ] Open questions are collected in one section.
- [ ] The spec is usable by a backend developer without rereading the PRD.
- [ ] The spec avoids project management guesses such as staffing and milestone dates.
