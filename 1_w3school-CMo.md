# Part 1: W3Schools SQL Lab 

*Introductory level SQL*

--

This challenge uses the [W3Schools SQL playground](http://www.w3schools.com/sql/trysql.asp?filename=trysql_select_all). Please add solutions to this markdown file and submit.

1. Which customers are from the UK?

SELECT * FROM Customers LIMIT 1;

| CustomerID | CustomerName         | ContactName   | Address        | City       | PostalCode | Country    |
| :--------- | :--------------------| :------------ |:---------------|:-----------| :--------- |:-----------|
| 1          | Alfreds Futterkiste  | Maria Anders  | Obere Str. 57  | Berlin     | 12209      |  Germany   |


SELECT CustomerName, Country FROM Customers WHERE Country = "UK";

Around the Horn
B's Beverages
Consolidated Holdings
Eastern Connection
Island Trading
North/South 
Seven Seas Imports

2. What is the name of the customer who has the most orders?

SELECT * FROM Orders LIMIT 5;

| OrderID | CustomerID  | EmployeeID  | OrderDate   | ShipperID |
|:------- | :---------- | :---------- | :---------- | :-------- |
| 10248   |  90         | 5           |  1996-07-04 | 3         |
| 10249   |  81         | 6           | 1996-07-05  | 1         |
| 10250   |  34         | 4           | 1996-07-08  | 2         |
| 10251   |  84         | 3           | 1996-07-08  | 1         |
| 10252   |  76         | 4           | 1996-07-09  | 2         |

SELECT CustomerID, COUNT(\*) AS TotalOrders FROM Orders GROUP BY CustomerID ORDER BY TotalOrders DESC LIMIT 3;

| CustomerID | TotalOrders |
| :--------- | :---------- |
| 20         | 10          |
| 87         | 7           |
| 65         | 7           |

SELECT CustomerID, CustomerName FROM Customers WHERE CustomerID = 20;

| CustomerID | CustomerName |
| :--------- | :----------- |
| 20         | Ernst Handel |

The name of customer who has the most order is Ernst Handel


3. Which supplier has the highest average product price?

SELECT * FROM Products LIMIT 3;

| ProductID | ProductName   | SupplierID | CategoryID | Unit                | Price |
| :-------- | :------------ | :--------- | :----------| :-------------------| :-----|
| 1         | Chais         | 1          | 1          | 10 boxes x 20 bags  | 18    |
| 2         | Chang         | 1          | 1          | 24 - 12 oz bottles  | 19    |
| 3         | Aniseed Syrup | 1          | 2          | 12 - 550 ml bottles | 10    |

SELECT SupplierID, ROUND(AVG(Price),2) as AveragePrice FROM Products GROUP BY SupplierID ORDER BY AveragePrice DESC LIMIT 3;

| SupplierID | AveragePrice |
| :--------- | :------------|
| 18         | 140.75       |
| 4          | 46           |
| 12         | 44.68        |

SELECT SupplierID, SupplierName FROM Suppliers WHERE SupplierID = 18;

| SupplierID | SupplierName                |
| :--------- | :---------------------------|
| 18         | Aux joyeux ecclésiastiques  |

Supplier with highest average product price is Aux joyeux ecclésiastiqes.

4. How many different countries are all the customers from? (*Hint:* consider [DISTINCT](http://www.w3schools.com/sql/sql_distinct.asp).)

SELECT COUNT (DISTINCT Country) FROM Customers;
21

SELECT Count(* ) AS DistinctCountries
FROM (SELECT DISTINCT Country FROM Customers);

Number of Records: 1

|DistinctCountries|
| :--------------:|
| 21              |

5. What category appears in the most orders?
Orders:
SELECT * FROM Orders LIMIT 3;

Number of Records: 3

|OrderID | CustomerID | EmployeeID |  OrderDate  | ShipperID |
|:------ | :--------- | :--------- | :---------- | :-------- |
| 10248  | 90         | 5          | 1996-07-04  | 3         |
| 10249  | 81         | 6          | 1996-07-05  | 1         |
| 10250  | 34         | 4          | 1996-07-08  | 2         |

OrderDetails:
SELECT * FROM OrderDetails LIMIT 3;

|OrderDetailID | OrderID | ProductID |Quantity |
|:------------ |:------- |:--------- |:------- |
|1             | 10248   | 11        | 12      |
|2             | 10248   | 42        | 10      |
|3             | 10248   | 72        | 5       |

SELECT * FROM Products LIMIT 3;

Number of Records: 3

| ProductID| ProductName        |  SupplierID | CategoryID | Unit                | Price   |
| :------- |:-------------------| :---------- | :--------  | :------------------ | :------ |
| 1        |Chais               | 1           | 1          | 10 boxes x 20 bags  | 18      |
| 2        |Chang               | 1           | 1          | 24 - 12 oz bottles  | 19      |
| 3        | Aniseed Syrup      | 1           | 2          | 12 - 550 ml bottles | 10      |

Appending CategoryID to OrderDetails:
SELECT OrderDetails.* , Products.CategoryID FROM OrderDetails LEFT JOIN Products ON OrderDetails.ProductID = Products.ProductID;

Number of Records: 518

| OrderDetailID  | OrderID | ProductID | Quantity | CategoryID |
|:-------------- | :------ | :-------- | :------- | :--------- |
| 1              | 10248   | 11        | 12       | 4          |
| 2              | 10248   | 42        | 10       | 5          |
| 3              | 10248   | 72        | 5        | 4          |

etc

Then count occurence of each Category:

SELECT CategoryID, COUNT(* ) AS TotalCatOrdered FROM(SELECT OrderDetails.* , Products.CategoryID FROM OrderDetails LEFT JOIN Products ON OrderDetails.ProductID = Products.ProductID) GROUP BY CategoryID ORDER BY TotalCatOrdered DESC;

| CategoryID | TotalCatOrdered|
|:---------: |:--------------:|
| 4          | 100            |
| 1          | 93             |
| 3          | 84             |
| 8          | 67             |
| 6          | 50             |
| 2          | 49             |
| 5          | 42             |
| 7          | 33             |


SELECT ProductName from Products WHERE CategoryID = 4;


Result:
Number of Records: 10
ProductName
Queso Cabrales
Queso Manchego La Pastora
Gorgonzola Telino
Mascarpone Fabioli
Geitost
Raclette Courdavault
Camembert Pierrot
Gudbrandsdalsost
Fløtemysost
Mozzarella di Giovanni

SELECT ProductName as Category_4_Products FROM Products WHERE CategoryID =4;

Result:

| Category_4_Products       |
| :------------------------ |
| Number of Records: 10     |
| ProductName               |
| Queso Cabrales            |
| Queso Manchego La Pastora |
| Gorgonzola Telino         |
| Mascarpone Fabioli        |
| Geitost                   |
| Raclette Courdavault      |
| Camembert Pierrot         |
| Gudbrandsdalsost          |
| Fløtemysost               |
| Mozzarella di Giovanni    |

Category that appears in most orders is Category 4 which corresponds to cheese.

Joining Orders and OrderDetails data:
SELECT Orders.* , OrderDetails.* FROM Orders LEFT JOIN OrderDetails ON 
Orders.OrderID = OrderDetails.OrderID;

Number of Records: 518

|OrderID  | CustomerID  | EmployeeID | OrderDate   | ShipperID | OrderDetailID | ProductID | Quantity |
|:------- | :---------- | :--------- | :---------- | :-------- | :------------ | :-------- | :--------|
| 10248   | 90          | 5          | 1996-07-04  | 3         | 1             | 11        | 12       |
| 10248   | 90          | 5          | 1996-07-04  | 3         | 2             | 42        | 10       |
| 10248   | 90          | 5          | 1996-07-04  | 3         | 3             | 72        | 5        |
etc

Then:

SELECT ProductID, COUNT(* ) AS TotalProductOrdered FROM(SELECT Orders.* , OrderDetails.* FROM Orders LEFT JOIN OrderDetails ON Orders.OrderID = OrderDetails.OrderID) GROUP BY ProductID ORDER BY TotalProductOrdered DESC;

Number of Records: 77

| ProductID | TotalProductOrdered |
|:--------- | :------------------ |
| 72        | 14                  |
| 59        | 14                  |
| 31        | 14                  |
| 71        | 13                  |
| 62        | 13                  |
| 60        | 12                  |
| 56        | 12                  |
| 54        | 12                  |

etc



6. What was the total cost for each order?

First appending Price column from Products to OrderDetails.

SELECT OrderDetails.* , Products.Price FROM OrderDetails LEFT JOIN Products ON OrderDetails.ProductID = Products.ProductID;

|OrderDetailID | OrderID | ProductID |Quantity | Price |
|:-------------|:--------|:----------|:--------|:------|
|1             |10248    |11         |12       |21     |
|2             |10248    |42         |10       |14     |
|3             |10248    |72         |5        |34.8   |
|4             |10249    |14         |9        |23.25  |
|5             |10249    |51         |40       |53     |

Then:
SELECT OrderID, ROUND(Quantity* Price,2) AS TotalPrice FROM(
SELECT OrderDetails.* , Products.Price FROM OrderDetails LEFT JOIN Products ON OrderDetails.ProductID = Products.ProductID) GROUP BY OrderID;

Number of Records: 196

| OrderID | TotalPrice |
|:------- | ---------: |
| 10248   | 252        |
| 10249   | 209.25     |
| 10250   | 96.5       |
| 10251   | 126        |
| 10252   | 3240       |
| 10253   | 250        |

etc

7. Which employee made the most sales (by total price)?



SELECT Orders.* , OrderDetails.* , Products.* FROM Orders LEFT JOIN OrderDetails ON 
Orders.OrderID = OrderDetails.OrderID LEFT JOIN Products ON OrderDetails.ProductID = Products.ProductID;

|OrderID|CustomerID| EmployeeID| OrderDate | ShipperID| OrderDetailID|ProductID|Quantity|... |Price|
|:-----:|----------|:----------|:----------|:---------|:-------------|:--------|:-------|----|-----|
|10248  | 90       | 5         | 1996-07-04| 3        | 1            | 11      | 12     |    |21   |
|10248  | 90       | 5         | 1996-07-04| 3        | 2            | 42      | 10     |    |14   |
|10248  | 90       | 5         | 1996-07-04| 3        | 3            | 72      | 5      |    |34.8 |


SELECT EmployeeID, ROUND(Quantity* Price,2) AS TotalPrice FROM(
SELECT Orders.* , OrderDetails.* , Products.* FROM Orders LEFT JOIN OrderDetails ON 
Orders.OrderID = OrderDetails.OrderID LEFT JOIN Products ON OrderDetails.ProductID = Products.ProductID) GROUP BY EmployeeID ORDER by TotalPrice DESC LIMIT 3;

| EmployeeID | TotalPrice |
|2           | 1170       |
|1           | 950        |
|9           | 380        |

Employe who made the most sales is EmployeeID = 2

8. Which employees have BS degrees? (*Hint:* look at the [LIKE](http://www.w3schools.com/sql/sql_like.asp) operator.)
SELECT * FROM Employees WHERE Notes LIKE '%BS%';

|EmployeeID|LastName    |FirstName|BirthDate   |Photo      |Notes                                             |
|:---------|:-----------|:--------|:-----------|------------------------------------------------------------- |
|3         | Leverling  | Janet   | 1963-08-30 |EmpID3.pic |Janet has a BS degree in chemistry from <br  />
                                                            Boston College). She has also completed a <br /> 
                                                            certificate program in food retailing management.<br /> 
                                                            Janet was hired as a sales associate and was <br /> 
                                                            promoted to sales representative <br /> 
                                                .                                               |
|5         |Buchanan    | Steven  |1955-03-04  |EmpID5.pic |Steven Buchanan graduated from  St. Andrews <br /> 
                                                            University, Scotland, with a BSC degree. Upon <br /> 
                                                            joining the company as a sales representative, he <br /> 
                                                            spent 6 months in an orientation program at the <br /> 
                                                            Seattle office and then returned to his permanent <br /> 
                                                            post in London, where he was promoted to sales <br />  
                                                            manager. Mr. Buchanan has completed the courses <br /> 
                                                            'Successful Telemarketing' and 'International Sales<br /> 
                                                            Management'.He is fluent in French.                |                             |


9. Which supplier of three or more products has the highest average product price? (*Hint:* look at the [HAVING](http://www.w3schools.com/sql/sql_having.asp) operator.)

SELECT SupplierID, SupplierName, ROUND(AVG(Price),2) as AveragePrice, COUNT(DISTINCT ProductID) as ProductCount FROM (SELECT * FROM Products LEFT JOIN Suppliers ON Suppliers.SupplierID = Products.SupplierID) GROUP BY SupplierID HAVING ProductCount >=3 ORDER BY AveragePrice DESC LIMIT 1;

|SupplierID |SupplierName  |AveragePrice|ProductCount|
|:----------|:------------:|:----------:|:----------:|
| 4         |Tokyo Traders | 46         | 3          |


