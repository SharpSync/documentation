---
icon: conveyor-belt-empty
---

# Product Management

In Odoo, products are created using the `product.template` model. Products have a 1:N relationship with their variants.

In CAD Systems, variants are known as configurations. In Odoo, variants are just that - variants. Their internal model name is `product.product`

<figure><img src="../../.gitbook/assets/image (26).png" alt=""><figcaption><p>product.template to product.product relationship</p></figcaption></figure>

When a new Product is created in Odoo, the system automatically creates a product variant. So immediately upon creation of a new product in Odoo, there is a 1:1 relationship with 1 product: 1 variant. The variant will be hidden, but it is there.

To create more variants, you navigate to the `Attributes and Variants` tab in Odoo. By adding more attributes, Odoo automatically creates new variants.



<figure><img src="../../.gitbook/assets/image (27).png" alt=""><figcaption><p>A product may have many attributes</p></figcaption></figure>

For the combination of attributes above, we can see many variants being created

<figure><img src="../../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

### Product Category of new products

When SharpSync creates a new product and Product Category (`product.template.categ_id`) is not mapped, the category depends on your Odoo version:

* **Odoo 17 and 18:** Odoo puts the product in its default category, **All**.
* **Odoo 19:** the product has no category. Odoo 19 does not require one.
* **Odoo 16:** SharpSync puts the product in the category with ID 1, which is normally **All**.

To choose the category of new products, map Product Category. The values are listed under `product.category` in [List Names](list-names.md).
