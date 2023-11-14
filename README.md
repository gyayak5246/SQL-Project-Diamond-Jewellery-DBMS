# SQL-Project-Diamond-Jewellery-DBMS
--
--
The Diamond Jewellery Database Management System is a project aimed at managing a jewellery store, where users can find and purchase the latest jewellery items. The system will include various components, such as product management, customer management, and billing management. 
--
--
-- create schema - diamond_jewellery_DBMS
--
--
-- import tables bill,Gold Base Price,Jewellery Type,Customer,Diamond
--
--
-- quries
--
--
-- Multiple select statement
Select CID, JAmount, Carat, Clarity, Color FROM BILL
WHERE Toj = 'Ring';	
--
--
-- Subqueriey
-- compared Total Purchase vs Avg Purchase
Select CID,JAmount,
(SELECT avg(JAmount) from bill) AS avg_damount
From bill;
--
--
---- Inner Join Statement
SELECT * FROM Customer,bill
where customer.cid = bill.cid;
--
--
-- Common Table Expressions (CTE)
WITH my_cte AS (
   Select CID,JAmount,
(SELECT avg(JAmount) from bill) AS avg_damount
From bill 
),
my_co As (
SELECt sum(JAmount) as total_JAmount FROM bill
)
SELECT * FROM my_cte, my_co;
--
--
-- TimeStamp Function
Select NOW()
--
--
-- Windows Function 
-- Aggregate 1
SELECT JMakecost, Gold,
SUM(JMakecost) OVER( PARTITION BY Gold ORDER BY JMakecost ) AS "Total",
AVG(JMakecost) OVER( PARTITION BY Gold ORDER BY JMakecost ) AS "Average", 
COUNT(JMakecost) OVER( PARTITION BY Gold ORDER BY JMakecost ) AS "Count", 
MIN(JMakecost) OVER( PARTITION BY Gold ORDER BY JMakecost ) AS "Min",
MAX(JMakecost) OVER( PARTITION BY Gold ORDER BY JMakecost ) AS "Max"
FROM BILL;
--
--
-- Windows Function 
-- Aggregate 2
SELECT JMakecost,
sum(JMakecost) OVER( ORDER BY JMakecost ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS "Total", 
AVG(JMakecost) OVER( ORDER BY JMakecost ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS "Average", 
COUNT(JMakecost) OVER(ORDER BY JMakecost ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS "Count", 
MIN(JMakecost) OVER( ORDER BY JMakecost ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS "Min", 
MAX(JMakecost) OVER (ORDER BY JMakecost ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS "Max"
FROM BILL;
--
--
-- Windows Function 
-- Ranking
SELECT BID,CID,JAmount,
ROW_NUMBER() OVER( ORDER BY JAmount) AS "ROW_NUMBER", 
RANK() OVER (ORDER BY JAmount) AS "RANK",
DENSE_RANK() OVER( ORDER BY JAmount) AS "DENSE_RANK", 
PERCENT_RANK() OVER( ORDER BY JAmount) AS "PERCENT_RANK" 
FROM BILL;
--
--
--Windows Function
--Analytic
SELECT JMakecost,
FIRST_VALUE(JMakecost) OVER( ORDER BY JMakecost) AS "FIRST_VALUE", 
LAST_VALUE(JMakecost) OVER( ORDER BY JMakecost) AS "LAST_VALUE",
LEAD(JMakecost) OVER( ORDER BY JMakecost) AS "'LEAD", 
LAG(JMakecost) OVER( ORDER BY JMakecost) AS "'LAG"
FROM bill;
--
-- Case Statement
select CID, JAmount,
CASE
when JAmount > 8000000 then 'High-level Customer'
WHEN JAmount between 5000000 AND 8000000 then 'Mid-level Customer'
ELSE 'LOW-level Customer'
END AS Client_Status
FROM BILL;
