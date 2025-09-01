-- ==============================
-- 🛒 E-commerce SQL Project
-- Database schema, sample data, and queries
-- ==============================

-- Create Database (for MySQL / PostgreSQL; skip if using SQLite)
CREATE DATABASE EcommerceDB;
USE EcommerceDB;

-- ==============================
-- 1. TABLES
-- ==============================

-- Customers Table
CREATE TABLE Customers (
    customer_id INT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    city VARCHAR(50)
);

-- Products Table
CREATE TABLE Products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100),
    price DECIMAL(10,2),
    category VARCHAR(50)
);

-- Orders Table
CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    FOREIGN KEY (customer_id) REFERENCES Customers(customer_id)
);

-- Order Details Table
CREATE TABLE OrderDetails (
    order_detail_id INT PRIMARY KEY,
    order_id INT,
    product_id INT,
    quantity INT,
    FOREIGN KEY (order_id) REFERENCES Orders(order_id),
    FOREIGN KEY (product_id) REFERENCES Products(product_id)
);

-- ==============================
-- 2. SAMPLE DATA
-- ==============================

INSERT INTO Customers VALUES
(1, 'Alice', 'alice@gmail.com', 'Delhi'),
(2, 'Bob', 'bob@gmail.com', 'Mumbai'),
(3, 'Charlie', 'charlie@gmail.com', 'Chennai');

INSERT INTO Products VALUES
(101, 'Laptop', 55000, 'Electronics'),
(102, 'Phone', 20000, 'Electronics'),
(103, 'Shoes', 2500, 'Fashion'),
(104, 'Watch', 3000, 'Accessories');

INSERT INTO Orders VALUES
(201, 1, '2025-08-01'),
(202, 2, '2025-08-03'),
(203, 1, '2025-08-05');

INSERT INTO OrderDetails VALUES
(1, 201, 101, 1),   -- Alice bought 1 Laptop
(2, 201, 103, 2),   -- Alice bought 2 Shoes
(3, 202, 102, 1),   -- Bob bought 1 Phone
(4, 203, 104, 1);   -- Alice bought 1 Watch

-- ==============================
-- 3. SAMPLE QUERIES
-- ==============================

-- Get all orders with customer names
SELECT o.order_id, c.name, o.order_date
FROM Orders o
JOIN Customers c ON o.customer_id = c.customer_id;

-- Find total spending by each customer
SELECT c.name, SUM(p.price * od.quantity) AS total_spent
FROM Customers c
JOIN Orders o ON c.customer_id = o.customer_id
JOIN OrderDetails od ON o.order_id = od.order_id
JOIN Products p ON od.product_id = p.product_id
GROUP BY c.name;

-- Find most sold product
SELECT p.product_name, SUM(od.quantity) AS total_sold
FROM Products p
JOIN OrderDetails od ON p.product_id = od.product_id
GROUP BY p.product_name
ORDER BY total_sold DESC
LIMIT 1;

-- Find customers from Mumbai who placed orders
SELECT DISTINCT c.name, c.city
FROM Customers c
JOIN Orders o ON c.customer_id = o.customer_id
WHERE c.city = 'Mumbai';


