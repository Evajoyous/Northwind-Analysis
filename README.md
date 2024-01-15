# Northwind Logistics Analysis

### Project Overview
This project is carried out on the database of a logistics company referred to as Northwind. It provides valuable insights into delivery, shipping, and logistics, with the ultimate goal of optimizing operations and enhancing customer satisfaction.  In this project, a journey of in-depth data analysis within the Northwind database is embarked upon.

### Data Sources

Logistics data: The primary dataset used for this analysis is the "northwind_data.csv" file, which contains detailed information about the logistics in the company.

### Tools

- Excel (For data cleaning)
- SQL (For data exploration)
- PowerBI (For data visualization)

### Data cleaning/Preparation 
The tasks below were carried out during the data preparation stage:
1. Loading and inspection of data
2. Handling missing values
3. Data cleaning and formatting

### Exploration of Data
The data exploration included querying the data to answer important questions such as:

- What is the average processing time for orders handled by each employee?
- Who are the top-performing and underperforming employees in terms of delivery?
- What are the average delivery times for each shipping company?
- What is the average order processing time from order creation to completion, segmenting it by geographic variables, order size, freight cost, and other important features?
- Who are the top customers in terms of order frequency, revenue, and their effect on shipping?

### Data Analysis

This includes some interesting codes/features worked with

```sql
-- showing the average shipping times
WITH CTE_1 AS (
SELECT DISTINCT O.SHIPVIA, S.COMPANYNAME, COUNT(O.SHIPVIA) AS TOTAL_SHIPPING_TIMES
FROM ORDERS O
JOIN SHIPPERS S ON S.SHIPPERID = O.SHIPVIA
GROUP BY O.SHIPVIA
ORDER BY COUNT(O.SHIPVIA))
 SELECT ROUND(AVG(TOTAL_SHIPPING_TIMES)) AS 'AVERAGE SHIPPING TIMES'
    		FROM CTE_1;   


   -- Showing frequently ordered products

WITH CTE_2 AS (
SELECT P.PRODUCTNAME AS PRODUCT_NAME, OD.PRODUCTID AS PRODUCT_ID, COUNT(OD.PRODUCTID) AS TOTAL_ORDERED_TIMES,
      	  CASE	
      		WHEN COUNT(OD.PRODUCTID) < 10 THEN 'POORLY ORDERED'
       		WHEN COUNT(OD.PRODUCTID) < 50 THEN 'AVERAGELY ORDERED'
        		ELSE 'FREQUENTLY ORDERED'
        		END AS PRODUCT_ORDER_RATE
FROM		`ORDER DETAILS` OD
JOIN PRODUCTS P ON P.PRODUCTID = OD.PRODUCTID
GROUP BY OD.PRODUCTID
ORDER BY COUNT(OD.PRODUCTID) DESC)
       
SELECT PRODUCT_NAME, PRODUCT_ID, TOTAL_ORDERED_TIMES
FROM
     CTE_2
WHERE PRODUCT_ORDER_RATE = 'FREQUENTLY ORDERED'
GROUP BY PRODUCT_ID
ORDER BY COUNT(PRODUCT_ID);

-- SHOWING LATE DELIVERIES BY PRODUCTS

SELECT P.PRODUCTNAME AS PRODUCT, COUNT(O.ORDERID) AS TOTAL_LATE_DELIVERIES
FROM `ORDER DETAILS` OD
JOIN ORDERS O ON O.ORDERID = OD.ORDERID
JOIN PRODUCTS P ON P.PRODUCTID = OD.PRODUCTID
WHERE SHIPPEDDATE > REQUIREDDATE
GROUP BY 1
ORDER BY 2 DESC;

```
### Results/Findings

The analysis reults are summarised as follows:
1. 
