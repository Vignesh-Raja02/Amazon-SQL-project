# Amazon Sales Analysis Project
![amazon_india_wide_image-3](https://github.com/user-attachments/assets/026cb94f-966e-42ea-bb17-e773258d590f)

Welcome to the Amazon Sales Analysis project! In this project, we delve into analyzing sales
data from Amazon to extract insights and trends that can help optimize sales strategies,
understand customer behavior, and improve business operations.

## Introduction

This project focuses on analyzing a dataset containing Amazon sales records, including
information such as sales dates, customer details, product categories, and revenue figures. By
leveraging SQL queries and data analysis techniques, we aim to answer various questions and
uncover valuable insights from the dataset.

## Dataset Overview

The dataset used in this project consists of [insert number] rows of data, representing Amazon
sales transactions. Along with the sales data, the dataset includes information about customers,
products, orders, and returns. Before analysis, the dataset underwent preprocessing to handle
missing values and ensure data quality.

## Analysis Questions Resolved

During the analysis, the following key questions were addressed using SQL queries and data
analysis techniques:

## Analysis Questions Resolved
During the analysis, the following key questions were addressed using SQL queries and data
analysis techniques:

1. Find out the top 5 customers who made the highest profits.
```sql
SELECT 
	o.customer_id,
	ROUND(SUM(o.sale -(o.quantity*p.cogs))::NUMERIC,2) AS profit
FROM orders AS o
JOIN products AS p ON p.product_id = o.product_id 
GROUP BY o.customer_id
ORDER BY SUM(o.sale -(o.quantity*p.cogs)) DESC
LIMIT 5
```

2. Find out the total quantity ordered per category.
```sql

SELECT 
	category,
	SUM(quantity) AS total_quantity
FROM orders
GROUP BY category
```

3. Identify the top 5 products that have generated the highest revenue.
```sql
SELECT 
	product_id,
	ROUND(SUM(sale)::NUMERIC,2) AS total_revenue
FROM orders
GROUP BY product_id
ORDER BY SUM(sale) DESC
LIMIT 5
```

4. Determine the top 5 products whose revenue has decreased compared to the previous year.
```sql

WITH current_year 
AS 
(	SELECT 
		product_id,
		ROUND(SUM(sale)::NUMERIC,2) AS total_revenue
	FROM orders
	WHERE order_date BETWEEN '2024-01-01' AND '2024-12-31'   --CURRENT_DATE - INTERVAL '365days'
	GROUP BY product_id
),
Previous_year 
AS
(	SELECT 
		product_id,
		ROUND(SUM(sale)::NUMERIC,2) AS total_revenue
	FROM orders
	WHERE order_date BETWEEN '2023-01-01' AND '2023-12-31'  
	GROUP BY product_id
)
		SELECT 
			p.product_id,
			p.total_revenue AS previousyear_revenue,
			c.total_revenue AS currentyear_revenuue,
			((c.total_revenue-p.total_revenue)/p.total_revenue)*100 AS comparison_percentage
		FROM previous_year AS p
		JOIN current_year AS c
		ON p.product_id = c.product_id
		ORDER BY comparison_percentage ASC 
		LIMIT 5
```

5. Identify the highest profitable sub-category.
```sql
SELECT 
	o.sub_category,
	ROUND(SUM(o.sale -(o.quantity*p.cogs))::NUMERIC,2) AS profit
FROM orders AS o
JOIN products AS p ON p.product_id = o.product_id 
GROUP BY o.sub_category
ORDER BY profit DESC
```

6. Find out the states with the highest total orders.
```sql
SELECT 
	state,
	COUNT(order_id) AS total_orders
FROM orders
GROUP BY state
ORDER BY total_orders DESC
```

7. Determine the month with the highest number of orders.
```sql
SELECT 
	TO_CHAR(order_date, 'YYYY-MM') AS month, 
	COUNT(order_id) AS total_orders
FROM orders 
GROUP BY TO_CHAR(order_date, 'YYYY-MM')
ORDER BY total_orders DESC
```

8. Calculate the profit margin percentage for each sale (Profit divided by Sales).
```sql

WITH profit 
AS
(	SELECT 
		o.order_id,
		ROUND(SUM(o.sale -(o.quantity*p.cogs))::NUMERIC,2) AS profit
	FROM orders AS o
	JOIN products AS p ON p.product_id = o.product_id 
	GROUP BY o.order_id
)

	SELECT 
		o.order_id,
	 	ROUND(((p.profit/o.sale)*100)::NUMERIC,2) AS profit_margin_percentage
	FROM orders AS o
	JOIN profit AS p ON o.order_id=p.order_id
```

9. Calculate the percentage contribution of each sub-category.
```sql
WITH sub_category_sale
AS 
(	 SELECT 
		SUM(sale) AS total_sale
	FROM orders
)
 	SELECT 
	 	o.sub_category,
		SUM(o.sale) AS total_sale,
		(SUM(o.sale)/s.total_sale)*100 AS percentage_contribution	
	FROM orders AS o
	JOIN sub_category_sale AS s ON 1=1
	GROUP BY o.sub_category, s.total_sale
```

10. Identify the top 2 categories that have received maximum returns and their return
percentage.
```sql

```

## Entity-Relationship Diagram (ERD)
![ERD_Amazon](https://github.com/user-attachments/assets/5fff8f22-a153-46ee-8c83-eaf8c359aa47)

An Entity-Relationship Diagram (ERD) has been created to visualize the relationships between
the tables in the dataset. This diagram provides a clear understanding of the data structure and
helps in identifying key entities and their attributes.

## Conclusion

Through this project, we aim to provide valuable insights into Amazon sales trends, customer
preferences, and other factors influencing e-commerce operations. By analyzing the dataset
and addressing the key questions, we hope to assist stakeholders in making informed decisions
and optimizing their sales strategies.

Feel free to explore the repository and contribute to further analysis or enhancements!

## Notice:
All customer names and data used in this project are computer-generated using AI and random
functions. They do not represent real data associated with Amazon or any other entity. This
project is solely for learning and educational purposes, and any resemblance to actual persons,
businesses, or events is purely coincidental.
