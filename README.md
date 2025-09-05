# Retail-Data-Analysis
SQL-based retail data analysis project using transaction, customer, and product datasets to explore customer behavior, sales trends, and product performance for actionable business insights.

## Project Overview:

This project focuses on analyzing retail data using SQL to derive valuable insights into customer demographics, sales performance, and product trends. By working with transactional, customer, and product category datasets, the project demonstrates how raw data can be transformed into meaningful business intelligence.

## Objectives:

Explore and clean retail datasets for analysis.
Understand customer demographics and segmentation.
Identify high-performing products and categories.
Analyze sales patterns across time and store types.
Provide insights to support decision-making in customer retention and revenue growth.

## Dataset Details:

### Transactions.csv
Customer purchase records.
 Fields: transaction_id, cust_id, tran_date, prod_subcat_code, prod_cat_code, Qty, Rate, Tax, total_amt, Store_type.

### Customer.csv
Demographic details of customers.
Fields: customer_Id, DOB, Gender, city_code, Date_Of_Registration.

### prod_cat_info.csv
Mapping of product categories and subcategories.
Fields: prod_cat_code, prod_cat, prod_sub_cat_code, prod_subcat.

## Tools Used:

SQL  – Data cleaning and analysis
Excel – Pre-processing and validation
GitHub – Version control and project sharing


create database Retail_DataAnalysis
use Retail_DataAnalysis

-- Q1. What is the total number of rows in each of the 3 tables in the database?

SELECT 'Customer' AS TableName, COUNT(*) AS TotalRows FROM Customer
     UNION ALL
SELECT 'Transactions', COUNT(*) FROM Transactions
      UNION ALL
SELECT 'Product Category', COUNT(*) FROM prod_cat_info;


--Q2. What is the total number of transactions that have a return?

SELECT COUNT(*) AS Returned_Transactions
FROM Transactions
WHERE total_amt < 0;


--Q3. Convert date variables into valid date formats.

-- Assuming tran_date is stored as VARCHAR
ALTER TABLE Transactions
ALTER COLUMN tran_date DATE;


--(or in SELECT:)

SELECT CONVERT(DATE, tran_date, 103) AS tran_date_corrected
FROM Transactions;


--Q4. What is the time range of the transaction data available for analysis?

SELECT MIN(tran_date) AS StartDate, 
       MAX(tran_date) AS EndDate,
       DATEDIFF(DAY, MIN(tran_date), MAX(tran_date)) AS TotalDays,
       DATEDIFF(MONTH, MIN(tran_date), MAX(tran_date)) AS TotalMonths,
       DATEDIFF(YEAR, MIN(tran_date), MAX(tran_date)) AS TotalYears
FROM Transactions;


--Q5. Which product category does the sub-category “DIY” belong to?

SELECT prod_cat
FROM prod_cat_info
WHERE prod_subcat = 'DIY';

-- Data Analysis

--Q6. Which channel is most frequently used for transactions?

SELECT Store_type, COUNT(*) AS TotalTransactions
FROM Transactions
GROUP BY Store_type
ORDER BY TotalTransactions DESC;


Q--7. What is the count of Male and Female customers in the database?

SELECT Gender, COUNT(*) AS TotalCustomers
FROM Customer
GROUP BY Gender;


--Q8. From which city do we have the maximum number of customers and how many?

SELECT city_code, COUNT(*) AS TotalCustomers
FROM Customer
GROUP BY city_code
ORDER BY TotalCustomers DESC
LIMIT 1;


--Q9. How many sub-categories are under the Books category?

SELECT COUNT(DISTINCT prod_subcat) AS SubCategories
FROM prod_cat_info
WHERE prod_cat = 'Books';


--Q10. What is the maximum quantity of products ever ordered?

SELECT MAX(Qty) AS MaxQuantityOrdered
FROM Transactions;


--Q11. What is the net total revenue generated in categories Electronics and Books?

SELECT SUM(total_amt) AS NetRevenue
FROM Transactions t
JOIN prod_cat_info p 
     ON t.prod_cat_code = p.prod_cat_code
    AND t.prod_subcat_code = p.prod_sub_cat_code
WHERE prod_cat IN ('Electronics','Books');


--Q12. How many customers have >10 transactions excluding returns?

SELECT cust_id, COUNT(*) AS TotalTransactions
FROM Transactions
WHERE total_amt > 0
GROUP BY cust_id
HAVING COUNT(*) > 10;


--Q13. What is the combined revenue earned from Electronics & Clothing categories, from Flagship stores?

SELECT SUM(total_amt) AS TotalRevenue
FROM Transactions t
JOIN prod_cat_info p 
     ON t.prod_cat_code = p.prod_cat_code
    AND t.prod_subcat_code = p.prod_sub_cat_code
WHERE prod_cat IN ('Electronics','Clothing')
  AND Store_type = 'Flagship store';


--Q14. What is the total revenue generated from Male customers in Electronics?

SELECT SUM(t.total_amt) AS MaleElectronicsRevenue
FROM Transactions t
JOIN Customer c ON t.cust_id = c.customer_id
JOIN prod_cat_info p 
     ON t.prod_cat_code = p.prod_cat_code
    AND t.prod_subcat_code = p.prod_sub_cat_code
WHERE c.Gender = 'M'
  AND p.prod_cat = 'Electronics';


--Q15. What are the top 5 sub-categories in terms of sales?

SELECT p.prod_subcat, SUM(t.total_amt) AS Revenue
FROM Transactions t
JOIN prod_cat_info p 
     ON t.prod_cat_code = p.prod_cat_code
    AND t.prod_subcat_code = p.prod_sub_cat_code
GROUP BY p.prod_subcat
ORDER BY Revenue DESC
LIMIT 5;

## Key Insights:

Customers in the 25–40 age group contributed the highest sales.
Electronics and Clothing were the most profitable product categories.
Festive months showed a significant spike in sales volume.
Female customers showed slightly higher average spending than male customers.
Store_Type = Flagship Store generated the highest revenue compared to other store types.


## Findings / Report:

Seasonal sales peaks indicate strong opportunities for targeted promotions.
Customer segmentation by age and gender can help in personalized marketing.
High-performing categories suggest where to focus inventory and campaigns.
Flagship stores outperform, indicating the importance of premium outlets.

## Conclusion:

This project highlights how SQL can be effectively used to analyze retail data and generate actionable business insights. By exploring customer behavior, product performance, and sales patterns, businesses can:
Optimize product offerings,Improve customer targeting, and Strengthen sales strategies.
The analysis demonstrates the end-to-end process of data exploration, cleaning, and insight generation using SQL.
