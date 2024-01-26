# Northwind Logistics Analysis

## Table of Contents
- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Cleaning/Preparation](#data-cleaning/preparation)
- [Exploration of Data](#exploration-of-data)
- [Data Analysis](#data-analysis)
- [Results/findings](#results/findings)
- [Proposed Recommendations based on the findings](#proposed-recommendations-based-on-the-findings)
- [References](#references)

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

The analysis results are summarised as follows:
1. Some employees demonstrate higher efficiency, while others may need improvement.
2. The most efficient employee in terms of on-time delivery rate is Nancy Davilio, with an on-time delivery rate of 98%
3. The best-performing product in terms of sales and revenue is ‘Cte de Blaye’, under the beverage product category.
4. The first quarter of the year attracts greater sales than the rest of the year.
5. There is no significant difference in the on-time delivery rate of the three shipping companies.

### Proposed Recommendations based on the findings

1. Implement performance tracking systems to monitor and reward efficient employees, thereby encouraging them, and also motivating others.
2. Provide additional training for employees with longer processing times.
3. Focus should be given to the expansion and promotion of products in the beverage category
4. Attention should be given to seasons in the year where sales are at the peak to maximize revenue
5. A system for a thorough analysis of customer feedback should be established, as this is absent from the company’s database.
6. Invest in collaborative platforms with suppliers for real-time information sharing.

### References


