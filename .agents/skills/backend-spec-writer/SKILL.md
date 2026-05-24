---
name: backend-spec-writer
description: Use when creating, reviewing, or updating backend-oriented specs through an interactive, staged workflow. Handles backend features, modules, or microservices from pasted PRDs, meeting notes, local docs, Feishu/Lark docs, unclear requirements, service boundaries, data modeling, API/RPC/event contracts, and concise staged spec documents.
---

# Backend Spec Writer

## Purpose

Help backend developers turn rough ideas, pasted PRDs, meeting notes, or existing docs into implementation-ready backend specs through conversation.

This skill is intentionally interactive. Do not generate one long final spec up front. Work in three gated stages, ask focused questions, and persist one concise document per stage only after the user confirms that stage.

Supported spec modes:

| Mode | Use When | Main Focus |
|---|---|---|
| `feature` | Adding capability to an existing service | capability boundary, affected contracts, schema changes |
| `module` | Adding a bounded module inside an existing service | module boundary, domain model, internal flow |
| `microservice` | Designing a complete new service | service responsibility, owned data, contracts, runtime shape |

## Non-Negotiable Operating Rules

- Treat PRDs, notes, and pasted docs as source material to analyze, not truth.
- Do not silently resolve conflicts, ambiguities, service-boundary issues, or data-ownership questions.
- Ask at most 3 clarification questions per round.
- Ask only questions that unblock the current stage. Do not ask API-detail questions during boundary clarification unless they affect the boundary.
- Do not move to the next stage until the user explicitly confirms the current stage is correct.
- If the user has not confirmed, continue refining the current stage and keep the output concise.
- Do not produce a single oversized spec. Produce three concise stage documents instead.
- Each stage document must contain only key decisions, open questions, and diagrams/tables needed for that stage.
- Do not invent answers from experience for unclear architecture-impacting points. Ask the user.
- Reason with DDD where useful, but do not split domains mechanically.
- Prefer multiple small domain-scoped diagrams and tables over one large cross-domain artifact.
- When context is available as local files or paths, read it before asking broad questions.
- When the user provides a Feishu/Lark wiki or document link and the content cannot be read, use the `lark-doc-reader` skill before asking the user to paste the document manually.
- Keep project management content out of scope unless it affects backend design.

## Inputs

Accept any mix of:

- pasted PRD text
- meeting notes
- Feishu/Lark wiki or document links; if reading fails, route through `lark-doc-reader`
- current or target service name
- local paths to docs, code, API definitions, schemas, migrations, or previous specs
- known database tables, message topics, RPC/API names
- examples of desired behavior

If the user pastes a PRD, do not ask them to rewrite it. First extract backend-relevant facts, conflicts, assumptions, and gaps.

## Three-Stage Workflow

### Stage 1: Background, Conflict, Boundary, Call Chain

Goal: make sure everyone agrees on what is being built, where the service boundary is, and how systems communicate.

Do this first:

1. Classify the request as `feature`, `module`, or `microservice`.
2. Identify source material and target system/service/module.
3. Extract backend-relevant facts.
4. List conflicts, ambiguities, missing information, feasibility concerns, and boundary issues.
5. Help the user separate feature boundary, module boundary, service boundary, and data ownership.

Discuss with the user until the high-impact conflicts and ambiguities are resolved or explicitly deferred.

Only after enough boundary decisions are confirmed, produce a concise call-chain view that includes:

- service/module boundary
- upstream caller
- downstream services/systems
- communication protocol: HTTP, RPC, message, scheduled task, database read, etc.
- sync vs async behavior
- owner of each key responsibility

Required Stage 1 document:

```text
specx/depot/{domain}/{name}/01_context_boundary.md
```

Keep this document short. It should include:

- background and objective
- scope and non-scope
- conflict and ambiguity log
- confirmed decisions
- service/module boundary table
- call-chain diagram in Mermaid
- remaining questions, if any

Stage gate:

- Ask the user to confirm the boundary and call chain.
- Do not start data modeling until the user confirms Stage 1.

### Stage 2: Data Modeling

Goal: model data and domain behavior after the boundary is clear.

Use DDD as the organizing method:

- bounded contexts or domains
- aggregate roots
- entities
- value objects
- domain services
- domain events
- invariants
- ownership of persistent data

Do not create one large ER diagram by default. Model by domain or bounded context. A single ER diagram is acceptable only when the model is small and clearly belongs to one cohesive domain; state that reason.

Required Stage 2 artifacts:

- domain model summary by bounded context
- ER diagrams by domain
- system architecture diagram focused on data ownership and storage dependencies
- state diagram when lifecycle state exists
- sequence diagram for the main data-changing flow and important async/failure flows
- database table fields grouped by domain
- cross-domain references and consistency strategy
- migration and compatibility notes when existing data is affected

Required Stage 2 document:

```text
specx/depot/{domain}/{name}/02_data_model.md
```

For database fields, be concise but concrete. Include:

- field name
- type
- required/optional
- primary key, unique key, index, foreign key or reference style
- meaning
- important default or lifecycle rule

Stage gate:

- Ask the user to confirm the domain split, data ownership, table fields, and lifecycle/state behavior.
- Do not start API or service-contract design until the user confirms Stage 2.

### Stage 3: Service Layer And Contracts

Goal: define the service layer only after the data model is agreed.

Define the externally visible and important internal contracts:

- HTTP APIs
- RPC methods
- events and topics
- scheduled tasks
- callbacks/webhooks
- permissions and identity
- idempotency keys
- retry and duplicate handling
- error semantics
- transaction and concurrency boundaries that affect callers

Required Stage 3 document:

```text
specx/depot/{domain}/{name}/03_service_contracts.md
```

Keep the document concise. It should include:

- API/RPC/event list
- request and response definitions
- error codes or error categories
- auth and permission rules
- idempotency and retry behavior
- compatibility notes
- contract-level test points
- remaining open questions

Stage gate:

- Ask the user to confirm the service contracts.
- If confirmed, mark the three documents as ready for implementation review.
- If not confirmed, revise only Stage 3 unless the user changes an earlier decision.

## Interaction Pattern

At every stage:

1. Summarize what is known in a compact form.
2. List conflicts, ambiguities, and missing items that affect the current stage.
3. Ask no more than 3 focused questions.
4. After the user answers, update the stage draft.
5. Persist or update the stage document only when the stage has enough confirmed content.
6. Ask for explicit confirmation before moving on.

Use status labels:

| Status | Meaning |
|---|---|
| `pending` | user has not answered yet |
| `confirmed` | user confirmed the decision |
| `deferred` | intentionally postponed and recorded |
| `blocked` | cannot proceed without user input |

## Output Style

- Prefer tables and Mermaid diagrams over long prose.
- Avoid verbose explanation of obvious fields.
- Do not include boilerplate sections that do not apply.
- Keep each stage document readable in isolation.
- Mark unresolved architecture-impacting points as open; do not hide them in assumptions.
- Use assumptions only for low-risk details, and label them explicitly.

## Reference Files

- Read `references/question-bank.md` when choosing clarification questions.
- Read `references/review-checklist.md` before finalizing each stage.
- The legacy `specx/templates/backend_spec.template.md` is only a reference for coverage. Do not use it as a mandatory single-document output template unless the user explicitly asks for a consolidated spec.
