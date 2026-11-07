USE BIKESTORES ;
-- Q1
-- Show all product names along with their brand name. Sort by brand name, 
--then by product name alphabetically.
SELECT 
    p.product_name,
    b.brand_name
FROM [production].[products] p
INNER JOIN [production].[brands] b 
    ON p.brand_id = b.brand_id
ORDER BY 
    b.brand_name ASC, 
    p.product_name ASC;


-- Q2
-- List all products with their category name and list price. 
--Sort by category name, then by price from cheapest to most expensive.
SELECT 
    p.product_name,
    c.category_name,
    p.list_price
FROM [production].[products] p
INNER JOIN [production].[categories] c 
    ON p.category_id = c.category_id
ORDER BY 
    c.category_name ASC, 
    p.list_price ASC;


-- Q3
-- Show all orders with the customer's full name and order date.
--Sort by order date from newest to oldest.
SELECT 
    o.order_id,
    CONCAT(c.first_name, ' ', c.last_name) AS customer_name,
    o.order_date
FROM [sales].[orders] o
INNER JOIN [sales].[customers] c 
    ON o.customer_id = c.customer_id
ORDER BY 
    o.order_date DESC;


-- Q4
-- Display each order item with the product name, quantity, unit price,
--and a computed column called "Line Total" (quantity × list_price). Sort by order ID.
SELECT 
    oi.order_id,
    p.product_name,
    oi.quantity,
    oi.list_price,
    (oi.quantity * oi.list_price) AS [Line Total]
FROM [sales].[order_items] oi
INNER JOIN [production].[products] p 
    ON oi.product_id = p.product_id
ORDER BY 
    oi.order_id ASC;


-- Q5
-- Show each order along with the store name where it was placed and the order date.
--Sort by store name.
SELECT 
    o.order_id,
    st.store_name,
    o.order_date
FROM [sales].[orders] o
INNER JOIN [sales].[stores] st 
    ON o.store_id = st.store_id
ORDER BY 
    st.store_name ASC;

-- Q6
-- Show each order with: order ID, customer full name, store name, 
--and the staff member's full name who handled it.
SELECT 
    o.order_id,
    CONCAT(c.first_name, ' ', c.last_name) AS customer_name,
    st.store_name,
    CONCAT(s.first_name, ' ', s.last_name) AS staff_name
FROM [sales].[orders] o
INNER JOIN [sales].[customers] c 
    ON o.customer_id = c.customer_id
INNER JOIN [sales].[stores] st 
    ON o.store_id = st.store_id
INNER JOIN [sales].[staffs] s 
    ON o.staff_id = s.staff_id;

-- Q7
-- List all products from the brand "Trek" along with their category name and price.
--Sort by price descending. (Use JOIN — do NOT filter by brand_id directly.)
SELECT 
    p.product_name,
    c.category_name,
    p.list_price
FROM [production].[products] p
INNER JOIN [production].[brands] b 
    ON p.brand_id = b.brand_id
INNER JOIN [production].[categories] c 
    ON p.category_id = c.category_id
WHERE b.brand_name = 'Trek'
ORDER BY 
    p.list_price DESC;

-- Q8
-- Find all customers from the state of "NY" who have placed at least one order.
--Show customer full name, city, and order date. (Use JOIN — do not use a subquery.)
SELECT 
    CONCAT(c.first_name, ' ', c.last_name) AS customer_name,
    c.city,
    o.order_date
FROM [sales].[customers] c
INNER JOIN [sales].[orders] o 
    ON c.customer_id = o.customer_id
WHERE c.state = 'NY';


-- Q9
-- Show all completed orders (order_status = 4) from the store "Rowlett Bikes".
--Display order ID, customer full name, and order date.
SELECT 
    o.order_id,
    CONCAT(c.first_name, ' ', c.last_name) AS customer_name,
    o.order_date
FROM [sales].[orders] o
INNER JOIN [sales].[customers] c 
    ON o.customer_id = c.customer_id
INNER JOIN [sales].[stores] st 
    ON o.store_id = st.store_id
WHERE o.order_status = 4 
  AND st.store_name = 'Rowlett Bikes';


-- Q10
-- List ALL customers and any orders they have placed.
--Include customers who have never placed an order (show NULL for order columns).
--Sort by customer ID.
SELECT 
    c.customer_id,
    CONCAT(c.first_name, ' ', c.last_name) AS customer_name,
    o.order_id,
    o.order_date
FROM [sales].[customers] c
LEFT JOIN [sales].[orders] o 
    ON c.customer_id = o.customer_id
ORDER BY 
    c.customer_id ASC;


-- Q11
-- Find all customers who have NEVER placed an order. 
--Show their full name and email.
SELECT 
    CONCAT(c.first_name, ' ', c.last_name) AS customer_name,
    c.email
FROM [sales].[customers] c
LEFT JOIN [sales].[orders] o 
    ON c.customer_id = o.customer_id
WHERE o.order_id IS NULL;

-- Q12
-- List all products and their stock quantity at every store. 
--Include products that have NO stock record at all. Show product name, store ID, and 
--quantity.SELECT
SELECT 
    p.product_name,
    st.store_id,
    st.quantity
FROM [production].[products] p
LEFT JOIN [production].[stocks] st 
    ON p.product_id = st.product_id;

-- Q13
-- Find all products that have NEVER been ordered (no record in order_items).
--Show product name and list price.
SELECT 
    p.product_name,
    p.list_price
FROM [production].[products] p
LEFT JOIN [sales].[order_items] oi 
    ON p.product_id = oi.product_id
WHERE oi.product_id IS NULL;


-- Q14
-- List each staff member along with the full name of their manager.
--Staff with no manager (top-level) should still appear — show NULL for manager name.
SELECT 
    CONCAT(s.first_name, ' ', s.last_name) AS staff_name,
    CONCAT(m.first_name, ' ', m.last_name) AS manager_name
FROM [sales].[staffs] s
LEFT JOIN [sales].[staffs] m 
    ON s.manager_id = m.staff_id;


-- Q15
-- Create a view called vw_bike_catalog that shows product_name, brand_name,
--category_name, model_year, and list_price. Then query it to show only products
--priced over $2,000, sorted by price descending.
-- View Create karne ki query:
SELECT 
    p.product_name,
    b.brand_name,
    c.category_name,
    p.model_year,
    p.list_price
FROM [production].[products] p
INNER JOIN [production].[brands] b 
    ON p.brand_id = b.brand_id
INNER JOIN [production].[categories] c 
    ON p.category_id = c.category_id;



-- Q16
-- BONUS: Create a view called vw_customer_orders showing: customer full name,
--order_id, order_date, store_name, and order_status.
--Then query it to show only orders where the customer city is "New York",
--sorted by order_date.
-- View Create karne ki query:
SELECT 
    CONCAT(c.first_name, ' ', c.last_name) AS customer_name,
    o.order_id,
    o.order_date,
    st.store_name,
    o.order_status,
    c.city
FROM [sales].[orders] o
INNER JOIN [sales].[customers] c 
    ON o.customer_id = c.customer_id
INNER JOIN [sales].[stores] st 
    ON o.store_id = st.store_id;




