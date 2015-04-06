#SQL#

SQL is a data definition and query language, that lets you access and manipulate databases.

Building a website that shows data from a database will need:
- A RDBMS
- A server-side scripting language
- Using SQL to get the data you require
- Using HTML/CSS to diaplsy it

##Main SQL Commands##

SELECT - extracts data from a datatbase
UPDATE - updates existing data in the database
DELETE - deletes data from the database
INSERT INTO - insers new data intp a database
CREATE DATABASE - creates a new database
ALTER DATABASE - modifies a database
CREATE TABLE - creates a new table
ALTER TABLE - modifies a table
CREATE INDEX - creates an index (search key)
DROP INDEX - deletes an index

##SELECT##

All Data from a table `SELECT * FROM table_name;`

Data Attributes from a table `SELECT Name,City FROM Customers;`

##SELECT DISTINCT##
Returns only one copy of a value from a column, even when there are repeated values
`SELECT DISTINCT City FROM Cities;`

##WHERE##
Used to return only those records that fulfill a specific criteria
`SELECT * FROM Customer WHERE Country='UK';`
WHERE can use the following operators:
```
=         Equals
<>        Not Equals (or sometimes !=)
>         Greater Than
<         Less Than
>=        Greater Than or equal
<=        Greater Than or equal
BETWEEN   Between an inclusive range
LIKE      Search for similar
IN        Specify multiple possible values for a column
```
##AND & OR##
AND - Displays a record if the first and second condition are true
`SELECT * FROM Customers WHERE Country='Germany' AND City='Munich';`
OR - Displays a record if the first and second condition are true
`SELECT * FROM Customers WHERE City='Cologne' OR City='Munich';`

They can also be used in combination:
```
SELECT * FROM Customers
WHERE Country='Germany'
AND (City='Cologne' OR City='Munich');
```

##ORDER BY##
Can sort the results by one or more columns
```
SELECT * FROM Customers
ORDER BY Country ASC, CustomerName DESC;
```
ASC = Ascensding DESC = Descending

##INSERT INTO##
Inserts new records into a table.

It is possible to use two different formats with this command:
Without specifying columns
```
INSERT INTO Customers
VALUES ('Cardinal','Tom B. Erichsen','Skagen 21','Stavanger','4006','Norway');
```
By specifying columns
```
INSERT INTO Customers (CustomerName, City, Country)
VALUES ('Cardinal', 'Stavanger', 'Norway');
```
