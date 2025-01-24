# Product Management

In Odoo, products are created using the `product.template` model. Products have a 1:N relationship with their variants.

In CAD Systems, variants are known as configurations. In Odoo, variants are just that - variants. Their internal model name is `product.product`

<figure><img src="../../.gitbook/assets/image (26).png" alt=""><figcaption><p>product.template to product.product relationship</p></figcaption></figure>

When a new Product is created in Odoo, the system automatically creates a product variant. So immediately upon creation of a new product in Odoo, there is a 1:1 relationship with 1 product: 1 variant. The variant will be hidden, but it is there.

To create more variants, you navigate to the `Attributes and Variants` tab in Odoo. By adding more attributes, Odoo automatically creates new variants.



<figure><img src="../../.gitbook/assets/image (27).png" alt=""><figcaption><p>A product may have many attributes</p></figcaption></figure>

For the combination of attributes above, we can see many variants being created

<figure><img src="../../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>
