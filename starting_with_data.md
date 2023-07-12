Question 1: Asides from using a CASE WHEN Statement to remove NULL VALUES, what other Function in a query removes NULL values

SQL Queries:
-- For instance, remove Nulls in the Ratio Column from sales_report
Answer: 
SELECT COALESCE (Ratio, 0) FROM sales_report

What is the product with the least sentimental score

SELECT products."name" FROM products
WHERE products."sentimentScore" = (SELECT MIN(products."sentimentScore") AS least_sentiment_score
FROM products);

Answer: "7 Dog Frisbee"

Question 2: How can you produce duplicates in the analytics table. Retreive columns, fullvisitorid, visitid, and date

SQL Queries:
SELECT analytics."fullVisitorId", analytics."visitId", analytics.date, COUNT(*)
FROM analytics
GROUP BY analytics."fullVisitorId", analytics."visitId", analytics.date
HAVING COUNT(*) > 1

Answer: I show there are 150612 duplicate rows 

Question 3: Retrieve product columns where ordered quantity, stocklevel are greater than 100 in India

SQL Queries:
SELECT p."productSKU", p."name", p."orderedQuantity", p."stockLevel", alls.country
FROM products p 
JOIN all_sessions alls ON alls."productSKU" = p."productSKU"
WHERE country = 'India' AND p."stockLevel" > 100 AND p."orderedQuantity" > 100
Answer:
I show there are 286 product SKU and Name with ordered quantity and stocklevel are greater than 100

Question 4: Retreive the mean level of stock, summation of all orders where product SKU has GGOE

SQL Queries:
SELECT AVG(sr."stockLevel") AS Average_StockCount, SUM(sr."total_Ordered") AS Total_OrderedProduct, sr."productSKU"
FROM sales_report sr
WHERE sr."productSKU" LIKE '%GGOE%'
GROUP BY sr."productSKU";

Answer: This query returns 382 rows of product containing GGOE

Question 5: Retreive the mean level of stock, summation of all orders where product SKU has GGOE and create a temporary table to analyze the data

SQL Queries:
CREATE TEMPORARY TABLE products_containingGGOE
SELECT AVG(sr."stockLevel") AS Average_StockCount, SUM(sr."total_Ordered") AS Total_OrderedProduct, sr."productSKU"
FROM sales_report sr
WHERE sr."productSKU" LIKE '%GGOE%'
GROUP BY sr."productSKU";
Answer:
