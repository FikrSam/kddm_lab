# Assignment 1: short note

**a. Customer identifiers.** `customer_id` is the primary key of `customers`, but Olist creates a new one for every order, so it identifies a customer *record of one order* (99,441 distinct values, exactly one order each). `customer_unique_id` identifies the real person behind those records: it has only 96,096 distinct values, and 2,997 people appear under more than one `customer_id` (up to 17). To count customers I would use `count(DISTINCT customer_unique_id)`.

**b. Junction table.** `order_items` is the junction table. Its composite key (`order_id`, `order_item_id`) plus the foreign keys `product_id` and `seller_id` resolve two many-to-many relationships: orders ↔ products (one order has up to 8 different products, one product appears in up to 467 orders) and orders ↔ sellers (up to 5 sellers in one order, up to 1,854 orders for one seller).

**c. Data vs. diagram.** The data is stricter than the diagram for customers → orders: every `customer_id` has exactly one order, but I drew "one or many" because a customer can place several orders and a per-order `customer_id` is a property of this extract, not a business rule. The circle on orders → order_items is justified by the data: 775 orders (mostly `unavailable` and `canceled`) have no items at all, so an order may have zero items.
