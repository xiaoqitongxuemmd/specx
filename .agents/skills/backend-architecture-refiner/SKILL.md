---
name: backend-architecture-refiner
description: Use when designing greenfield backend systems, workflows, orchestration systems, platform modules, or event-driven services through an interactive architecture refinement process focused on runtime semantics, ownership boundaries, consistency, and contract convergence.
---

# Backend Architecture Refiner

# Purpose

Help backend engineers progressively refine backend architectures through interactive architecture convergence.

The goal is NOT to generate giant upfront specifications.

The goal is to:

- identify architecture-impacting unknowns
- surface hidden assumptions
- refine runtime semantics
- clarify ownership boundaries
- refine consistency semantics
- progressively converge toward implementation-ready contracts

Documents are snapshots of the current refinement state, not final truth.

---

# Core Philosophy

## Unknowns First

Prioritize discovering architecture-impacting unknowns before generating structure or specifications.

Avoid speculative design when ambiguity may affect architecture.

---

## Runtime Matters

Treat runtime behavior as a first-class design concern.

Continuously reason about:

- retries
- duplicate delivery
- ordering
- replay
- timeout
- partial failure
- lifecycle transitions
- consistency boundaries

---

## Constraint-Driven Design

System constraints take precedence over idealized modeling styles.

Designs must reflect runtime reality.

---

## Progressive Refinement

Do not generate a full specification upfront.

Architecture refinement must happen progressively through staged interaction.

---

# Mandatory Interaction Model

This skill MUST operate interactively.

At every phase:

1. summarize known information
2. identify architecture-impacting unknowns
3. ask focused clarification questions
4. wait for confirmation
5. refine only the current phase

Do not proceed to the next phase without explicit confirmation.

If ambiguity may affect architecture:

prefer asking questions over speculative refinement.

---

# Forbidden Behaviors

Do NOT:

- generate all phases at once
- generate giant upfront specifications
- silently resolve ambiguity
- invent ownership semantics
- invent runtime guarantees
- skip confirmation gates
- convert unresolved risks into assumptions
- prioritize completeness over correctness

---

# Required Response Structure

## Known Information

Confirmed facts only.

---

## Design Drivers

Current runtime and architectural constraints.

---

## Unresolved Architecture Questions

Only architecture-impacting unknowns.

---

## Risks

Current architectural and runtime risks.

---

## Current Phase Refinement

Refine only the current phase.

---

## Confirmation Gate

Explicitly ask whether to continue.

---

# Phase Workflow

## Phase 0 — Input & Context Acquisition

Goal:

Acquire sufficient context before architecture refinement begins.

Prioritize collecting:

- PRDs
- workflow descriptions
- sequence diagrams
- API/event definitions
- runtime constraints
- ownership information
- upstream/downstream interactions
- deployment/runtime assumptions

Do not begin speculative architecture generation from underspecified prompts.

Use:

```text
references/templates/phase_0_input_acquisition_template.md
```

---

## Phase 1 — Context & Constraints

Goal:

Clarify:

- system purpose
- ownership boundaries
- communication patterns
- operational constraints
- runtime expectations
- design drivers

Do NOT jump directly into data modeling.

Output:

```text
specx/{domain}/{name}/phase_1_context.md
```

Use:

```text
references/templates/phase_1_context_template.md
```

Before entering Phase 2, confirm:

- ownership
- runtime assumptions
- consistency expectations
- communication boundaries
- design drivers

---

## Phase 2 — Runtime Semantics & Data Modeling

Goal:

Refine:

- lifecycle semantics
- retry semantics
- event flow
- consistency boundaries
- ownership semantics
- persistence responsibilities

Runtime semantics are primary.

Data models support runtime semantics.

Output:

```text
specx/{domain}/{name}/phase_2_runtime_semantics.md
```

Use:

```text
references/templates/phase_2_runtime_template.md
```

Before entering Phase 3, confirm:

- lifecycle semantics
- retry semantics
- consistency semantics
- ownership semantics
- event behavior

---

## Phase 3 — Contracts & Interfaces

Goal:

Define stable contracts only after runtime semantics stabilize.

Contracts must reflect runtime semantics.

Contract design must include a complete implementation interface inventory,
not just a few representative contracts.

The inventory must be derived from:

- user-visible use cases
- lifecycle/state transitions
- commands that mutate owned data
- queries required by frontend, downstream services, operators, or jobs
- event producers and event consumers
- callbacks, webhooks, scheduled jobs, and async workers
- permission boundaries and actor-specific operations
- retry, idempotency, timeout, and ordering-sensitive interactions

For every required operation, explicitly decide whether it is implemented as:

- public API
- internal RPC
- domain/application service method
- event topic/message
- async task/job entrypoint
- webhook/callback
- batch/import/export interface

Do not hide missing design behind vague statements like "provide CRUD APIs".
List each interface that must be implemented, even when the exact schema is
still draft. If an interface is intentionally deferred or rejected, record the
reason.

Output:

```text
specx/{domain}/{name}/phase_3_contracts.md
```

Use:

```text
references/templates/phase_3_contract_template.md
```

Before finalization, confirm:

- contract semantics
- retry/idempotency behavior
- timeout behavior
- ordering guarantees
- auth boundaries

---

# Reference Usage Rules

Use references under:

```text
references/
```

as reasoning support material.

References help stabilize:

- runtime reasoning
- ownership reasoning
- consistency reasoning
- failure modeling
- output structure

---

## Important References

| Purpose | Reference |
|---|---|
| review quality | architecture-review-checklist.md |
| clarification strategy | architecture-clarification-bank.md |

---

## Specialized Templates

| Concern | Template |
|---|---|
| ownership analysis | ownership_matrix_template.md |
| lifecycle modeling | state_machine_template.md |
| runtime risk analysis | runtime_risk_template.md |

---

# Critical Philosophy

A clean-looking architecture document does NOT imply a correct architecture.

Continuously challenge:

- assumptions
- ownership
- runtime semantics
- consistency expectations
- operational viability

The primary goal is NOT document generation.

The primary goal is architecture refinement through uncertainty reduction.
