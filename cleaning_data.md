What issues will you address by cleaning the data?

Some issues that I will be addressing is any duplicates, missing/null values, and selecting DISTINCT rows to verify and check my analysis. Whilst completing my analysis, I have found several rows across multiple columns that had missing/null values. The columns that had missing values were transactions, timeonsite, country, and city. 

Queries:
Below, provide the SQL queries you used to clean your data.

-----------------------------
--ALL_SESSIONS TABLE CLEANING 
-----------------------------
--Identify any missing/null values in the city and country column--
SELECT country, city
FROM all_sessions
WHERE country IS NULL
 	 OR city IS NULL
 	 OR country = '(not set)'
  	 OR city = '(not set)'
	 OR city = 'not available in demo dataset'
--Total rows count = 8656

-------------------------------------------
--Preivew rows in totalTransacationRevenue AND Transactions columns where rows have no values or is NULL
--totaltransacationrevenue column --
--------------------------------------------
SELECT totalTransacationRevenue
FROM all_sessions
WHERE totalTransacationRevenue IS NULL 


--transactions column --
SELECT transactions
FROM all_sessions
WHERE transactions IS NULL 

--Selecting distinct rows from fullvisitorid column in all_sessions table--
SELECT DISTINCT fullvisitorid 
FROM all_sessions 
--total row count = 14223

--Selecting distinct rows from visitid column in all_sessions table--
SELECT DISTINCT visitid
FROM all_sessions
--total row count = 14556

--Identifying null values in timeOnSite column--
SELECT timeonsite
FROM all_sessions
WHERE timeonsite IS NULL 
--total row count = 3300

--------------------------------------
--Analtyics table Data Cleaning 
--------------------------------------
--Selecting distinct rows from visitid and fullvisitorid from analtyics table
SELECT DISTINCT visitid, fullvisitorid
FROM analytics 

-----------------------------------------------------------------------------------------
--Using aggregate COUNT function with DISTINCT between all_sessions and analytics table 
-----------------------------------------------------------------------------------------
--Using FULL JOIN ---
SELECT COUNT (DISTINCT visitid)
FROM all_sessions 
FULL JOIN analytics USING(visitid)
--total count on visitid = 159,538

--Using JOIN ---
SELECT COUNT (DISTINCT visitid)
FROM all_sessions
JOIN analytics USING(visitid)
--total count between both tables = 3,660

--Using FULL JOIN --
SELECT COUNT (DISTINCT fullvisitorid)
FROM all_sessions
FULL JOIN analytics USING (fullvisitorid)
--total count between both tables = 130,345

--Using JOIN ---
SELECT COUNT(DISTINCT fullvisitorid)
FROM all_sessions 
JOIN analytics USING (fullvisitorid)
--total count between both tables using JOIN function = 3,896
-----------------------------------------------------------
----- CLEANING TIME COLUMN ------
------------------------------------------------------------
--Select distinct values from time column where there are non-NULL values--
SELECT DISTINCT "time"
FROM all_sessions
WHERE "time" IS NOT NULL 
--distinct function will eliminate duplicate values from column set
--Total row count of non-null values = 9600


--Create a temporary table for time column to perform specific queries--
CREATE TEMPORARY TABLE all_sessions_temp AS
SELECT *
FROM all_sessions;

--Confirm that temp table was created successfully--
SELECT "time"
FROM all_sessions_temp

-- Update the "time" column to a more readable date/time format--
UPDATE all_sessions_temp
SET "time" = TO_TIMESTAMP("time"::double precision);

-- View the updated "time" column--
SELECT "time"
FROM all_sessions_temp;








