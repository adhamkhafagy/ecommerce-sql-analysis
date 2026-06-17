🛒 Portfolio Project: E-Commerce User Behavior & Sales Analysis
📌 Project Overview
The goal of this project is to analyze customer behavior and sales performance for an e-commerce platform. Using user event log data, this analysis evaluates the sales funnel, identifies where users drop off, and highlights the most profitable traffic sources and products.

Tools Used: SQL (PostgreSQL/SQLite)
Dataset: user_events.csv (9,000+ rows of event logs including page views, cart additions, checkouts, and purchases).

🔍 Part 1: The E-Commerce Sales Funnel
Business Question: How many users successfully move from viewing a page to making a purchase, and where is the biggest drop-off in the funnel?

SQL Query
SELECT 
    event_type, 
    COUNT(DISTINCT user_id) AS unique_users,
    COUNT(event_id) AS total_events
FROM events
GROUP BY event_type
ORDER BY unique_users DESC;

Results
|Event_Type    |	| Unique_Users|	 |Total_Events|
|page_view     |	| 5000	      |  | 5000       |
|add_to_cart   |	| 1553	      |  | 1553       |
|checkout_start|	| 1103	      |  | 1103       |
|payment_info  |	| 899	      |  | 899        |
|purchase      |	| 826	      |  | 826        |

Key Insights
The biggest drop-off occurs between page_view → add_to_cart
Only 16.5% of users complete a purchase
Users who reach checkout stages have a high probability of converting



💰 2. Revenue by Traffic Source
Business Question

Which acquisition channels generate the highest revenue?

SQL Query
SELECT 
    traffic_source,
    COUNT(DISTINCT user_id) AS purchasing_users,
    SUM(amount) AS total_revenue,
    ROUND(AVG(amount), 2) AS avg_order_value
FROM events
WHERE event_type = 'purchase'
GROUP BY traffic_source
ORDER BY total_revenue DESC;

Results
|Traffic_Source|	|purchasing_users|	|total_revenue|	|avg_order_value|
|Organic       |	|343             |	|37,279.98    |	|108.69         |
|Paid Ads      |	|204             |	|21,487.54    |	|105.33         |
|Email         |	|177             |	|17,876.75    |	|101.00         |
|Social        |	|102             |	|11,330.84    | |111.09         |
