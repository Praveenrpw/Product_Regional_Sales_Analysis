
### Data Analysis Using SQL
### Database name (sales)

-- use shipments;

-- update orders Set region="East" where unit_profit=-0.5 limit 1;


1. -- display the table
SELECT * FROM orders;


2. -- write a sql query to list all distinct cities where orders have been shipped
select distinct City from orders;


3. -- calculate the total selling price and total profits for all orders
select 
Order_Id, sum(Quantity * Unit_Selling_Price) as 'Total_Selling_Price', 
cast(sum(Quantity * Unit_Profit) as decimal(10, 2)) as 'Total_Profitt' 
from  orders 
group by Order_Id 
order by Total_Profitt
desc; 


4. -- write a query to find all orders from the 'Technology' from category, that were shipped using 'second class' ship mode, ordered by 
   -- order date
select
Order_Id, Order_Date, Category
from orders 
where category = 'Technology' and Ship_Mode = 'Second Class'
order by Order_Date; 


5. -- write a query to find the average order value
select cast(avg(Quantity * Unit_Selling_Price) as decimal(10, 2))
as Average_Order_Value 
from orders;


6. -- find the city with the highest total quantity of products ordered
select City, sum(Quantity) as 'Total_Quantity'
from orders 
group by City
order by Total_Quantity
desc
limit 1;


7. -- find the city and ship mode with the highest total quantity of products ordered
select City, Ship_Mode,
sum(Quantity) as 'Total_Quantity'
from orders
group by City, Ship_Mode
order by Total_Quantity
desc 
limit 5;


8. -- use a widow function to rank orders in each region by quantity in desc order
select Order_Id, Region, Quantity as 'Total_Quanitity', 
dense_rank() over (partition by Region Order by Quantity desc) as rankings
from orders
order by Region, rankings;


9. -- find the month of each order_id
select Order_Id, Order_Date, month(Order_Date) as Month from orders;


10. -- list all the orders placed in the first quarter of any year (Jan to Mar), including the total cost for these orders.
select Order_Id, Order_Date, sum(Quantity * Unit_Selling_Price) as Total_Cost
from orders
where month(Order_Date) in (1, 2, 3)
group by Order_Id, Order_Date
order by Total_Cost desc;


11. -- find top 10 highest profit generating products
select Product_Id, cast(sum(Total_Profit) as decimal(10, 2)) as Highest_Profits
from orders
group by Product_Id
order by Highest_Profits desc
limit 10;


12. -- aternate using window function
-- cte (Common Table Expression)
with cte as(
    select Product_Id, cast(sum(Total_Profit) as decimal(10, 2)) as Profit, 
    row_number() over(order by sum(Total_Profit) desc) as rn
    from orders
    group by Product_Id
)
select Product_Id, Profit
from cte where rn <= 10;


13. -- find top 10 highest profit generating products using dense_rank
select Product_Id, cast(sum(Total_Profit) as decimal(10, 2)) as Highest_Profits, 
dense_rank() over(order by sum(Total_Profit) desc) as rankings
from orders
group by Product_Id
order by Highest_Profits desc
limit 10;


14. -- find top 10 highest revenue generating products
select Product_ID,
sum(Quantity * Unit_Selling_Price) as Total_Revenue
from orders
group by Product_ID
order by Total_Revenue desc
limit 10;


15. -- find top 10 highest revenue generating products by rankings
select Product_ID,
sum(Quantity * Unit_Selling_Price) as Total_Revenue,
rank() over (order by SUM(Quantity * Unit_Selling_Price) desc) as Rankings
from orders
group by Product_ID
order by Rankings
limit 10;


16. -- find top 10 highest total sales city and ship mode of generating products
select product_id, city, ship_mode, sum(Total_Sales) as Total_Sales, 
row_number() over(order by sum(Total_Sales) desc) as rankings
from orders
group by product_id, city, ship_mode
order by rankings
limit 10;


17. -- find the highest selling products in each region 
with cte as(
	select Region, Product_Id, cast(sum(Quantity * Unit_Selling_Price) as decimal(10, 2)) as sales,
	row_number() over (partition by Region order by sum(Quantity * Unit_Selling_Price) desc) as rnk
	from orders
	group by Region, Product_Id
	order by Region, rnk
)
select * from cte where rnk <= 3;


18. -- find the products in each region of total profit with rankings based on region
with cte as(
	select Region, Product_Id, cast(sum(Quantity * Unit_selling_Price) as decimal(10, 2)) as Total_Profit,
    row_number() over(partition by Region order by sum(Quantity * Unit_Selling_Price) desc) as rankings
    from orders
    group by Region, Product_Id
    order by Region, rankings
)
select * from cte where rankings <= 3;
    

