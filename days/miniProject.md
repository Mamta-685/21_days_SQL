# 🍕 The Great Pizza Analytics Challenge 📊

## Project Overview

This mini-project focuses on using **SQL** to transform raw sales data for IDC Pizza into actionable business insights. The queries cover fundamentals like filtering and aggregation, and joins.

---

## SQL Queries

### Phase 1: Foundation & Inspection

#### 1. Install IDC_Pizza.dump as IDC_Pizza server
*(No SQL query required for this setup step)*

#### 2. List all unique pizza categories.
```sql
SELECT DISTINCT category FROM pizza_types;
```

#### 3. Display pizza_type_id, name, and ingredients, replacing NULL ingredients with "Missing Data". Show first 5 rows.
```sql

SELECT pizza_type_id, name, COALESCE(ingredients, 'Missing Data') AS ingredients
FROM pizza_types
LIMIT 5;
```

#### 4. Check for pizzas missing a price (IS NULL).
```sql

SELECT * FROM pizzas WHERE price IS NULL;
```

### Phase 2: Filtering & Exploration
#### 1. Orders placed on '2015-01-01'.
```sql

SELECT * FROM orders WHERE date = '2015-01-01';
```

#### 2. List pizzas with price descending.
```sql

SELECT * FROM pizzas ORDER BY price DESC;
```

#### 3. Pizzas sold in sizes 'L' or 'XL'.
```sql

SELECT * FROM pizzas WHERE size IN ('L', 'XL');
```

#### 4. Pizzas priced between $15.00 and $17.00.
```sql

SELECT * FROM pizzas WHERE price BETWEEN 15.00 AND 17.00;
```

#### 5. Pizzas with "Chicken" in the name.
```sql

SELECT * FROM pizza_types WHERE name LIKE '%Chicken%';
```

#### 6. Orders on '2015-02-15' or placed after 8 PM.
```sql

SELECT * FROM orders WHERE date = '2015-02-15' OR time > '20:00:00';
```

### Phase 3: Sales Performance
#### 1. Total quantity of pizzas sold.
```sql

SELECT SUM(quantity) AS total_pizzas_sold FROM order_details;
```

#### 2. Average pizza price.
```sql

SELECT ROUND(AVG(price), 2) AS avg_pizza_price FROM pizzas;
```

#### 3. Total order value per order (JOIN, SUM, GROUP BY).
```sql

SELECT od.order_id, SUM(od.quantity * p.price) AS total_order_value
FROM order_details od
JOIN pizzas p ON od.pizza_id = p.pizza_id
GROUP BY od.order_id;
```

#### 4. Total quantity sold per pizza category (JOIN, GROUP BY).
```sql

SELECT pt.category, SUM(od.quantity) AS total_quantity
FROM order_details od
JOIN pizzas p ON od.pizza_id = p.pizza_id
JOIN pizza_types pt ON p.pizza_type_id = pt.pizza_type_id
GROUP BY pt.category;
```

#### 5. Categories with more than 5,000 pizzas sold (HAVING).
```sql

SELECT pt.category, SUM(od.quantity) AS total_quantity
FROM order_details od
JOIN pizzas p ON od.pizza_id = p.pizza_id
JOIN pizza_types pt ON p.pizza_type_id = pt.pizza_type_id
GROUP BY pt.category
HAVING SUM(od.quantity) > 5000;
```

#### 6. Pizzas never ordered (LEFT/RIGHT JOIN).
```sql

SELECT p.pizza_id, p.size, p.price
FROM pizzas p
LEFT JOIN order_details od ON p.pizza_id = od.pizza_id
WHERE od.pizza_id IS NULL;
```

#### 7. Price differences between different sizes of the same pizza (SELF JOIN).
```sql

SELECT p1.pizza_type_id, p1.size AS size1, p1.price AS price1, p2.size AS size2, p2.price AS price2, (p2.price - p1.price) AS price_diff
FROM pizzas p1
JOIN pizzas p2 ON p1.pizza_type_id = p2.pizza_type_id AND p1.size < p2.size
ORDER BY p1.pizza_type_id;
```
---


