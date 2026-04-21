# 模板与示例

## 推荐模块包结构

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

## 指标模块模板

```python
class ExampleIndicator(ModuleBase):
    name = "example_indicator"
    version = "1.0.0"
    type = ModuleType.INDICATOR
    description = "为下游模块计算滚动值"
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

## 信号模块模板

```python
class ExampleSignal(ModuleBase):
    name = "example_signal"
    version = "1.0.0"
    type = ModuleType.SIGNAL
    description = "把指标状态转换成标准化信号"
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

## 输出与引用契约

当模块要把结果提供给下游消费时，遵循以下规则：

1. 使用稳定的 `module_id` 写入输出；
2. 每个版本只维护一种文档化输出结构；
3. 优先使用 `indicator_ref`、`signal_ref` 等显式引用，不要依赖位置顺序；
4. 明确说明模块会写入指标映射、信号映射、通用模块输出，还是同时写入多个表面；
5. 如果下游依赖某个具体字段，必须在模块参考文档中公开该字段名。

## 每个模块建议采用的文档模板

1. 模块摘要；
2. 所属类型与依赖；
3. 参数表；
4. 输入假设；
5. 输出契约；
6. 失败模式；
7. 兼容性说明；
8. 策略使用示例。

## 发布清单

- 契约方法已实现；
- 参数校验存在；
- 提供可复现配置示例；
- 单元测试和契约测试通过；
- 如果项目要求双语，则中英文文档齐全。