19. -- find top 3 highest selling products in each region 
with cte as(
	select Region, Product_Id, cast(sum(Quantity * Unit_Selling_Price) as decimal(10, 2)) as sales
	from orders
	group by Region, Product_Id
)
select * from (
	select *, 
	row_number() over (partition by Region order by sales desc) as rn
	from cte
) A
where rn <= 3;

20. -- find month over month growth comparision for 2022 and 2023 sales ex: jan 2022 vs jan 2023
with cte as (
	select year(Order_Date) as Order_Year, month(Order_Date) as Order_Month,
	sum(Quantity * Unit_Selling_Price) as sales
	from orders
	group by year(Order_Date), month(Order_Date)
)
select Order_Month,
round(sum(case when Order_Year = 2022 then sales else 0 end), 2) as sales_2022,
round(sum(case when Order_Year = 2023 then sales else 0 end), 2) as sales_2023
from cte
group by Order_Month
order by Order_Month;


21. -- month over month growth comparision and percentage growth between 2022 and 2023
with cte as(
	select month(Order_Date) as Order_Month, year(Order_Date) as Order_Year, 
    sum(Quantity * Unit_Selling_Price) as Sales
	from orders
    where year(Order_Date) in (2022, 2023)
    group by year(Order_Date), month(Order_Date)
)
select 
	Order_Month,
    round(sum(case when Order_Year = 2022 then Sales else 0 end), 2) as Sales_2022,
    round(sum(case when Order_Year = 2023 then Sales else 0 end), 2) as Sales_2023,
    round(
		(sum(case when Order_Year = 2023 then Sales else 0 end) - sum(case when Order_Year = 2022 then Sales else 0 end)) 
        / nullif(sum(case when Order_Year = 2022 then Sales else 0 end), 0) * 100, 2) as growth_percentage
from cte
group by Order_Month
order by Order_Month;

22. -- month over month as (jan, feb, ..) growth comparision and percentage growth between 2022 and 2023
with cte as(
	select monthname(Order_Date) as Order_Month, year(Order_Date) as Order_Year, 
    sum(Quantity * Unit_Selling_Price) as Sales
	from orders
    where year(Order_Date) in (2022, 2023)
    group by year(Order_Date), month(Order_Date), monthname(Order_Date)
)
select 
	Order_Month,
    round(sum(case when Order_Year = 2022 then Sales else 0 end), 2) as Sales_2022,
    round(sum(case when Order_Year = 2023 then Sales else 0 end), 2) as Sales_2023,
    round(
		(sum(case when Order_Year = 2023 then Sales else 0 end) - sum(case when Order_Year = 2022 then Sales else 0 end)) 
        / nullif(sum(case when Order_Year = 2022 then Sales else 0 end), 0) * 100, 2) as growth_percentage
from cte
group by Order_Month
order by field(Order_Month, 'January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December');


23. -- for each category which month had highest sales
with cte as (
	select Category, 
	date_format(Order_Date, '%Y-%m') as Order_Year_Month,
	cast(sum(Quantity * Unit_Selling_Price) as decimal(10, 2)) as Total_Sales,
	row_number() over (partition by Category order by SUM(Quantity * Unit_Selling_Price) desc) as rn
    from orders
    group by Category, date_format(Order_Date, '%Y-%m')
)
select Category, Order_Year_Month, Total_Sales
from cte
where rn = 1;


24. -- which sub category had highest growth by sales in 2023 compare to 2022
with cte as(
	select Sub_Category, year(Order_Date) as Order_Year,
	cast(sum(Quantity * Unit_Selling_Price) as decimal(10, 2)) as Total_Sales
	from orders
	group by Sub_Category, year(Order_Date)
-- order by year(Order_Date), month(Order_date)
), 
cte2 as (
	select Sub_Category,
	round(sum(case when Order_Year = 2022 then Total_Sales else 0 end), 2) as Sales_2022,
	round(sum(case when Order_Year = 2023 then Total_Sales else 0 end), 2) as Sales_2023
	from cte
	group by Sub_Category
)
select Sub_Category, Sales_2022 as 'Sales_in_2022', Sales_2023 as 'Sales_in_2023',
(Sales_2023 - Sales_2022) as 'Diff_in_Amt'
from cte2
order by Diff_in_Amt desc limit 1;







ROW_NUMBER() should rank rows within each partition (Region)



