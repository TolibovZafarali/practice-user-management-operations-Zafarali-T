DROP TABLE IF EXISTS customers;
DROP TABLE IF EXISTS orders;

Here I'm dropping the table if they exist, so whenever I run the query, it will not have conflicts with the existing tables.

CREATE TABLE customers (
    id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50),
    last_name VARCHAR(50)
);

CREATE TABLE orders (
    id INT PRIMARY KEY,
    customer_id INT NULL,
    order_date DATE,
    total_amount DECIMAL(10 , 2 ),
    FOREIGN KEY (customer_id)
        REFERENCES customers (id)
);

Here, I'm creating two tables: customers and orders. And I'm adding a column "customer_id" to the orders so it can be easy to JOIN two tables later.

INSERT INTO customers (id, first_name, last_name) VALUES
	(1, 'John', 'Doe'),
	(2, 'Jane', 'Smith'),
	(3, 'Alice', 'Smith'),
	(4, 'Bob', 'Brown');

INSERT INTO orders (id, customer_id, order_date, total_amount) VALUES
	(1, 1, '2023-01-01', 100.00),
	(2, 1, '2023-02-01', 150.00),
	(3, 2, '2023-01-01', 200.00),
	(4, 3, '2023-04-01', 250.00),
	(5, 3, '2023-04-01', 300.00),
	(6, NULL, '2023-04-01', 100.00);

Here, I'm inserting the values that were provided to us from the lesson.

SELECT DISTINCT
    orders.id,
    orders.order_date,
    customers.first_name,
    customers.last_name,
    orders.total_amount
FROM
    customers
        INNER JOIN
    orders ON orders.customer_id = customers.id;

Here, I'm using the JOIN statement to combine data from the two tables above.

SELECT 
    orders.customer_id,
    COUNT(orders.id),
    customers.first_name,
    customers.last_name
FROM
    orders
        INNER JOIN
    customers ON orders.customer_id = customers.id
GROUP BY orders.customer_id;

Here, I'm using a GROUP BY query with aggregate function, COUNT.

SELECT 
    orders.order_date, SUM(total_amount) AS total_spent
FROM
    orders
GROUP BY orders.order_date
HAVING SUM(total_amount) > 200;

SELECT 
    orders.order_date,
    customers.first_name,
    customers.last_name,
    customers.id
FROM
    orders
        INNER JOIN
    customers ON orders.customer_id = customers.id
WHERE
    customers.last_name = 'Doe';

In these queries, I'm applying both WHERE and HAVING clauses.

SELECT 
    id, order_date, total_amount
FROM
    orders
WHERE
    total_amount <= (SELECT 
            AVG(total_amount)
        FROM
            orders);

SELECT 
    id, order_date, total_amount, customer_id
FROM
    orders
WHERE
    customer_id IN (SELECT 
            id
        FROM
            customers
        WHERE
            last_name = 'Smith');

And finally, in these tables, I'm representing how to use SubQueries.