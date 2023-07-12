Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
--Created a view to see the cleaned transaction data and city and country
Create VIEW Revenue_Transact AS
	SELECT country, city, CAST("totalTransactionRevenue" AS NUMERIC)/1000000 AS clean_transact
	FROM all_sessions
WHERE all_sessions."totalTransactionRevenue"
is NOT NULL
	
	SELECT * from revenue_transact --- to give the countyr, city and cleaned total transaction revenue 
 
--Then I used the view created to find what country and city has the highest level of transaction	
SELECT 
SUM(clean_transact) AS revenue, country 
FROM revenue_transact
GROUP BY country
HAVING SUM(clean_transact) IS NOT NULL 
ORDER BY revenue DESC

---Query to find City with highest level of transaction
SELECT SUM(clean_transact) AS revenue, city 
FROM revenue_transact
GROUP BY city
HAVING SUM(clean_transact)IS NOT NULL 
ORDER BY revenue DESC

Answer: The United states has the highest level with (13,154). Followed by Israel(602) and Australia(358)
        The City that has the highest level of transaction is San Francisco(1,564) in the United States, followed by Sunnyvale(992) and Atlanta(854)




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
-- I had to determine what the total count of product per city first and then calculate an average of the product by city by writing a subquery.

SELECT AVG(product_numbercount) AS product_average_count, city
FROM
(SELECT COUNT (*) as product_numbercount, city from all_sessions
GROUP BY city) as product_number_city    --- This was the query to determine the total number of product
GROUP BY city, product_numbercount
ORDER BY product_average_count DESC


Select Avg(product_numbercount) AS product_average_count, country
FROM

(select count (*) as product_numbercount, country from all_sessions
group by country) as product_number_country
Group by country, product_numbercount
order by product_average_count DESC


Answer: The average count of City is 1,177 in Mountain View, 647 in New York, 464 in San Francisco, 366 in SunnyVale
        The average product count is 8,727 in United States, 719 in India, 668 in United Kingdom, 642 in Canada, 336 in Germany





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
SELECT DISTINCT alls."visitId", alls.country, sr."total_Ordered", alls."v2ProductCategory"
FROM all_sessions alls
JOIN sales_report sr ON sr."productSKU" = alls."productSKU"
WHERE sr."total_Ordered" > 0
  AND sr."total_Ordered" = (SELECT MAX("total_Ordered") FROM sales_report)
GROUP BY alls."visitId", alls.country, sr."total_Ordered", alls."v2ProductCategory";

SELECT DISTINCT alls."visitId", alls.country, alls.city, sr."total_Ordered", alls."v2ProductCategory"
FROM all_sessions alls
JOIN sales_report sr ON sr."productSKU" = alls."productSKU"
WHERE sr."total_Ordered" > 0
  AND sr."total_Ordered" = (SELECT MIN("total_Ordered") FROM sales_report)
  AND
city <> 'not available in demo dataset'
AND city <> '(not set)'
GROUP BY alls."visitId", alls.country, alls.city, sr."total_Ordered", alls."v2ProductCategory";



Answer: I notate that the Product category Home/Office/ was the maximum unique categroy ordered across the countries
	I also notate same product category for city




**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
SELECT alls."v2ProductName", country, 
SUM(p."orderedQuantity") AS Total_ordered_quantity
FROM all_sessions alls
JOIN products P 
ON P."productSKU" = alls."productSKU"
WHERE city <> 'not available in demo dataset'
AND city <> '(not set)'
GROUP BY alls."v2ProductName", country
ORDER BY Total_ordered_quantity DESC


SELECT alls."v2ProductName", city, 
SUM(p."orderedQuantity") AS Total_ordered_quantity
FROM all_sessions alls
JOIN products P 
ON P."productSKU" = alls."productSKU"
WHERE city <> 'not available in demo dataset'
AND city <> '(not set)'
GROUP BY alls."v2ProductName", city
ORDER BY Total_ordered_quantity DESC


Answer: The country with the top selling product is United States
	The city with the top selling product is Mountain View





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:
Select CAST(a.revenue AS NUMERIC)/1000000 AS clean_revenue, (alls.country)
From all_sessions alls
JOIN analytics a on alls."fullVisitorId" = a."fullVisitorId"
WHERE alls. country IN (Select country from all_sessions) AND REVENUE is NOT NULL AND
city <> 'not available in demo dataset'
AND city <> '(not set)'
GROUP BY clean_revenue, alls.country



Answer: The United States generated more revenue than any other country. 







