# E-Commerce Marketplace Revenue, Retention & SLA Optimization (SQL Case Study)

An end-to-end **Business Analyst SQL Case Study** analyzing the **Brazilian E-Commerce Public Dataset by Olist** using **DuckDB SQL** inside **Google Colab**. This project evaluates marketplace growth, customer retention, payment behavior, regional freight economics, order cancellations, and delivery SLA impact on customer satisfaction.

---

## Business Problem & Objectives

E-commerce marketplaces often experience strong top-line GMV growth while losing margin and customer trust due to low repeat purchase rates, high regional shipping overhead, and late deliveries.

This analysis answers **10 core business questions** across **6 relational tables** to uncover actionable opportunities for:
1. **Revenue & Growth Strategy:** Tracking realized monthly revenue, Average Order Value (AOV), and Month-over-Month (MoM) momentum.
2. **Merchandising & Seller Focus:** Identifying top revenue-generating product categories and high-performing sellers.
3. **Customer Retention:** Measuring unique repeat buyer behavior versus one-time shoppers.
4. **Checkout & Payment Optimization:** Analyzing payment method share and installment adoption.
5. **Logistics & SLA Reliability:** Evaluating on-time vs. late delivery buckets, regional freight-to-price ratios, cancellation rates, and their impact on customer review scores.

---

## Dataset & Schema Overview

- **Source:** [Brazilian E-Commerce Public Dataset by Olist (Kaggle)](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
- **Tables Analyzed:**
  - `orders` — Order lifecycle timestamps, status (`delivered`, `canceled`, `unavailable`), and estimated delivery dates
  - `order_items` — Item-level product pricing, freight values, and seller IDs
  - `customers` — Customer mapping (`customer_unique_id`) and state-level geography
  - `products` & `product_category_name_translation` — Product catalog with standardized English category names
  - `payments` — Payment modes (`credit_card`, `boleto`, `voucher`, `debit_card`) and installment counts
  - `reviews` — Post-delivery customer review scores (`1` to `5` stars)

---

## Tech Stack & SQL Techniques Used

- **Environment & Engine:** Google Colab, Python (`pandas`), **DuckDB** (in-memory analytical SQL)
- **SQL Concepts Applied:**
  - Multi-table `INNER` and `LEFT` `JOIN`s across 6 relational tables
  - Common Table Expressions (`WITH` / CTEs) for modular query structuring
  - Window Functions (`LAG()`, `DENSE_RANK()`, `SUM() OVER ()`) for MoM growth, seller ranking, and revenue share
  - Conditional Aggregations (`COUNT(*) FILTER (WHERE ...)`, `CASE WHEN`)
  - Date & Time Analytics (`DATE_TRUNC`, `DATE_DIFF`) and `NULL` handling (`COALESCE`, `NULLIF`)

---

## Detailed Business Questions & Key Findings (10 Queries)

### 1. Monthly Revenue & Order Volume Trend
- **Business Question:** How have monthly delivered orders and gross merchandise revenue evolved over time across the marketplace?
- **What I Found:** Filtering strictly on `delivered` orders established a clean baseline of realized revenue and Average Order Value (AOV), showing steady upward order volume growth driven primarily by expanding order frequency rather than sharp AOV spikes.

### 2. Month-over-Month (MoM) Revenue & Order Growth
- **Business Question:** What is the month-over-month (MoM) percentage growth in delivered gross revenue and order volume, and which months experienced the sharpest acceleration or slowdown?
- **What I Found:** Using `LAG()` window functions revealed sharp promotional/seasonal surges (notably around November/Black Friday) followed by predictable post-peak normalization, helping pinpoint key demand cycles for campaign planning.

### 3. Top Revenue-Generating Product Categories
- **Business Question:** Which product categories generate the highest realized revenue and order volume, and how do their average item prices and revenue share compare across the catalog?
- **What I Found:** The top 10 categories (led by **Health & Beauty**, **Watches & Gifts**, and **Bed Bath Table**) account for **[X%]** of total realized revenue, clearly separating high-ticket categories from high-volume everyday categories.

### 4. Repeat Customer Rate & Customer Spend Comparison
- **Business Question:** What share of unique buyers place repeat orders versus one-time orders, and how does the average total spend of repeat customers compare with one-time customers?
- **What I Found:** Grouping by `customer_unique_id` revealed a low repeat customer rate of **[X%]**, while repeat buyers generated **[X%]** higher cumulative spend than one-time shoppers—making post-purchase retention the single biggest growth lever.

### 5. Delivery Performance (On-Time vs. Late Deliveries)
- **Business Question:** How do actual delivery dates compare against estimated delivery dates for delivered orders, and what percentage of orders fall into on-time versus late delivery buckets?
- **What I Found:** **[X%]** of orders arrived on or before the estimated delivery date, whereas **[X%]** arrived late (segmented into `1-3 days`, `4-7 days`, and `7+ days` late), exposing specific carrier delay tails.

### 6. Payment Method & Installment Behavior
- **Business Question:** How is total transaction value distributed across payment methods (`credit_card`, `boleto`, `voucher`, `debit_card`), and what is the average number of installments per payment type?
- **What I Found:** Credit cards accounted for **[X%]** of total payment value with an average of **[X]** installments per order, confirming that flexible EMI options are essential for high-value checkout conversions.

### 7. Regional Revenue & Freight Cost Burden by State
- **Business Question:** Which customer states (`customer_state`) generate the highest delivered product revenue and order volume, and how does freight cost (`freight_value`) as a percentage of product price vary across states?
- **What I Found:** Southeastern states (led by **SP**, **RJ**, and **MG**) drove the majority of GMV with lower freight-to-price ratios, whereas distant northern/northeastern states faced significantly higher shipping overhead relative to item price.

### 8. Order Status & Cancellation Rate by Product Category
- **Business Question:** Which product categories experience the highest share of cancelled or unavailable orders relative to total order attempts, and how much potential revenue is lost to non-delivered orders?
- **What I Found:** Filtering categories with at least 50 orders isolated specific segments with elevated `canceled` and `unavailable` rates, pointing to catalog stock-sync issues and seller fulfillment gaps.

### 9. Delivery Delay Impact on Customer Review Scores
- **Business Question:** How does delivery punctuality (`On-Time / Early` vs. `Late`) affect average customer review scores (`review_score`) and the share of low ratings (`1` or `2` stars)?
- **What I Found:** Late deliveries suffered a steep drop in average review score (**[X]** stars vs. **[X]** stars for on-time orders) and a sharply higher `1-2` star share (**[X%]**), proving that logistics SLA compliance directly drives customer satisfaction.

### 10. Top Sellers Performance & Delivery Speed
- **Business Question:** Who are the top 10 revenue-generating sellers on the marketplace, what share of total GMV do they drive, and how fast do they ship orders compared to their delivery SLA?
- **What I Found:** Ranking sellers via `DENSE_RANK()` showed that the top 10 sellers contribute **[X%]** of marketplace GMV, while contrasting their average delivery days highlighted high-volume sellers requiring logistics SLA support.

---

## Repository Structure

```text
olist-ecommerce-sql-case-study/
├── README.md                                 # Project overview, methodology, 10 business questions, and findings
└── olist_ecommerce_sql_case_study.ipynb      # Complete Colab notebook with SQL queries, outputs, and insights
