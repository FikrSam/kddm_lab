# ERD: Olist source tables

Crow's foot notation, five tables from the `raw` schema. The Mermaid source is in [`erd.mmd`](erd.mmd).
`PK, FK` on `order_items.order_id` and `PK` on `order_item_id` mark the composite primary key.

![ERD](erd.png)

```mermaid
erDiagram
    CUSTOMERS ||--|{ ORDERS : places
    ORDERS ||--o{ ORDER_ITEMS : contains
    PRODUCTS ||--|{ ORDER_ITEMS : "is sold as"
    SELLERS ||--|{ ORDER_ITEMS : sells

    CUSTOMERS {
        varchar customer_id PK
        varchar customer_unique_id
        varchar customer_zip_code_prefix
        varchar customer_city
        varchar customer_state
    }
    ORDERS {
        varchar order_id PK
        varchar customer_id FK
        varchar order_status
        timestamp order_purchase_timestamp
        timestamp order_approved_at
        timestamp order_delivered_carrier_date
        timestamp order_delivered_customer_date
        timestamp order_estimated_delivery_date
    }
    ORDER_ITEMS {
        varchar order_id PK, FK
        bigint order_item_id PK
        varchar product_id FK
        varchar seller_id FK
        timestamp shipping_limit_date
        double price
        double freight_value
    }
    PRODUCTS {
        varchar product_id PK
        varchar product_category_name
        bigint product_name_lenght
        bigint product_description_lenght
        bigint product_photos_qty
        bigint product_weight_g
        bigint product_length_cm
        bigint product_height_cm
        bigint product_width_cm
    }
    SELLERS {
        varchar seller_id PK
        varchar seller_zip_code_prefix
        varchar seller_city
        varchar seller_state
    }
```

## Cardinalities (measured in `notebooks/01_explore_source_data.ipynb`, section 6)

| relationship | parent side | child rows per parent (min / max) | child side drawn |
|---|---|---|---|
| customers **places** orders | exactly one (FK non-null, 0 orphans) | 1 / 1 | one or many |
| orders **contains** order_items | exactly one (FK non-null, 0 orphans) | 0 / 21 (775 orders have no items) | zero or many |
| products **is sold as** order_items | exactly one (FK non-null, 0 orphans) | 1 / 527 | one or many |
| sellers **sells** order_items | exactly one (FK non-null, 0 orphans) | 1 / 2033 | one or many |
