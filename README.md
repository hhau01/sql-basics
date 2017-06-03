# SQL Basics
Fundamentals of SQL

#### Table of Contents
* [Overview of SQL](#what-is-sql)
* [Create, alter, and delete databases & tables](#create-and-use-database)
* [Insert, update, and delete records](#insert-table-data)
* [Query a database using select](#select-data)
* [Query a database using multiple operators](#common-sql-operators)
* [Indexes](#indexes)
* Constraints ([Primary](#create-table), [foreign](#foreign-key-constraint))
* [Join tables together](#joins)
* [Use aliases](#aliases)
* [Built-in SQL Functions](#sql-aggregate-functions)
* [Using Views](#sql-views)

#### What is SQL
* Structured Query Language
* Designed to work with databases
* Been around for a really long time
* Used on individual & corporate servers

#### Popular Databases That Use SQL
* MySQL
* PostgreSQL
* Oracle
* MS SQL Server
* SQLite
* dBase
* Hadoop
* MaxDB
* MariaDB
* Openbase

#### SQL Tools
* Command Line Client
* Adminer
* Firebird
* MySQL Workbench (mySQL)
* PHPMyAdmin
* PG Admin III (PostgreSQL)
* Sequel Pro (Mac)
* HeidiSQL

#### Create and Use Database

```sql
CREATE DATABASE test;
```

```sql
USE test;
```

#### Create Table

```sql
CREATE TABLE Customers (
	id INT NOT NULL AUTO_INCREMENT,
	FirstName VARCHAR(255),
	LastName VARCHAR(255),
	Email VARCHAR(255),
	Address VARCHAR(255),
	City VARCHAR(255),
	State VARCHAR(255),
	Zipcode INT(5),
	PRIMARY KEY(id)
);
```

* **NOT NULL:** this value cannot be null
* **AUTO_INCREMENT:** it will automatically increment every time another is added (e.g. 1, 2, 3, ...10)
* **VARCHAR(255):** alphanumeric with character length of 255
* **INT:** integer value
* **PRIMARY KEY:** unique identifier --typically `id` is used

#### Insert Table Data

Single Entry:
```sql
INSERT INTO Customers (FirstName, LastName, Email, Address, City, State, Zipcode) VALUES 
('Billy', 'Jean', 'billyjean@gmail.com', '5225 Figueroa Mountain Road', 'Los Olivos', 'California', '93441');
```

Multiple Entries:
```sql
INSERT INTO Customers (FirstName, LastName, Email, Address, City, State, Zipcode) VALUES 
('John', 'Doe', 'jdoe@gmail.com', '555 Fake St.', 'San Francisco', 'California', '12345'), 
('Jane', 'Doe', 'janed@gmail.com', '123 Folk Ave.', 'Redwood City', 'California', '96548'), 
('Bill', 'Joe', 'billjoe@gmail.com', '235 Main St.', 'Manhattan', 'New York', '10036'), 
('Johnny', 'Rocket', 'jrocket@gmail.com', '625 Rocket St.', 'Rocketown', 'New Jersey', '16548');
```

#### Update Table Data
```sql
UPDATE Customers
SET Email = 'test@gmail.com'
WHERE id = 3;
```
\*\*IMPORTANT: DO NOT FORGET THE `WHERE` OTHERWISE ALL THE `Emails` WILL BE OVERWRITTEN\*\*

#### Delete Table Data
```sql
DELETE FROM Customers
WHERE id = 3;
```
\*\*IMPORTANT: DO NOT FORGET THE `WHERE` OTHERWISE ALL THE `Customers` WILL BE DELETED\*\*

#### Alter Table

```sql
ALTER TABLE Customers ADD TestColumn VARCHAR(255);
```

```sql
ALTER TABLE Customers
MODIFY COLUMN TestColumn INT(11);
```

```sql
ALTER TABLE Customers
DROP COLUMN TestColumn;
```

```sql
ALTER TABLE Customers
ADD COLUMN Age INT;
```

#### Select Data

Select all from `Customers` table:
```sql
SELECT * FROM Customers;
```

Select all `FirstName` and `LastName` from `Customers` table:
```sql
SELECT FirstName, LastName FROM Customers;
```

Select all `Customers` where the `id = 2`:
```sql
SELECT * FROM Customers WHERE id = 2;
```

Alphabetical order by `LastName` default: ascending (a, b, c...):
```sql
SELECT * FROM Customers ORDER BY LastName;
```

Alphabetical order by `LastName` descending (z, y, x...):
```sql
SELECT * FROM Customers ORDER BY LastName DESC;
```

Select all `State` from `Customers` table:
```sql
SELECT State FROM Customers;
```

No duplicates:
```sql
SELECT DISTINCT State FROM Customers;
```

Select all `Customers` where `Age < 30`:
```sql
SELECT * FROM Customers WHERE Age < 30;
```

<br>

#### Common SQL Operators

Operator | Description | Example
|---|---|---|
= | Equal to | Author='Abbott'
<> | Not equal to (Many DBMSs accept != in addition to <>) | Dept <> 'Sales'
\> | Greater than | Hire_Date > '2012-01-31'
< | Less than | Bonus < 50000.00
\>= | Greater than or equal | Dependents >= 2
<= | Less than or equal | Rate <= 0.05
BETWEEN | Between an inclusive range | Cost BETWEEN 100.00 AND 500.00
LIKE | Match a character pattern | First_Name LIKE 'WILL%'
IN | Equal to one of multiple possible values | DeptCode IN(101, 103, 209)
IS or IS NOT | Compare to null(missing data) | Address IS NOT NULL
IS NOT DISTINCT FROM | Is equal to value or both are nulls(missing data) | Debt IS NOT DISTINCE FROM - Receivables
AS | Used to change a field name when viewing results | SELECT Employee AS 'Department1'

<br>

#### Operator Examples

```sql
SELECT * FROM Customers
WHERE Age
BETWEEN 22 AND 40;
```

Select all where `City` ends in the letter `n`:
```sql
SELECT * FROM Customers
WHERE City LIKE '%n';
```
_`%` is a wildcard similar to the regular expression *_

Select all where `City` contains the letter `n`:
```sql
SELECT * FROM Customers
WHERE City LIKE '%n%';
```

Select all where `City` does not contain the letter `n`:
```sql
SELECT * FROM Customers
WHERE City NOT LIKE '%n%';
```

Select all `Customers` from `New York` and `California`:
```sql
SELECT * FROM Customers
WHERE State IN ('New York', 'California');
```

<br>

#### Indexes
* An index can be created in a table to find data more quickly and efficiently
* Users do not see indexes, they are just used to speed up searches/queries
* Only create indexes on columns (and tables) that will be frequently searched against
* Similar to an index in the back of a book

Create index `CIndex`:
```sql
CREATE INDEX CIndex
ON Customers(City);
```

Delete index `CIndex`:
```sql
DROP INDEX CIndex
ON Customers;
```

<br>

#### Foreign Key Constraint
* The `FOREIGN KEY` constraint is used to prevent actions that would destroy links between tables.
* The `FOREIGN KEY` constraint also prevents invalid data from being inserted into the foreign key column, because it has to be one of the values contained in the table it points to.

Let's create a few tables.
```sql
CREATE TABLE Products (
	id INT NOT NULL AUTO_INCREMENT,
	Name VARCHAR(255),
	Price INT,
	PRIMARY KEY(id)
);
```

```sql
CREATE TABLE Orders (
	id INT NOT NULL AUTO_INCREMENT,
	OrderNumber INT,
	ProductId INT,
	CustomerId INT,
	Age INT,
	OrderDate DATETIME default CURRENT_TIMESTAMP,
	PRIMARY KEY(id),
	FOREIGN KEY(CustomerId) REFERENCES Customers(id),
	FOREIGN KEY(ProductId) REFERENCES Products(id)
);
```

To allow naming of `FOREIGN KEY` constraint:
```sql
CREATE TABLE Orders (
	id INT NOT NULL AUTO_INCREMENT,
	OrderNumber INT,
	ProductId INT,
	CustomerId INT,
	Age INT,
	OrderDate DATETIME default CURRENT_TIMESTAMP,
	PRIMARY KEY(id),
	CONSTRAINT FK_CustomerId FOREIGN KEY(CustomerId) REFERENCES Customers(id),
	CONSTRAINT FK_ProductId FOREIGN KEY(ProductId) REFERENCES Products(id)
);
```

On Alter Table:
```sql
ALTER TABLE Orders
ADD FOREIGN KEY(CustomerId) REFERENCES Customer(id),
ADD FOREIGN KEY(ProductId) REFERENCES Products(id);
```

To allow naming of `FOREIGN KEY` constraint:
```sql
ALTER TABLE Orders
ADD CONSTRAINT FK_CustomerId FOREIGN KEY(CustomerId) REFERENCES Customer(id),
ADD CONSTRAINT FK_ProductId FOREIGN KEY(ProductId) REFERENCES Products(id);	
```

Drop `FOREIGN KEY` constraint:
```sql
ALTER TABLE Orders
DROP FOREIGN KEY FK_CustomerId;
```

#### Joins

* Used to combine rows from two or more tables based on a common field between them
* Types: `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, `FULL JOIN`
* Simple way to visualize this is with a Venn Diagram

Before we can use Joins, we must populate our data:
```sql
INSERT INTO Products(Name, Price) VALUES
('Product One', 10),
('Product Two', 5),
('Product Three', 65),
('Product Four', 45),
('Product Five', 100);
```

```sql
INSERT INTO Orders(OrderNumber, ProductId, CustomerId) VALUES
(001, 1, 4),
(002, 3, 1),
(003, 1, 1),
(004, 1, 2),
(005, 1, 1),
(006, 1, 6),
(007, 4, 4),
(008, 4, 5),
(009, 2, 6);
```

**INNER JOIN**
```sql
SELECT Customers.FirstName, Customers.LastName, Orders.OrderNumber
FROM Customers
INNER JOIN Orders
ON Customers.id = Orders.CustomerId
ORDER BY Orders.OrderNumber;
```
![inner](https://www.w3schools.com/sql/img_innerjoin.gif "INNER JOIN")

**LEFT JOIN**
```sql
SELECT Customers.FirstName, Customers.LastName, Orders.OrderNumber, Orders.OrderDate
FROM Customers
LEFT JOIN Orders ON Customers.id = Orders.CustomerId
ORDER BY Customers.LastName;
```
![left](https://www.w3schools.com/sql/img_leftjoin.gif "LEFT JOIN")

**RIGHT JOIN**
```sql
SELECT Orders.OrderNumber, Customers.FirstName, Customers.LastName
FROM Orders
RIGHT JOIN Orders 
ON Orders.CustomerId = Customers.id
ORDER BY Orders.OrderNumber;
```
![right](https://www.w3schools.com/sql/img_rightjoin.gif "RIGHT JOIN")

**FULL JOIN**
```sql
SELECT Orders.OrderNumber, Customers.FirstName, Customers.LastName
FROM Orders
FULL OUTER JOIN Customers ON Orders.CustomerId = Customer.id
ORDER BY Orders.OrderNumber;
```
![full](https://www.w3schools.com/sql/img_fulljoin.gif "FULL JOIN")

Pull from `Orders`, `Customers`, and `Products` tables:
```sql
SELECT Orders.OrderNumber, Customers.FirstName, Customers.LastName, Products.Name
FROM Orders
	INNER JOIN Products
		ON Orders.ProductId = Products.id
	INNER JOIN Customers
		ON Orders.CustomerId = Customers.id
ORDER BY Orders.OrderNumber;
```

<br>

#### Aliases

Make column names more readable
```sql
SELECT FirstName AS 'First Name', LastName AS 'Last Name' FROM Customers;
```

Combine columns:
```sql
SELECT CONCAT(FirstName, ' ', LastName) AS 'Name', CONCAT(Address, ', ', City, ', ', State) AS 'Address' FROM Customers;
```

Aliases for tables:
```sql
SELECT o.id, o.OrderDate, c.FirstName, c.LastName
FROM Customers AS c, Orders AS o;
```

<br>

#### SQL Aggregate Functions

Get average:
```sql
SELECT AVG(Age) FROM Customers;
```

Get count:
```sql
SELECT COUNT(Age) FROM Customers;
```

Get max:
```sql
SELECT MAX(Age) FROM Customers;
```

Get min:
```sql
SELECT MIN(Age) FROM Customers;
```

Get sum:
```sql
SELECT SUM(Age) FROM Customers;
```

To uppercase:
```sql
SELECT UCASE(FirstName) FROM Customers;
```

To lowercase:
```sql
SELECT LCASE(FirstName) FROM Customers;
```

Select all `Customers` older than 30 and group them by `Age`:
```sql
SELECT Age, COUNT(Age) 
FROM Customers
WHERE Age > 30
GROUP BY Age;
```

Select all `Customers` where the count for `Age` is greater than 2:
```sql
SELECT Age, COUNT(Age) 
FROM Customers
GROUP BY Age
HAVING COUNT(Age) >= 2;
```

<br>

#### SQL Views

A view is a composition of a table in the form of a predefined SQL query. A view can be created from one or many tables which depends on the written SQL query.

Views allow users to do the following:
* Structure data in a way that users or classes of users find natural or intuitive.
* Restrict access to the data in such a way that a user can see and (sometimes) modify exactly what they need and no more.
* Summarize data from various tables which can be used to generate reports.

**Create a View:**
```sql
CREATE VIEW CustomersEmails AS
SELECT FirstName, LastName, Email
FROM Customers;
```

Query the View the same way you query an actual table:
```sql
SELECT * FROM CustomersEmails;
```

The `WITH CHECK OPTION` ensures that all updates and inserts satisfy the condition(s) in the view definition:
```sql
CREATE VIEW CustomersRetired AS
SELECT FirstName, LastName, Age
FROM Customers
WHERE Age >= 65
WITH CHECK OPTION;
```

**Updating a View:**

A view can be updated under certain conditions:
* The `SELECT` clause may not contain the keyword `DISTINCT`.
* The `SELECT` clause may not contain summary functions.
* The `SELECT` clause may not contain set functions.
* The `SELECT` clause may not contain set operators.
* The `SELECT` clause may not contain an `ORDER BY` clause.
* The `FROM` clause may not contain multiple tables.
* The `WHERE` clause may not contain subqueries.
* The query may not contain `GROUP BY` or `HAVING`.
* Calculated columns may not be updated.
* All `NOT NULL` columns from the base table must be included in the view in order for the `INSERT` query to function.

If a View satisfies all above-mentioned rules, it can be updated:
```sql
UPDATE CustomersEmails
SET Email = 'johndoe@gmail.com'
WHERE FirstName = 'John'
AND LastName = 'Doe';
```

**NOTE: This change is reflected on the base table Customers as well**

**Inserting Rows into a View:**

Rows of data can be inserted into a View, but the same rules that apply to the `UPDATE` command also apply to the `INSERT` command.

**Deleting Rows from a View:**
```sql
DELETE FROM CustomersEmails
WHERE LastName = 'Doe';
```
**NOTE: This change is reflected on the base table Customers as well**

**Dropping Views:**

Drop a View when it is no longer needed.

```sql
DROP VIEW CustomersEmails;
```

##### Thank you for stopping by! :+1:
<br>