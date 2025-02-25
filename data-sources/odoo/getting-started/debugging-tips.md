# Debugging tips

It can be useful during the setup process to have access to information that helps you to troubleshoot.

Below are some common fields that can be setup using rules. Once the setup has been completed, be sure to hide or remove these mappings as it can clutter the BOM Comparison page.

### Variant Link

The Variant URL is the URL link to the variant (product.product) that SharpSync found during the search process.

Create a new mapping with the following values

| Setting                   | Value             |
| ------------------------- | ----------------- |
| Primary source property   | (Unmapped)        |
| Secondary source property | (Unmapped)        |
| Prefer Secondary Value    | checked           |
| Rendering type            | Url               |
| New Import Rule Mapping   | Text Manipulation |
| Process for Primary       | unchecked         |
| Process for Odoo          | checked           |
| Rule value                | See below \*\*    |

Paste the below code into the textbox. It makes use of the `rowData`object's `secondaryViewHref`property to create a new url

```javascript
const urlObj = new URL(rowData.secondaryViewHref);
const hashString = urlObj.hash.substring(1);  
const params = new URLSearchParams(hashString);

const variantId = params.get("variant_id");
if (variantId !== null) {
  params.set("id", variantId);
}

if (params.get("model") === "product.template") {
  params.set("model", "product.product");
}

urlObj.hash = params.toString();

return urlObj.toString().trim();
```



This will display a URL to the product variant in the UI in the form of a clickable link.

### Variant Id

The Variant Id is the id of the product variant (product.product) that SharpSync found during the search process.

Create a new mapping with the following values

| Setting                   | Value             |
| ------------------------- | ----------------- |
| Primary source property   | (Unmapped)        |
| Secondary source property | (Unmapped)        |
| Prefer Secondary Value    | checked           |
| Rendering type            | Free Text         |
| New Import Rule Mapping   | Text Manipulation |
| Process for Primary       | unchecked         |
| Process for Odoo          | checked           |
| Rule value                | See below \*\*    |

Paste the below code into the textbox. It makes use of the `rowData`object's `secondaryViewHref`property to create a new url

```javascript
const urlObj = new URL(rowData.secondaryViewHref);
const hashString = urlObj.hash.substring(1);
const params = new URLSearchParams(hashString);

const variantId = params.get("variant_id");
if (variantId !== null) {
  return variantId;
}

return "";
```

This will display the id in the form of an integer (e.g. 5600 or 5601) in the UI
