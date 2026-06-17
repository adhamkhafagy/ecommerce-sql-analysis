
# 🛒 Portfolio Project: E-Commerce User Behavior & Sales Analysis

## 📌 Project Overview

The goal of this project is to analyze customer behavior and sales performance for an e-commerce platform. Using user event log data, this analysis evaluates the sales funnel, identifies where users drop off, and highlights the most profitable traffic sources and products to drive strategic business decisions.

## 🛠️ Technologies & Skills Used

* **Database:** SQL (PostgreSQL / SQLite)
* **Techniques:** Common Table Expressions (CTEs), Window Functions (`LAG`), Aggregations, Data Grouping, Type Casting.
* **Business Concepts:** Funnel Analysis, Conversion Rate Optimization (CRO), Average Order Value (AOV).

## 📊 Dataset

The analysis is based on a dataset named `user_events.csv`, which contains over 9,000+ rows of event logs representing user interactions on the platform.

* **Key Columns:** `event_id`, `user_id`, `event_type` (page_view, add_to_cart, checkout_start, payment_info, purchase), `traffic_source`, `product_id`, and `amount`.

---

## 🔍 Phase 1: Overall Sales Funnel Counts

**Business Question:** How many users successfully move from viewing a page to making a purchase?

### SQL Query

```sql
SELECT 
    event_type, 
    COUNT(DISTINCT user_id) AS unique_users,
    COUNT(event_id) AS total_events
FROM events
GROUP BY event_type
ORDER BY unique_users DESC;

```

### Results

| Event Type | Unique Users | Total Events |
| --- | --- | --- |
| `page_view` | 5,000 | 5,000 |
| `add_to_cart` | 1,553 | 1,553 |
| `checkout_start` | 1,103 | 1,103 |
| `payment_info` | 899 | 899 |
| `purchase` | 826 | 826 |

---

## 📉 Phase 2: Step-by-Step Funnel Conversion Rate

**Business Question:** What is the step-by-step conversion rate, and at which specific transition do we lose the highest percentage of users?

### SQL Query

```sql
WITH Funnel AS (
    SELECT 
        event_type, 
        COUNT(DISTINCT user_id) AS users,
        CASE 
            WHEN event_type = 'page_view' THEN 1
            WHEN event_type = 'add_to_cart' THEN 2
            WHEN event_type = 'checkout_start' THEN 3
            WHEN event_type = 'payment_info' THEN 4
            WHEN event_type = 'purchase' THEN 5
        END AS step_order
    FROM events
    GROUP BY event_type
)
SELECT 
    event_type AS funnel_step,
    users AS current_users,
    LAG(users) OVER(ORDER BY step_order) AS previous_step_users,
    ROUND((users * 100.0) / LAG(users) OVER(ORDER BY step_order), 2) AS step_conversion_pct
FROM Funnel
ORDER BY step_order;

```

### Results

| Funnel Step | Current Users | Previous Step Users | Step Conversion Rate (%) |
| --- | --- | --- | --- |
| `page_view` | 5,000 | *NULL* | *NULL* |
| `add_to_cart` | 1,553 | 5,000 | **31.06%** |
| `checkout_start` | 1,103 | 1,553 | **71.02%** |
| `payment_info` | 899 | 1,103 | **81.50%** |
| `purchase` | 826 | 899 | **91.88%** |

### 💡 Phase 1 & 2 Insights

* **Overall Conversion:** The overall conversion rate from `page_view` to `purchase` is **16.5%** (826 / 5,000), which is exceptionally healthy for e-commerce.
* **High Bottom-Funnel Intent:** Once a user adds an item to their cart, conversion rates are strong. 71% proceed to checkout, and an impressive **91.8%** of users who enter their payment info successfully complete the purchase. This indicates the checkout UI is smooth and largely free of friction.
* **Top-of-Funnel Bottleneck:** The single biggest leak in the funnel is between viewing a product and adding it to the cart, with only **31.06%** of users making the jump.

---

## 💰 Phase 3: Revenue by Traffic Source

**Business Question:** Which marketing channels are driving the most revenue, and what is the Average Order Value (AOV) per channel?

### SQL Query

```sql
SELECT 
    traffic_source,
    COUNT(DISTINCT user_id) AS purchasing_users,
    SUM(amount) AS total_revenue,
    ROUND(AVG(amount), 2) AS avg_order_value
FROM events
WHERE event_type = 'purchase'
GROUP BY traffic_source
ORDER BY total_revenue DESC;

```

### Results

| Traffic Source | Purchasing Users | Total Revenue ($) | Avg Order Value ($) |
| --- | --- | --- | --- |
| `organic` | 343 | 37,279.98 | 108.69 |
| `paid_ads` | 204 | 21,487.54 | 105.33 |
| `email` | 177 | 17,876.75 | 101.00 |
| `social` | 102 | 11,330.84 | 111.09 |

### 💡 Insight

* **Organic Traffic** is the absolute powerhouse of the business, bringing in both the highest number of purchasing users and the most revenue (~$37.2k).
* **Social Media** brings in the fewest users but has the **highest Average Order Value ($111.09)**.

---

## 🏆 Phase 4: Top Performing Products

**Business Question:** Which individual products are generating the highest total revenue?

### SQL Query

```sql
SELECT 
    product_id,
    COUNT(event_id) AS times_purchased,
    SUM(amount) AS total_revenue
FROM events
WHERE event_type = 'purchase' AND product_id IS NOT NULL
GROUP BY product_id
ORDER BY total_revenue DESC
LIMIT 5;

```

### Results

| Product ID | Times Purchased | Total Revenue ($) |
| --- | --- | --- |
| `205` | 147 | 15,905.07 |
| `404` | 141 | 15,377.31 |
| `101` | 134 | 14,688.18 |
| `305` | 138 | 14,622.48 |
| `201` | 134 | 13,925.94 |

### 💡 Insight

* **Products 205 and 404** are the top sellers, combining for over $31,000 in revenue.
* Sales volume across the top 5 products is quite balanced, showing a healthy product catalog that isn't overly reliant on a single "hero" product.

---

## 🎯 Conclusion & Strategic Recommendations

Based on the analysis of the user funnel, traffic sources, and product performance, here are the key recommendations to improve overall e-commerce metrics:

1. **Optimize the Product Discovery Phase:** The largest bottleneck in the funnel is the 69% drop-off between viewing a page and adding an item to the cart. The business should A/B test product page layouts, improve product imagery, prominently display customer reviews, or offer a temporary "first-time buyer" discount to incentivize that initial cart addition.
2. **Scale Social Media Marketing:** While Organic traffic drives the highest volume, Social Media drives the highest Average Order Value ($111.09). Reallocating a portion of the marketing budget to targeted social media campaigns (e.g., Instagram, TikTok) could effectively attract more high-value customers.
3. **Leverage Top-Selling Products for Upselling:** Products 205 and 404 are consistent top-performers. The business should use these as "anchor" products by featuring them on the landing page or creating "Frequently Bought Together" bundles with lower-performing items to increase overall cart sizes.

---

## 💻 How to Run This Project

1. Clone the repository to your local machine.
2. Ensure you have a SQL environment set up (e.g., PostgreSQL, MySQL, or DB Browser for SQLite).
3. Import `user_events.csv` into a new table named `events`.
4. Run the queries provided in `queries.sql` to replicate the analysis.

---

Would you like me to write out the `CREATE TABLE` and `INSERT` schema SQL commands so anyone reviewing your portfolio can instantly build the database and test your queries?
