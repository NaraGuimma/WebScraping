# SQL


**SQL queries**
SELECT columns
FROM table
JOIN table ON condition
WHERE condition
AND condition
OR condition
GROUP BY column
ORDER BY column
LIMIT integer


**Aggregative functions**

* COUNT records :

SELECT COUNT(*), column 
FROM table
GROUP BY column


* SUM records:

SELECT SUM(column1), column2
FROM table
GROUP BY column2
