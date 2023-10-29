Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
SELECT country, city, COALESCE(SUM(transactionRevenue::numeric), 0) AS totalTransactionRevenue
FROM all_sessions
WHERE country IS NOT NULL
  AND city IS NOT NULL
  AND city NOT IN ('not set', 'not available in dataset')
GROUP BY country, city
ORDER BY totalTransactionRevenue DESC
LIMIT 100;


Answer: The Country and city that has the highest level of transaction revenues on the site is the United states, Sunnyvale with a totaltransactionrevenue of
200,000,000. 


**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries: 
SELECT country, city, ROUND(AVG(COALESCE(productquantity::numeric, 0))) AS AvgProductsOrdered
FROM all_sessions
WHERE country IS NOT NULL
  AND city IS NOT NULL
GROUP BY country, city
ORDER BY AvgProductsOrdered DESC
LIMIT 100;

Answer: Within a limit of 100 products, the average number of products ordered from visitors in eachity and country is 1 product per visitor. 


**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**

SQL Queries:
SELECT country, city, v2ProductCategory,
  COUNT(*) AS ProductCount
FROM all_sessions
WHERE country IS NOT NULL
  AND city IS NOT NULL
  AND v2ProductCategory IS NOT NULL
GROUP BY country, city, v2ProductCategory
ORDER BY country, city, ProductCount DESC;

Answer: --Some of the patterns in the types of products ordered is that a majority of products ordered from visitors is apparel. I have also noticed that the number of products ordered is at least 1. 

**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







