# Module Types

## Supported categories

The platform currently exposes the following module categories through the public base contract.

| Type | Purpose | Typical output |
| --- | --- | --- |
| `indicator` | Computes derived values from market data or context | Numeric or structured indicator values |
| `signal` | Produces directional or stateful trade intent | Signal state, confidence, rationale |
| `filter` | Refines, suppresses, or qualifies signal flow | Pass or block decisions |
| `risk` | Evaluates trade or position risk constraints | Risk assessment or constraints |
| `sizer` | Determines exposure or quantity sizing | Size recommendation |
| `position` | Produces position-level action decisions | Open, close, hold, reduce |
| `execution` | Bridges position intent into an execution-compatible action | Execution payload or accepted action |
| `output` | Emits reporting, logging, or derived records | Output artifacts |
| `portfolio` | Computes portfolio-level aggregation or overlay behavior | Portfolio metrics or directives |

## Authoring guidance by type

### Indicator modules

Use indicator modules for deterministic transformations. They should avoid side effects and focus on producing reusable values for downstream modules.

### Signal modules

Signal modules should explain the decision state clearly. Prefer explicit structured outputs over opaque booleans when downstream traceability matters.

### Filter modules

Filters are best when they are composable and transparent. Document the exact blocking conditions and how they interact with missing inputs.

### Risk modules

Custom risk modules may express local, strategy-specific checks, but they must not attempt to reproduce platform-level system risk internals.

### Sizer and position modules

These modules should separate quantity decisions from directional decisions whenever possible. That makes strategies easier to reason about and validate.

### Execution modules

Execution modules are a controlled extension point. Treat them as approval-based capabilities, not the default path for third-party module work. Keep them generic, configuration-driven, and abstracted from infrastructure secrets.

### Output and portfolio modules

These modules should remain observational or aggregative. Avoid embedding control flow that surprises upstream business logic.

## Category selection checklist

- If it derives a value from data, it is probably an indicator.
- If it decides trade intent, it is probably a signal or position module.
- If it approves or rejects an action, it is probably a filter or risk module.
- If it determines quantity, it is a sizer.
- If it bridges intent into execution-ready action, it is an execution module.
- If it records or aggregates, it is an output or portfolio module.
