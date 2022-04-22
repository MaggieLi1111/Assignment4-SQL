# Assignment4-SQL


-- 1. Create a view named “view_product_order_[your_last_name]”, list all products and total ordered quantity for that product.

CREATE VIEW View_Product_Order_Gaddam
AS
SELECT ProductName,SUM(Quantity) As TotalOrderQty 
FROM [Order Details] OD JOIN Products P ON P.ProductID = OD.ProductID
GROUP BY ProductName


select * from view_product_order_li

-- 2. Create a stored procedure “sp_product_order_quantity_[your_last_name]” that accept product id as an input 
-- and total quantities of order as output parameter.


Create Procedure sp_product_order_quantity_li
@ProductID int,
@TotalQuantities int out
AS
Begin 
Select @TotalQuantities = sum(quantity) 
from [Order Details] od join Products p on od.ProductID = p.ProductID
where p.ProductID = @ProductID
print @totalQuantities
end

DECLARE @real int
EXECUTE dbo.sp_product_order_quantity_li 2, @real out




-- 3. Create a stored procedure “sp_product_order_city_[your_last_name]” that accept product name as an input and top 5 cities that ordered most that product combined with the total quantity of that product ordered from that city as output.


create procedure sp_Product_order_city_li 
@ProductName nvarchar(50)
as 
begin 
select top 5 ShipCity, sum(Quantity) as total
from orders o join [Order Details] od on o.OrderID = od.OrderID join products p on p.ProductID = od.ProductID
where ProductName = @ProductName
group by shipcity
order by total desc
end


execute sp_Product_order_city_li 'chang'


-- 4. Create 2 new tables “people_your_last_name” “city_your_last_name”. 
-- City table has two records: {Id:1, City: Seattle}, {Id:2, City: Green Bay}. 
-- People has three records: {id:1, Name: Aaron Rodgers, City: 2}, {id:2, Name: Russell Wilson, City:1}, 
-- {Id: 3, Name: Jody Nelson, City:2}. 
-- Remove city of Seattle. If there was anyone from Seattle, put them into a new city “Madison”. 
-- Create a view “Packers_your_name” lists all people from Green Bay. 
-- If any error occurred, no changes should be made to DB. (after test) Drop both tables and view.



CREATE TABLE People_Gaddam
(
id int ,
name nvarchar(100),
city int
)

create table City_Gaddam
(
id int,
city nvarchar(100)
)
BEGIN TRAN 
insert into City_Gaddam values(1,'Seattle')
insert into City_Gaddam values(2,'Green Bay')

insert into People_Gaddam values(1,'Aaron Rodgers',1)
insert into People_Gaddam values(2,'Russell Wilson',2)
insert into People_Gaddam values(3,'Jody Nelson',2)

if exists(select id from People_Gaddam where city = (select id from City_Gaddam where city = 'Seatle'))
begin
insert into City_Gaddam values(3,'Madison')
update People_Gaddam
set city = 'Madison'
where id in (select id from People_Gaddam where city = (select id from City_Gaddam where city = 'Seatle'))
end
delete from City_Gaddam where city = 'Seattle'

CREATE VIEW Packers_Gaddam
AS
SELECT name FROM People_Gaddam WHERE city = 'Green Bay'

select * from Packers_Gaddam
commit
drop table People_Gaddam
drop table City_Gaddam
drop view Packers_Gaddam


-- 5. Create a stored procedure “sp_birthday_employees_[you_last_name]” that creates a new table “birthday_employees_your_last_name” and fill it with all employees that have a birthday on Feb. (Make a screen shot) drop the table. Employee table should not be affected.


ALTER PROC sp_birthday_employee_gaddam
AS
BEGIN
SELECT * INTO #EmployeeTemp
FROM Employees WHERE DATEPART(MM,BirthDate) = 02
SELECT * FROM #EmployeeTemp
END


-- 6. How do you make sure two tables have the same data?

USE EXCEPT KEYWORD

SELECT * FROM Customers
EXCEPT
SELECT * FROM Customers
