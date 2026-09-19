# ITO5202 Assignment 1: Dataset Proposal

> This document reproduces the dataset proposal submitted and approved as the hurdle
> requirement for ITO5202 Assessment 1.

---

## SECTION 1: Dataset Details
**Name:** Brazilian E-Commerce Public Dataset by Olist
**Dataset Source URL:** https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce 
**Reasoning:** The dataset comprises of transactional data from Olist, which is a Brazilian online marketplace, and covers orders places between October 2016 and September 2018.

## SECTION 2: Dataset Scope and Scale

The chosen Olist data contains nine related data files:

| Files | Number of Rows | Number of Columns | Approx. file size |
|---|---|---|---|
| olist_geolocation_dataset.csv | 1,000,163 | 5 | ~61MB |
| olist_order_items_dataset.csv | 112,650 | 7 | ~15MB |
| olist_order_payments_dataset.csv | 103,886 | 5 | ~6MB |
| olist_orders_dataset.csv | 99,441 | 8 | ~17MB |
| olist_customers_dataset.csv | 99,441 | 5 | ~9MB |
| olist_order_reviews_dataset.csv | 88,224 | 7 | ~14MB |
| olist_products_dataset.csv | 32,951 | 9 | ~2MB |
| olist_sellers_dataset.csv | 3,095 | 4 | ~0.2MB |
| product_category_name_translation.csv | 71 | 2 | ~1KB |

Thus, we note that the dataset exceeds the 100,000 row minimum requirement.

---

## SECTION 3: Column Inventory


### olist_geolocation_dataset.csv

| Column Name | Data Type | Column Description |
|---|---|---|
| `order_id` | string | Foreign key. Unique order identifier. |
| `order_item_id` | integer | Item identifier within the order. |
| `product_id` | string | Foreign key. Unique product identifier. |
| `seller_id` | string | Foreign key. Unique seller identifier. |
| `shipping_limit_date` | timestamp | Deadline data for handover by seller. |
| `price` | double | Price of the item (in BRL). Numerical measure. |
| `freight_value` | double | Price charged for freight for the item. Numerical measure. |

### olist_orders_dataset.csv

| Column Name | Data Type | Column Description |
|---|---|---|
| `order_id` | string | Primary key. Unique order identifier. |
| `customer_id` | string | Foreign key. Per order customer identifier. |
| `order_status` | string | Lifecycle state of an order. |
| `order_purchase_timestamp` | timestamp | Date/time of order placement. |
| `order_approved_at` | timestamp | Payment approval date/time. Nullable |
| `order_delivered_carrier_date` | timestamp | Date/time order handed over to carrier. Nullable |
| `order_delivered_customer_date` | timestamp | Date/time of actual order delivery. Numerical measure. |
| `order_estimated_delivery_date` | timestamp | Date/time of expected order delivery. |

### olist_customers_dataset.csv

| Column Name | Data Type | Column Description |
|---|---|---|
| `customer_id` | string | Foreign key. Per order customer identifier. |
| `customer_unique_id` | string | Unique customer identifier (persistent). |
| `customer_zip_code_prefix` | integer | Zip code that customer is located in. |
| `customer_city` | string | City that customer is located in. |
| `customer_state` | string | State that customer is located in. |

### olist_sellers_dataset.csv

| Column Name | Data Type | Column Description |
|---|---|---|
| `seller_id` | string | Primary key. Unique seller identifier. |
| `seller_zip_code_prefix` | integer | Zip code that seller is located in. |
| `seller_city` | string | City that seller is located in. |
| `seller_state` | string | State that seller is located in. |

### olist_geolocation_dataset.csv

| Column Name | Data Type | Column Description |
|---|---|---|
| `geolocation_zip_code_prefix` | integer | postcode lookup. |
| `geolocation_lat` | double | Latitude of postcode. |
| `geolocation_lng` | double | Longitude of postcode. |
| `geolocation_city` | string | City name of postcode. |
| `geolocation_state` | string | State that postcode is located in. |

### olist_products_dataset.csv

