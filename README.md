# PROJECT IS Sunrise Supermarket SYSTEM
## NAME:DUSHIMIMANA Emmanuel
## Id:20251SEN201

## Short Summary About Sunrise Supermarket.
### This project implements a database system for Sunrise Supermarket using Oracle database .It creates and populates tables foe customers, products, orders and order_items, then uses JOINs, CTEs and Window Functions to analyze customer purchases, spending order history and sales trends.

## Can you ask how to run it?
### -Open Oracle SQL Developer.
### -Connect to the Oracle database.
### -Run the table creation SQL scripts.
### -Run the sample data insertion scripts.
### -Run the analysis queries provided in the project.
### -Review the query results and screenshots included in the repository.

### to do this Assignment there are same SQL/DBMS tool help to perform my assignment is ORACLE DATABASE.
### Before to run query I created all tables such as customers, products, orders and order_items this is query used.
```sql CREATETABLE customers (
customer_id NUMBER PRIMARYKEY,
customer_name VARCHAR2(100),
email VARCHAR2(100),
city VARCHAR2(50)
);
```

```sql CREATETABLE products (
product_id NUMBER PRIMARYKEY,
product_name VARCHAR2(100),
category VARCHAR2(50),
price NUMBER(10,2)
);
```
```sql CREATETABLE orders (
order_id
NUMBERPRIMARYKEY,
customer_id NUMBER REFERENCES customers(customer_id),
order_date DATE
);
```
```sql CREATETABLE order_items (
order_item_id NUMBER PRIMARYKEY,
order_id
NUMBERREFERENCESorders(order_id),
product_id NUMBER REFERENCES products(product_id),
quantity NUMBER
);
```


## then I insert records according and follow this instructions such as Populate each table with realistic sample data: at least 5 customers, 8 products (across at least 3 categories), 15 orders, and 25 order items, spread across multiple dates so trends are visible.
### this  show insert query and their output data to customer.
https://github.com/dushimimanaemmy/Assignment_1_DUSHIMIMANA_Emmanuel_Id_20251SEN201/blob/7ae403c9c1649328421275d8d4d9ba51088712fd/Output/Output_insert_Customer.png

### this  show insert query and their output data to products.
https://github.com/dushimimanaemmy/Assignment_1_DUSHIMIMANA_Emmanuel_Id_20251SEN201/blob/5b5ea7aa3d4297830cd94bd8425aa8cb1bc70ebd/Output/Output_product%20iinsert.png

### this  show insert query and their output data to products.
https://github.com/dushimimanaemmy/Assignment_1_DUSHIMIMANA_Emmanuel_Id_20251SEN201/blob/222ef54d2507c17ee017246eeca752fb3e4d3b12/Output/Output_orders%20.png

## JOIN, CTE and Window Function Queries( So I'm start answer for each question their purpose, Query, Screenshot or query results and and explain what it answer).

## In question one about to list Every Order with Customer Information.
### Purpose of scenario is query uses an INNER JOIN between the orders and customers tables. It shows each order together with the customer's name, city, and order date.
### Query: 
```sql SELECT o.order_id, c.customer_name, c.city, o.order_date FROM orders o 
INNER JOIN customers c ON o.customer_id = c.customer_id ORDER BY o.order_date;
```
### what it answers It helps management know who placed each order, where the customer is located, and when the order was placed.
### Output is open this link:
https://github.com/dushimimanaemmy/Assignment_1_DUSHIMIMANA_Emmanuel_Id_20251SEN201/blob/eedbfdadf6ecc96074bb1f80e2b3919354ae1f6c/Output/Output_Q1.png

## Question two: It about list Every Order Item with Product Information.
### For the purpose of this query joins the order_items and products tables to display the product purchased, its category, price, and quantity ordered.

### Query is:
```sql
SELECT oi.order_item_id, p.product_name, p.category, p.price, oi.quantity 
FROM order_items oi INNER JOIN products p ON oi.product_id = p.product_id ORDER BY oi.order_item_id;
```
### what it answers It shows which products customers are buying, their categories, prices, and the quantities purchased.
### OUTPUT open this link: 
https://github.com/dushimimanaemmy/Assignment_1_DUSHIMIMANA_Emmanuel_Id_20251SEN201/blob/097247c5a209a4a64ca8bc6cee14ea07bdbadc90/Output/Output_Q2(1).png

https://github.com/dushimimanaemmy/Assignment_1_DUSHIMIMANA_Emmanuel_Id_20251SEN201/blob/ca4afc8710d469e5f7820a8a610482b2b7ff21da/Otput_Q2(2).png


## Question Three: It about show All Customers and Their Orders.
### For the purpose of this query uses a LEFT JOIN so that all customers are displayed, including customers who have not placed an order.
### QUERY IS: 
```sql SELECT c.customer_id, c.customer_name, c.city, o.order_id, o.order_date 
FROM customers c LEFT JOIN orders o ON c.customer_id = o.customer_id 
ORDER BY c.customer_id, o.order_date;
```
### what it answers It helps management identify both active customers and customers who have not yet placed any orders.
## OUTPUT IS open this link:
https://github.com/dushimimanaemmy/Assignment_1_DUSHIMIMANA_Emmanuel_Id_20251SEN201/blob/48760a63a13c931179a1cdb7bf7e215560e1851a/Output/Output_Q3.png

