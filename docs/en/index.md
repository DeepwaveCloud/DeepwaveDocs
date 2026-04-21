# DeepWave Module Developer Docs

This portal provides the approved documentation set for external and internal teams that build reusable modules for the DeepWave strategy composition framework.

## Purpose

This site explains how to design, implement, validate, test, and maintain custom modules without exposing proprietary execution logic or operational internals. It is intentionally written around stable contracts instead of implementation details.

## Intended audience

- Module authors building reusable business modules.
- Solution architects designing strategy assembly standards.
- QA engineers validating module compatibility before release.
- Technical writers maintaining extension documentation and templates.

## What is documented

- Public module contracts based on the platform module base class.
- Supported module categories and the role of each category in a composed strategy.
- Validation rules for module presence, ordering, and runtime suitability.
- Configuration patterns, DSL examples, and packaging guidance.
- Testing, debugging, and release readiness expectations.

## What is intentionally not documented

- Proprietary runtime orchestration and internal event choreography.
- Broker, exchange, or execution adapter implementation details.
- Core risk algorithms, threshold policies, or infrastructure secrets.
- Internal service topology, credentials, or production endpoint information.

!!! note "Public contract first"
    Treat every integration as contract-driven. If a behavior is not described in this portal or a separately approved API contract, assume it is internal and subject to change.

## Documentation map

| Section | Focus |
| --- | --- |
| Getting Started | Entry path, boundaries, and workflow |
| Architecture | Safe architectural mental model for module authors |
| Module Developer Guide | Contracts, types, validation, config, examples |
| Reference | Public boundary and shared vocabulary |
| Governance | How documentation and module standards evolve |

## Recommended onboarding path

1. Read `Getting Started` to understand roles, lifecycle, and non-goals.
2. Read `Public Architecture` to understand where modules fit.
3. Read `Module Contract` and `Module Types` before writing code.
4. Use `Templates and Examples` to scaffold new modules safely.
5. Complete `Testing and Debugging` before submission.

## Documentation principles

- Use explicit contracts, not tribal knowledge.
- Prefer deterministic examples over narrative-only guidance.
- Keep examples generic and environment-neutral.
- Document extension points, never core implementation secrets.
- Keep Chinese and English pages functionally equivalent.
