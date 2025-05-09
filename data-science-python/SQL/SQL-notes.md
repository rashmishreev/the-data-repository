# Intermediate SQL

### SQL Best practices

Simon Holywells SQL Style Guide: https://www.sqlstyle.guide/

- **Standard practice:** Keyword capitalization & New lines

- When creating a table, an SQL mistake is including spaces in a field name. To query that table, we'll need to enclose the field name in double-quotes to indicate that, despite being two words, the name refers to just one field.
  - *Example:* `release year` instead of `release_year`

```sql
SELECT title, "release year", country
FROM films
LIMIT 3;
```

### Common errors in SQL

- Misspelling fields, Incorrect capitalization, Incorrect or missing punctuation 
- Missing comma's
- Misspelt Keyword Errors



## Selecting Data

1. COUNT(): Returns the number of records with a value in a field
   1. COUNT(field_name): Counts the values in a field | `Note: Includes only non-missing values`
   2. COUNT(*): Counts records in a field. | `Note: Includes missing-values`

2. DISTINCT: Selects all the unique values from a field
3. COUNT(DISTINCT field_name): Counts the number of unique values in a field 
   - COUNT() `does not return the count of distinct values by default`; it counts all non-NULL values. To count distinct values, COUNT(DISTINCT column_name) should be used.
4. SUM() returns the summation of all non-NULL values.
5. AVG() does not return the average of distinct values by default. It returns the average of all non-NULL values unless DISTINCT is specified.
6. MAX() returns the maximum value for the specified column.

<details>

<summary>SQL Order of Execution</summary>
<br> <!-- Adds space between summary and content -->

1. **FROM**      → Identify the source tables
2. **JOIN**      → Combine tables based on conditions
3. **WHERE**     → Filter rows before grouping
4. **GROUP BY**  → Aggregate data into groups
5. **HAVING**    → Filter groups after aggregation
6. **SELECT**    → Choose columns or expressions
7. **DISTINCT**  → Remove duplicate rows
8. **ORDER BY**  → Sort the result set
9. **LIMIT / OFFSET** → Restrict number of rows

*Example Query Execution Order*

```sql
SELECT DISTINCT column_name  -- Step 6 & 7
FROM table_name              -- Step 1
JOIN another_table           -- Step 2
ON table_name.id = another_table.id  
WHERE condition              -- Step 3
GROUP BY column_name         -- Step 4
HAVING condition             -- Step 5
ORDER BY column_name         -- Step 8
LIMIT 10 OFFSET 5;           -- Step 9
```
</details>


<details>
<summary>SQL Comparison Operators</summary>
<br> <!-- Adds space between summary and content -->

SQL comparison operators are used in the `WHERE` clause to filter records based on conditions.

| Operator | Description | Example |
|----------|------------|---------|
| `=` | Equal to | `SELECT * FROM users WHERE age = 30;` |
| `<>` or `!=` | Not equal to | `SELECT * FROM users WHERE age <> 30;` |
| `>` | Greater than | `SELECT * FROM users WHERE age > 30;` |
| `<` | Less than | `SELECT * FROM users WHERE age < 30;` |
| `>=` | Greater than or equal to | `SELECT * FROM users WHERE age >= 30;` |
| `<=` | Less than or equal to | `SELECT * FROM users WHERE age <= 30;` |
| `BETWEEN` | Within a range (inclusive) | `SELECT * FROM users WHERE age BETWEEN 20 AND 30;` |
| `IN` | Matches any value in a list | `SELECT * FROM users WHERE country IN ('USA', 'Canada', 'UK');` |
| `NOT IN` | Excludes values in a list | `SELECT * FROM users WHERE country NOT IN ('USA', 'Canada');` |
| `LIKE` | Pattern matching with wildcards | `SELECT * FROM users WHERE name LIKE 'A%';` |
| `NOT LIKE` | Excludes pattern match | `SELECT * FROM users WHERE name NOT LIKE 'A%';` |
| `IS NULL` | Checks for NULL values | `SELECT * FROM users WHERE email IS NULL;` |
| `IS NOT NULL` | Checks for non-NULL values | `SELECT * FROM users WHERE email IS NOT NULL;` |

