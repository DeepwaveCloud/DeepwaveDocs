# Contribution Workflow

## Governance objective

Enterprise documentation is part of the module delivery standard. A module is not complete unless its documentation, examples, and test evidence are aligned.

## Required contribution package

Every new module contribution should include:

- implementation code,
- tests,
- parameter reference,
- one usage example,
- troubleshooting notes,
- boundary review if a new capability is introduced.

## Documentation review criteria

Reviewers should confirm:

- the module category is correct,
- public terminology is consistent,
- examples are safe and environment-neutral,
- no internal or proprietary details are exposed,
- English and Chinese pages remain aligned where required.

## Change control rules

### Minor change

Examples: typo fixes, clarifications, non-breaking example updates.

### Contract-affecting change

Examples: parameter rename, output shape change, lifecycle expectation change, new approved module type.

Contract-affecting changes require coordinated updates to tests, examples, and both language versions of the docs.

## Recommended publication flow

1. Draft the module and reference doc.
2. Complete unit, contract, and composition tests.
3. Perform boundary review.
4. Update both language trees.
5. Build the static site and verify links.
6. Publish with release notes if the contract changed.
