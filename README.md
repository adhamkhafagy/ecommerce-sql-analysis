

---

# 🛒 Portfolio Project: E-Commerce User Behavior & Sales Analysis

## 📌 Project Overview

The goal of this project is to analyze customer behavior and sales performance for an e-commerce platform. Using user event log data, this analysis evaluates the sales funnel, identifies where users drop off, and highlights the most profitable traffic sources and products.

**Tools Used:** SQL (PostgreSQL/SQLite)
**Dataset:** `user_events.csv` (10,000+ rows of event logs including page views, cart additions, checkouts, and purchases).

---

## 🔍 Part 1: The E-Commerce Sales Funnel

**Business Question:** How many users successfully move from viewing a page to making a purchase, and where is the biggest drop-off in the funnel?

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

### 💡 Insight

* **Overall Conversion:** The overall conversion rate from `page_view` to `purchase` is **16.5%** (826 / 5,000), which is exceptionally healthy for e-commerce.
* **Largest Drop-off:** The most significant drop-off occurs between `page_view` and `add_to_cart`, where nearly **69%** of users leave without adding anything to their cart. Improving product page engagement or clarity should be a top priority.

---

## 💰 Part 2: Revenue by Traffic Source

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
* **Social Media** brings in the fewest users, but has the **highest Average Order Value ($111.09)**. There is an opportunity here to increase ad spend on social channels to attract high-paying customers.

---

## 🏆 Part 3: Top Performing Products

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

* **Product 205 and 404** are the top sellers, combining for over $31,000 in revenue.
* Sales volume across the top 5 products is quite balanced (ranging between 134 to 147 units sold), showing a healthy product catalog that isn't overly reliant on a single "hero" product.

---