**Wildcard Characters for `LIKE` Operator**  
- `%` → Represents zero, one, or multiple characters (`'A%'` → Starts with 'A')  
- `_` → Represents a single character (`'_a%'` → Second letter is 'a')  
</details>



<details>
<summary>SQL Logical Operators</summary>

<br> <!-- Adds space between summary and content -->

Logical operators are used in SQL to combine multiple conditions in the `WHERE` clause.

| Operator  | Description | Example |
|-----------|------------|---------|
| `ALL` | Returns **TRUE** if all subquery values meet the condition | `SELECT * FROM employees WHERE salary > ALL (SELECT salary FROM interns);` → Retrieves employees with a salary **higher than all interns**. |
| `AND` | Returns **TRUE** if **all conditions** are met | `SELECT * FROM users WHERE age > 18 AND country = 'USA';` → Retrieves users **older than 18** **AND** from the USA. |
| `ANY` | Returns **TRUE** if **any subquery value** meets the condition | `SELECT * FROM employees WHERE salary > ANY (SELECT salary FROM interns);` → Retrieves employees earning **more than at least one intern**. |
| `BETWEEN` | Returns **TRUE** if a value is **within a range** (inclusive) | `SELECT * FROM orders WHERE order_date BETWEEN '2023-01-01' AND '2023-12-31';` → Retrieves orders placed in **2023**. |
| `EXISTS` | Returns **TRUE** if the subquery **returns at least one record** | `SELECT * FROM customers WHERE EXISTS (SELECT 1 FROM orders WHERE customers.id = orders.customer_id);` → Retrieves customers **who have placed orders**. |
| `IN` | Returns **TRUE** if a value is **in a list** | `SELECT * FROM users WHERE country IN ('USA', 'Canada', 'UK');` → Retrieves users **from USA, Canada, or the UK**. |
| `LIKE` | Returns **TRUE** if a value matches a **pattern** | `SELECT * FROM users WHERE name LIKE 'A%';` → Retrieves users **whose names start with 'A'**. |
| `NOT` | Returns **TRUE** if a condition is **NOT TRUE** | `SELECT * FROM users WHERE NOT country = 'USA';` → Retrieves users **who are not from the USA**. |
| `OR` | Returns **TRUE** if **at least one condition** is met | `SELECT * FROM users WHERE age > 30 OR city = 'New York';` → Retrieves users **older than 30 OR from New York**. |
| `SOME` | Same as `ANY` - Returns **TRUE** if **any subquery value** meets the condition | `SELECT * FROM employees WHERE salary > SOME (SELECT salary FROM interns);` → Retrieves employees earning **more than at least one intern**. |


### **Using Logical Operators Together**

#### Example 1: Combining `AND`, `OR`, and `IN`
```sql
SELECT * FROM users 
WHERE (age > 18 AND country IN ('USA', 'Canada')) 
   OR (age < 18 AND country = 'UK');
```

##### Example 2: Using `AND` and `OR`
```sql
SELECT * FROM users 
WHERE (age > 25 AND country = 'USA') 
   OR (age < 18 AND country = 'Canada');
```

#### Example 3: Using `BETWEEN` and `AND`
```SQL
SELECT * FROM employees 
WHERE age BETWEEN 25 AND 40 
AND (department = 'HR' OR department = 'Finance');
```
**Best Practices**
- Use parentheses () to group conditions and improve readability.
- Avoid unnecessary OR conditions as they may slow down queries.
- Prefer IN instead of multiple OR conditions for better performance.

</details>
  


<details>
<summary>SQL Aggregate Functions</summary>

<br> <!-- Adds space between summary and content -->

SQL **Aggregate Functions** perform calculations on multiple rows of data and return a **single** result. These functions are commonly used with the `GROUP BY` clause.

