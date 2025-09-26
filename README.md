# Task4_ElevateLabs
I have learnt about installation of SQL Workbench, SQLite
I took the help of Chatgpt and Youtube for practicing.
I have created, inserted, performed JOIN functions, WHERE functions and Aggregate functions by understanding them.

I have done the following coding for my practice in SQL Workbench

-- Create Database
CREATE DATABASE ecommerce;
USE ecommerce;

-- Customers Table
CREATE TABLE Customers (
    customer_id INT PRIMARY KEY,
    name VARCHAR(50),
    email VARCHAR(100),
    city VARCHAR(50)
);

-- Products Table
CREATE TABLE Products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(50),
    price DECIMAL(10,2),
    category VARCHAR(50)
);

-- Orders Table
CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    product_id INT,
    quantity INT,
    order_date DATE,
    FOREIGN KEY (customer_id) REFERENCES Customers(customer_id),
    FOREIGN KEY (product_id) REFERENCES Products(product_id)
);

-- Insert Customers (3 values)
INSERT INTO Customers VALUES
(1, 'Alice Johnson', 'alice@example.com', 'New York'),
(2, 'Bob Smith', 'bob@example.com', 'Los Angeles'),
(3, 'Charlie Brown', 'charlie@example.com', 'Chicago');

-- Insert Products (3 values)
INSERT INTO Products VALUES
(101, 'Laptop', 800.00, 'Electronics'),
(102, 'Smartphone', 500.00, 'Electronics'),
(103, 'Desk Chair', 120.00, 'Furniture');

-- Insert Orders (4 values)
INSERT INTO Orders VALUES
(1001, 1, 101, 1, '2025-09-20'),
(1002, 2, 103, 2, '2025-09-21'),
(1003, 3, 102, 1, '2025-09-22'),
(1004, 1, 103, 1, '2025-09-23');

select name, city from Customers; 

select * from Customers 
where city = 'New York' ;

select product_name, price from Products 
WHERE price > 200;
--Total revenue (SUM) per customer
SELECT c.name, SUM(p.price * o.quantity) AS total_spent
FROM Orders o
JOIN Customers c ON o.customer_id = c.customer_id
JOIN Products p ON o.product_id = p.product_id
GROUP BY c.name;

--Avg quantity per product
SELECT p.product_name, AVG(o.quantity) AS avg_quantity_ordered
FROM Orders o
JOIN Products p ON o.product_id = p.product_id
GROUP BY p.product_name;
--Left join (All customers, common value from order)
SELECT c.customer_id, c.name, o.order_id, o.product_id, o.quantity
FROM Customers c
LEFT JOIN Orders o ON c.customer_id = o.customer_id;
--Right Join(All orders , common values from customers)
SELECT o.order_id, o.product_id, o.quantity, c.name, c.city
FROM Orders o
RIGHT JOIN Customers c ON o.customer_id = c.customer_id;

-- Full Join (Customer + Orders)
SELECT c.customer_id, c.name, o.order_id, o.product_id, o.quantity
FROM Customers c
LEFT JOIN Orders o ON c.customer_id = o.customer_id

UNION

SELECT c.customer_id, c.name, o.order_id, o.product_id, o.quantity
FROM Customers c
RIGHT JOIN Orders o ON c.customer_id = o.customer_id;
