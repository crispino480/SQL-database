
## **Querying DataBase** (MYSQL)


###### Q1.
Write a SELECT statement that returns four columns from the Products table: product_code, product_name, list_price, and discount_percent. Add an ORDER BY clause to this statement that sorts the result set by list price in descending sequence. 
###### Query.
*`SELECT product_code, product_name, list_price, discount_percent
FROM products
ORDER BY  list_price DESC;`*



### **+++ Querying the database - Single Table**

Q.2 
Write a SELECT statement that returns three columns from the Customers table named first_name, last_name and full_name that combines the last_name and first_name columns.
Format this column with the last name, a comma, a space, and the first name like this:
Doe, John
Sort the result set by the last_name column in ascending sequence.
Return only the customers whose last name begins with letters from M to Z.
NOTE: When comparing strings of characters, ‘M’ comes before any string of characters that begins with ‘M’. For example, ‘M’ comes before ‘Murach’.


**Query**

*`SELECT first_name, last_name, CONCAT(last_name,', ', first_name) AS full_name
from customers
WHERE last_name REGEXP '^[M-Z]'
ORDER BY last_name`*



Q.3
Write a SELECT statement that returns these columns from the Products table:
product_name The product_name column
list_price The list_price column
date_added The date_added column
Return only the rows with a list price that’s greater than 500 and less than 2000.
Sort the result set by the date_added column in descending sequence.

**Query:**

*`
SELECT product_name, list_price, date_added
FROM products
WHERE list_price >500 AND list_price <2000
ORDER BY date_added DESC
`*
Q.4
Write a SELECT statement that returns these column names and data from the Products table:
product_name The product_name column
list_price The list_price column
discount_percent The discount_percent column
discount_amount A column that’s calculated from the previous two columns
discount_price A column that’s calculated from the previous three columns
Round the discount_amount and discount_price columns to 2 decimal places.
Sort the result set by the discount_price column in descending sequence.
Use the LIMIT clause so the result set contains only the first 5 rows.

**Query:**

*`
SELECT product_name, list_price, discount_percent,  ROUND(list_price * discount_percent/100, 2) AS discount_amount,
ROUND(list_price - (list_price * discount_percent/100), 2) AS discount_price
FROM products
ORDER BY discount_price DESC 
LIMIT 5
`*

Q.5
Write a SELECT statement that returns these column names and data from the Order_Items table:
item_id 

item_price The item_price column
discount_amount The discount_amount column
quantity The quantity column
price_total A column that’s calculated by multiplying the item price by the quantity
discount_total A column that’s calculated by multiplying the discount amount by the quantity
item_total A column that’s calculated by subtracting the discount amount from the item price and then multiplying by the quantity
Only return rows where the item_total is greater than 500.
Sort the result set by the item_total column in descending sequence.


**Query:**

*`
SELECT item_id, item_price, discount_amount, quantity, item_price * quantity AS price_total, 
discount_amount * quantity AS discount_total, (item_price - discount_amount) * quantity AS item_total 
FROM order_items
WHERE (item_price - discount_amount) * quantity > 500
ORDER BY item_total  DESC
`*

Q.6
Write a SELECT statement that returns these columns from the Orders table:
order_id The order_id column
order_date The order_date column
ship_date The ship_date column
Return only the rows where the ship_date column contains a null value.
Sort the result set by the order_id column in descending sequence.


**Query:**

*`
SELECT order_id, order_date, ship_date
FROM orders
WHERE ship_date IS NULL
ORDER BY order_id DESC
`*

Q.7
Write a SELECT statement without a FROM clause that creates a row with these columns:
price 100 (dollars)
tax_rate .07 (7 percent)
tax_amount The price multiplied by the tax
total The price plus the tax
To calculate the fourth column, add the expressions you used for the first and third columns.

*`
Query:
 SELECT 100 AS price , .07 AS tax_rate, 100 * .07 AS tax_amount, 100 + 0.07 * 100 AS total
`*

### +++ Querying the database - Multiple Tables

Q.8
Write a SELECT statement that joins the Categories table to the Products table and returns these columns: category_name, product_name, list_price.
Sort the result set by the category_name column and then by the product_name column in ascending sequence.

**Query:**

*`
SELECT category_name, product_name, list_price
FROM categories c
JOIN products p ON c.category_id = p.category_id
ORDER BY category_name, product_name ASC
`*

Q.9
Write a SELECT statement that joins the Customers table to the Addresses table and returns these columns: first_name, last_name, line1, city, state, zip_code.
Return one row for each address for the customer with an email address of allan.sherwood@yahoo.com
Sort the result set by the zip_code column in ascending sequence.

**Query:**

*`
SELECT first_name, last_name, line1, city, state, zip_code
FROM customers c
RIGHT JOIN addresses a ON c.customer_id = a.customer_id
WHERE email_address ='allan.sherwood@yahoo.com'
ORDER BY zip_code ASC
`*

Q.10
Write a SELECT statement that joins the Customers table to the Addresses table and returns these columns: first_name, last_name, line1, city, state, zip_code.
Return one row for each customer, but only return addresses that are the shipping address for a customer.
Sort the result set by the zip_code column in ascending sequence.

**Query:**

*`
SELECT first_name, last_name, line1, city, state, zip_code
FROM customers c JOIN addresses a
ON a.address_id = c.shipping_address_id
ORDER BY zip_code
`*

Q.11
Write a SELECT statement that joins the Customers, Orders, Order_Items, and Products tables. This statement should return these columns: last_name, first_name, order_date, product_name, item_price, discount_amount, and quantity.
Use aliases for the tables.
Sort the final result set by the last_name, order_date, and product_name columns.

**Query:**

*`
SELECT last_name, first_name, order_date, product_name, item_price,  discount_amount, quantity
FROM customers c JOIN orders o ON c.customer_id = o.customer_id
JOIN order_items k ON o.order_id = k.order_id
JOIN products p ON k.product_id = p.product_id
ORDER BY last_name, order_date, product_name
`*

Q.12
Use the UNION operator to generate a result set consisting of three columns from the Orders table: 
ship_status A calculated column that contains a value of SHIPPED or NOT SHIPPED
order_id The order_id column
order_date The order_date column
If the order has a value in the ship_date column, the ship_status column should contain a value of SHIPPED. Otherwise, it should contain a value of NOT SHIPPED.
Sort the final result set by the order_date column.

**Query:**

*`SELECT 'SHIPPED' As ship_status, order_id, order_date
FROM orders
WHERE ship_date IS NOT NUll
UNION
SELECT 'NOT SHIPPED' As ship_status, order_id, order_date
FROM orders
WHERE ship_date IS NUll
`*

### +++Modifying the database

Q.13
Write an INSERT statement that adds this row to the Categories table:
category_name: Brass
Code the INSERT statement so MySQL automatically generates the category_id column.

**query:**

*`
INSERT into categories (category_name) VALUES ('Brass')
`*

Q.14
Write an UPDATE statement that modifies the drums category in the Categories table. This statement should change the category_name column to “Woodwinds”, and it should use the category_id to identify the row.

**Query:**

*`
UPDATE categories 
SET category_name = 'Woodwinds'
WHERE category_id = 3
`*

Q.15
Write a DELETE statement that deletes the Keyboards category in the Categories table. This statement should use the category_id column to identify the row.

**Query:**

*`
DELETE FROM categories
WHERE category_id= 4
`*

Q.16
Write an INSERT statement that adds this row to the Products table:
product_id: The next automatically generated ID 
category_id: 4
product_code: dgx_640
product_name: Yamaha DGX 640 88-Key Digital Piano
description: Long description to come.
list_price: 799.99
discount_percent: 0
date_added: Today’s date/time.
Use a column list for this statement.

**Query:**

*`
INSERT INTO products 
(category_id, product_code, product_name, description, list_price, discount_percent,date_added)
VALUES
(4, 'dgx_640', 'Yamaha DGX 640 88-Key Digital Piano', 'Long description to come.', 799.99, 0, '2021-07-30')
`*

Q.17
Write an UPDATE statement that modifies the 'Fender Stratocaster' product. This statement should change the discount_percent column from 30% to 35%.


**Query:**

*`
UPDATE products
SET discount_percent = 35.00 
WHERE product_id = 1
`*

Q.18
Write an INSERT statement that adds this row to the Customers table:
email_address: rick@raven.com
password: (empty string)
first_name: Rick
last_name: Raven
Use a column list for this statement.

**Query:**

*`
INSERT INTO customers
(email_address, password, first_name, last_name)
VALUES
('rick@raven.com', '','Rick', 'Raven')
`*

Q.19
Write an UPDATE statement that modifies the Customers table. Change the first_name column to “Al” for the customer with an email address of 'allan.sherwood@yahoo.com'.

**Query:**

*`
UPDATE customers
SET first_name ='Al'
WHERE customer_id = 1
`*
