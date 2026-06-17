

---

# 🛒 Portfolio Project: E-Commerce User Behavior & Sales Analysis

## 📌 Project Overview

The goal of this project is to analyze customer behavior and sales performance for an e-commerce platform. Using user event log data, this analysis evaluates the sales funnel, identifies where users drop off, and highlights the most profitable traffic sources and products.

**Tools Used:** SQL (PostgreSQL/SQLite)
**Dataset:** `user_events.csv` (9,000+ rows of event logs including page views, cart additions, checkouts, and purchases).

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

Adding a strategic conclusion is an excellent idea. Hiring managers and stakeholders don't just want to see that you can write SQL; they want to see that you can translate data into actionable business decisions.

Here is a **Conclusion & Strategic Recommendations** section you can copy and paste directly at the bottom of your `README.md` file, right after Part 3.

---

## 🎯 Conclusion & Strategic Recommendations

Based on the analysis of the user funnel, traffic sources, and product performance, here are the key recommendations to improve overall e-commerce performance:

* **Optimize the Product Discovery Phase:** The largest bottleneck in the funnel is the 69% drop-off between viewing a page and adding an item to the cart. The business should A/B test product page layouts, improve product imagery, prominently display customer reviews, or offer a temporary "first-time buyer" discount to incentivize that initial cart addition.
* **Scale Social Media Marketing:** While Organic traffic drives the highest volume, Social Media drives the highest Average Order Value ($111.09). Reallocating a portion of the marketing budget to targeted social media campaigns (e.g., Instagram, TikTok) could effectively attract more of these high-value customers.
* **Leverage Top-Selling Products for Upselling:** Products 205 and 404 are consistent top-performers. The business should use these as "anchor" products by featuring them on the landing page or creating "Frequently Bought Together" bundles with lower-performing items to increase overall cart sizes.
* **Investigate Cart-to-Checkout Friction:** Although the 16.5% overall conversion rate is strong, there is still an opportunity to reduce friction between the cart and the final purchase. Implementing a one-click checkout or guest checkout option could help convert the remaining users who abandon their carts.

