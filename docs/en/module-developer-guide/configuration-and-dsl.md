# Configuration and DSL

## Configuration philosophy

The platform favors typed, validated, configuration-driven composition. Module authors should expose stable parameters and keep environment-specific concerns outside the module implementation.

## Configuration layers

### Module-level parameters

Each module owns its own parameter contract through `params_schema` and semantic validation.

### Platform module configuration

The wider platform also uses typed configuration models for shared module settings and strategy assembly. Module developers should align naming, defaults, and data types with those platform conventions without depending on internal subsystem details.

## Example strategy composition

```yaml
strategy:
  name: trend_following_reference
  modules:
    - type: indicator
      name: moving_average_fast
      params:
        lookback: 20
    - type: indicator
      name: moving_average_slow
      params:
        lookback: 50
    - type: signal
      name: crossover_signal
      params:
        fast_ref: moving_average_fast
        slow_ref: moving_average_slow
    - type: risk
      name: volatility_guard
      params:
        max_volatility: 0.18
    - type: position
      name: directional_position
      params:
        mode: long_short
    - type: execution
      name: simulated_execution
      params:
        timeout_ms: 5000
```

## Parameter design rules

- Use explicit names.
- Use bounded numeric values where possible.
- Prefer primitive types and small structured objects.
- Separate tuning parameters from environment configuration.
- Document defaults and failure conditions.

## Example parameter table

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `lookback` | integer | Yes | Number of prior observations used |
| `threshold` | float | No | Decision sensitivity, usually bounded |
| `mode` | string | No | Named operating mode |

## DSL authoring guidance

If your team maintains a strategy DSL or generated config format, keep the module-facing part stable and declarative. A module should be configurable without requiring authors to know anything about private runtime services.

## Configuration anti-patterns

- embedding hostnames, credentials, or secrets in module examples,
- reusing one parameter for multiple unrelated meanings,
- relying on undocumented implicit defaults,
- exposing internal adapter settings as public module parameters.
