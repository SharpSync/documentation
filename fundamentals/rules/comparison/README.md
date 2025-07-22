---
icon: sparkles
---

# Comparison

Comparison rules run after the import rules and before the display rules. They are used to transform data in the comparison BOM.

At the moment only comparison rules for scope `row` are supported. Unlike most other rules that are scoped for the `cell` or `cellObject` , these will change some row level parameter based on the rule parameters (example the result of a rule can uncheck the "Process Row" checkbox).
