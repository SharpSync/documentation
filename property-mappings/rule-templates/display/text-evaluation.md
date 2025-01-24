# Text evaluation

Type: Display

Description: Evaluates the cell value given the JavaScript expression.

This is an advanced rule which requires programming knowledge. It is more difficult to use, but much more powerful.

See the [Advanced Scripting ](../../../advanced/advanced-scripting.md)section for more detail.

<details>

<summary>Example: Check material name</summary>

* Cell value: Stainless Steel 304
* Rule value:&#x20;

<pre class="language-javascript"><code class="lang-javascript"><strong>/* if the string is blank, return unknown */
</strong><strong>if (!s &#x26; !rowData.isAssemblyRow) 
</strong><strong>  return { message: 'unknown material' }
</strong></code></pre>

* Result: Rule passes.&#x20;

</details>

<details>

<summary>Example: Warn on empty or invalid rows for non-assemblies</summary>

* Cell value: \[blank]
* Rule value:

<pre class="language-javascript"><code class="lang-javascript"><strong>/* if the string is blank and its a component, return warning */
</strong><strong>if (!rowData.isAssemblyRow &#x26;&#x26; !s) 
</strong><strong>  return { message: `No material specified for this component`};
</strong><strong>
</strong><strong>/* check for valid combinations */
</strong><strong>if (s.toLower().includes('steel') &#x26;&#x26; rowData.cells.partNumber.endsWith('RR'))
</strong><strong>    return { message: `Invalid material specified for this component`};
</strong><strong>
</strong></code></pre>

* Result:&#x20;
  * Rule fails for non-assembly rows. Cell border is marked orange or red (depending on your pass / block setting)&#x20;
  * Rule passes for assembly rows

</details>

<details>

<summary>Example: Empty revision must block</summary>

* Cell value: \[blank]
* Property mapping setting: Is Readonly = checked
* Rule value:

<pre class="language-javascript" data-overflow="wrap"><code class="lang-javascript"><strong>/* if the string is blank and its a component, return warning */
</strong><strong>if (!s &#x26;&#x26; pm.isReadOnly) 
</strong><strong>  return { message: `The revision is blank. Please update at the source and reload the BOM`};
</strong></code></pre>

* Result:&#x20;
  * Rule fails and warning is shown, but only if the Property Mapping is marked as read-only.

</details>



For further details, reach out to us on our Support Ticket system.