### **List of SQL Aggregate Functions**

| Function  | Description | Example |
|-----------|------------|---------|
| `COUNT()` | Returns the number of rows that match a condition | `SELECT COUNT(*) FROM users;` → Returns total users. |
| `SUM()` | Returns the total sum of a numeric column | `SELECT SUM(salary) FROM employees;` → Returns the total salary of all employees. |
| `AVG()` | Returns the average value of a numeric column | `SELECT AVG(price) FROM products;` → Returns the average product price. |
| `MIN()` | Returns the smallest value in a column | `SELECT MIN(age) FROM users;` → Returns the youngest user's age. |
| `MAX()` | Returns the largest value in a column | `SELECT MAX(salary) FROM employees;` → Returns the highest salary. |
| `GROUP_CONCAT()` *(MySQL only)* | Concatenates values into a single string | `SELECT GROUP_CONCAT(name) FROM users;` → Returns names as a single string. |
| `STRING_AGG()` *(PostgreSQL, SQL Server)* | Concatenates values with a delimiter | `SELECT STRING_AGG(name, ', ') FROM users;` → Returns names separated by commas. |

#### **Using Aggregate Functions with `GROUP BY`**

#### Example 1: Counting Users per Country
```sql
SELECT country, COUNT(*) AS user_count 
FROM users 
GROUP BY country;
```
</details>

---

## SQL `ROUND()` Function

The `ROUND()` function in SQL is used to **round numeric values** to a specified number of decimal places.

### **Syntax**
```sql
ROUND(number, decimal_places)
```

#### **Parameters and Behavior of `ROUND()`**

| Parameter Value | Description | Example | Output |
|----------------|------------|---------|--------|
| `ROUND(number, N)` | Rounds `number` to `N` decimal places | `ROUND(123.456, 2)` | `123.46` |
| `ROUND(number, 0)` | Rounds `number` to the nearest whole number | `ROUND(123.456, 0)` | `123` |
| `ROUND(number, -1)` | Rounds `number` to the nearest **multiple of 10** | `ROUND(123.456, -1)` | `120` |
| `ROUND(number, -2)` | Rounds `number` to the nearest **multiple of 100** | `ROUND(123.456, -2)` | `100` |
| `ROUND(number, -3)` | Rounds `number` to the nearest **multiple of 1000** | `ROUND(56789, -3)` | `57000` |
| `ROUND(-number, N)` | Rounds negative numbers similarly to positive ones | `ROUND(-123.456, 2)` | `-123.46` |


#### **Key Notes**
- If `N` is **positive**, rounding happens to the **right** of the decimal point.
- If `N` is **zero**, the number is rounded to the nearest **whole number**.
- If `N` is **negative**, rounding happens **left** of the decimal point (to tens, hundreds, thousands, etc.).

<details>
<summary> JOINS </summary>

</br>

1. **INNER JOIN**

Returns only the matching rows between two tables.

```sql
SELECT a.*, b.*
FROM tableA a
INNER JOIN tableB b
ON a.id = b.id;
```

2. **LEFT JOIN (LEFT OUTER JOIN)**

Returns all rows from the left table, and matching rows from the right table. If no match, NULLs are returned.

```sql
SELECT a.*, b.*
FROM tableA a
LEFT JOIN tableB b
ON a.id = b.id;
```
3. **RIGHT JOIN (RIGHT OUTER JOIN)**

Returns all rows from the right table, and matching rows from the left table. If no match, NULLs are returned.

```sql
SELECT a.*, b.*
FROM tableA a
RIGHT JOIN tableB b
ON a.id = b.id;
```

4. **FULL JOIN (FULL OUTER JOIN)**

Returns all rows from both tables. If no match, NULLs are returned for missing values.

```sql
SELECT a.*, b.*
FROM tableA a
FULL JOIN tableB b
ON a.id = b.id;
```

5. **CROSS JOIN**

Returns the Cartesian product of both tables (every row in A joins with every row in B).

```sql
SELECT a.*, b.*
FROM tableA a
CROSS JOIN tableB b;
```

*Results in (rows in A) × (rows in B) records.*

6. **SELF JOIN**

A table joins with itself.

```sql
SELECT a.*, b.*
FROM tableA a
JOIN tableA b
ON a.some_column = b.some_column;
```
*Used for hierarchical data like employees & managers.*

7. **ANTI JOIN (Using NOT EXISTS or NOT IN)**
   
Returns rows from the left table where there is no match in the right table.
-  It can be particularly useful for identifying whether an incorrect number of records appears in a join.

```sql
SELECT a.*
FROM tableA a
WHERE NOT EXISTS (
    SELECT 1 FROM tableB b WHERE a.id = b.id
);
```
*Finds unmatched records.*

8. SEMI-JOIN
   
Returns rows from the left table where a match exists in the right table, but it does not return columns from the right table.

- Similar to an INNER JOIN, but only returns columns from the left table.
- Often used in EXISTS or IN subqueries.
- Helps improve query performance when you only need to check for existence.

Example 1: Using `EXISTS` (Preferred Way)
```sql
SELECT customer_id, customer_name 
FROM customers c
WHERE EXISTS (
    SELECT 1 
    FROM orders o
    WHERE o.customer_id = c.customer_id
);
```
Example 2: Using `IN`
```sql
SELECT customer_id, customer_name 
FROM customers
WHERE customer_id IN (SELECT customer_id FROM orders);
```
</details>

<details>
<summary>SQL SET OPERATIONS</summary>
</br>


**Difference between `INNER JOIN` and `INTERSECT`**

| Feature         | `INTERSECT`                          | `INNER JOIN`                        |
|---------------|----------------------------------|--------------------------------|
| **Purpose**   | Finds common rows between two result sets | Combines matching rows from two tables based on a condition |
| **Columns**   | Both queries must have the same number of columns and matching data types | Can join tables with different structures using a join condition |
| **Duplicates** | Removes duplicates (returns distinct values) | Keeps all matching records (including duplicates) |
| **Condition**  | Implicit (compares all columns in both queries) | Explicit (defined using `ON` clause) |
| **Use Case**   | Finding identical rows in two queries | Combining related data from multiple tables |

</br>

**Comparison between `Anti-Join` and `EXCEPT`**

| Feature        | Anti-Join                          | EXCEPT                                |
|---------------|----------------------------------|--------------------------------------|
| **Purpose**   | Returns rows from one table that do not have a match in another table | Returns rows from the first query that are not in the second query |
| **Implementation** | Uses `LEFT JOIN` with `WHERE other_table.column IS NULL` | Uses `EXCEPT` keyword between two SELECT statements |
| **Condition**  | Explicit condition in `ON` clause | Implicitly compares all columns in both queries |
| **Columns**    | Can have different structures and columns | Both queries must have the same number of columns and matching data types |
| **Duplicates** | Keeps all non-matching records | Removes duplicates (returns distinct values) |
| **Use Case**   | Finding records in one table that have no match in another | Finding distinct rows in one query that are missing in another |

</br>

*Example of Anti-Join (Using LEFT JOIN)*
```sql
SELECT t1.*
FROM table1 t1
LEFT JOIN table2 t2 ON t1.id = t2.id
WHERE t2.id IS NULL;
```
💡 Finds records in table1 that do not exist in table2.

*Example of EXCEPT*
```sql
SELECT id FROM table1
EXCEPT
SELECT id FROM table2;
```
💡 Finds unique IDs in table1 that do not exist in table2 (removes duplicates).

</details>

<details>
<summary>SQL Subqueries</summary>
</br>

1. Subquery using `ANTI-JOIN` & `SEMI-JOIN`
2. Subquery in `WHERE` clause
3. Subquery in `SELECT` clause
4. Subquery in `FROM` clause
</details>
