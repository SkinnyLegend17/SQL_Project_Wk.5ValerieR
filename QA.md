What are your risk areas? Identify and describe them.

Some risk areas that are involved in my QA process are missing/null values, duplicate values in several columns across the datasets, inconsistency in values, and outliers in the dataset. With these risk factors in play, it will influence the integrity of the data, as well as the quality. 


QA Process:
Describe your QA process and include the SQL queries used to execute it.

Here are some of the SQL queries that I used to identify the various risk areas:

--Step 1: Identifying Missing/Null values ----

--Check for missing values in a transactions column 
SELECT COUNT(*) AS MissingValue
FROM all_sessions
WHERE transactions IS NULL 

--Total # of missing values in transactions column = 15053 
--This will affect the data integrity and any other queries that will involve collecting information from the transactions column. 

--Step 2: Identifying Duplicate values across multiple columns
--Checking duplicate VisitorID's --
SELECT
  fullVisitorId,
  COUNT(*) AS NumOfDuplicates
FROM all_sessions
GROUP BY fullVisitorId
HAVING COUNT(*) > 1

--Total row count = 794 
--Based on the above query, I have made note that there are several visitorID's that are duplicated.


--Step 3: Checking for inconsistencies in v2productname column
--Checking for inconsistencies in v2productcategory column 
SELECT
  v2productcategory,
  COUNT(DISTINCT productQuantity) AS NumOfUniqueValues
FROM all_sessions
GROUP BY v2productcategory
HAVING COUNT(DISTINCT productQuantity) > 1

--By excuting the above query, I have noticed that that there is a product in row 3 in the output that is "(not set)". Therefore, we cannot determine the product category. 












