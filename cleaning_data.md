What issues will you address by cleaning the data?

- I ensured the accurate data types were used for the complete import of the files
- I ensured that values with Nulls in the products table columns sentimentscore and sentimentguide where made zero using the CASE WHEN statements
- I ensured that the Nulls in the ratio column in the sales_report table were made zero in order to remove Nulls.
- I ensured I cleaned the product price by dividing by 1000000 for the all_sessions table
- I ensured duplicates were removed from the analytics and all_sessions table. 



Queries:
Below, provide the SQL queries you used to clean your data.

-- Cleaning of products table by making null values as seen in sentimentscore and sentimentmagnitude columns equate 0 
SELECT * from products 
WHERE products."SentimentScore" is NULL

SELECT * from products 
WHERE products."SentimentMagnitude" is Null

SELECT products."ProductSKU", products."Name", products."OrderedQuantity", products."StockLevel",
products."RestockingLeadTime",
CASE 
	WHEN products."SentimentScore" is Null THEN 0
	Else products."SentimentScore"
END AS SentimentScores,
CASE 
	WHEN products."SentimentMagnitude" is Null THEN 0
	Else products."SentimentMagnitude"
	END AS SentimentMagnitudes
FROM products 

--Cleaning Sales_Report table by making null values in ratio column equate 0
SELECT sales_report."productSKU", sales_report."total_Ordered", sales_report."name",
sales_report."stockLevel" , sales_report."restockingLeadTime", sales_report."sentimentScore",
sales_report."sentimentMagnitude", 
--"Sales_Report"."Ratio""
CASE 
	WHEN sales_report.Ratio is Null THEN 0
	Else sales_report.Ratio
END AS Ratios
FROM sales_report

SELECT "totalTransactionRevenue", country, city, CAST
("totalTransactionRevenue" AS NUMERIC)/1000000 as totaltransactrev 
from all_sessions where all_sessions."totalTransactionRevenue" is
NOT NULL
GROUP BY all_sessions."totalTransactionRevenue"

--Cleaning of unit_price by dividing by 1000000. I converted the data type from varchar to numeric in order to divide. 
SELECT aAnalytics."Unit_Price",
(analytics."Unit_Price"/1000000) as real_unitprice FROM "Analytics"

--Removing duplicates in the analytics table.

SELECT DISTINCT ("FullVisitorId", "VisitId", "VisitNumber", "VisitStartTime", "Date"),  
"ChannelGrouping", "SocialEngagementType", "Units_sold", "Pageviews", "TimeonSite",
"Bounces", "Unit_Price", "Revenue" 
from analytics
