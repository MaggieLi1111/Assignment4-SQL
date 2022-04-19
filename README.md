# Assignment4-SQL


-- 1. Create a view named “view_product_order_[your_last_name]”, list all products and total ordered quantity for that product.

create view view_product_order_li
as
select p.ProductName, count(od.ProductID) as totoal
from products p join [Order Details] od on od.ProductID = p.ProductID
Group by p.ProductName

select * from view_product_order_li

-- 2. Create a stored procedure “sp_product_order_quantity_[your_last_name]” that accept product id as an input 
-- and total quantities of order as output parameter.

ALTER PROC sp_Product_Order_Quantity_Gaddam
@ProductID INT,
@TotalOrderQty INT OUT
AS
BEGIN
SELECT @TotalOrderQty = SUM(Quantity)  FROM [Order Details] OD JOIN Products P ON P.ProductID = OD.ProductID
WHERE P.ProductID = @ProductID
END



DECLARE @Tot INT
EXEC sp_Product_Order_Quantity_Gaddam 11,@Tot OUT
PRINT @Tot 


-- 3. Create a stored procedure “sp_product_order_city_[your_last_name]” that accept product name as an input and top 5 cities that ordered most that product combined with the total quantity of that product ordered from that city as output.


ALTER PROC sp_Product_Order_City_Gaddam
@ProductName NVARCHAR(50)
AS
BEGIN
SELECT TOP 5 ShipCity,SUM(Quantity) FROM [Order Details] OD JOIN Products P ON P.ProductID = OD.ProductID JOIN Orders O ON O.OrderID = OD.OrderID
WHERE ProductName=@ProductName
GROUP BY ProductName,ShipCity
ORDER BY SUM(Quantity) DESC
END


EXEC sp_Product_Order_City_Gaddam 'Queso Cabrales'


-- 4. Create 2 new tables “people_your_last_name” “city_your_last_name”. 
-- City table has two records: {Id:1, City: Seattle}, {Id:2, City: Green Bay}. 
-- People has three records: {id:1, Name: Aaron Rodgers, City: 2}, {id:2, Name: Russell Wilson, City:1}, 
-- {Id: 3, Name: Jody Nelson, City:2}. 
-- Remove city of Seattle. If there was anyone from Seattle, put them into a new city “Madison”. 
-- Create a view “Packers_your_name” lists all people from Green Bay. 
-- If any error occurred, no changes should be made to DB. (after test) Drop both tables and view.

create view view_product_order_li
as
select p.ProductName, count(od.ProductID) as totoal
from products p join [Order Details] od on od.ProductID = p.ProductID
Group by p.ProductName

select * from view_product_order_li

create table city_li
(
id int,
city varchar(20)
)


insert into city_li (id, city)
values (2,'Green bay'), (1,'Seattle')

create table people_li
(
id int,
[name] varchar(20),
city int
)

insert into people_li (id, [name], city)
values (1, 'Aaron Rodgers', 2), (2, 'Russell Wilson', 1), (3,'Jody Nelson', 2)


update city_li 
set city = 'Madison'
where city = 'Seattle'

create view Packers_DanyangLi
as
select c.city, p.[name]
from city_li c left join people_li p on c.id = p.city
where c.city = 'green bay'

DROP table people_li, city_li



-- 5. Create a stored procedure “sp_birthday_employees_[you_last_name]” that creates a new table “birthday_employees_your_last_name” and fill it with all employees that have a birthday on Feb. (Make a screen shot) drop the table. Employee table should not be affected.

ALTER PROC sp_birthday_employee_gaddam
AS
BEGIN
SELECT * INTO #EmployeeTemp
FROM Employees WHERE DATEPART(MM,BirthDate) = 02
SELECT * FROM #EmployeeTemp
END


-- 6. How do you make sure two tables have the same data?

SELECT * FROM Table1
UNION
SELECT * FROM Table2

If records is the same number of each table, they don't have same data.
