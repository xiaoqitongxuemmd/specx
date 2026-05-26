# SpecX

SpecX is a lightweight Codex workspace for turning uncertain product input into implementation-ready backend architecture specs.

This repository is not an application codebase. It contains agent guidance, reusable skills, reference checklists, and templates that help Codex work as a backend design partner instead of a document formatter.

## What This Repository Contains

```text
specx/
├── AGENTS.md
├── README.md
└── .agents/
    └── skills/
        ├── backend-architecture-refiner/
        │   ├── SKILL.md
        │   ├── references/
        │   │   ├── architecture-clarification-bank.md
        │   │   └── architecture-review-checklist.md
        │   └── templates/
        │       ├── ownership_matrix_template.md
        │       ├── phase_0_input_acquisition_template.md
        │       ├── phase_1_context_template.md
        │       ├── phase_2_runtime_template.md
        │       ├── phase_3_contract_template.md
        │       ├── runtime_risk_template.md
        │       └── state_machine_template.md
        └── lark-doc-reader/
            └── SKILL.md
```

The GitHub branch intentionally does not include generated or project-specific spec output under `specx/`. It is kept focused on reusable agent workflow assets.

## Core Workflow

SpecX is designed around a staged backend design flow:

1. Acquire and normalize input
   - PRDs, meeting notes, Feishu/Lark docs, existing contracts, or rough product ideas.
2. Clarify context and ownership
   - Service boundaries, data ownership, domain responsibilities, and integration points.
3. Refine runtime semantics
   - State transitions, concurrency, idempotency, retries, consistency, failure behavior, and observability.
4. Freeze implementation contracts
   - APIs, RPCs, events, tasks, persistence model, compatibility expectations, and test strategy.

## Key Files

- `AGENTS.md`
  - Project-level agent behavior guidance.
  - Requires critical thinking, explicit pushback, boundary clarification, and practical backend design standards.

- `.agents/skills/backend-architecture-refiner/SKILL.md`
  - Main staged workflow for backend architecture refinement.
  - Uses Phase 0 through Phase 3 to move from raw input to contract-ready architecture.

- `.agents/skills/backend-architecture-refiner/references/architecture-clarification-bank.md`
  - Question bank for identifying missing decisions, weak assumptions, and architecture-impacting ambiguity.

- `.agents/skills/backend-architecture-refiner/references/architecture-review-checklist.md`
  - Review checklist for validating backend architecture specs before treating them as implementation-ready.

- `.agents/skills/backend-architecture-refiner/templates/`
  - Markdown templates for staged spec artifacts, ownership matrices, runtime risks, and state machines.

- `.agents/skills/lark-doc-reader/SKILL.md`
  - Workflow for reading Feishu/Lark documents with `lark-cli` and turning external documents into usable design input.

## Design Principles

- Treat source material as evidence, not truth.
- Surface contradictions before polishing documents.
- Clarify ownership before defining contracts.
- Prefer the simplest design that satisfies the requirement.
- Record assumptions, rejected options, and unresolved architecture risks.
- Make backend specs useful for implementation, not only for presentation.

## Expected Output From The Skills

The architecture refinement flow should produce practical backend design artifacts such as:

- responsibility and ownership boundaries
- domain model and persistence assumptions
- API, RPC, event, and task contracts
- ER diagrams when persistent data exists
- state diagrams when lifecycle state exists
- sequence diagrams for main and failure flows
- idempotency, retry, concurrency, and transaction decisions
- migration, compatibility, rollback, audit, and observability notes
- unit, integration, contract, and acceptance test strategy

## Repository Scope

This GitHub branch is for reusable SpecX workflow assets only:

- agent instructions
- architecture refinement skills
- Lark document reading workflow
- reusable references and templates

Project-specific generated specs should be kept outside this branch unless they are deliberately being published as examples.
