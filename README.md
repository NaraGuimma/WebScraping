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




**Going further - Resources**



* You can find a great documentation about SQL on https://www.sqlservertutorial.net/sql-server-basics/
* Explore the many-to-many relation https://dzone.com/articles/how-to-handle-a-many-to-many-relationship-in-datab and the join tables https://learn.co/lessons/sql-join-tables-readme
* Different kind of JOIN http://www.sql-join.com/sql-join-types
* Did you know you can use SQL to insert, update and delete records into a database? Learn more about the so-called SQL CRUD https://www.sqlshack.com/crud-operations-in-sql-server/
* Enhance your SQL skills on DataCamp https://www.datacamp.com/courses/tech:sql
