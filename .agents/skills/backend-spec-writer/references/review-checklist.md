# Backend Spec Review Checklist

Use this checklist before asking the user to confirm each stage. The goal is concise, staged output, not a single long spec.

## Global Rules

- [ ] The current output belongs to only one stage.
- [ ] No later-stage design has been introduced before confirmation.
- [ ] The output is concise and avoids boilerplate sections.
- [ ] Open questions are visible and labeled by impact.
- [ ] Confirmed decisions are recorded.
- [ ] Assumptions are limited to low-risk details.
- [ ] At most 3 clarification questions are asked in the current round.

## Stage 1: Context, Boundary, Call Chain

- [ ] Source PRD, notes, or docs are summarized briefly.
- [ ] Conflicts are listed, not hidden.
- [ ] Ambiguities and missing information are listed.
- [ ] Feature/module/microservice boundary is clear.
- [ ] Scope and non-scope are clear.
- [ ] Upstream and downstream systems are identified.
- [ ] Communication protocol is shown for each important interaction.
- [ ] Sync vs async behavior is explicit.
- [ ] Responsibility and data ownership questions are resolved or marked open.
- [ ] A concise Mermaid call-chain diagram is included when enough information is available.
- [ ] The user is asked to confirm Stage 1 before data modeling starts.

## Stage 2: Data Modeling

- [ ] Data model is organized by domain or bounded context.
- [ ] Domain splitting is justified; if one domain is enough, the document says why.
- [ ] Aggregate roots, entities, value objects, domain services, and domain events are identified where useful.
- [ ] Persistent data ownership is clear.
- [ ] ER diagrams are domain-scoped; no oversized single ER diagram hides boundaries.
- [ ] Database table fields are grouped by domain.
- [ ] Tables include primary keys, important unique constraints, indexes, required fields, and audit/version fields where needed.
- [ ] Cross-domain references and consistency strategy are explicit.
- [ ] System architecture diagram focuses on data ownership and storage dependencies.
- [ ] State diagram exists when lifecycle state exists, or N/A is explained.
- [ ] Sequence diagram covers the main data-changing flow and important async/failure flows.
- [ ] Migration, compatibility, retention, or cleanup needs are noted when relevant.
- [ ] The user is asked to confirm Stage 2 before API/service contracts start.

## Stage 3: Service Layer And Contracts

- [ ] API/RPC/event/task contracts are defined only after data model confirmation.
- [ ] Request, response, and error behavior are clear.
- [ ] Caller identity, auth, permission, and audit requirements are described.
- [ ] Idempotency, retry, and duplicate handling are defined.
- [ ] Concurrency and transaction boundaries that affect callers are considered.
- [ ] Existing APIs, events, data, and clients are not broken, or breakage is clearly called out.
- [ ] Contract-level test points are listed.
- [ ] Remaining open questions are collected in one section.
- [ ] The user is asked to confirm Stage 3 before marking the spec ready.

## Microservice Readiness

Apply only when the mode is `microservice`.

- [ ] The reason for creating a separate service is justified.
- [ ] Runtime shape is described: API, worker, consumer, cron, or mixed.
- [ ] Configuration and secrets that affect contracts or operations are listed.
- [ ] Deployment dependencies and startup assumptions are listed.
- [ ] Health checks, metrics, logs, and alerts are defined where they affect service readiness.
- [ ] Failure handling, degradation, and recovery are described.

## Final Readiness

- [ ] Stage 1, Stage 2, and Stage 3 documents exist or have been intentionally skipped by user request.
- [ ] Each document is readable independently.
- [ ] The spec is usable by a backend developer without rereading the PRD for core decisions.
- [ ] Unresolved architecture-impacting questions prevent final approval and are marked as open.
- [ ] The output avoids project management guesses such as staffing and milestone dates.
