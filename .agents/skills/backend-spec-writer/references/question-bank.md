# Backend Spec Question Bank

Use this file to choose focused clarification questions. Ask at most 3 questions per round.

## First Round

1. Is this a `feature`, `module`, or `microservice`?
2. Which business domain, system, service, or repository does it belong to?
3. What source material should be used: pasted PRD, file path, existing code path, API docs, schema, or meeting notes?

## PRD Digestion

- What user-visible goal is the PRD trying to achieve?
- Which parts of the PRD are explicitly in scope?
- Which parts are explicitly out of scope?
- Which statements conflict with each other?
- Which requirements affect backend contracts, data, or state?
- Which requirements appear to be frontend-only or process-only?

## Service Boundary

- Which service owns this capability?
- Which service owns the data being created or modified?
- Which upstream systems call this service?
- Which downstream systems must be called or notified?
- Is this service authoritative, or does it mirror external state?
- Which operations must remain inside another service?

## API, RPC, Events

- Is the entry point HTTP, gRPC/RPC, message event, scheduled task, or manual operation?
- What operations are required?
- Which operation must be synchronous?
- Which operation can be asynchronous?
- What are the request identity and idempotency keys?
- What errors must be visible to callers?
- Is backward compatibility required for existing APIs?

## Data Model

- What bounded context or domain does each concept belong to?
- Can the model stay in one cohesive domain, or does the business language clearly indicate multiple domains?
- What are the aggregate roots?
- Which objects are entities, and which are value objects?
- Which invariants must be protected inside an aggregate?
- Are there cross-domain references that should use IDs instead of direct joins?
- Are there domain events that other modules/services need to consume?
- What entities are created, updated, queried, or deleted?
- Which fields are required, optional, unique, or indexed?
- What is the primary key strategy?
- Are soft deletes, audit fields, or version fields required?
- Does existing data need migration?
- Are there retention, archive, or cleanup rules?

## State, Idempotency, Concurrency

- Does the object have lifecycle states?
- Which state transitions are allowed?
- Can a terminal state be retried, reopened, or recreated?
- What happens under duplicate requests?
- What happens if a callback/event arrives late or out of order?
- What concurrency conflicts must be prevented?

## Permissions, Audit, Security

- Who can create, update, approve, cancel, or delete?
- Is the caller identity trusted from upstream, token, session, or service credential?
- Is tenant/team/project isolation required?
- Which operations require audit logs?
- Are secrets, credentials, or sensitive fields involved?

## Microservice-Specific

- Why should this be a separate microservice instead of a module?
- What is the service's single responsibility?
- Which data does the service own exclusively?
- Which dependencies are hard requirements at startup?
- What should happen when downstream dependencies fail?
- What metrics and alerts define service health?
- How is the service deployed: API server, worker, cron job, consumer, or mixed?

## Testing

- Which unit tests prove the domain rules?
- Which integration tests prove DB/API/event behavior?
- Which contract tests protect upstream/downstream integrations?
- Which migration tests are required?
- Which failure cases must be covered?
