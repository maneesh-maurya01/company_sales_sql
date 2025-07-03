# sales management Analysis
## Project Overview
### Database db_sales
This is a database-driven project designed to track and manage all core sales activities within a business. This project emphasizes database design, normalization, and SQL proficiency through real-world scenarios such as generating sales reports, identifying best-selling products, and calculating revenue trends.
### Project Objective
1. Design a Relational Database Schema
2. Data Cleaning: Identify and remove any records with missing or null values.
3. Exploratory Data Analysis (EDA): Perform basic exploratory data analysis to understand the dataset.
4. Business Analysis: Use SQL to answer specific business questions and derive insights from the sales data.

### Project Structure
## 1. Create Database
* The project starts by creating a database named `sales_db`
* Tabble creation A table named `sales_table` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.
 
```ruby
CREATE DATABASE sales_db;

CREATE TABLE sales_table
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,	
    sale_time TIME,
    customer_id INT,	
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,	
    cogs FLOAT,
    total_sale FLOAT
);
```
## 2. Data Exploration & Cleaning
* Record Count: Determine the total number of records in the dataset.
* Customer Count: Find out how many unique customers are in the dataset.
* Category Count: Identify all unique product categories in the dataset.
* Null Value Check: Check for any null values in the dataset and delete records with missing data.
```ruby
SELECT COUNT(*) FROM sales_table;
SELECT COUNT(DISTINCT customer_id) FROM sales_table;
SELECT DISTINCT category FROM sales_table;

SELECT * FROM sales_table
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

DELETE FROM sales_table
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
```

## 3. Data Analysis & Findings
The following SQL queries were developed to answer specific business questions:

Q. Write a SQL query to retrieve all columns for sales made on '2022-11-05:
```ruby
SELECT *
FROM sales_table
WHERE sale_date = '2022-11-05';
```


Q. Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022:
```ruby
SELECT 
  *
FROM sales_table
WHERE 
    category = 'Clothing'
    AND 
    TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
    AND
    quantity >= 4
```

Q. Write a SQL query to calculate the total sales (total_sale) for each category:
```ruby
SELECT 
    category,
    SUM(total_sale) as net_sale,
    COUNT(*) as total_orders
FROM sales_table
GROUP BY 1
```

Q. Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.:
```ruby
SELECT
    ROUND(AVG(age), 2) as avg_age
FROM sales_table
WHERE category = 'Beauty'
```


Q. Write a SQL query to find all transactions where the total_sale is greater than 1000.:
```ruby
SELECT * FROM sales_table
WHERE total_sale > 1000
```

Q. Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.:
```ruby
SELECT 
    category,
    gender,
    COUNT(*) as total_trans
FROM sales_table
GROUP 
    BY 
    category,
    gender
ORDER BY 1
```


Q. Write a SQL query to calculate the average sale for each month. Find out best selling month in each year:
```ruby
SELECT 
       year,
       month,
    avg_sale
FROM 
(    
SELECT 
    EXTRACT(YEAR FROM sale_date) as year,
    EXTRACT(MONTH FROM sale_date) as month,
    AVG(total_sale) as avg_sale,
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank
FROM sales_table
GROUP BY 1, 2
) as t1
WHERE rank = 1
```


Q. Write a SQL query to find the top 5 customers based on the highest total sales :
```ruby
SELECT 
    customer_id,
    SUM(total_sale) as total_sales
FROM sales_table
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5
```


Q. Write a SQL query to find the number of unique customers who purchased items from each category.:
```ruby
SELECT 
    category,    
    COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM sales_table
GROUP BY category
```


Q. Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17):
```ruby
WITH hourly_sale
AS
(
SELECT *,
    CASE
        WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END as shift
FROM sales_table
)
SELECT 
    shift,
    COUNT(*) as total_orders    
FROM hourly_sale
GROUP BY shift
```

## Findings
**Customer Demographics:** The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.

**High-Value Transactions:** Several transactions had a total sale amount greater than 1000, indicating premium purchases.

**Sales Trends:** Monthly analysis shows variations in sales, helping identify peak seasons.

**Customer Insights:** The analysis identifies the top-spending customers and the most popular product categories.

## Reports
**Sales Summary:** A detailed report summarizing total sales, customer demographics, and category performance.

**Trend Analysis:** Insights into sales trends across different months and shifts.

**Customer Insights:** Reports on top customers and unique customer counts per category.

## Conclusion
 This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.






