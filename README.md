# 🛒 Retail Sales Analysis — SQL Project

**By NWEZE SAMUEL SAVIOUR**

Welcome to my beginner-friendly SQL project where I explore, clean, and analyze sales data using structured query language. This project is perfect for developing a strong foundation in SQL, especially for those transitioning into data analytics roles or sharpening their data querying skills.

---

## 📌 Project Overview

* **Project Title:** Retail Sales Analysis
* **Skill Level:** Beginner
* **Tools Used:** PostgreSQL, pgAdmin, SQL
* **Database Name:** `Retail_Sales_Analysis`

The goal of this project is to simulate a real-world business scenario by working with raw retail sales data. From building the database to performing business analysis, this end-to-end project demonstrates how SQL can be used to turn raw data into actionable insights.

---

## 🎯 Objectives

1. **Database Setup:**
   Create and populate a PostgreSQL database called `Retail_Sales_Analysis`, with a table `retail_sales` to house transaction-level data.

2. **Data Cleaning:**
   Handle inconsistencies by identifying and removing `NULL` values, duplicates, and formatting errors.

3. **Exploratory Data Analysis (EDA):**
   Understand customer behavior, product performance, and regional trends using descriptive SQL queries.

4. **Business Insights:**
   Derive key metrics and answer specific business questions through advanced SQL queries using functions like `GROUP BY`, `WINDOW FUNCTIONS`, `CTE'S`, and more.

---

## 🧱 Project Structure

### 1. 🗄️ Database & Table Creation

* **Database:** `Retail_Sales_Analysis`
* **Table:** `retail_sales`
* **Columns Include:**

  * `transactions_id` (Primary Key)
  * `sale_date`
  * `sale_time`
  * `customer_id`
  * `gender`
  * `age`
  * `category` (e.g., Electronics, Clothing, Beauty)
  * `quantity`
  * `price_per_unit`
  * `cogs` (Cost of Goods Sold)
  * `total_sale`

---

## 📊 What You'll Learn

✅ How to design and populate SQL tables
✅ Techniques for cleaning messy datasets in SQL
✅ Writing effective queries to explore and summarize data
✅ Using SQL to answer real-world business questions
✅ Identifying trends in sales performance and customer behavior

---

## 📁 Folder Structure (if applicable)

```
/Retail-Sales-Analysis-SQL
│
├── sql_scripts/
│   ├── create_tables.sql
│   ├── insert_data.sql
│   ├── cleaning_queries.sql
│   └── analysis_queries.sql
│
├── README.md
└── dataset/
    └── retail_sales.csv
```

---

## 🔗 Connect With Me

If you're interested in data analysis, SQL, or would like to collaborate on future projects, feel free to connect!

📧 Email: hillarious4real@yahoo.com
🔗 LinkedIn:: linkedin.com/in/samuel-saviour-nweze-35a2681bb
🌐 Portfolio: 📎 samuel-nweze22.github.io
---

🧹 2. Data Exploration & Cleaning
This stage involves understanding the structure and quality of the data before moving to analysis.

📊 Record Count:
Calculated the total number of records available in the dataset to understand data volume.

🧑‍💼 Customer Count:
Identified the number of unique customers to get insights into the customer base.

📦 Category Count:
Retrieved all distinct product categories to evaluate product diversity and group sales metrics accordingly.

🚫 Null Value Check:
Checked for any missing values (NULLs) across columns and handled them by deleting or imputing affected records to ensure data accuracy


--CREATION OF TABLES

'''sql

	DROP TABLE IF EXISTS retail_sales;
'''
 
 '''sql
 
	CREATE TABLE retail_sales
				(
					transactions_id INT PRIMARY KEY,
					sale_date DATE,
					sale_time TIME,
					customer_id INT,
					gender VARCHAR(15),
					age INT,
					category VARCHAR(15),
					quantity INT,
					price_per_unit FLOAT,
					cogs FLOAT,
					total_sale FLOAT
					
				);
'''
   
--To count the total number of rows
'''sql

	
	  SELECT * FROM retail_sales
	  LIMIT 10;

'''  
  
  
'''sql

	SELECT
	  		COUNT(*)
	  		FROM retail_sales;  
  '''



--To find null values
'''sql

	  SELECT * FROM retail_sales
	  WHERE
	  	transactions_id IS NULL
	  	OR
	  	sale_date IS NULL
	  	OR
	  	sale_time IS NULL
	  	OR
	  	customer_id IS NULL
	  	OR
	  	gender IS NULL
	  	OR
	  	age IS NULL
	  	OR
	  	category IS NULL
	  	OR
	  	quantity IS NULL
	  	OR
	  	price_per_unit IS NULL
	  	OR
	  	cogs IS NULL
	  	OR
	  	total_sale IS NULL;
'''

--To delete null values that aren't useful
'''sql

	DELETE FROM retail_sales
	WHERE
		quantity IS NULL
		OR
		price_per_unit IS NULL
		OR
		cogs IS NULL
		OR
		total_sale IS NULL;
'''

