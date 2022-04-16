# Assignment4-SQL


-- 1.      Create a view named “view_product_order_[your_last_name]”, list all products and total ordered quantity for that product.

create view view_product_order_li
as
select p.ProductName, count(od.ProductID) as totoal
from products p join [Order Details] od on od.ProductID = p.ProductID
Group by p.ProductName

select * from view_product_order_li
