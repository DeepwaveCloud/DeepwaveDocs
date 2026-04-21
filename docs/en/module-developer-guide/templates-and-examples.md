# Templates and Examples

## Recommended module package structure

```text
my-module-package/
  README.md
  module.py
  tests/
    test_module.py
    test_integration_contract.py
  docs/
    module-reference.md
```

## Indicator template

```python
class ExampleIndicator(ModuleBase):
    name = "example_indicator"
    version = "1.0.0"
    type = ModuleType.INDICATOR
    description = "Computes a rolling value for downstream modules"
    params_schema = {"lookback": {"type": "int", "required": True}}

    def validate_params(self, params):
        return params["lookback"] > 1

    def init(self, bt_ctx):
        self.history = []

    async def next_async(self, bt_ctx):
        price = bt_ctx.data.get("close")
        self.history.append(price)
        value = sum(self.history[-self.params["lookback"]:]) / self.params["lookback"]
        bt_ctx.indicators[self.module_id] = value
        bt_ctx.module_outputs[self.module_id] = value
        return value
```

## Signal template

```python
class ExampleSignal(ModuleBase):
    name = "example_signal"
    version = "1.0.0"
    type = ModuleType.SIGNAL
    description = "Turns indicator state into a normalized signal"
    params_schema = {"indicator_ref": {"type": "str", "required": True}}

    def validate_params(self, params):
        return bool(params["indicator_ref"])

    def init(self, bt_ctx):
        self.last_signal = "hold"

    async def next_async(self, bt_ctx):
        value = bt_ctx.indicators.get(self.params["indicator_ref"])
        signal = "buy" if value else "hold"
        result = {"signal": signal, "source": self.params["indicator_ref"]}
        bt_ctx.signals[self.module_id] = result
        bt_ctx.module_outputs[self.module_id] = result
        return result
```

## Output and reference contract

When a module publishes values for downstream use, follow these rules:

1. Write outputs using the module's stable `module_id`.
2. Use one documented output shape per version.
3. Prefer explicit references such as `indicator_ref`, `signal_ref`, or similarly named parameters over positional assumptions.
4. Document whether the module writes into shared indicator or signal maps, general module outputs, or both.
5. If downstream modules depend on a specific field, publish that field name in the module reference.

## Documentation template for every module

Use this structure in the module-specific README or reference page:

1. Module summary.
2. Category and dependencies.
3. Parameter table.
4. Input assumptions.
5. Output contract.
6. Failure modes.
7. Compatibility notes.
8. Example strategy usage.

## Example release checklist

- contract methods implemented,
- parameter validation present,
- deterministic example config provided,
- unit and contract tests passing,
- bilingual documentation prepared if required by your program.
