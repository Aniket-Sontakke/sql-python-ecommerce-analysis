# SQL and Python E-Commerce Customer Analysis

This repository contains an end-to-end data analysis project using SQL and Python on an e-commerce dataset.  
The project focuses on analyzing customer behavior, sales trends, and customer retention using structured SQL queries executed through Python in a Jupyter Notebook environment.

---

## Project Objectives

- Analyze customer purchase behavior
- Identify first-time and repeat customers
- Measure customer retention within a six-month window
- Perform sales and revenue analysis
- Practice advanced SQL concepts such as Common Table Expressions (CTEs), joins, and window functions
- Execute and validate SQL queries using Python

---

## Technology Stack

- Python
- MySQL
- SQL (CTEs, JOINs, Window Functions, Date Functions)
- mysql-connector-python
- Jupyter Notebook

---

## Dataset Overview

The dataset consists of multiple relational CSV files representing an e-commerce platform.  
A total of seven tables were available, out of which selected tables were used based on the analysis scope.

**Note:** Dataset files are not included in this repository.

---

## Tables Used

### customers.csv
Contains unique customer identifiers and basic customer information.  
Used as the base table for customer-level analysis.

### orders.csv
Stores order-level details including order timestamps.  
Used to identify first purchases, repeat purchases, and order timelines.

### order_items.csv
Contains item-level details for each order.  
Used for product-level and sales-related analysis.

### payments.csv
Stores payment-related information for orders.  
Used to calculate revenue and analyze payment behavior.

### products.csv
Provides product-level metadata.  
Used to associate orders with product categories.

---

## Tables Not Used

### sellers.csv
Contains seller-related information.  
Excluded as seller performance analysis was outside the primary scope of this project.

### geolocation.csv
Contains geographic location data.  
Excluded since location-based analysis was not required.

---

## Types of Analysis Performed

### Basic Analysis
- Unique customer locations
- Order counts by year
- Total sales by category
- Installment payment percentage
- Customer distribution by state

### Intermediate Analysis
- Monthly order trends
- Average products per order
- Revenue contribution by category
- Price vs purchase frequency analysis
- Seller revenue ranking

### Advanced Analysis
- Moving average of customer order values
- Cumulative monthly sales
- Year-over-year sales growth
- Customer retention analysis
- Top spending customers per year

---

## Customer Retention Analysis

Customer retention is defined as the percentage of customers who make another purchase within six months of their first order.

### Analysis Approach

1. A Common Table Expression (CTE) identifies the first purchase date for each customer.
2. A second CTE identifies repeat purchases made within six months.
3. A final query calculates the retention percentage.
4. SQL queries are executed and validated using Python.

---

## Sample SQL Logic

```sql
WITH first_orders AS (
    SELECT customer_id,
           MIN(order_purchase_timestamp) AS first_order
    FROM customers
    JOIN orders USING (customer_id)
    GROUP BY customer_id
),
repeat_orders AS (
    SELECT DISTINCT f.customer_id
    FROM first_orders f
    JOIN orders o
      ON o.customer_id = f.customer_id
     AND o.order_purchase_timestamp > f.first_order
     AND o.order_purchase_timestamp < DATE_ADD(f.first_order, INTERVAL 6 MONTH)
)
SELECT
    100 * COUNT(repeat_orders.customer_id) / COUNT(first_orders.customer_id)
    AS retention_percentage
FROM first_orders
LEFT JOIN repeat_orders
ON first_orders.customer_id = repeat_orders.customer_id;
