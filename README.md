# 🛒 E-Commerce User Behavior & Revenue Analytics Case Study

## 📌 Overview

This project analyzes user behavior across an e-commerce platform to understand customer journey performance, revenue drivers, and product performance. The goal is to identify conversion bottlenecks and provide actionable insights to improve revenue and user experience.

The analysis was performed using **Microsoft SQL Server** for data querying and transformation, with Python used for supplementary analysis and validation.

---

## 🎯 Business Objectives

* Measure user progression through the sales funnel
* Identify major drop-off points in the customer journey
* Evaluate revenue performance by traffic source
* Identify top-performing products
* Provide data-driven recommendations to improve conversion rates

---

## 🧰 Tools & Technologies

* Microsoft SQL Server
* SQL Server Management Studio (SSMS)
* Python (Pandas)
* Jupyter Notebook
* Git & GitHub

---

## 📂 Dataset Overview

The dataset contains user event-level logs from an e-commerce platform.

### Key Columns

| Column         | Description                              |
| -------------- | ---------------------------------------- |
| user_id        | Unique user identifier                   |
| event_id       | Unique event identifier                  |
| event_type     | Type of user action                      |
| event_date     | Timestamp of event                       |
| product_id     | Product involved in event                |
| traffic_source | Acquisition channel                      |
| amount         | Transaction value (purchase events only) |

---

# 📊 1. Sales Funnel Analysis

## Business Question

How effectively do users move through the purchasing funnel?

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
Results
Event Type	Unique Users	Total Events
page_view	5000	5000
add_to_cart	1553	1553
checkout_start	1103	1103
payment_info	899	899
purchase	826	826

---

# 💰 2. Revenue by Traffic Source

## Business Question

Which acquisition channels generate the highest revenue?

### SQL  Query

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

---

# 🏆 3. Top Performing Products

## Business Question

Which products generate the highest revenue?

### SQL  Query

```sql
SELECT TOP 5
    product_id,
    COUNT(event_id) AS times_purchased,
    SUM(amount) AS total_revenue
FROM events
WHERE event_type = 'purchase'
    AND product_id IS NOT NULL
GROUP BY product_id
ORDER BY total_revenue DESC;
```

---

# 📁 Project Structure

```text
ecommerce-sql-server-analysis/
│
├── data/
│   └── user_events.csv
│
├── sql/
│   ├── create_table.sql
│   ├── import_data.sql
│   ├── funnel_analysis.sql
│   ├── revenue_analysis.sql
│   └── product_analysis.sql
│
├── notebooks/
│   └── validation_analysis.ipynb
│
├── images/
│   ├── funnel_chart.png
│   ├── revenue_by_source.png
│   └── top_products.png
│
└── README.md
```

---

# 👨‍💻 Author

**Adham Refaat Soliman**

Data Analyst | SQL | Power BI | Python

This project demonstrates SQL-based business analysis, funnel analytics, revenue reporting, and product performance evaluation using Microsoft SQL Server.