| Column Name | Data Type | Column Description |
|---|---|---|
| `product_id` | string | Primary key. Unique product identifier. |
| `product_category_name` | string | Category that product belongs to (defined list) |
| `product_name_length` | integer | Character length of product name. |
| `product_description_length` | integer | Character length of product description. |
| `product_photos_qty` | integer | Number of photos included in listing. |
| `product_weight_g` | integer | Item weight (in grams). Numerical measure. |
| `product_length_cm` | integer | Length of package (in centimetres). |
| `product_height_cm` | integer | Height of package (in centimetres). |
| `product_width_cm` | integer | Width of package (in centimetres). |

### product_category_name_translation.csv

| Column Name | Data Type | Column Description |
|---|---|---|
| `product_category_name` | string | Category that product belongs to (Defined list). |
| `product_category_name_english` | string | Category that product belongs to (English translation). |

We should note also that the other two tables within the dataset have not been included as
they are not necessary for our analysis, however have been retained as sources that could
be explored at a later stage. These are:

- olist_order_payments_dataset.csv (`order_id`, `payment_sequential`, `payment_type`,
  `payment_installments`, `payment_value`), and
- olist_order_reviews_dataset.csv (`review_id`, `order_id`, `review_score`,
  `review_comment_title`, `review_comment_message`, `review_creation_date`,
  `review_answer_timestamp`).

---

## SECTION 4: High Cardinality Column Identification

**Primary:** `customer_zip_code_prefix` (contains approximately 15,000 distinct values). 

**Material level of skew:** The data contains a heavy concentration in Sao Paulo city, so a lot of the postcodes that are visible are skewed disproportionately towards this region, and will thus be useful for comparing hash and range portioning, which will likely output distributions of noticeably different shapes.

**Secondary control:** `order_id` (contains 98,666 distinct values).

We may consider this as a secondary comparison given that it is likely to be distributed more evenly.

----

## SECTION 5: Proposed business case

In our analysis, we want to look at freight costs across different shipping lanes and over time.
As such, we can define a shipping lane using the seller's state and the customer's state, and compare freight charges with factors such as item value, package weight and estimated shipping distance.

Through the analysis above, we want to explore one key goal, which is to identify shipping lanes where freight charges appear relatively high or low and to examine whether freight recovery changes over time. Ultimately through our analysis, we want to identify routes where freight pricing or delivery costs may warrant further investigation.

----

## SECTION 6: Suitability for distributed analytics

The Olist dataset is suitable for distributed analytics because it contains approximately 1.55 million records across several related datasets. It is important to note therefore, that our proposed analysis requires multiple joins between order items, orders, customers, sellers, products and geolocation data. Some measures must also be derived before analysis, including shipping lane, freight-to-price ratio, package volume and approximate seller-to- customer distance.

More specifically, our proposed analysis will involve:

- Joining multiple datasets to construct the required analytical view
- Deriving measures that are not directly available in the source data
- Aggregating results by shipping lane and time period
- Comparing performance across routes and over time
- Ranking or filtering routes based on aggregated results; and
- Potentially applying window functions to examine historical trends

Due to the above, we can conclude that the dataset provides us with sufficient analytical complexity to support implementation using both the Spark DataFrame API and Spark SQL, as well as later analysis of partitioning and execution performance.

## SECTION 7: Columns relevant to Assessment 2

Though a specific machine-learning task has not yet been selected, we can see that the dataset contains several variables that may support us with later predictive analysis.

We can define a handful of potential target variables, such as:
- `freight_value`, for a possible regression task
- A derived late-delivery indicator based on actual and estimated delivery dates
- A derived delivery duration measure

We can also define some predictor variables, such as:
- Price
- `product_weight_g`
- Product dimensions and derived package volume
- Product category
- Seller and customer location
- Derived shipping distance
- Order purchase date and other temporal features
- Payment and review information from the additional datasets

----

## SECTION 8: Data quality considerations

It is important to note that there are several data quality issues which we will need to consider during our implementation.

Most notably, the geolocation dataset contains multiple records for some postcode prefixes, so these records will need to be aggregated before they are joined to the main analytical dataset. Secondly, some delivery-related fields contain null values for orders that were not completed. As such, our main analysis will therefore need to focus on delivered orders where appropriate.

We should also note in our analysis to check for missing location or category information, as well as unusually high freight-to-price ratios that may result from very low-value items.