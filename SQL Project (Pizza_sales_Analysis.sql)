-- Q1. Retrieve the total number of orders placed.

select count(order_id) as "total no. of orders"
from orders;

-- Q2. Calculate the total revenue generated from pizza sales.

select round(sum(quantity * price),2) as total_revenue_generated
from pizzas
join order_details using(pizza_id);


-- Q3. Identify the highest-priced pizza.

select name,price
from pizzas
join pizza_types using(pizza_type_id)
order by price desc
limit 1;


-- Q4. Identify the most common pizza size ordered.

select size, count(order_details_id) as count_of_orders
from pizzas
join order_details using(pizza_id)
group by size
order by  count_of_orders desc
limit 1;

-- Q5. List the top 5 most ordered pizza types along with their quantities.

select name, sum(quantity) as count_of_orders
from order_details
join pizzas using(pizza_id)
join pizza_types using(pizza_type_id)
group by name
order by count_of_orders desc
limit 5;


-- Q6. Join the necessary tables to find the total quantity of each pizza category ordered.

select category, sum(quantity) as "total quantity"
from pizzas
join pizza_types using(pizza_type_id)
join order_details using(pizza_id)
group by category;


-- Q7. Determine the distribution of orders by hour of the day.

select hour(order_time) as hours, count(order_id) as count_of_orders
from orders
group by hours;

-- Q8. Find the category-wise distribution of pizzas.

select category, count(name) as pizza_distribution
from pizza_types
group by category;


-- Q9. Group the orders by date and calculate the average number of pizzas ordered per day.

select round(avg(total_quantity)) as avg_quantity
from(
select order_date, sum(quantity) as total_quantity 
from orders
join order_details using(order_id)
group by order_date) as order_qty;

-- Q10. Determine the top 3 most ordered pizza types based on revenue.

select pizza_type_id, sum((quantity*price)) as revenue
from order_details
left join pizzas using(pizza_id)
group by pizza_type_id
order by revenue desc
limit 3;


-- Q11. Calculate the percentage contribution of each pizza type to total revenue.

select pizza_type_id, round(sum(quantity*price),2) as revenue, 
    round((sum(quantity*price)/(select sum(price*quantity) from pizzas join order_details using(pizza_id)))*100,2) as percent_cont
from pizzas
join order_details using(pizza_id)
group by pizza_type_id;

-- Q12. Analyze the cumulative revenue generated over time.

select order_date, total_revenue, sum(total_revenue) over(order by order_date) as cum_rev
from(
select order_date, round(sum(quantity*price)) as total_revenue
from pizzas
join order_details using(pizza_id)
join orders using(order_id)
group by order_date) as result;


-- Q13. Determine the top 3 most ordered pizza types based on revenue for each pizza category.

select *
from(
select *, dense_rank() over(partition by category order by revenue desc) as pizza_rank
from(
select category, pizza_type_id, round(sum(quantity*price)) as revenue
from order_details 
join pizzas using(pizza_id)
join pizza_types using(pizza_type_id)
group by pizza_type_id, category) as result) as result2
where pizza_rank<=3;


