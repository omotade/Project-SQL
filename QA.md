What are your risk areas? Identify and describe them.

Review the data types and ensure they are accurate and relevant to the data set.
Validate the data to query for missing data in a table
Validate the data to check for duplicates

QA Process:
Describe your QA process and include the SQL queries used to execute it.
-- SQL Query to review data types

-- Data validation SQL Query

SELECT * 
FROM sales_report 
WHERE sales_report."sentimentscore" is NULL

SQL Query to check for duplicates in analytics table

SELECT analytics."fullVisitorId", COUNT(*)
FROM analytics
GROUP BY analytics."fullVisitorId"
HAVING COUNT(*) > 1
