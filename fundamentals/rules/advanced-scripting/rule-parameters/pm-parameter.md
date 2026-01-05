# pm Parameter

The `pm` parameter is the settings for the Property Mapping. This includes, but is not limited to the following parameters:



<table><thead><tr><th width="202">Parameter</th><th width="325">Description</th></tr></thead><tbody><tr><td><code>isReadOnly</code></td><td>Whether the user can edit the data on-screen</td></tr><tr><td><code>primaryAccessor</code></td><td>The field mapped to in the primary source</td></tr><tr><td><code>secondaryAccessor</code></td><td>The field mapped to in the destination (secondary) source</td></tr><tr><td><code>renderingType</code></td><td>The property mapping object</td></tr><tr><td>etc</td><td></td></tr></tbody></table>

#### Example&#x20;

To access the Property Mapping's `isReadOnly` parameter in a script, use&#x20;

```javascript
if (pm.isReadOnly)
{
  // do something
}
```

For more in-depth detail, refer to [property-mappings](../../../property-mappings/ "mention").

If you require more information, please reach out to us on the support channel.
