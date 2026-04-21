# Public Architecture

## Architectural intent

The module framework is designed so that strategy behavior can be assembled from reusable building blocks rather than embedded in a single monolithic strategy class. Module authors interact with a public extension layer while the platform retains control over orchestration, validation, and runtime services.

## Public architecture view

```mermaid
flowchart LR
    A[Strategy Definition] --> B[Module Registry]
    B --> C[Validation and Preparation]
    C --> D[Execution Order Resolver]
    D --> E[Runtime Context]
    E --> F[Composed Module Chain]
    F --> G[Module Outputs]
```

## Public components

### Module contract

The base class defines the lifecycle, metadata, parameter validation, execution entrypoints, and metrics interface expected from every module.

### Module registry

The registry is the controlled discovery and instantiation boundary. It resolves module classes, supports aliases, and enables dynamic loading patterns without requiring changes to the core platform.

### Validation and preparation

Validation ensures the module set is structurally valid for the target runtime mode. Preparation can normalize order and apply approved defaults before the runtime consumes the module list.

### Runtime context

Modules operate on a shared context object carrying market-derived data, intermediate outputs, and execution-time state exposed through the approved interface.

## Approved context allowlist

Module authors should only assume the following context surfaces are stable for ordinary module work:

- market-derived data fields published through the documented context data map,
- upstream indicator values,
- upstream signal values,
- module outputs written under stable module identifiers,
- current bar or current time markers when those are surfaced by the host runtime.

Module authors should not assume direct ownership of broker handles, transport objects, internal validation flags, or host-managed runtime state unless a separate contract explicitly grants access.

## Design constraints for module authors

- Keep module logic business-focused.
- Do not assume direct access to internal infrastructure services.
- Do not hardcode environment-specific execution details.
- Do not rely on undocumented sequencing side effects.
- Treat the context as a shared integration surface, not a dumping ground.

## Dependency injection guidance

The platform uses a dependency injection model for infrastructure services. Module authors should depend on constructor input, context input, or approved configuration, not on container internals. When a test requires substitutes, use mocks and provider overrides in the consuming application or test harness, not inside the module itself.

## Safe architecture boundary

The following are appropriate to reference in external developer docs:

- module categories,
- ordering rules,
- lifecycle methods,
- metadata and schema responsibilities,
- composition-safe examples.

The following are not appropriate:

- internal event bus implementation,
- execution adapter topology,
- live trading state synchronization internals,
- backend integration secrets.
