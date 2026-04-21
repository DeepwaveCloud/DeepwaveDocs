# Public API Boundary

## Boundary statement

This portal documents only the approved extension surface for module developers. Everything outside this surface must be treated as internal unless separately approved.

## Approved extension surface

### Base module contract

The public contract includes:

- module metadata fields,
- parameter schema conventions,
- lifecycle methods,
- metrics and metadata accessors,
- shared context usage patterns.

### Registry-facing concepts

The public model includes:

- registration,
- alias support,
- discovery and resolution concepts,
- safe instantiation expectations.

#### Author-facing discovery rules

- Module names must be stable, human-readable, and safe to reference from configuration.
- Aliases are a compatibility tool, not a naming shortcut. When an alias is introduced, the canonical name must remain documented.
- A module is considered discoverable only when it is packaged and exposed through an approved registration or discovery path in the host application.
- Dynamic discovery behavior may vary by deployment. Module authors must not assume every environment enables automatic discovery.

#### Identifier and output rules

- `module_id` should remain stable within a strategy definition.
- Outputs intended for downstream consumption must be written under deterministic keys.
- Downstream references such as `indicator_ref` or similar pointers must target documented module identifiers, not implicit ordering.
- If a module changes its output shape, that is a contract change and must be versioned and documented.

### Validation-facing concepts

The public model includes:

- canonical ordering,
- live and backtest composition expectations,
- preparation concepts that affect documentation and examples.

### Controlled extension areas

Execution-oriented extensions are not a default public customization path. They are controlled capabilities with stronger review, validation, and runtime constraints than ordinary indicator, signal, filter, risk, sizer, position, output, or portfolio modules.

## Internal-only areas

The following topics are intentionally excluded from this site:

- proprietary execution flow internals,
- infrastructure container implementation details,
- live adapter protocols,
- state persistence mechanisms,
- system-wide risk enforcement logic,
- operational thresholds and production tuning.

## How to request a boundary change

If a module team needs a currently internal capability to become public, open a documentation governance request that includes:

1. use case,
2. proposed contract,
3. security and stability considerations,
4. backward compatibility expectations.

No team should publish internal code excerpts as external guidance without approval.