--To replace null values in a single column
'''sql

	UPDATE retail_sales
	SET age = 25
	WHERE age IS NULL;
'''

--Data Exploration

--how many sales do we have?
'''sql

	SELECT 
		COUNT(*) as Total_Sales 
		FROM retail_sales;
'''	

--how many unique customers do we have?
'''sql

	SELECT 
		COUNT(DISTINCT customer_id) as Total_customers
		FROM retail_sales;
'''


--how many unique categories do we have?
'''sql

	SELECT 
		COUNT(DISTINCT category) as category
		FROM retail_sales;	
'''


--DATA ANALYSIS BUSINESS KEY PROBLEMS AND ANSWERS

--Q1 Write a query to retrieve all the columns for sales made on 2022-11-05
'''sql

	SELECT *
		FROM retail_sales
		WHERE sale_date = '2022-11-05';
'''


--Q2 Write a query to retrieve all transactions where the category is clothing and the quantity sold is more than 4 in the month of Nov, 2022.	
'''sql

	SELECT *
		FROM retail_sales
		WHERE category = 'Clothing' AND quantity >=4
		AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11';
'''

--Q3 Write a query to calculate total sales (total_sale) for each category
'''sql

	SELECT category,
		SUM(total_sale) as Net_Sales,
		COUNT(*) as Total_Orders
		FROM retail_sales
		GROUP BY category;
	'''

--Q4 Write a query to find the average age of customers who purchased items from beauty category
'''sql

	  SELECT 
	  	ROUND(AVG(age), 2) as Avg_age
	  	FROM retail_sales
	  	WHERE category = 'Beauty';
'''

--Q5 Write a query to find all transactions where total sales is greater than 1000	
'''sql

	SELECT *
		FROM retail_sales
		WHERE total_sale > 1000
'''


--Q6 Write a query to find the total number of transactions made by each gender in each category.
'''sql

	SELECT gender, category,
		COUNT(DISTINCT transactions_id) as total_transactions
		FROM retail_sales
		GROUP BY gender, category
		ORDER BY gender;
'''


--Q7 Write a query to calculate the average sale for each month. Find out best-selling months in each year.
'''sql

	SELECT year, month,avg_sales
		FROM 
	(
	SELECT
		EXTRACT(YEAR FROM sale_date) as year,
		EXTRACT(MONTH FROM sale_date) as  month,
		AVG(total_sale) as avg_sales, 
		RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank
		FROM retail_sales
		GROUP BY 1, 2 
	) as t1
	WHERE rank = 1
'''


--Q8 Write a query to find top 5 customers based on the highest total sales
'''sql

	SELECT customer_id,
		SUM(total_sale) as total_sales
		FROM retail_sales
		GROUP BY customer_id
		ORDER BY total_sales DESC
		LIMIT 5
'''

--Q9 Write a query to find the number of unique customers who purchased items from each category
'''sql

	SELECT category,
		COUNT(DISTINCT customer_id) as unique_customers
		FROM retail_sales
		GROUP BY category;
'''


--Q10 Write a query to create each shift and number of orders (Example, morning <12, afternoon between 12 and 17, and evening > 17)
'''sql

	WITH hourly_sale
	AS
	(
	SELECT *,
		CASE
			WHEN EXTRACT(HOUR FROM sale_time) < 12  THEN 'Morning'
			WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
			ELSE 'Evening'
		END	as shift
	FROM retail_sales
	)
	SELECT
		shift,
		COUNT(*) as total_orders
	FROM hourly_sale
	GROUP BY shift; 
'''
--END OF THE PROJECT


---

## 📌 Findings

* A total of sales records were analyzed.
* The store served unique customers across several product categories.
* Clothing and Electronic recorded the highest number of transactions.
* A significant portion of the data had null values in the `age` column, which was handled by imputing a default age.
* Most sales occurred in the Morning shift (before 12 PM).
* The Beauty category had the youngest average customers.
* November 2022 was one of the most active sales months based on average sales.

---

## 📊 Reports

* 📅 Date-Specific Queries: Identified all transactions for specific dates like `'2022-11-05'`.
* 👕 Category Insights: Filtered out transactions for specific categories with conditions like quantity sold and month.
* 💸 Sales Summary: Calculated total and average sales by category, customer, and month.
* 🧑‍🤝‍🧑 Customer Behavior: Analyzed customer engagement by gender and product category.
* 🕒 Time-Based Sales: Created time shifts (Morning, Afternoon, Evening) to observe peak hours.
* 🏆 Top Customers: Listed customers with the highest contribution to total sales.

---

## ✅ Conclusion

This project demonstrates how SQL can be effectively used to clean data, perform exploratory analysis, and generate actionable insights in a retail sales environment. Through various queries, I was able to:

* Understand customer demographics and purchasing behavior.
* Discover sales trends across categories and time periods.
* Identify top-performing customers and products.

The analysis provides a strong foundation for data-driven business decisions in retail operations and showcases my SQL proficiency in a real-world context.

---


