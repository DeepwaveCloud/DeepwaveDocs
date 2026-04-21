# Glossary

## Core terms

**Module**
: A reusable strategy building block implementing the platform contract.

**Module type**
: The declared category that determines the module's role in composition and validation.

**Context**
: The shared runtime object carrying current data, intermediate values, and outputs between modules.

**Composition**
: An ordered list of modules assembled into one strategy definition.

**Registry**
: The discovery and resolution boundary responsible for locating and instantiating module classes.

**Preparation**
: The normalization step that can reorder modules or apply approved defaults before validation completes.

**Validation**
: Structural and semantic checks ensuring a module set is safe and compatible with the target runtime mode.

**Runtime mode**
: The target execution context, such as live-oriented or backtest-oriented operation.

**Contract test**
: A test that verifies a module conforms to the public extension surface rather than only its own internal logic.

**Public API boundary**
: The set of approved contracts and behaviors safe to expose to external module developers.
