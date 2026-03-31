# 🍔 Swiggy Sales Analysis | SQL Data Analytics Project

![Project Banner](screenshots/project_banner.png)

## 📌 Project Overview

This project analyzes **Swiggy food delivery data** using **PostgreSQL and SQL** to extract meaningful business insights.

The main objective is to transform raw food delivery data into a **structured analytical model** using **data cleaning, dimensional modeling, and SQL queries**.

This project simulates a **real-world data analyst workflow** used in food delivery and e-commerce companies.

---

# 🛠 Tools & Technologies

- SQL
- PostgreSQL
- pgAdmin
- CSV Dataset
- Data Modeling (Star Schema)

---

# 📂 Dataset Information

The dataset contains food delivery order records with the following fields:

| Column | Description |
|------|-------------|
| State | State where order was placed |
| City | City of the order |
| Order_Date | Date of the order |
| Restaurant_Name | Name of restaurant |
| Location | Area/location |
| Category | Cuisine type |
| Dish_Name | Name of dish |
| Price_INR | Dish price |
| Rating | Customer rating |
| Rating_Count | Number of ratings |

---

# ⚙️ Database Setup

A database called **swiggy_database** was created in PostgreSQL.

The dataset was imported using the following command:

```sql
COPY swiggy_data
FROM 'C:\Hyper Tech\SQL\Swiggy_Data.csv'
DELIMITER ','
CSV HEADER;
;

🧹 Data Cleaning

Data cleaning steps performed:

NULL value detection
Blank/empty field detection
Duplicate row detection
Duplicate removal using ROW_NUMBER() window function

Example query:

SELECT *,
ROW_NUMBER() OVER(
PARTITION BY state, city, order_date, restaurant_name,
location, category, dish_name
ORDER BY order_date
) AS row_num
FROM swiggy_data;

🏗 Data Modeling (Star Schema)

To optimize analytical queries, a Star Schema model was created.

Dimension Tables
dim_date
dim_location
dim_restaurant
dim_category
dim_dish
Fact Table
fact_swiggy_orders
Star Schema Structure
           dim_date
              |
dim_location — fact_swiggy_orders — dim_restaurant
              |
          dim_category
              |
            dim_dish

📊 Key Performance Indicators (KPIs)

The following KPIs were calculated:

Total Orders
Total Revenue
Average Dish Price
Average Rating

Example query:

SELECT COUNT(*) AS total_orders
FROM fact_swiggy_orders;

📈 Business Insights
Top 10 Cities by Orders
SELECT city, COUNT(*) AS total_orders
FROM fact_swiggy_orders
GROUP BY city
ORDER BY total_orders DESC
LIMIT 10;

Top Restaurants by Orders
SELECT restaurant_name, COUNT(*) AS orders
FROM fact_swiggy_orders
GROUP BY restaurant_name
ORDER BY orders DESC
LIMIT 10;

Most Ordered Dishes
SELECT dish_name, COUNT(*) AS total_orders
FROM fact_swiggy_orders
GROUP BY dish_name
ORDER BY total_orders DESC
LIMIT 10;

Revenue by State
SELECT state,
SUM(price_inr) AS revenue
FROM fact_swiggy_orders
GROUP BY state
ORDER BY revenue DESC;

💰 Customer Spending Analysis

Orders were categorized into spending buckets.

SELECT
CASE
WHEN price_inr < 100 THEN 'Under 100'
WHEN price_inr BETWEEN 100 AND 199 THEN '100-199'
WHEN price_inr BETWEEN 200 AND 299 THEN '200-299'
WHEN price_inr BETWEEN 300 AND 499 THEN '300-499'
ELSE '500+'
END AS spending_bucket,
COUNT(*) AS orders
FROM fact_swiggy_orders
GROUP BY spending_bucket;

🧠 SQL Concepts Used
Data Cleaning
GROUP BY
CASE WHEN
Window Functions
Aggregations
Dimensional Modeling
Star Schema Design
📊 Business Value

This analysis helps businesses understand:

Customer food preferences
High-performing restaurants
Popular dishes and cuisines
Regional order demand
Customer spending behavior

