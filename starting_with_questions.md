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
--Create a CTE named "Ranked Products", which will be used to rank the top selling products
--based on the number of occurances in each country and city combo.
WITH RankedProducts AS (
  SELECT country, city, v2productname, v2ProductCategory,
    ROW_NUMBER() OVER (PARTITION BY country, city ORDER BY COUNT(*) DESC) AS Rank
FROM all_sessions
WHERE v2productcategory IS NOT NULL
    AND country IS NOT NULL
    AND city IS NOT NULL
  GROUP BY country, city, v2productname, v2productcategory
)
SELECT
  country,
  city,
  v2productname,
  v2productcategory AS TopSellingProduct,
  RANK() OVER (PARTITION BY country, city ORDER BY Rank) AS ProductRank
FROM RankedProducts
WHERE Rank = 1
ORDER BY country, city, v2productname, productrank DESC 

Answer:
With the above query, we can make note of the types of products that had been sold. In the topsellingproduct column, I can see that a majority of products sold are apparel based, or are from Youtube. Across each city and country, I can see that the types of products sold are apparel or from Youtube. A key thing
to note in the generated table is that there are a number of Countries and Cities that are not set or not available in the dataset. 

**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:
--Calculate the total revenue and avg revenue for each city and country--
-- Top revenue generators-
SELECT
  country,
  city,
  RoundedTotalRevenue
FROM (
  SELECT
    country,
    city,
    ROUND(SUM(COALESCE(transactionRevenue::numeric, 0)), 2) AS RoundedTotalRevenue,
    RANK() OVER (ORDER BY SUM(COALESCE(transactionRevenue::numeric, 0)) DESC) AS RevenueRank
  FROM all_sessions
  WHERE country IS NOT NULL
    AND city IS NOT NULL
  GROUP BY country, city
) AS RankedRevenue
WHERE RevenueRank = 1

-- Average revenue per country and city
SELECT
  country,
  city,
  RoundedAverageRevenue
FROM (
  SELECT
    country,
    city,
    ROUND(AVG(COALESCE(transactionRevenue::numeric, 0)), 2) AS RoundedAverageRevenue,
    RANK() OVER (ORDER BY AVG(COALESCE(transactionRevenue::numeric, 0)) DESC) AS AvgRevenueRank
  FROM all_sessions
  WHERE country IS NOT NULL
    AND city IS NOT NULL
  GROUP BY country, city
) AS RankedAvgRevenue
WHERE AvgRevenueRank = 1

Answer:
With the above queries, I can make the conclusion that the United States is the top revenue generator, generating a total revenue of 2,190,950,000. The city with the highest average revenue is Sunnyvale, United States.








