# RETAIL_INSIGHTS-A_SQL-DRIVEN_APPROACH
# 🛒 RETAIL INSIGHTS: A SQL-DRIVEN APPROACH

This project explores and analyzes a retail sales dataset using SQL. It includes everything from data cleaning and preprocessing to advanced business insights using SQL queries. It is perfect for showcasing practical SQL skills used in real-world data analytics.

---

## 📌 OBJECTIVES

- Clean and validate raw retail data
- Perform exploratory data analysis using SQL
- Generate insights on product performance, customer behavior, and time-based trends
- Demonstrate use of SQL functions, aggregations, CTEs, and window functions

---

## 🧾 DATASET DESCRIPTION

The dataset contains transactional records of a retail store, including:

- Sale Date and Time  
- Product Category  
- Customer Demographics (Gender, Age)  
- Quantity, Price, Total Sale Value  
- Cost of Goods Sold (COGS)

---

## 🗃️ TABLE SCHEMA

```sql
CREATE TABLE retail (
  transactions_id INT PRIMARY KEY,
  sale_date DATE,
  sale_time TIME,
  customer_id INT,
  gender VARCHAR(25),  
  age INT,
  category VARCHAR(50),
  quantity INT,
  price_per_unit FLOAT,  
  cogs FLOAT,  
  total_sale FLOAT
);


ANALYSIS COVERED
1.Total number of records and customers
2.Unique product categories
3.Sales on a specific date (2022-11-05)
4.Category-wise sales with filters
5.Top 5 customers by total spend
6.Highest value transactions
7.Best-selling month in each year using RANK()
8.Average age of customers by category
9.Shift-wise sales analysis (Morning, Afternoon, Evening)
10.Customer and gender-based segmentation
11.Null handling and data cleanup

create table retail(
transactions_id int primary key,
sale_date date,
sale_time time,
customer_id int,
gender varchar(25),	
age int,
category varchar(50),
quantiy	int,
price_per_unit float,	
cogs float,	
total_sale float
);
select * from retail;

--Data Exploration & Cleaning

--Record Count**: Determine the total number of records in the dataset.
select count (*) from retail;

--Customer Count**: Find out how many unique customers are in the dataset.
select count (distinct customer_id) from retail;

--Category Count**: Identify all unique product categories in the dataset.
select count (distinct category) from retail;
select transactions_id, category from retail;

--Null Value Check**: Check for any null values in the dataset 
select * from retail
where 
sale_date is null or sale_time is null or customer_id is null or gender is null or 
age is null or category is null or quantiy is null or price_per_unit is null or
cogs is null or total_sale is null;

--and delete records with missing data
delete from retail
where
sale_date is null or sale_time is null or customer_id is null or gender is null or 
age is null or category is null or quantiy is null or price_per_unit is null or
cogs is null or total_sale is null;

--**Write a SQL query to retrieve all columns for sales made on '2022-11-05**:
select * from retail
where sale_date = '2022-11-05';

select transactions_id, sale_date
from retail
where sale_date >= '2022-11-05';

/**Write a SQL query to retrieve all transactions where the category is 
'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**/
select * from retail
where category = 'Clothing' and 
to_char(sale_date, 'yyyy-mm') = '2022-11' and
quantiy = 4;

--**Write a SQL query to calculate the total sales (total_sale) for each category.**:
select distinct category,  
sum(total_sale) as sale_of_year,
count (*) as total_order
from retail
group by category;

/*--**Write a SQL query to find the average age of customers who purchased 
items from the 'Beauty' category.**/:
select
round(avg(age), 2) as avg_age_of_customer
from retail
where category ='Beauty';

/**Write a SQL query to find all transactions where the total_sale 
is greater than 1000.**/:
select * from retail
where total_sale >1000;

/**Write a SQL query to find the total number of transactions (transaction_id) 
made by each gender in each category.**/
select category, gender, 
count (*) as total_trans
from retail
group  by
category,
gender
Order by total_trans desc
limit 5;

/**Write a SQL query to calculate the average sale for each month. 
Find out best selling month in each year**/
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
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date)
	ORDER BY AVG(total_sale) DESC) as rank
FROM retail
GROUP BY 1, 2
) as t1
WHERE rank = 1;
/**Write a SQL query to find the top 5 customers based on the highest total 
sales*/
SELECT 
    customer_id,
    SUM(total_sale) as total_sales
FROM retail
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;

/**Write a SQL query to find the number of unique customers who purchased 
items from each category.*/
SELECT 
    category,    
    COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM retail
GROUP BY category;

/**Write a SQL query to create each shift and number of orders 
(Example Morning <12, Afternoon Between 12 & 17, Evening >17)*/
WITH hourly_sale
AS
(
SELECT *,
    CASE
        WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END as shift
FROM retail
)
SELECT 
    shift,
    COUNT(*) as total_orders    
FROM hourly_sale
GROUP BY shift;

## Conclusion

This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.

## How to Use

1. **Clone the Repository**: Clone this project repository from GitHub.
2. **Set Up the Database**: Run the SQL scripts provided in the `database_setup.sql` file to create and populate the database.
3. **Run the Queries**: Use the SQL queries provided in the `analysis_queries.sql` file to perform your analysis.
4. **Explore and Modify**: Feel free to modify the queries to explore different aspects of the dataset or answer additional business questions.


Thank you for your support, and I look forward to connecting with you!