## Question Four is about to display customers Who Spent Above the Average.
### For the purpose of this uses a CTE (Common Table Expression) to calculate the total amount spent by each customer. It then calculates the average customer spending and returns only customers whose spending is above that average.
### QUERY IS: 
WITH
customer_spending AS ( SELECT c.customer_id, c.customer_name, SUM(oi.quantity * p.price) AS total_spent 
FROM customers c JOIN orders o ON c.customer_id = o.customer_id JOIN order_items oi ON o.order_id = oi.order_id
JOIN products p ON oi.product_id = p.product_id GROUP BY c.customer_id, c.customer_name ) SELECT customer_id, customer_name, total_spent
FROM customer_spending WHERE total_spent > ( SELECT AVG(total_spent) FROM customer_spending ) ORDER BY total_spent DESC;
### what it answers Itidentifies customers who spend more than the average customer. Management can use this information to understand their high-spending customers.
## OUTPUT IS open this link:
https://github.com/dushimimanaemmy/Assignment_1_DUSHIMIMANA_Emmanuel_Id_20251SEN201/blob/fc5c27e84744193868ca3775eae659dcaec53ec5/Output/Ouput_Q4.png

## Question five is about to display Rank Customers by Total Amount Spent.
### For the purpose of thisquery uses the RANK() window function to rank customers according to their total spending, from the highest amount to the lowest.
### QUERY IS : 
```sql SELECT c.customer_id, c.customer_name, SUM(oi.quantity * p.price) AS total_spent, RANK() OVER ( ORDER BY SUM(oi.quantity * p.price) DESC ) AS spending_rank
FROM customers c JOIN orders o ON c.customer_id = o.customer_id JOIN order_items oi ON o.order_id = oi.order_id JOIN products p ON oi.product_id = p.product_id 
GROUP BY c.customer_id, c.customer_name ORDER BY spending_rank;
```
### what it answers It shows the relative spending position of each customer, allowing management to see which customers contribute the most sales.
## OUTPUT IS open this link:
https://github.com/dushimimanaemmy/Assignment_1_DUSHIMIMANA_Emmanuel_Id_20251SEN201/blob/1a56a55e1a094f9475d2debb2b96c6f22e7d1b2f/Output/Ouptut_Q5.png

## Question six is about to display Number Each Customer's Orders.
### For the purpose of query uses the ROW_NUMBER() window function to number each customer's orders according to the order date.
### QUERY IS: 
```sql SELECT o.order_id, c.customer_name, o.order_date, ROW_NUMBER() OVER ( PARTITION BY c.customer_id ORDER BY o.order_date )
AS order_number FROM orders o JOIN customers c ON o.customer_id = c.customer_id 
ORDER BY c.customer_name, o.order_date;
```
### what it answers It shows the first, second, third, and later orders made by each customer.
## OUTPUT IS open this link:
https://github.com/dushimimanaemmy/Assignment_1_DUSHIMIMANA_Emmanuel_Id_20251SEN201/blob/d37c3472b4c5ddebf34e9e82d022bb6191cf7dd9/Output/Output_Q6.png

## Question seven is about to running Total of Revenue Over Time .
### For the purpose of this query calculates the revenue generated by each order and uses a window function to calculate the running total of revenue over time.
### QUERY IS:
```sql SELECT o.order_date, o.order_id, SUM(oi.quantity * p.price) 
AS order_revenue, SUM(SUM(oi.quantity * p.price)) OVER ( ORDER BY o.order_date, o.order_id )
AS running_total_revenue FROM orders o JOIN order_items oi ON o.order_id = oi.order_id JOIN products p ON oi.product_id = p.product_id GROUP BY o.order_date, o.order_id 
ORDER BY o.order_date, o.order_id;
```
### what it answers It helps management see how total revenue accumulates over time and understand the sales trend.
## OUTPUT IS open this link:
https://github.com/dushimimanaemmy/Assignment_1_DUSHIMIMANA_Emmanuel_Id_20251SEN201/blob/3b7421b28bc69b42bc31f16600df716607634ca9/Output/Output_Q7.png


## Question eight is about to display Days Between Customer Orders.
### For the purpose of this query uses the LAG() window function to compare each customer's current order date with their previous order date.
### QUERY IS:
```sql SELECT customer_name, order_id, order_date, previous_order_date, order_date - previous_order_date AS days_between_orders
FROM (
    SELECT c.customer_name, o.order_id, o.order_date, LAG(o.order_date) OVER (PARTITION BY c.customer_id ORDER BY o.order_date)
    AS previous_order_date,COUNT(*) OVER ( PARTITION BY c.customer_id ) AS order_count
    FROM customers c INNER JOIN orders o  ON c.customer_id = o.customer_id
)
WHERE order_count > 1
  AND previous_order_date IS NOT NULL
ORDER BY customer_name, order_date;
```
### what it answers It This query uses the LAG() window function to retrieve each customer's previous order date. Oracle subtracts the previous order date from the current order date to calculate the number of days between orders. The query only displays customers who have placed more than one order.
## OUTPUT IS open this link:
https://github.com/dushimimanaemmy/Assignment_1_DUSHIMIMANA_Emmanuel_Id_20251SEN201/blob/3b7421b28bc69b42bc31f16600df716607634ca9/Output/Output_Q8.png


## Business Interpretation
### The results help Sunrise Supermarket management understand customer purchasing behavior and sales performance. They show which customers place orders, which products are purchased, how much customers spend, customer order frequency, and how revenue changes over time. The analysis also identifies customers who spend above the average and shows the time between repeat orders. This information can help management understand customer behavior, monitor sales, and plan future marketing and sales activities.

## Challenges Encountered and How They Were Resolved
### First challenge  was understanding CTE and window functions such as RANK(), ROW_NUMBER(), and LAG(). I resolved this by testing each query in Oracle SQL Developer and checking the results step by step.

### I also encountered Second challenges  when working with dates and calculating the number of days between customer orders. I resolved this by using Oracle date subtraction with the LAG() function.
