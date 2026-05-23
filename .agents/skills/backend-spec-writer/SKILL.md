---
name: backend-spec-writer
description: Use when creating, reviewing, or updating backend-oriented specs through conversation, including backend features, modules, or complete microservices. Handles pasted PRDs, unclear or conflicting requirements, API/RPC contracts, database design, state machines, idempotency, migrations, observability, and test plans.
---

# Backend Spec Writer

## Purpose

Help backend developers turn rough ideas, pasted PRDs, meeting notes, or existing docs into an implementation-ready backend spec.

The skill supports three spec modes:

| Mode | Use When | Main Focus |
|---|---|---|
| `feature` | Adding capability to an existing service | API changes, schema changes, compatibility, tests |
| `module` | Adding a bounded module inside an existing service | module boundary, domain model, internal flow |
| `microservice` | Designing a complete new service | service responsibility, contracts, storage, deployment, SLOs |

## Operating Rules

- Do not assume the PRD is correct. Treat PRDs and notes as input material to analyze.
- If requirements conflict, are vague, or blur service boundaries, classify the issue and ask only architecture-impacting questions first.
- Do not silently resolve major conflicts. Propose 1-2 reasonable interpretations and ask the user to confirm.
- Record confirmed decisions in the spec under "Requirement Clarification Record".
- Model data with a DDD mindset: identify bounded contexts, aggregates, entities, value objects, domain services, and domain events where useful.
- Do not split domains mechanically. If one bounded context can keep the model cohesive and simple, keep it together and explain the reason.
- Prefer short clarification rounds: ask at most 3 questions at a time.
- When context is available as local files or paths, read it before asking broad questions.
- When the user provides a Feishu/Lark wiki or document link and the content cannot be read, use the `lark-doc-reader` skill to handle MCP setup or user reauthorization before asking the user to paste the document manually.
- Keep project management content out of scope unless it affects backend design. Do not invent staffing, milestones, or delivery dates.
- If the user asks for implementation after the spec is done, switch to normal coding workflow and use the spec as source context.

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

## Workflow

### 1. Classify The Request

Identify:

- mode: `feature`, `module`, or `microservice`
- whether this is new spec, update existing spec, or review only
- source material: pasted PRD, file path, existing code, or free-form description
- target system/service/module, if known

If unclear, ask:

1. Is this a `feature`, `module`, or `microservice` spec?
2. Which system/service/domain does it belong to?
3. Do you have existing PRD/docs/code paths to use as input?

### 2. Digest Input Material

Summarize backend-relevant information:

- goals and non-goals
- actors and upstream/downstream systems
- required operations
- data entities
- state transitions
- API/RPC/events/tasks
- permissions and audit requirements
- non-functional requirements

Then classify problems:

| Problem Type | Meaning |
|---|---|
| conflict | Two source statements cannot both be true |
| ambiguity | A requirement has multiple valid interpretations |
| missing | Backend-required information is absent |
| boundary | Responsibility between systems/services is unclear |
| feasibility | Requirement may be expensive, unsafe, or hard to operate |

### 3. Clarify High-Impact Gaps

Ask only the questions that affect architecture or contracts first:

- data ownership
- domain boundary and aggregate ownership
- service boundary
- sync vs async flow
- state machine behavior
- idempotency and retry semantics
- compatibility with existing APIs/data
- security and permission model

For smaller gaps, write explicit assumptions in the draft and mark them as confirmable.

### 4. Draft The Spec

Use `specx/templates/backend_spec.template.md` as the default output template.

For `feature`, keep microservice-only sections concise or mark them "Not applicable".
For `module`, emphasize internal boundaries and integration with the host service.
For `microservice`, complete service responsibility, deployment, storage ownership, observability, and failure-mode sections.

Recommended output location:

```text
specx/depot/{domain}/{name}_{mode}_backend_spec_v{version}.md
```

### 5. Self-Review

Review the draft with `references/review-checklist.md`.

Before finalizing, report:

- confirmed decisions
- remaining open questions
- assumptions
- architecture risks
- sections that need user review

## Reference Files

- Read `references/question-bank.md` when choosing clarification questions.
- Read `references/review-checklist.md` before reviewing or finalizing a spec.
- Use `specx/templates/backend_spec.template.md` as the default template.
