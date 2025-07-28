# 🛒 E-commerce Sales & Customer Analysis using SQL And Python

![E-Commerce Sales Dashboard](1703121210_e-commerce.png)

This project demonstrates a deep dive into customer behavior, order trends, sales performance, and retention using **advanced SQL techniques** on an e-commerce dataset.

---

## 📂 Project Highlights

✅ Performed **business-critical analysis** using SQL queries to extract actionable insights from customer, order, and payment data.

✅ Used **window functions**, **aggregations**, **joins**, and **CTEs** to answer both **business and technical questions**.

✅ Covered analysis at all levels:
- **Basic**: Order counts, unique cities, total sales
- **Intermediate**: Monthly trends, average items per order, revenue contribution
- **Advanced**: Retention rate, year-over-year growth, moving averages

---

## 💻 Technologies Used

- **MySQL / PostgreSQL / SQL syntax**
- **Subqueries & CTEs**
- **Window Functions**
- **Date Functions**
- **Join operations**

---

## 🔍 Key Questions Answered

### 📊 Basic Level:
- How many orders are yet to be delivered?
- What are the most common order statuses?
- How many unique cities are customers located in?
- Total revenue per country/state
- Number of customers per state

### 📈 Intermediate Level:
- Number of orders placed per month in 2018
- Average number of products per order by city
- Percentage of revenue by product category
- Correlation between product price & frequency
- Top revenue-generating sellers

### 📈 Advanced Level:
- Moving average of order values per customer over time
- Cumulative sales per month by year
- Year-over-year growth of total sales
- 6-month customer retention rate
- Top 3 customers by spend per year

---

## 📘 Sample Queries

```sql
-- Count orders not delivered
SELECT
    order_id,
    order_status,
    order_delivered_customer_date
FROM
    orders
WHERE
    order_delivered_customer_date IS NULL;

-- Calculate YoY Growth
WITH AnnualSales AS (
    SELECT
        YEAR(order_purchase_timestamp) AS year,
        SUM(payment_value) AS total_sales
    FROM orders
    JOIN payments USING(order_id)
    GROUP BY year
)
SELECT
    *,
    LAG(total_sales) OVER (ORDER BY year) AS previous_year_sales,
    ROUND(
        (total_sales - LAG(total_sales) OVER (ORDER BY year)) * 100.0 / LAG(total_sales) OVER (ORDER BY year),
        2
    ) AS yoy_growth_percent
FROM AnnualSales;
