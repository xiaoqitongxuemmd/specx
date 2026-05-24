# Backend Spec Question Bank

Use this file to choose focused clarification questions. Ask at most 3 questions per round. Ask questions for the current stage only.

## Stage 1: Background, Conflict, Boundary, Call Chain

### Request Classification

1. Is this a `feature`, `module`, or `microservice`?
2. Which business domain, system, service, or repository does it belong to?
3. What source material should be used: pasted PRD, file path, existing code path, API docs, schema, or meeting notes?

### PRD Digestion

- What user-visible goal is the PRD trying to achieve?
- Which parts of the PRD are explicitly in scope?
- Which parts are explicitly out of scope?
- Which statements conflict with each other?
- Which requirements affect backend contracts, data, or state?
- Which requirements appear to be frontend-only or process-only?

### Conflicts And Ambiguities

- Which two source statements cannot both be true?
- Which requirement has multiple valid interpretations?
- Which missing detail changes service boundary, data ownership, or API contract?
- Which decision can be safely deferred?
- Which decision blocks the next stage?

### Service And Feature Boundary

- Which service/module should own this capability?
- Which service owns the data being created or modified?
- Which upstream systems call this capability?
- Which downstream systems must be called or notified?
- Is this service authoritative, or does it mirror external state?
- Which operations must remain inside another service?
- What is explicitly outside this feature/module/microservice?

### Call Chain

- What is the entry point: HTTP, RPC, message, scheduled task, callback, or manual operation?
- Which interactions must be synchronous?
- Which interactions can be asynchronous?
- What protocol is used for each upstream/downstream communication?
- What happens if a downstream service fails?
- Which step owns the final user-visible result?

## Stage 2: Data Modeling

### Domain Split

- What bounded context or domain does each concept belong to?
- Can the model stay in one cohesive domain, or does the business language clearly indicate multiple domains?
- Which domain owns each persistent table or record?
- Which data is read-only from another service?
- Which cross-domain references should use IDs instead of direct joins?

### Aggregates And Objects

- What are the aggregate roots?
- Which objects are entities, and which are value objects?
- Which invariants must be protected inside an aggregate?
- Which domain services are needed because the behavior does not belong to a single entity?
- Which domain events should be emitted for other modules/services?

### Tables And Fields

- What entities are created, updated, queried, or deleted?
- Which fields are required, optional, unique, or indexed?
- What is the primary key strategy?
- Are soft deletes, audit fields, version fields, or tenant fields required?
- Are there existing tables that must be reused or migrated?
- Are there retention, archive, or cleanup rules?

### State, Idempotency, Concurrency

- Does the object have lifecycle states?
- Which state transitions are allowed?
- Can a terminal state be retried, reopened, or recreated?
- What happens under duplicate requests?
- What happens if a callback/event arrives late or out of order?
- What concurrency conflicts must be prevented?
- What transaction boundary protects each invariant?

### Data Diagrams

- Which domain needs its own ER diagram?
- Is a single ER diagram small and cohesive enough to be acceptable?
- Which state diagram is required for lifecycle behavior?
- Which sequence diagram proves the main data-changing flow?
- Which async or failure sequence changes persisted state?

## Stage 3: Service Layer And Contracts

### API, RPC, Events

- Which operations are required?
- Is each operation HTTP, RPC, event, scheduled task, callback, or internal method?
- Who calls each operation?
- What request fields are required?
- What response fields are required?
- What errors must be visible to callers?
- Is backward compatibility required for existing APIs?

### Permissions, Audit, Security

- Who can create, update, approve, cancel, or delete?
- Is caller identity trusted from upstream, token, session, or service credential?
- Is tenant/team/project isolation required?
- Which operations require audit logs?
- Are secrets, credentials, or sensitive fields involved?

### Idempotency, Retry, Error Semantics

- What is the request identity or idempotency key?
- Which duplicate calls should return the previous result?
- Which errors are retryable?
- Which errors are terminal?
- What timeout or downstream failure behavior is visible to callers?

### Compatibility And Testing

- Which existing clients or consumers must remain compatible?
- Which contract tests protect upstream/downstream integrations?
- Which integration tests prove DB/API/event behavior?
- Which failure cases must be covered?
- What acceptance criteria are testable from the service contract?

## Microservice-Specific Questions

Use these in the relevant stage, not all at once.

- Why should this be a separate microservice instead of a module?
- What is the service's single responsibility?
- Which data does the service own exclusively?
- Which dependencies are hard requirements at startup?
- What should happen when downstream dependencies fail?
- How is the service deployed: API server, worker, cron job, consumer, or mixed?
- What metrics and alerts define service health?
