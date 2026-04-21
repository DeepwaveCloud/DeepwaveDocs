# Validation and Ordering

## Why validation exists

The platform validates module composition before runtime so that structural errors are caught early and consistently.

## Canonical ordering

The approved execution order is:

1. `indicator`
2. `signal`
3. `filter`
4. `risk`
5. `position`
6. `execution`
7. `output`
8. `portfolio`

This order reflects dependency flow from derived data to decisioning, action shaping, execution bridging, and reporting.

## Runtime-mode validation rules

### Live-oriented composition

The validated live composition requires:

- at least one indicator module,
- at least one signal module,
- at least one position module,
- exactly one execution module,
- execution placed after position modules,
- no ordering violations.

### Backtest-oriented composition

Backtest composition still requires valid ordering. If an execution module is present, it must be a simulation-safe execution variant approved for non-live use.

## Preparation behavior

Before validation completes, the preparation layer may sort modules into canonical order and may add an execution module through approved platform defaults. Module authors must not rely on those defaults. Always document the expected full module set in examples.

## Failure patterns to avoid

- placing a filter before its required signal producer,
- assuming position logic can run before indicator calculations,
- defining multiple execution modules in one composition,
- omitting a position module from a live composition,
- using production-specific execution assumptions in backtest examples.

## Review checklist

Before releasing a module, verify:

- the module type matches its actual responsibility,
- the example strategy order is canonical,
- required upstream inputs are listed,
- missing-input behavior is documented,
- runtime mode compatibility is explicit.
