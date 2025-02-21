# Intermediate SQL

## SQL Best practices

Simon Holywells SQL Style Guide: https://www.sqlstyle.guide/

- Standard practice: Keyword capitalization & New lines

- When creating a table, a SQL mistake is including spaces in a field name. To query that table, we'll need to enclose the field name in double-quotes to indicate that, despite being two words, the name refers to just one field.
  - Example: release year instead of release_year
        ```SQL
        SELECT title, "release year", country
        FROM films
        LIMIT 3;
        ```

---
<details>
<summary>SQL Order of Execution</summary>

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

---

## Debugging SQL

- Common
  - Misspelling
  - Incorrect capitalization
  - Incorrect or missing punctuation 
- Comma Errors: Missing comma's
- Misspelt Keyword Errors

--- 

## Selecting Data

1. COUNT(): Returns the number of records with a value in a field
   1. COUNT(field_name): Counts the values in a field | `Note: Includes only non-missing values`
   2. COUNT(*): Counts records in a field. | `Note: Includes missing-values`

2. DISTINCT: Selects all the unique values from a field
3. COUNT(DISTINCT field_name): Counts the number of unique values in a field

---

<details>
<summary>SQL Comparison Operators</summary>
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

---

<details>
<summary>SQL Logical Operators</summary>

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
  
---

<details>
<summary>SQL Aggregate Functions</summary>

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
