# SQL: Reference Guide

## 1. Database Basics & SELECT Queries
### Database Management Systems (DBMS)
* A DBMS is software used to manage databases.
* **SQL (Structured Query Language)** is used for relational databases where data is stored in tables with relations between them.

### SELECT Basics
* **Aliases (`AS`)**: Rename columns or tables for readability.
  ```sql
  SELECT name AS customer_name FROM customers;
  ```
* **Unique Rows (`DISTINCT`)**: Retrieve only unique rows/values.
  ```sql
  SELECT DISTINCT state FROM customers;
  ```
* **Operator Precedence**: `AND` has higher precedence than `OR`. Use parentheses to group conditions.

### Filtering Data
* **Set Membership (`IN`)**:
  ```sql
  SELECT * FROM customers WHERE country IN ('PK', 'IN', 'US');
  ```
* **Range Filtering (`BETWEEN`)**: inclusive of boundary values.
  ```sql
  SELECT * FROM customers WHERE points BETWEEN 100 AND 300;
  ```

---

## 2. Pattern Matching & Regular Expressions
### Using `LIKE`
* `%`: Represents zero or more characters.
  ```sql
  -- Matches any last name containing "b"
  SELECT * FROM customers WHERE last_name LIKE '%b%';
  ```
* `_`: Represents exactly one character.
  ```sql
  -- Matches last names that are exactly 6 characters long and end with "b"
  SELECT * FROM customers WHERE last_name LIKE '_____b';
  ```

### Using `REGEXP` (Regular Expressions)
More powerful pattern matching than `LIKE`.
* `^`: Matches the beginning of a string. (`^field` starts with "field").
* `$`: Matches the end of a string. (`field$` ends with "field").
* `|`: Logical OR. (`field|mac` searches for either word).
* `[]`: Matches any character inside the brackets.
  * `[gim]e`: Searches for "ge", "ie", or "me".
  * `[a-h]e`: Searches for any character from "a" to "h" followed by an "e".

---

## 3. Sorting, Limiting, and NULLs
### Sorting (`ORDER BY`)
* Sort by specific columns in ascending (`ASC`, default) or descending (`DESC`) order:
  ```sql
  SELECT * FROM customers ORDER BY state DESC, first_name DESC;
  ```
* Sort by column position (avoid in production code):
  ```sql
  SELECT firstname, lastname FROM customers ORDER BY 1, 2;
  ```
* Sort by calculated values:
  ```sql
  SELECT * FROM products ORDER BY (quantity * price) DESC;
  ```

### Handling NULL Values
* Check for null or non-null values:
  ```sql
  SELECT * FROM customers WHERE phone IS NULL;
  SELECT * FROM customers WHERE phone IS NOT NULL;
  ```

### Limiting Results (`LIMIT`)
* **Select top N records**:
  ```sql
  SELECT * FROM customers LIMIT 3;
  ```
* **Pagination (Offset, Limit)**: Skip the first 6 rows and show the next 3.
  ```sql
  SELECT * FROM customers LIMIT 6, 3;
  ```

---

## 4. Joins & Unions
### Joins
* **Explicit Join**: Clearly specifies tables and joining condition with `ON`.
  ```sql
  SELECT * FROM customers c JOIN orders o ON c.id = o.customer_id;
  ```
* **Implicit Join**: Tables listed in `FROM`, conditions specified in `WHERE`.
  > [!WARNING]
  > If you forget the `WHERE` clause in an implicit join, SQL returns a **CROSS JOIN** (Cartesian product).
  ```sql
  SELECT * FROM customers c, orders o WHERE c.id = o.customer_id;
  ```
* **Outer Joins (LEFT / RIGHT JOIN)**:
  * **LEFT JOIN**: Returns all records from the left table, and matching records from the right. If no match, right-table columns are `NULL`.
  * **RIGHT JOIN**: Returns all records from the right table, and matching records from the left.
* **USING Clause**: Simplifies join condition if column names match exactly in both tables.
  ```sql
  SELECT * FROM customers JOIN orders USING (customer_id);
  ```
* **NATURAL JOIN**: Automatically joins tables based on columns with matching names (not recommended due to unpredictability if schema changes).
* **CROSS JOIN**: Combines every record from the first table with every record of the second table.
* **UNION**: Stacks rows vertically between two `SELECT` statements.
  * *Note: The number of columns must match in both select queries. Column names in the final result are taken from the first `SELECT` statement.*
  ```sql
  SELECT * FROM table1 WHERE points > 100
  UNION
  SELECT * FROM table1 WHERE status = 'Active';
  ```

---

## 5. Data Types
* **CHAR vs VARCHAR**:
  * `CHAR(n)`: Fixed length (e.g., `CHAR(50)` always uses 50 characters, padding with spaces if needed).
  * `VARCHAR(n)`: Dynamic length (stores up to `n` characters using only the space required).
* **Numeric**: `TINYINT` (1 byte: -128 to 127), `SMALLINT`, `MEDIUMINT`, `INT`, `BIGINT`, `DECIMAL(precision, scale)` (e.g., `DECIMAL(9,2)` means 9 total digits, 2 after decimal), `FLOAT`, `DOUBLE`.
* **Boolean**: Evaluates to `TRUE` (1) or `FALSE` (0).
* **ENUM**: `ENUM('Small', 'Medium', 'Large')`. 
  > [!TIP]
  > Changing enum values is expensive since SQL drops the table to redefine it. Instead, use a lookup table for sizes.
* **DateTime**: `DATE`, `TIME`, `DATETIME` (8 bytes, ranges beyond 2038), `TIMESTAMP` (4 bytes, maxes out in 2038), `YEAR`.
* **BLOB**: `TINYBLOB`, `BLOB`, `MEDIUMBLOB`, `LONGBLOB` (Binary Large Objects). Avoid storing blobs directly in databases; store file paths instead.
* **JSON**: Stores structured key-value pairs.
  ```sql
  UPDATE products SET properties = '{"weight": 10}' WHERE id = 1;
  SELECT id, properties->'$.weight' FROM products;
  ```

---

## 6. SQL Functions
### Numeric Functions
* `ROUND(val, decimals)` (e.g., `ROUND(5.73, 1)` = 5.7)
* `TRUNCATE(val, decimals)` (e.g., `TRUNCATE(5.731, 1)` = 5.7)
* `CEILING()`, `FLOOR()`, `ABS()`, `RAND()`

### String Functions
* `LENGTH()`, `UPPER()`, `LOWER()`
* `LTRIM()`, `RTRIM()`, `TRIM()`
* `LEFT(str, length)` (e.g., `LEFT('kindergarten', 4)` = 'kind')
* `RIGHT()`, `SUBSTRING(str, start, len)` (e.g., `SUBSTRING('abcde', 2, 4)` = 'bcde')
* `LOCATE(sub, str)` (e.g., `LOCATE('n', 'kindergarten')` = 3)
* `REPLACE(str, from, to)` (e.g., `REPLACE('kindergarten', 'garten', 'garden')` = 'kindergarden')
* `CONCAT()`

### Date and Time Functions
* `NOW()`, `CURDATE()`, `CURTIME()`
* `YEAR()`, `MONTH()`, `DAY()`, `HOUR()`, `MINUTE()`, `SECOND()`
* `DAYNAME()`, `MONTHNAME()`
* `EXTRACT(unit FROM datetime)` (e.g., `EXTRACT(DAY FROM NOW())`)
* `DATE_FORMAT(datetime, format)` (e.g., `DATE_FORMAT(NOW(), '%m %d %Y')`)
* `TIME_FORMAT(datetime, format)` (e.g., `TIME_FORMAT(NOW(), '%h')`)
* `DATE_ADD(datetime, INTERVAL value unit)` (e.g., `DATE_ADD(NOW(), INTERVAL 1 YEAR)`)
* `DATE_SUB()`, `DATEDIFF(date1, date2)`, `TIME_TO_SEC()`

### Conditional / Null Handling Functions
* **`IFNULL`**: Fallback value if primary value is NULL.
  ```sql
  SELECT IFNULL(shipper_id, 'Not Assigned') FROM orders;
  ```
* **`COALESCE`**: Returns the first non-null value in a list of columns/arguments.
  ```sql
  SELECT COALESCE(shipper_id, comments, 'Not Assigned') FROM orders;
  ```
* **`IF`**: Inline conditional.
  ```sql
  SELECT IF(YEAR(order_date) = YEAR(NOW()), 'Active', 'Not Active') FROM orders;
  ```
* **`CASE`**: Multi-conditional statement.
  ```sql
  SELECT CASE 
      WHEN YEAR(order_date) = YEAR(NOW()) THEN 'active'
      WHEN YEAR(order_date) = YEAR(NOW()) - 1 THEN 'last year'
      WHEN YEAR(order_date) < YEAR(NOW()) THEN 'archived'
      ELSE 'Future'
  END FROM orders;
  ```

---

## 7. Views
A view is a virtual table representing the result of a `SELECT` statement. Views simplify query complexity, restrict direct table access, and minimize impact of database changes.
* **Create a View**:
  ```sql
  CREATE VIEW sales_by_client AS 
  SELECT * FROM clients c JOIN invoices i USING (client_id);
  ```
* **Replace or Drop View**:
  ```sql
  CREATE OR REPLACE VIEW sales_by_client AS ...;
  DROP VIEW sales_by_client;
  ```
* **Updatable Views**: You can perform `INSERT`/`UPDATE`/`DELETE` on a view if it does **not** contain:
  * `DISTINCT`
  * Aggregate functions (`SUM`, `MAX`, `AVG`, etc.)
  * `GROUP BY` or `HAVING`
  * `UNION`
* **`WITH CHECK OPTION`**: Prevents view updates or inserts that would make the updated rows disappear from the view's query result.
  ```sql
  CREATE OR REPLACE VIEW hr_employees AS 
  SELECT * FROM employees WHERE department = 'HR'
  WITH CHECK OPTION;
  -- If you attempt to update a row to department = 'IT', it will throw an error.
  ```

---

## 8. Stored Procedures and Functions
Used to organize SQL code, run queries faster (execution plans are cached), and improve security.

### Stored Procedures
```sql
DELIMITER $$
CREATE PROCEDURE get_clients(p_state CHAR(2))
BEGIN 
    IF p_state IS NULL THEN 
        SET p_state = 'CA'; 
    END IF;
    SELECT * FROM clients c WHERE c.state = p_state;
END$$
DELIMITER ;
```
* **Calling & Dropping**:
  ```sql
  CALL get_clients('CA');
  DROP PROCEDURE IF EXISTS get_clients;
  ```
* **Parameter Validation**:
  ```sql
  IF amount <= 0 THEN 
      SIGNAL SQLSTATE '22003' SET MESSAGE_TEXT = 'invalid payment amount'; 
  END IF;
  ```
* **Output Parameters & Variables**:
  ```sql
  -- Define: OUT count INT
  -- Set: SELECT COUNT(*) INTO count FROM clients;
  -- Call: CALL get_client_count(@count); SELECT @count;
  -- Local variable inside BEGIN...END: DECLARE risk_factor INT;
  ```

### Stored Functions
Unlike procedures, functions must return exactly one value.
```sql
CREATE FUNCTION get_risk_factor(id INT)
RETURNS INT
DETERMINISTIC   -- Always returns same output for same inputs
READS SQL DATA  -- Reads tables but doesn't modify them
BEGIN
    DECLARE risk_factor INT;
    -- logic to compute risk factor
    RETURN risk_factor;
END
```

---

## 9. Triggers and Events
### Triggers
Triggers are automatically executed before or after a database modification (insert, update, delete) to enforce business rules or maintain consistency.
```sql
DELIMITER $$
CREATE TRIGGER payments_after_insert
    AFTER INSERT ON payments
    FOR EACH ROW
BEGIN
    UPDATE invoices 
    SET payment_total = payment_total + NEW.amount
    WHERE invoice_id = NEW.invoice_id;	
END$$
DELIMITER ;
```
* Use `NEW` to access columns of the inserted row; use `OLD` during updates/deletes.
* View Triggers: `SHOW TRIGGERS;`
* **Auditing**: Triggers can log modifications (user, timestamp, actions) to an audit table.

### Events
Events are scheduled tasks that execute SQL code at specific times.
```sql
CREATE EVENT delete_expired_sessions
ON SCHEDULE EVERY 1 HOUR
DO BEGIN
    DELETE FROM sessions WHERE expiry < NOW();
END
```
* Disable Event: `ALTER EVENT delete_expired_sessions DISABLE;`

---

## 10. Transactions & Concurrency
### Transactions
A transaction represents a single unit of work containing multiple statements.
* **ACID Properties**:
  * **Atomicity**: All statements succeed, or none do (rollback).
  * **Consistency**: DB remains in a valid state.
  * **Isolation**: Concurrent transactions are isolated from one another.
  * **Durability**: Committed changes are permanent.
```sql
START TRANSACTION;
INSERT INTO orders (customer_id) VALUES (1);
INSERT INTO order_items (order_id, product_id) VALUES (LAST_INSERT_ID(), 2);
COMMIT; -- Or ROLLBACK; if there's an error
```

### Concurrency Problems
* **Lost Updates**: Two transactions update the same row concurrently, and the latter overwrites the former. (SQL locks rows to prevent this).
* **Dirty Reads**: Transaction A reads changes from Transaction B that haven't been committed yet.
  * *Fix: Use isolation level `READ COMMITTED`.*
* **Non-repeating Reads (Inconsistent Reads)**: Reading the same data twice in one transaction yields different values because another transaction modified it.
  * *Fix: Use isolation level `REPEATABLE READ`.*
* **Phantom Reads**: New records appear in a query result when run a second time in the same transaction because another transaction inserted them.
  * *Fix: Use isolation level `SERIALIZABLE` (most isolated, slowest performance).*
* **Changing Isolation Level**:
  ```sql
  SET SESSION TRANSACTION ISOLATION LEVEL SERIALIZABLE;
  ```

### Deadlocks
* Occurs when two transactions block each other indefinitely by holding locks the other transaction needs.
* Keep transactions short and update tables in the same order across different transactions to minimize deadlocks.

---

## 11. Data Modeling & Normalization
### Modeling Stages
1. **Conceptual Model**: Business concepts and relationships (ER Diagram or UML).
2. **Logical Model**: Technology-independent tables, columns, relationships ($1-1, 1-N, N-N$, junction tables).
3. **Physical Model**: Implementation in a specific DBMS (e.g., MySQL), mapping data types, FK actions (`CASCADE`, `RESTRICT`, `SET NULL`, `NO ACTION`).

### Database Normalization
* **First Normal Form (1NF)**: Each cell contains a single value; no repeating columns (e.g., don't use columns `tag1`, `tag2`; create a separate `tags` table).
* **Second Normal Form (2NF)**: Meets 1NF, and each table describes a single entity where every column is dependent on the primary key.
* **Third Normal Form (3NF)**: Meets 2NF, and columns must not be transitively dependent (no derived or calculated columns like `total_price` if `qty` and `price` exist).

---

## 12. Altering Tables, Storage Engines & Character Sets
### Altering Tables
* **Add/Modify/Drop Columns**:
  ```sql
  ALTER TABLE customers ADD last_name VARCHAR(50) AFTER first_name;
  ALTER TABLE customers MODIFY COLUMN first_name VARCHAR(55) DEFAULT '';
  ALTER TABLE customers DROP COLUMN first_name;
  ```
* **Add Foreign Key Constraints**:
  ```sql
  ALTER TABLE orders ADD FOREIGN KEY fk_orders_customers (customer_id) 
  REFERENCES customers (customer_id) ON UPDATE CASCADE ON DELETE CASCADE;
  ```

### Character Sets
* Character sets map characters to binary representations (e.g., `utf8`, `latin1`). 
* Changing database character set:
  ```sql
  ALTER DATABASE dbname CHARACTER SET latin1;
  ```

### Storage Engines
* Storage engines determine how MySQL stores data and which features (like transactions or foreign keys) are supported.
* **InnoDB**: Supports transactions, row-level locking, and foreign keys. (Default & recommended).
* **MyISAM**: High read speed, table-level locking, no transactions.
  ```sql
  ALTER TABLE customers ENGINE = InnoDB;
  ```

---

## 13. Indexing
Indexes are sorted representations of columns stored in memory (typically in B-Trees) with pointers to the disk location. They speed up searches at the cost of disk space and write speed.
* **Create Index**:
  ```sql
  CREATE INDEX idx_state ON customers (state);
  SHOW INDEXES IN customers;
  ```
* **Cardinality**: Number of unique values in an index. High cardinality yields better index performance. Update statistics with:
  ```sql
  ANALYZE TABLE customers;
  ```
* **Prefix Indexes**: Indexing part of a string column (e.g., first 10 characters) to save memory.
* **Full-Text Indexes**: Used for search engine functionality; tokenizes columns to build an inverted index.
  ```sql
  CREATE FULLTEXT INDEX idx_title_body ON posts (title, body);
  SELECT * FROM posts WHERE MATCH(title, body) AGAINST('react redux');
  ```
* **Composite Indexes**: Index on multiple columns.
  * *Rule 1*: Order by frequency of query use.
  * *Rule 2*: Put columns with higher cardinality first.
  * SQL allows a maximum of 16 columns in a composite index.
* **Covering Index**: An index that contains all columns required by a query (including the primary key), allowing the DB to resolve the query entirely without accessing the underlying table.
* **Redundancy Callout**: Do not create redundant indexes like having both `(a, b)` and `(a)` indexes, since the composite index already covers query prefixes starting with `a`.

---

## 14. User Management & Security
```sql
-- Create User
CREATE USER john@127.0.0.1 IDENTIFIED BY 'password';
-- View Users
SELECT * FROM mysql.user;
-- Change Password
SET PASSWORD FOR john@127.0.0.1 = 'newpassword';
-- Grant Permissions
GRANT SELECT, INSERT, UPDATE, DELETE, EXECUTE ON sql_store.* TO john@127.0.0.1;
GRANT ALL ON *.* TO john@127.0.0.1;
-- View Grants
SHOW GRANTS FOR john@127.0.0.1;
-- Revoke Permissions
REVOKE INSERT, UPDATE ON sql_store.* FROM john@127.0.0.1;
-- Drop User
DROP USER john@127.0.0.1;
```

---

## Summary
This note covers:
* SQL Query basics, filtering (`IN`, `BETWEEN`), and pattern matching (`LIKE`, `REGEXP`).
* Sorting and limiting results.
* Merging data using explicit/implicit Joins, Outer Joins, and Unions.
* MySQL data types (Strings, Numerics, DateTimes, JSON) and functions.
* Creating Views and utilizing `WITH CHECK OPTION`.
* Stored Procedures, custom Stored Functions, Triggers, and Events.
* ACID transaction properties, concurrency issues, isolation levels, and deadlocks.
* Conceptual/logical/physical models and normalization rules (1NF, 2NF, 3NF).
* Database maintenance: table altering, character sets, storage engines, indexing (B-Trees, Composite, covering indexes).
* User access control and permission management (`GRANT`/`REVOKE`).
