# s Parameter

The `s` parameter is the current string representation of a value from a source.

When running rules, this parameter is processed in sequence. It changes for every rule that is run on the data.

{% hint style="info" %}
The `s` parameter is calculated per source. So it is different for each source, with each iteration of a rule
{% endhint %}

### Scenario 1: Applied to source 1 only

Let's say you have an initial Quantity value of 5.

Let's say you have some individual rules running a calculation that adds 5 and rounds to nearest 1.

The rule is only applied to source 1

<table><thead><tr><th width="230">Rule</th><th width="148">'s' for source 1</th><th width="154">'s' for source 2</th><th>Display value</th></tr></thead><tbody><tr><td>[value from source]</td><td>5</td><td>5</td><td>5</td></tr><tr><td>Add 5</td><td>5</td><td>5</td><td>10</td></tr><tr><td>Add 5</td><td>10</td><td>5</td><td>15</td></tr><tr><td>Round to nearest 1.0</td><td>15</td><td>5</td><td>15.0</td></tr></tbody></table>

The parameter is calculated for each individual source. It is _not_ the result of both

#### What you would see on-screen:

A cell with a blue border around it, because the final value for both sources are _different._

### Scenario 2: Applied to both sources

If the rule is turned on for both sources, then the result would be this:

<table><thead><tr><th width="230">Rule</th><th>'s' for source 1</th><th>'s' for source 2</th><th>Display value</th></tr></thead><tbody><tr><td>[value from source]</td><td>5</td><td>5</td><td>5</td></tr><tr><td>Add 5</td><td>5</td><td>5</td><td>10</td></tr><tr><td>Add 5</td><td>10</td><td>10</td><td>15</td></tr><tr><td>Round to nearest 1.0</td><td>15</td><td>15</td><td>15.0</td></tr></tbody></table>

#### What you would see on-screen:

A cell with **no** border around it, because the final value for both sources are _the same._

Import rules only manipulate or use the string value `s` which is passed to it, for the source that it is enabled for. _It does not process the data from both sources, it processes the data for an individual source._
