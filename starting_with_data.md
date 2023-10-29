Question 1: Which country has the highest and lowest engagement? eg. time on site, number of pageviews)?

SQL Queries:
--Country with highest engagement
SELECT
  country,
  TO_CHAR(MAKE_INTERVAL(secs := AVG(timeOnsite::numeric)::bigint), 'HH24:MI:SS') AS AvgTimeOnSite
FROM all_sessions
WHERE country IS NOT NULL
GROUP BY country
ORDER BY AVG(timeOnsite::numeric) DESC
LIMIT 100;

--Country with the lowest engagement 
SELECT
  country,
  TO_CHAR(MAKE_INTERVAL(secs := AVG(timeOnsite::numeric)::bigint), 'HH24:MI:SS') AS AvgTimeOnSite
FROM all_sessions
WHERE country IS NOT NULL
GROUP BY country
ORDER BY AVG(timeOnsite::numeric) ASC
LIMIT 100;

Answer: 
The country with the highest engagement is Peru, with the average time on site is approximately 14 minutes.The country with the lowest engagement is Brunei, with visitors spending no more than approximately 3 seconds on site.

Question 2: What is the average number of visitors that make repeat purchases?

SQL Queries:
SELECT COALESCE(AVG(RepeatPurchases), 0) AS AvgRepeatPurchases
FROM (
  SELECT
    fullVisitorId,
    COUNT(DISTINCT transactionId) AS RepeatPurchases
  FROM all_sessions
  WHERE transactionId IS NOT NULL
  GROUP BY fullVisitorId
  HAVING COUNT(DISTINCT transactionId) > 1
) AS RepeatPurchases

--Check the distinct counts of transactions for each visitor to verify--
SELECT
  fullVisitorId,
  COUNT(DISTINCT transactionId) AS TransactionCount
FROM all_sessions
WHERE transactionId IS NOT NULL
GROUP BY fullVisitorId;

Answer:
The average number of visitors who make repeat purchases is zero. Given this analysis, visitors made at least one transaction. 

Question 3: Which products have the highest sales volume?

SQL Queries:
SELECT v2productname, COUNT(DISTINCT transactionId) AS SalesVolume
FROM all_sessions
WHERE transactionId IS NOT NULL
GROUP BY v2ProductName
ORDER BY SalesVolume DESC
LIMIT 10

Answer:
Based on the above query, the product with the highest sales volume is a Nest Cam indoor security camera.



Question 4: 

SQL Queries:

Answer:



Question 5: 

SQL Queries:

Answer:
