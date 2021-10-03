
## **Querying DataBase - Database Management** (MYSQL)

**Set you Database to run the scripts:**

   1 - *[Install a copy of MySQL on a local machine ](https://dev.mysql.com/doc/refman/8.0/en/installing.html)*
 
   2 - *[Lab Guitar Shop Database Creation Script](https://drive.google.com/file/d/1x-vxsSx1uNGdjGrGonAzuRmBytHZsbjZ/view)*


###### Q1.
Write a SELECT statement that returns four columns from the Products table: product_code, product_name, list_price, and discount_percent. Add an ORDER BY clause to this statement that sorts the result set by list price in descending sequence. 
###### Query.

```
SELECT product_code, product_name, list_price, discount_percent
FROM products
ORDER BY  list_price DESC;
```



### **+++ Querying the database - Single Table**

###### Q2. 
Write a SELECT statement that returns three columns from the Customers table named first_name, last_name and full_name that combines the last_name and first_name columns.
Format this column with the last name, a comma, a space, and the first name like this:
Doe, John
Sort the result set by the last_name column in ascending sequence.
Return only the customers whose last name begins with letters from M to Z.
NOTE: When comparing strings of characters, ‘M’ comes before any string of characters that begins with ‘M’. For example, ‘M’ comes before ‘Murach’.


**Query**

```
SELECT first_name, last_name, CONCAT(last_name,', ', first_name) AS full_name
from customers
WHERE last_name REGEXP '^[M-Z]'
ORDER BY last_name
```



###### Q3.
Write a SELECT statement that returns these columns from the Products table:
product_name The product_name column
list_price The list_price column
date_added The date_added column
Return only the rows with a list price that’s greater than 500 and less than 2000.
Sort the result set by the date_added column in descending sequence.

**Query:**

```
SELECT product_name, list_price, date_added
FROM products
WHERE list_price >500 AND list_price <2000
ORDER BY date_added DESC
```
###### Q4.
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

```
SELECT product_name, list_price, discount_percent,  ROUND(list_price * discount_percent/100, 2) AS discount_amount,
ROUND(list_price - (list_price * discount_percent/100), 2) AS discount_price
FROM products
ORDER BY discount_price DESC 
LIMIT 5
```

###### Q5.
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

```
SELECT item_id, item_price, discount_amount, quantity, item_price * quantity AS price_total, 
discount_amount * quantity AS discount_total, (item_price - discount_amount) * quantity AS item_total 
FROM order_items
WHERE (item_price - discount_amount) * quantity > 500
ORDER BY item_total  DESC
```

###### Q6.
Write a SELECT statement that returns these columns from the Orders table:
order_id The order_id column
order_date The order_date column
ship_date The ship_date column
Return only the rows where the ship_date column contains a null value.
Sort the result set by the order_id column in descending sequence.


**Query:**

```
SELECT order_id, order_date, ship_date
FROM orders
WHERE ship_date IS NULL
ORDER BY order_id DESC
```

###### Q7.
Write a SELECT statement without a FROM clause that creates a row with these columns:
price 100 (dollars)
tax_rate .07 (7 percent)
tax_amount The price multiplied by the tax
total The price plus the tax
To calculate the fourth column, add the expressions you used for the first and third columns.

```
Query:
 SELECT 100 AS price , .07 AS tax_rate, 100 * .07 AS tax_amount, 100 + 0.07 * 100 AS total
```

### +++ Querying the database - Multiple Tables

###### Q8.
Write a SELECT statement that joins the Categories table to the Products table and returns these columns: category_name, product_name, list_price.
Sort the result set by the category_name column and then by the product_name column in ascending sequence.

**Query:**

```
SELECT category_name, product_name, list_price
FROM categories c
JOIN products p ON c.category_id = p.category_id
ORDER BY category_name, product_name ASC
```

###### Q9.
Write a SELECT statement that joins the Customers table to the Addresses table and returns these columns: first_name, last_name, line1, city, state, zip_code.
Return one row for each address for the customer with an email address of allan.sherwood@yahoo.com
Sort the result set by the zip_code column in ascending sequence.

**Query:**

```
SELECT first_name, last_name, line1, city, state, zip_code
FROM customers c
RIGHT JOIN addresses a ON c.customer_id = a.customer_id
WHERE email_address ='allan.sherwood@yahoo.com'
ORDER BY zip_code ASC
```

###### Q10.
Write a SELECT statement that joins the Customers table to the Addresses table and returns these columns: first_name, last_name, line1, city, state, zip_code.
Return one row for each customer, but only return addresses that are the shipping address for a customer.
Sort the result set by the zip_code column in ascending sequence.

**Query:**

```
SELECT first_name, last_name, line1, city, state, zip_code
FROM customers c JOIN addresses a
ON a.address_id = c.shipping_address_id
ORDER BY zip_code
```

###### Q11.
Write a SELECT statement that joins the Customers, Orders, Order_Items, and Products tables. This statement should return these columns: last_name, first_name, order_date, product_name, item_price, discount_amount, and quantity.
Use aliases for the tables.
Sort the final result set by the last_name, order_date, and product_name columns.

**Query:**

```
SELECT last_name, first_name, order_date, product_name, item_price,  discount_amount, quantity
FROM customers c JOIN orders o ON c.customer_id = o.customer_id
JOIN order_items k ON o.order_id = k.order_id
JOIN products p ON k.product_id = p.product_id
ORDER BY last_name, order_date, product_name
```

###### Q12.
Use the UNION operator to generate a result set consisting of three columns from the Orders table: 
ship_status A calculated column that contains a value of SHIPPED or NOT SHIPPED
order_id The order_id column
order_date The order_date column
If the order has a value in the ship_date column, the ship_status column should contain a value of SHIPPED. Otherwise, it should contain a value of NOT SHIPPED.
Sort the final result set by the order_date column.

**Query:**

```
SELECT 'SHIPPED' As ship_status, order_id, order_date
FROM orders
WHERE ship_date IS NOT NUll
UNION
SELECT 'NOT SHIPPED' As ship_status, order_id, order_date
FROM orders
WHERE ship_date IS NUll
```

### +++Modifying the database

###### Q13.
Write an INSERT statement that adds this row to the Categories table:
category_name: Brass
Code the INSERT statement so MySQL automatically generates the category_id column.

**query:**

```
INSERT into categories (category_name) VALUES ('Brass')
```

###### Q14.
Write an UPDATE statement that modifies the drums category in the Categories table. This statement should change the category_name column to “Woodwinds”, and it should use the category_id to identify the row.

**Query:**

```
UPDATE categories 
SET category_name = 'Woodwinds'
WHERE category_id = 3
```

###### Q15.
Write a DELETE statement that deletes the Keyboards category in the Categories table. This statement should use the category_id column to identify the row.

**Query:**

```
DELETE FROM categories
WHERE category_id= 4
```

###### Q16.
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

```
INSERT INTO products 
(category_id, product_code, product_name, description, list_price, discount_percent,date_added)
VALUES
(4, 'dgx_640', 'Yamaha DGX 640 88-Key Digital Piano', 'Long description to come.', 799.99, 0, '2021-07-30')
```

###### Q17.
Write an UPDATE statement that modifies the 'Fender Stratocaster' product. This statement should change the discount_percent column from 30% to 35%.


**Query:**

```
UPDATE products
SET discount_percent = 35.00 
WHERE product_id = 1
```

###### Q18.
Write an INSERT statement that adds this row to the Customers table:
email_address: rick@raven.com
password: (empty string)
first_name: Rick
last_name: Raven
Use a column list for this statement.

**Query:**

```
INSERT INTO customers
(email_address, password, first_name, last_name)
VALUES
('rick@raven.com', '','Rick', 'Raven')
```

###### Q19.
Write an UPDATE statement that modifies the Customers table. Change the first_name column to “Al” for the customer with an email address of 'allan.sherwood@yahoo.com'.

**Query:**

```
UPDATE customers
SET first_name ='Al'
WHERE customer_id = 1
```




### +++Aggregate Queries

###### Q.20

Write a SELECT statement that returns these columns:
The count of the number of orders in the Orders table named order_count
The sum of the tax_amount columns in the Orders table named tax_total
The average of the tax_amount columns in the Orders table named tax_average

**Query:**

```
SELECT COUNT(*) AS order_count, SUM(tax_amount) AS tax_total,
AVG(tax_amount) tax_average
FROM orders
```

###### Q.21
Write a SELECT statement that returns one row for each category that has products with these columns:
The category_name column from the Categories table
The count of the products in the Products table aliased as product_count
The list price of the most expensive product in the Products table aliased as most_expensive_product
Sort the result set so the category with the most products appears first.

**Query:**

```
FROM categories c JOIN products p
ON c.category_id = p.category_id
GROUP BY c.category_id
HAVING product_count <> 0
ORDER BY product_count DESC
```

###### Q.22
Write a SELECT statement that returns one row for each customer that has orders with these columns:
The email_address column from the Customers table
The sum of the item price in the Order_Items table multiplied by the quantity in the Order_Items table aliased as item_price_total
The sum of the discount amount column in the Order_Items table multiplied by the quantity in the Order_Items table aliased as discount_amount_total
Sort the result set in descending sequence by the item price total for each customer.

**Query:**

```
SELECT c.customer_id, c.email_address, SUM(item_price * quantity) AS item_price_total, 
SUM(discount_amount * quantity) AS discount_amount_total
FROM customers c JOIN orders o
ON c.customer_id = o.customer_id
JOIN order_items r
ON o.order_id = r.order_id
GROUP BY c.customer_id
HAVING COUNT(o.order_id) <> 0
ORDER BY item_price_total DESC
```

###### Q.23
Write a SELECT statement that returns one row for each customer that has orders with these columns:
The email_address column from the Customers table
A count of the number of orders aliased as order_count
The total amount for each order aliased as order_total (Hint: First, subtract the discount amount from the price. Then, multiply by the quantity.)
Return only those rows where the customer has more than 1 order.
Sort the result set in descending sequence by the sum of the line item amounts.

**Query:**

```
SELECT DISTINCT  c.email_address, COUNT(DISTINCT o.order_id) AS order_count, 
(SUM((item_price - discount_amount) * quantity )) AS order_total
FROM orders o JOIN customers c
ON o.customer_id = c.customer_id
JOIN order_items r
ON o.order_id = r.order_id
GROUP BY c.email_address
HAVING COUNT(DISTINCT o.order_id) >= 2
ORDER BY order_total DESC
```

Q.24
Modify the solution to lab 4 so it only counts and totals line items that have an item_price value that’s greater than 400.
Here are the directions for lab 4:
Write a SELECT statement that returns one row for each customer that has orders with these columns:
The email_address column from the Customers table
A count of the number of orders aliased as order_count
The total amount for each order aliased as order_total (Hint: First, subtract the discount amount from the price. Then, multiply by the quantity.)
Return only those rows where the customer has more than 1 order.
Sort the result set in descending sequence by the sum of the line item amounts.

**Query:**

```
SELECT DISTINCT  c.email_address, COUNT(DISTINCT o.order_id) AS order_count, 
(SUM((item_price - discount_amount) * quantity )) AS order_total
FROM orders o JOIN customers c
ON o.customer_id = c.customer_id
JOIN order_items r
ON o.order_id = r.order_id
WHERE  r.item_price > 400
GROUP BY c.email_address
HAVING COUNT(DISTINCT o.order_id) >= 2
ORDER BY order_total DESC
```

### +++ Subqueries

Q.25
Write a SELECT statement that answers this question: Which products have a list price that’s greater than the average list price for all products?
Return the prodcut_id, product_name and list_price columns for each product.
Sort the result set by the list_price column in descending sequence.

**Query:**

```
SELECT product_id, product_name, list_price
FROM products
WHERE list_price > (SELECT AVG (list_price) FROM products)
ORDER BY list_price DESC
```

###### Q.26
Write a SELECT statement that returns three columns: email_address, order_id, and the order total for each customer. To do this, you can group the result set by the email_address and order_id columns. In addition, you must calculate the order total from the columns in the Order_Items table.
Write a second SELECT statement that uses the first SELECT statement in its FROM clause. The main query should return three columns: the customer’s email address and the largest order (aliased as max_order_total) and the smallest order_id (aliased as min_order_id). To do this, you can group the result set by the email_address. Sort the result set by the largest order in descending sequence.

**Query:**

```
SELECT email_address, MAX(a1.order_total) AS max_order_total, MIN(a1.order_id) AS min_order_id
FROM (SELECT email_address, o.order_id, SUM((item_price - discount_amount) * quantity) AS order_total
FROM customers c JOIN orders o
ON c.customer_id = o.customer_id
JOIN order_items i 
ON o.order_id = i.order_id
GROUP BY email_address, order_id) AS a1
GROUP BY email_address
ORDER BY max_order_total DESC;
```

###### Q.27
Write a SELECT statement that returns the name, list_price and discount percent of each product that has a unique discount percent. In other words, don’t include products that have the same discount percent as another product.
Sort the result set by the product_name column.

**Query:**

```
SELECT product_name, list_price, discount_percent
FROM products
WHERE discount_percent 
NOT IN (SELECT discount_percent 
FROM products 
GROUP BY discount_percent
HAVING COUNT(discount_percent) > 1)
ORDER BY product_name
```

###### Q.28
Use a correlated subquery to return one row per customer, representing the customer’s oldest order (the one with the earliest date). Each row should include these three columns: email_address, oldest_order_id, and oldest_order_date. Only include customers who have placed an order.
Sort the result set by the oldest_order_date and oldest_order_id columns.

**Query:**

```
SELECT email_address,  o.order_id AS oldest_order_id, o.order_date AS oldest_order_date
FROM customers c JOIN orders o ON c.customer_id = o.customer_id
WHERE o.order_date = (SELECT MIN(order_date) FROM orders WHERE customer_id = o.customer_id)
GROUP BY email_address, oldest_order_id,  oldest_order_date
HAVING COUNT(o.order_id) <> 0
ORDER BY oldest_order_date , oldest_order_id
```

### +++Data Types

###### Q.29
Write a SELECT statement that returns these columns from the Products table:
The list_price column
A column that uses the FORMAT function to return the list_price column with 1 digit to the right of the decimal point.  Name this column price_format
A column that uses the CONVERT function to return the list_price column as an integer.  Name this column price_convert
A column that uses the CAST function to return the list_price column as an integer.  Name this column price_cast
Sort the result set by the list_price column in ascending sequence.

**Query:**

```
SELECT list_price, FORMAT(list_price, 1) AS price_format, CONVERT(list_price, SIGNED) AS price_convert, 
CAST(list_price AS UNSIGNED) AS price_cast
FROM products
ORDER BY list_price
```

###### Q.30
Write a SELECT statement that returns these columns from the Products table:
The date_added column
A column that uses the CAST function to return the date_added column with its date only (year, month, and day). Name this column added_date
A column that uses the CAST function to return the date_added column with just the year and the month.  Name this column added_char7
A column that uses the CAST function to return the date_added column with its full time only (hour, minutes, and seconds). Name this column added_time
Sort the result set by the date_added column in ascending sequence.

**Query:**

```
SELECT date_added, CAST(date_added AS DATE) AS added_date, CAST(date_added AS CHAR(7)) AS added_char7,
CAST(date_added AS TIME) AS added_time
FROM products 
ORDER BY date_added
```

### +++Using Functions

###### Q.31
Write a SELECT statement that returns these columns from the Products table:
The list_price column
The discount_percent column
A column named discount_amount that uses the previous two columns to calculate the discount amount and uses the ROUND function to round the result so it has 2 decimal digits
Sort the result set by the discount_amount column in ascending sequence.

***Query:***

```
SELECT list_price, discount_percent, ROUND(list_price * discount_percent/100, 2) AS discount_amount
FROM products 
ORDER By discount_amount
```

###### Q.32
Write a SELECT statement that returns these columns from the Orders table:
The order_date column
A column that uses the DATE_FORMAT function to return the four-digit year that’s stored in the order_date column.  Alias the column order_year
A column that uses the DATE_FORMAT function to return the order_date column in this format: Mon-DD-YYYY. In other words, use abbreviated months and separate each date component with dashes. Alias the column order_date_formatted
A column that uses the DATE_FORMAT function to return the order_date column with only the hours and minutes on a 12-hour clock with an am/pm indicator. Alias the column order_time
A column that uses the DATE_FORMAT function to return the order_date column in this format: MM/DD/YY HH:MM. In other words, use two-digit months, days, and years and separate them by slashes. Use 2-digit hours and minutes on a 24-hour clock. And use leading zeros for all date/time components. Alias the column  order_datetime
Sort the result set by the order_date column in ascending sequence.

**Query:**

```
SELECT order_date, DATE_FORMAT(order_date, "%Y") AS order_year, 
DATE_FORMAT(order_date, "%b-%d-%Y") AS order_date_formatted,
DATE_FORMAT(order_date, "%l:%i %p" ) AS order_time,
DATE_FORMAT(order_date, "%m/%d/%y %H:%i") AS order_datetime
FROM orders
ORDER BY order_date
```

###### Q.33

Write a SELECT statement that returns these columns from the Orders table:
The card_number column
The length of the card_number column aliased as card_number_length
The last four digits of the card_number column aliased as last_four_digits
A column that displays an X for each digit of the card_number column except for the last four digits. If the card number contains 16 digits, it should be displayed in this format: XXXX-XXXX-XXXX-1234, where 1234 are the actual last four digits of the number. If the card number contains 15 digits, it should be displayed in this format: XXXX-XXXXXX-X1234. (Hint: Use an IF function to determine which format to use.).  This column should be aliased as formatted_number
Sort the result set by the card_number column in ascending sequence.

**Query:**

```
SELECT card_number, LENGTH(card_number) AS card_number_length, 
substr(card_number, LENGTH(card_number) - 3, 4) AS last_four_digits,
IF(LENGTH(card_number) = 16, CONCAT("XXXX-XXXX-XXXX-",SUBSTR(card_number,13,4)),
CONCAT("XXXX-XXXXXX-X",SUBSTR(card_number,12,4)))
AS formatted_number
FROM orders
ORDER BY card_number
```


###### Q.34 
Write a SELECT statement that returns these columns from the Orders table:
The order_id column
The order_date column
A column named approx_ship_date that’s calculated by adding 2 days to the order_date column
The ship_date column if it doesn’t contain a null value
A column named days_to_ship that shows the number of days between the order date and the ship date
add a WHERE clause that retrieves just the orders for March 2018 that have shipped.
Sort the result set by the order_id column in ascending sequence.

**Query:**

```
SELECT order_id, order_date, 
if (order_date IS NOT NULL  , DATE_ADD(order_date, INTERVAL 2 DAY), order_date) AS approx_ship_date,
 DATEDIFF(ship_date, order_date) AS days_to_ship
FROM orders
WHERE order_date BETWEEN '2018-03-01' and '2018-04-01' AND ship_date IS NOT NULL
ORDER BY order_id
```



## ++ Advanced Database Administration



### +++Creating databases, tables and indexes

###### Q.35
Write a script that adds an index to the my_guitar_shop database for the zip code field in the addresses table.  

**Query:**

```
CREATE INDEX findex_ix 
ON addresses(zip_code)
```

###### Q.36
Write a script that adds an index to the my_guitar_shop database for the last_name and first_name fields in the Customers table.  

**Query:**

```
CREATE INDEX name_ix
ON customers(last_name, first_name)
```


###### Q.37
Write an ALTER TABLE statement that adds two new columns to the Products table in the my_guitar_shop database.
Add one column for product price named “product_price” that provides for three digits to the left of the decimal point and two to the right. This column should have a default value of 9.99.
Add one column for the date and time named "new_date_added" that the product was added to the database.

**Query:**

```
ALTER TABLE products
ADD COLUMN (product_price DECIMAL(3,2) DEFAULT 9.99, new_date_added datetime)
```

###### Q.38
Write an ALTER TABLE statement that modifies the Customers table in the my_guitar_shop database. You should be adding a new field called sms_number to store the phone number for text messages.  The field should be designed so it can store a maximum of 20 characters.

**Query:**

```
ALTER TABLE customers 
ADD sms_number VARCHAR(20)
```

### +++Creating views

###### Q.39
Create a view named customer_addresses that shows the shipping and billing addresses for each customer.
This view should return these columns from the Customers table: customer_id, email_address, last_name and first_name.
This view should return these columns from the Addresses table: bill_line1, bill_line2, bill_city, bill_state, bill_zip, ship_line1, ship_line2, ship_city, ship_state, and ship_zip.

**Query:**

```
CREATE VIEW customer_addresses  AS
SELECT c.customer_id, email_address, last_name, first_name, 
line1 AS bill_line1, line2 AS bill_line2, city AS bill_city, state AS bill_state,
zip_code AS bill_zip, line1 AS ship_line1,line2 AS ship_line2, city AS ship_city, state AS ship_state,
zip_code AS ship_zip
FROM customers c JOIN addresses a
ON c.customer_id = a.customer_id
WHERE a.address_id = c.billing_address_id 
```

###### Q.40
Create a view named order_item_products that returns columns from the Orders, Order_Items, and Products tables.
This view should return these columns from the Orders table: order_id, order_date, tax_amount, and ship_date.
This view should return the product_name column from the Products table.
This view should return these columns from the Order_Items table: item_price, discount_amount, final_price (the discount amount subtracted from the item price), quantity, and item_total (the calculated total for the item - quantity * price).

**Query:**

```
CREATE VIEW order_item_products AS
SELECT o.order_id, order_date, tax_amount, ship_date, product_name, item_price, discount_amount, 
(item_price - discount_amount) AS final_price, quantity, ( quantity * (item_price - discount_amount) ) AS item_total 
FROM orders o JOIN order_items i
ON o.order_id = i.order_id
JOIN products p
ON i.product_id = p.product_id
```

###### Q.41
Create a view named product_summary that uses the same table you used in in exercise 2. This view should return summary information about each product.
Each row should include product_name, order_count (the quantity of the product that has been ordered) and order_total (the total sales revenue for the product).

**Query:**

```
CREATE VIEW product_summary AS 
SELECT product_name, COUNT(i.order_id) AS order_count, SUM(((item_price - discount_amount) * quantity)) AS order_total
FROM orders o JOIN order_items i
ON o.order_id = i.order_id
JOIN products p
ON i.product_id = p.product_id
GROUP BY product_name
```

###### Q.42
Create a view named best_products that uses the same table you used in in exercise 3. The view should have the same columns returned. The view should only return the five best selling products by order_total.

**Query:**

```
CREATE VIEW best_products AS 
SELECT product_name, COUNT(i.order_id) AS order_count, SUM(((item_price - discount_amount) * quantity)) AS order_total
FROM orders o JOIN order_items i
ON o.order_id = i.order_id
JOIN products p
ON i.product_id = p.product_id
GROUP BY product_name
ORDER BY order_total DESC LIMIT 5
```

### +++Stored Program Coding

###### Q.43 
Write a script that creates a stored procedure named test. This stored procedure should declare a variable and set it to the count of all products in the Products table. The procedure should return a 1 row, 1 column result set with a column named result.  If the count is greater than or equal to 7, the stored procedure should return the value “The number of products is greater than or equal to 7”. Otherwise, it should return the value “The number of products is less than 7”.

**Query:**


       -- USE my_guitar_shop;
       -- DROP PROCEDURE IF EXISTS test;

       -- DELIMITER //
       CREATE PROCEDURE test()
       BEGIN
       DECLARE count_products INT2;
   
       SELECT COUNT(product_id) INTO count_products 
       FROM products;

       IF count_products >=7 THEN 
       select 'The number of products is greater than or equal to 7' AS RESULT; 
       ELSE 
       select 'The number of products is less than 7' AS RESULT;
       END IF;
       END 
 
       -- DELIMITER ;


###### Q.44
Write a script that creates and calls a stored procedure named test. This procedure should calculate the common factors of the numbers 10 and 20. To find a common factor, you can use the modulo operator (%) to check whether a number that’s less than 10 can be evenly divided into both numbers. The procedure should return a 1 row, 1 column result set with a column named result.  The return result should be a string that includes the common factors like this:
Common factors of 10 and 20: 1 2 5 10

**Query:**

```
        -- USE my_guitar_shop;

        -- DROP PROCEDURE IF EXISTS test;
        -- DELIMITER //
 
           CREATE PROCEDURE test()
           BEGIN

           DECLARE common_factors VARCHAR(200) DEFAULT 'Common factors of 10 and 20:';
           DECLARE i INT DEFAULT 1;

           WHILE i <= 10 DO

            IF 10 % i = 0 THEN
               SET common_factors = CONCAT(common_factors,' ', i);
            END IF;
	             SET i=i+1;
            END WHILE;

            SELECT common_factors AS result;

             END 

             -- DELIMITER ;

```


###### Q.45

Write a script that creates and calls a stored procedure named test. This stored procedure should create a cursor for a result set that consists of the product_name and list_price columns for each product with a list price that’s greater than $700. The rows in this result set should be sorted in descending sequence by list price. The procedure should return a 1 row, 1 column result set with a column named result.  The procedure should set the return value to a string variable that includes the product_name and list price for each product so it looks something like this:
*Gibson SG*,*2517.00*|*Gibson Les Paul*,*1199.00*|
Here, each value is enclosed in asterisk(*), each column is separated by a comma (,) and each row is separated by a pipe character (|).

**Query:**

```

-- USE my_guitar_shop;

 -- DROP PROCEDURE IF EXISTS test;

 -- DELIMITER //
CREATE PROCEDURE test()
BEGIN
DECLARE row_not_found TINYINT DEFAULT FALSE;
DECLARE p_name VARCHAR(200);
DECLARE l_price DECIMAL(9,2);
DECLARE result VARCHAR(200) DEFAULT '';
-- DECLARE list_price1 DECIMAL(9,2);

DECLARE cursor_result CURSOR FOR
SELECT product_name, list_price 
FROM products
WHERE list_price > 700
ORDER BY list_price DESC;

DECLARE CONTINUE HANDLER FOR NOT FOUND
  SET row_not_found =TRUE;
  
 OPEN  cursor_result;

WHILE row_not_found = FALSE DO
  FETCH cursor_result INTO p_name, l_price;
  IF row_not_found = FALSE THEN
  SET result = CONCAT(result,'*',p_name,'*',',','*',l_price,'*','|');
  END IF;
END WHILE;

CLOSE cursor_result;

SELECT result ;

END  -- //
 -- DELIMITER ;
```


###### Q.46

Write a script that creates a stored procedure named test. This procedure should attempt to insert a new category named “Guitars” into the Categories table. The procedure should return a 1 row, 1 column result set with a column named result.  If the insert is successful, the procedure should return the value:
1 row was inserted.
If the update is unsuccessful, the procedure should return the value:
Row was not inserted - duplicate entry.

**Query:**

```
-- USE my_guitar_shop;

-- DROP PROCEDURE IF EXISTS test;

-- DELIMITER //
CREATE PROCEDURE test()
BEGIN

DECLARE row_not_found TINYINT DEFAULT FALSE;
DECLARE Succeed TINYINT DEFAULT FALSE;

-- DECLARE insert_cursor CURSOR FOR
-- SELECT category_id, category_name FROM categories;

DECLARE CONTINUE HANDLER FOR NOT FOUND
  SET row_not_found = TRUE;
DECLARE CONTINUE HANDLER FOR 1062
   SET  succeed = FALSE;
   
-- OPEN   insert_cursor;


-- WHILE row_not_found = FALSE DO

  IF row_not_found = FALSE THEN
    INSERT INTO categories  (category_name)VALUES
    ('Guitars' );
	IF succeed = TRUE THEN
       SELECT '1 row was inserted.' AS result;
    ELSE
       SELECT 'Row was not inserted - duplicate entry.' AS result;
	END IF; 
  END IF; 
   
-- END WHILE;

-- END CURSOR;

 
END; -- //

-- DELIMITER ;
```

### +++Creating Stored Procedures & Functions


###### Q.47

Write a script that creates a stored function named discount_price that calculates the discount price of an item in the Order_Items table (discount amount subtracted from item price). To do that, this function should accept one parameter for the item ID, and it should return the value of the discount price for that item.

**Query:**


    CREATE FUNCTION discount_price
    (
       term_id VARCHAR(80)
    )

    RETURNS  DECIMAL(9,2)
    DETERMINISTIC READS SQL DATA
    BEGIN
    DECLARE discount_value DECIMAL(9,2);

    SELECT item_price - discount_amount INTO discount_value
    FROM order_items
    WHERE item_id = term_id;

    RETURN (discount_value);

    END; 
 


###### Q.48

Write a script that creates a stored procedure named test. This stored procedure should declare a variable and set it to the count of all products in the Products table. The stored procedure should accept an OUT  parameter where a message is passed out of the procedure.  If the count is greater than or equal to 7, the stored procedure should return the value “The number of products is greater than or equal to 7”. Otherwise, it should return the value “The number of products is less than 7”.

**Query:**

```
-- use my_guitar_shop;

-- DROP PROCEDURE IF EXISTS test;

-- DELIMITER //
CREATE PROCEDURE test
(
  OUT message VARCHAR(200)
)

BEGIN

DECLARE cnt_products INT DEFAULT 0;

SELECT COUNT(*) INTO cnt_products FROM products;

IF cnt_products >= 7 THEN
  SET message = 'The number of products is greater than or equal to 7'; 
ELSE 
   SET message = 'The number of products is less than 7';
END IF;   
 
END -- //
-- DELIMITER ;
```


###### Q.49

Write a script that creates a stored procedure named test. This stored procedure should create a cursor for a result set that consists of the product_name and list_price columns for each product with a list price that’s greater than $700. The rows in this result set should be sorted in descending sequence by list price. The stored procedure should accept an OUT  parameter where a message is passed out of the procedure.  Then, the procedure should set the out parameter to a string variable that includes the product_name and list price for each product so it looks something like this:
*Gibson SG*,*2517.00*|*Gibson Les Paul*,*1199.00*|
Here, each value is enclosed in asterisk(*), each column is separated by a     comma (,) and each row is separated by a pipe character (|).

**Query:**

```
CREATE PROCEDURE test
(
OUT message VARCHAR(200)
)

BEGIN

DECLARE row_not_found TINYINT DEFAULT FALSE;
DECLARE p_name VARCHAR(200);
DECLARE l_price DECIMAL(9,2);

DECLARE result VARCHAR(200) DEFAULT '';

DECLARE cursor_result CURSOR FOR
SELECT product_name, list_price 
FROM products
WHERE list_price > 700 
ORDER BY list_price DESC;

DECLARE CONTINUE HANDLER FOR NOT FOUND
  SET row_not_found = TRUE;
  

OPEN  cursor_result;

WHILE row_not_found = FALSE DO
  FETCH cursor_result INTO p_name, l_price;
  IF row_not_found = FALSE THEN
	 SET result = CONCAT(result,'*',p_name,'*',',','*',l_price,'*','|');
  END IF;
END WHILE;

CLOSE cursor_result;

SET message = result;

END;
```

###### Q.50
Write a script that creates a stored procedure named insert_category. Code the procedure so that it adds a new row to the Categories table. To do that, the procedure should have a parameter for the category name.

**Query:**

```
CREATE PROCEDURE insert_category
(
  category_name VARCHAR(200)
)

BEGIN
DECLARE row_not_found TINYINT DEFAULT FALSE;

DECLARE CONTINUE HANDLER FOR NOT FOUND
  SET row_not_found = TRUE;
  
INSERT INTO categories (category_name) VALUES
(category_name);

END ;

```





























