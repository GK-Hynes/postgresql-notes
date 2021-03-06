# PostreSQL Notes

Notes on learning PostgreSQL with Jose Portilla's Complete SQL Bootcamp.

## Databases

Databases are systems that allow users to store and organize data.

Compare spreadsheets and databases:

- Tabs = tables
- Columns = columns
- Rows = rows

But

**Spreadsheets:**

- Good for one-time analysis
- Work for reasonably sized data sets
- Can be used by untrained people

**Databases:**

- Data integrity
- Can handle massive amounts of data
- Can quickly combine different data sets
- Steps can be automated
- Can support websites and apps

## PostgreSQL

PostgreSQL is a free and open-source relational database management system.

Benefits:

- Open source
- Widely used on the internet
- Cross-platform

## pgAdmin

pgAdmin is an open source, web-based GUI for managing PostgreSQL.

## SQL Statements

### SELECT

`SELECT` is the most common statement used. It allows you to retrieve information from a table.

`SELECT` can be combined with other statements to perform more complex queries.

```SQL
SELECT column_name FROM table_name;
```

You can use `*` to select all the columns from a table.

In general, it's not good practice to use `*` if you don't need all columns. It automatically queues everything, increasing traffic between the dataase server and the application, which can slow down the retrieval of results.

### SELECT DISTINCT

The `DISTINCT` keyword can be used to only return distinct values in a column if the column has duplicate values.

```SQL
SELECT DISTINCT column_name FROM table_name;
```

You can add parentheses to the column for clarity. They are necessary when adding more calls, such as `COUNT`.

```SQL
SELECT DISTINCT(column_name) FROM table_name;
```

`DISTINCT` is useful for finding unique values in a column.

### COUNT

The `COUNT` function returns the number of input rows that match a specific condition of a query.

You can apply `COUNT` to a specific column or pass `COUNT(*)`. These should produce the same results sicne each column has the same number of rows.

```SQL
SELECT COUNT(column_name) FROM table_name;
```

`COUNT` is more useful when combined with other commands, such as `DISTINCT` to get the number of unique values in a column.

```SQL
SELECT COUNT(DISTINCT column_name) FROM table_name;
```

### SELECT WHERE

The `WHERE` statement allows you to specify conditions on columns for the rows to be returned.

```SQL
SELECT column_1, column_2 FROM table_name WHERE conditions;
```

The `WHERE` clause appears immediately after the `FROM` clause of the `SELECT` statement.

The conditions filter the rows returned from the `SELECT` statment.

The standard comparison operators apply. Not equal to can be represented by `<>` or `!=`

Logical operators (`AND`, `OR`, `NOT`) allow you to combine multiple comparison operators.

### ORDER BY

PostgreSQL sometimes returns the same request query results in a different order.

You can use `ORDER BY` to sort rows based on a column value, in either ascending or descending order.

```SQL
SELECT column_1, column_2 FROM table_name ORDER BY column_1 ASC / DESC;
```

`ORDER BY` goes towards the end of the query since you want to do selecting and filtering first before sorting.

`ASC` = ascending order. `DESC` = descending order. If left blank, `ORDER BY` uses `ASC` by default.

You can `ORDER BY` multiple columns in the order you provide them. This helps when the first column has duplicate entries. These can then be ordered by the second column you specify.

```SQL
SELECT column_1, column_2, column_3 FROM table_name ORDER BY column_1, column_3;
```

Technically, you can `ORDER BY` columns that you do not explicitly `SELECT`, but it's less readable.

### LIMIT

The `LIMIT` command lets you limit the number of rows returned for a query. It's useful when you don't want to return every row in a table but only view the top few rows to get an idea of the table layout.

It's also useful when combined with `ORDER BY`.

`LIMIT` goes at the end of a query request and is the last command to be executed.

```SQL
SELECT * FROM table_name LIMIT 1;
```

### BETWEEN

The BETWEEN operator can be used to match a value against a range of values (inclusive of the upper and lower values).

```SQL
value BETWEEN low AND high
```

This is the same as

```SQL
value >= low AND <= high
```

You can also combine `BETWEEN` with the `NOT` operator (exclusive of the upper and lower values).

```SQL
value NOT BETWEEN low AND high
```

Which would be the same as

```SQL
value < low OR value > high
```

The `BETWEEN` operator can also be used with date. You need to format dates in the ISO 8601 standard format: YYYY-MM-DD

```SQL
date BETWEEN '2007-01-01' AND '2007-02-01'
```

When using `BETWEEN` with dates that include timestamp information, pay attention to using `BETWEEN` versus <=, >= comparison operators, since a datetime starts at 0:00.

### IN

In certain cases you want to check for multiple possible value options, for example, if a user's name appears `IN` a list of known names.

Use the `IN` operator to create a condition that checks if a value is included in a list of multiple options.

```SQL
value IN (option_1, option_2, ... option_n)
```

### LIKE and ILIKE

The `LIKE` operator lets you perform pattern matching against string data with the use of wildcard characters:

`%` matches any sequences of characters (including none)

`_` matches any single character

For example, all names that begin with an 'A'

```SQL
WHERE name LIKE 'A%'
```

All names that end with an 'a'

```SQL
WHERE name LIKE '%a'
```

`LIKE` is case-sensitive, `ILIKE` is case-insensitive.

```SQL
WHERE title LIKE 'Mission Impossible _'
```

You can use multiple underscores for multiple characters:

```SQL
WHERE value LIKE 'Version#__'
```

You can combine pattern matching operators.

```SQL
WHERE name LIKE '_her%'
```

PostgreSQL supports [full regex compatibilities](https://www.postgresql.org/docs/current/functions-matching.html).

## Aggregate Functions

An aggregate function takes multiple inputs and returns a single output. [Docs for Aggregate Functions](https://www.postgresql.org/docs/current/functions-aggregate.html).

The most common aggregate functions are:

- `AVG()` - returns the average value
- `COUNT()` - returns the number of values
- `MAX()` - returns the maximum value
- `MIN()` - returns the minimum value
- `SUM()` - returns the sum of all values

Aggregate function calls happen only in the `SELECT` clause or the `HAVING` clause.

#### Note

`AVG()` returns a floating point value several decimal places long. You can use `ROUND()` to specify precision after the decimal.

`COUNT()` returns the number of rows, so by convention you can just use `COUNT(*)`.

### GROUP BY

The `GROUP BY` clause divides the rows returned by the `SELECT` statement into groups.

For each group, you can apply an aggregate function, for example, to calculate the sum of items or count the number of items in the groups.

```SQL
SELECT category_col , AGG(data_col) FROM table_name GROUP BY category_col;
```

The `GROUP BY` clause must appear right after a `FROM` or `WHERE` statement.

In the `SELECT` statment, columns must either have an aggregate function or be in the `GROUP BY` call.

`WHERE` statements should not refer to the aggregation result.

If you want to sort results based on the aggregate, make sure to reference the entire function.

### HAVING

The `HAVING` clause lets you filter **after** an aggregation has taken place.

You can't use `WHERE` to filter based off of aggregate results, because those happen **after** a `WHERE` is executed.

`HAVING` lets you use the aggregate result as a filter along with a `GROUP BY`.

```SQL
SELECT category_col , AGG(column_2) FROM table_name GROUP BY column_1 HAVING condition;
```

## JOINS

`JOINS` let you combine information from multiple tables.

The main reason for the different `JOIN` types is to decide how to deal with information only present in one of the joined tables.

### AS

The `AS` clause lets you create an "alias" for a column or result.

```SQL
SELECT column AS new_name FROM table;
```

or

```SQL
SELECT SUM(column) AS new_name FROM table;
```

The `AS` operator gets executed at the very end of a query, meaning that you can't use the `ALIAS` inside a `WHERE` or `HAVING` clause. You'll have to use the original column name when filtering or comparing.

### INNER JOINS

An `INNER JOIN` will result with the set of records that match in both tables. Think of the overlap in a Venn diagram. See [Jeff Atwood's post](https://blog.codinghorror.com/a-visual-explanation-of-sql-joins/)

![INNER JOIN](https://blog.codinghorror.com/content/images/uploads/2007/10/6a0120a85dcdae970b012877702708970c-pi.png)

```SQL
SELECT * FROM TableA INNER JOIN TableB ON TableA.col_match = TableB.col_match;
```

You can remove duplicate columns by specifying only the columns you want.

```SQL
SELECT reg_id, Logins.name, log_id FROM Registrations INNER JOIN Logins ON Registrations.name = Logins.name;
```

If you write just `JOIN`, PostgreSQL will treat it as an `INNER JOIN` but for readability use `INNER JOIN`.

### OUTER JOINS

`OUTER JOINS` let you specify how to deal with the values only present in one of the tables being joined.

### FULL OUTER JOINS

A `FULL OUTER JOIN` grabs everything whether it is present in one or both tables.

![FULL OUTER JOIN](https://blog.codinghorror.com/content/images/uploads/2007/10/6a0120a85dcdae970b012877702725970c-pi.png)

```SQL
SELECT * FROM TableA FULL OUTER JOIN TableB ON TableA.col_match = TableB.col_match;
```

You can qualify a `FULL OUTER JOIN` with a `WHERE` statement to only get rows unique to either table (the opposite of an `INNER JOIN`).

[FULL OUTER JOIN qualified by WHERE clause](https://blog.codinghorror.com/content/images/uploads/2007/10/6a0120a85dcdae970b012877702769970c-pi.png)

```SQL
SELECT * FROM TableA FULL OUTER JOIN TableB ON TableA.col_match = TableB.col_match WHERE TableA.id IS null OR TableB.id IS null;
```

### LEFT OUTER JOIN

A `LEFT OUTER JOIN` results in the set of records that are in the left table. If there is no match in the right table, the results are null.

![LEFT OUTER JOIN](https://blog.codinghorror.com/content/images/uploads/2007/10/6a0120a85dcdae970b01287770273e970c-pi.png)

```SQL
SELECT * FROM TableA LEFT OUTER JOIN TableB ON TableA.col_match = TableB.col_match;
```

Table order matters for `LEFT OUTER JOINS` because you are specifying the left table.

You can qualify a `LEFT OUTER JOIN` with a `WHERE` statement, for example to get rows exclusive to the left table.

![LEFT OUTER JOIN qualified by WHERE clause](https://blog.codinghorror.com/content/images/uploads/2007/10/6a0120a85dcdae970b012877702754970c-pi.png)

```SQL
SELECT * FROM TableA LEFT OUTER JOIN TableB ON TableA.col_match = TableB.col_match WHERE TableB.id IS null;
```

### RIGHT OUTER JOINS

A `RIGHT OUTER JOIN` is essentially the same as a `LEFT OUTER JOIN`, except the tables are switched. This would be the same as switching the table order in a `LEFT OUTER JOIN`.

```SQL
SELECT * FROM TableA RIGHT OUTER JOIN TableB ON TableA.col_match = TableB.col_match;
```

You can also qualify a `RIGHT OUTER JOIN` with a `WHERE` statement, for example to get rows exclusive to the right table.

```SQL
SELECT * FROM TableA RIGHT OUTER JOIN TableB ON TableA.col_match = TableB.col_match WHERE TableA.id IS null;
```

It depends on how you have the tables organized in your mind when it comes to choosing a `LEFT JOIN` or `RIGHT JOIN`.

### UNIONS

The `UNION` operator is used to combine the result-set of two or more `SELECT` statements.

It basically serves to directly concatenate two results together, essentially "pasting" them together.

```SQL
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2;
```

For example, if you want to paste together all sales from two quarters.

```SQL
SELECT * FROM sales2020_Q1
UNION
SELECT * FROM sales2020_Q2;
```

## More Advanced SQL Commands

### Timestamps and Extract

These functions, which report back time and date information, are more useful when creating tables and databases, rather than when querying a database.

PostgreSQL can hold date and time information:

- `TIME` - contains only time
- `DATE` - contains only date
- `TIMESTAMP` - Contains date and time
- `TIMESTAMPTZ` - Contains date, time and timezone

Consider your needs carefully when designing a table and database and choosing a time data type.

Depending on the situation, you might not need the full level of `TIMESTAMPTZ`.

Remember, you can always remove historical information but you can't add it.

### Commands for checking times

`SHOW TIMEZONE` will tell you what timezone you're in.

`SELECT NOW()` gives you the timestamp information for right now.

`SELECT TIMEOFDAY()` gives you a string describing the current date, time and timezone.

`SELECT CURRENT_TIME` gives you the current time with timezone.

`SELECT CURRENT_DATE` gives you the current date.

### EXTRACT()

The `EXTRACT()` function lets you "extract" or obtain a sub-component of a date value:

- YEAR
- MONTH
- DAY
- WEEK
- QUARTER

```SQL
EXTRACT(YEAR FROM date_col)
```

### AGE()

The `AGE()` function calculates and returns how old a given timestamp is.

```SQL
AGE(date_col)
```

### TO_CHAR()

The `TO_CHAR()` is a general function that converts data types to text, which is useful for timestamp formatting.

```SQL
TO_CHAR(date_col, 'mm-dd-yyyy')
```

### Mathematical Functions and Operators

Mathematical operations can be carried out between columns using mathematical operators such as `+-*/`.

More complex calculations can make use of mathematical functions, such as `ROUND()`.

```SQL
SELECT ROUND(rental_rate/replacement_cost, 2) * 100 FROM film;
```

### String Functions and Operators

String functions and operators let you edit, combine and alter data columns. For example,

`string || string` will concat strings.

`LENGTH(string)` will give you the length of a string.

`LEFT(string, n)` returns the first `n` characters in a string.

`LOWER(string)` will lowercase a string.

See the [PostreSQL documentation](https://www.postgresql.org/docs/12/functions-string.html) for more examples.

### Subquery

A subquery lets you construct complex queries, essentially performing a query on the results of another query.

The syntax involves two `SELECT` statements.

For example, if you wanted to know which students scored above the average grade.

```SQL
SELECT student, grade FROM test_scores WHERE grade > (SELECT AVG(grade) FROM test_scores);
```

The subquery is performed first since it's inside the parenthesis.

You can also use the `IN` operator in conjunction with a subquery to check against multiple returned results.

```SQL
SELECT student, grade FROM test_scores WHERE student IN (SELECT student FROM honor_roll_list);
```

If your subquery returns a single value, you can use a comparison operator. If your subquery returns a number of values, you'll need to use the `IN` operator.

### EXISTS

The `EXISTS` operator tests for the existence of rows in a subquery.

Typically, a subquery is passed into the `EXISTS()` function to check if any rows are returned with the subquery.

```SQL
SELECT column_name FROM table_name WHERE EXISTS (SELECT colun_name FROm table_name WHERE condition);
```

### Self Join

A self join is a query in which a table is joined to itself.

Self joins are useful for comparing values in a column of rows within the same table.

A self join can be viewed as a join of two copies of the same table.

The table is not actually copied, but SQL performs the command as though it were.

There is no special keyword for a self join, it's standard `JOIN` syntax with the same table in both parts.

However, when using a self join, it's necessary to use an alias for the table, otherwise the table names would be ambiguous.

```SQL
SELECT tableA.col, tableB.col
FROM table AS tableA
JOIN table AS tableB
ON tableA.come_col = tableB.other_col;
```

For example, your want to use your employees table to print out the names of every employee and who they report to.

| emp_id | name   | report_id |
| ------ | ------ | --------- |
| 1      | Andrew | 3         |
| 2      | Chloe  | 1         |
| 3      | Alexa  | 4         |

```SQL
SELECT emp.name, report.name AS rep
FROM employees AS emp
JOIN employees AS report ON
emp.emp_id = report.report_id;
```

| name   | rep      |
| ------ | -------- |
| Andrew | Alexa    |
| Chloe  | Andrew   |
| Alexa  | Michelle |

## Creating Databases and Tables

### Data Types

The main data types in SQL are:

- **Boolean** - true or false
- **Character** - char, varchar and text
- **Numeric** - integer and floating-point number
- **Temporal** - date, time, timestamp and interval
- **UUID** - universally unique identifier
- **Array** - stores an array of strings, numbers, etc.
- **JSON** - JavaScript Object Notation
- **Hstore** - key-value pair
- Special types, such as network addresses and geometric data.

When creating databases and tables, carefully consider which data types are appropriate for the data to be stored.

Review the [docs](https://www.postgresql.org/docs/12/datatype.html) to see the limits of different data types.

For example, you might think of storing phone numbers as a `BIGINT` data type. But you might not need a number at all.

Since you're not performing arithematic with the phone numbers, it makes more sense to store them as a `VARCHAR` data type instead. Also, leading zeros could cause issues, since 7 and 07 are treated the same numerically but are not the same phone number.

Always search for best practices online.

When creating a database and table, take your time to plan for long term storage. Remember, you can always remove historical information, but you can't go back in time and add it in.

### Primary and Foreign Keys

#### Primary Keys

A primary key is a column, or group of columns, used to uniquely identify a row in a table.

For example, customers could have a unique, non-null customer_id column as their primary key.

Primary keys are also important since they allow us to easily discern what columns should be used for joining tables together.

pgAdmin tells you if a column is a primary key by including PK in the column information.

#### Foreign Keys

A foreign key is a field, or group of fields, in a table that uniquely identify a row in another table.

A foreign key is defined in a table that references the primary key of the other table.

The table that contains the foreign key is called the referencing table or child table.

The table which the foreign key references is called the referenced table or parent table.

A table can have multiple foreign keys depending on its relationships with other tables.

For example, in a dvd rental database payment table, each payment row would have a unique payment_id (a primary key) and identify the customer who made the payment through a customer_id (a foreign key since it references the customer table's primary key).

When creating tables and defining columns, we can use constraints to define columns as being a primary key, or attaching a foreign key relationship to another table.

### Constraints

Constraints are the rules enforced on data columns on a table.

They are used to prevent invalid data from being entered into the database.

This ensures the accuracy and reliability of the data in the database.

Constraints can generally be divided into two main categories:

**Column Constraints** - these constrain the data in a column to adhere to certain conditions.

**Table Constraints** - these are applied to the entire table rather than to an individual column.

Some of the most common column constraints are:

**NOT NULL** Constraint - this ensures that a column cannot have a NULL value. For example, requiring an email address.

**UNIQUE** Constraint - this ensures that all values in a column are different. For example, each customer having a unique id.

**PRIMARY key** - uniquely identifies each row/record in a table.

**FOREIGN key** - constrains data based on columns in other tables.

**CHECK** Constraint - ensures that all values in a column satisfy certain conditions.

**EXCLUSION** Constraint - ensures that if any two rows are compared on the specified column or expression using the specified operator, not all of these comparisons will return TRUE.

Table Constraints include:

**CHECK (condition)** - checks a condition when inserting or updating data.

**REFERENCES** - constrain the value stored in the column that must exist in a column in another table.

**UNIQUE (column_list)** - forces the values stored in the columns listed inside the parentheses to be unique.

**PRIMARY KEY (column_list)** - allows you to define the primary key that consists of multiple columns.

### CREATE

The general syntax for creating a table is:

```SQL
CREATE TABLE table_name (
  column_name TYPE column_constraint,
  column_name TYPE column_constraint,
  table_constraint table_constraint
  ) INHERITS existing_table_name;
```

For example:

```SQL
CREATE TABLE players (
  player_id SERIAL PRIMARY KEY,
  age SMALLINT NOT NULL,
  );
```

### SERIAL

In PostgreSQL, a sequence is a special kind of database object that generates a sequence of integers.

A sequence is often used as the primary key column in a table.

`SERIAL` will create a sequence object and set the next value generated by the sequecne as the default value for the column.

This is perfect for a primary key, because it logs unique integer entries for you automatically upon insertion.

If a row is later removed, the column with the `SERIAL` data type will **not** adjust, marking the fact that a row was removed from the sequence. For example, 1,2,3,5,6.

### INSERT

`INSERT` lets you add rows to a table.

The general syntax is:

```SQL
INSERT INTO table(column1, column2, ...)
VALUES
  (value1, value2, ...),
  (value1, value2, ...)
```

You can also insert values from another table:

```SQL
INSERT INTO table(column1, column2, ...)
SELECT column1, column2, ...
FROM another_table
WHERE condition;
```

Keep in mind, the inserted row values must match up for the table, including constraints.

`SERIAL` columns don't need to be provided a value.

### UPDATE

The `UPDATE` keyword lets you change the values of the columns in a table.

The general syntax is:

```SQL
UPDATE table
SET column1 = value1,
    column2 = value2, ...
WHERE condition;
```

You can reset everything without a `WHERE` condition:

```SQL
UPDATE account
SET last_login = CURRENT_TIMESTAMP;
```

You can update based off another column:

```SQL
UPDATE account
SET last_login = created_on;
```

You can use another table's values (an `UPDATE` join):

```SQL
UPDATE TableA
SET original_col = TableB.new_col
FROM TableB
WHERE TableA.id = TableB.id;
```

You can also return the rows that were affected:

```SQL
UPDATE account
SET last_login = created_on
RETURNING account_id, last_login;
```

### DELETE

The `DELETE` clause lets you remove rows from a table.

```SQL
DELETE FROM table
WHERE row_id = 1;
```

You can delete rows based on their presence in other tables. Think of this like a delete join.

```SQL
DELETE FROM tableA
USING tableB
WHERE tableA.id = tableB.id;
```

You can delete all rows from a table.

```SQL
DELETE FROM table;
```

Similar to the `UPDATE` command, you can add in a `RETURNING` call to return the rows that were removed.

```SQL
DELETE FROM job
WHERE job_name = 'Engineer'
RETURNING job_id, job_name
```

### ALTER

The `ALTER` clause lets you make changes to an existing table structure, such as:

- Adding, dropping or renaming columns
- Changing a column's data types
- Setting `DEFAULT` values for a column
- Adding `CHECK` constraints
- Renaming the table

```SQL
ALTER TABLE table_name action;
```

Adding columns:

```SQL
ALTER TABLE table_name ADD COLUMN new_col TYPE;
```

Removing columns:

```SQL
ALTER TABLE table_name DROP COLUMN col_name;
```

Altering constraints:

```SQL
ALTER TABLE table_name ALTER COLUMN col_name SET NOT NULL;
```

[See the docs for more `ALTER` commands](https://www.postgresql.org/docs/12/sql-altertable.html).

### DROP

`DROP` lets you completely remove a column from a table.

In PostgreSQL, this will also automatically remove all of its indexes and constraints involving the column.

However, it will not remove columns used in views, trigger, or stored procedures without the additional `CASCADE` clause.

The general syntax is:

```SQL
ALTER TABLE table_name DROP COLUMN col_name;
```

To remove all dependencies:

```SQL
ALTER TABLE table_name DROP COLUMN col_name CASCADE;
```

You can check that the column exists to avoid an error:

```SQL
ALTER TABLE table_name DROP COLUMN IF EXISTS col_name;
```

### CHECK

The `CHECK` constraint lets you create more customized constraints that adhere to a certain condition, such as making sure that all inserted integer values fall below a certain thresold.

The general syntax is:

```SQL
CREATE TABLE example(
  ex_id SERIAL PRIMARY KEY,
  age SMALLINT CHECK (age > 21),
  parent_age SMALLINT CHECK (parent_age > age)
);
```

## Conditional Expressions and Operators

These keywords and functions let you add logic to your commands and workflows in SQL.

### CASE

You can use the `CASE` statement to only execute SQL code when certain conditions are met. This is silimar to `IF/ELSE` statements in other programming languages.

There are two main ways to use a `CASE` statement: a general `CASE` or a `CASE` expression.

Both methods can lead to the same results.

The syntax for a general `CASE` statement is:

```SQL
CASE
WHEN condition1 THEN result1
WHEN condition2 THEN result2
ELSE some_other_result
END
```

For example, if you want to reward your first 100-200 customers:

```SQL
SELECT customer_id,
CASE
	WHEN (customer_id <= 100) THEN 'Premium'
	WHEN (customer_id BETWEEN 100 AND 200) THEN 'Plus'
	ELSE 'Normal'
END AS customer_class
FROM customer;
```

The `CASE` expression syntax first evaluates an expression then compares the result with each value in the `WHEN` clauses sequentially.

```SQL
CASE expression
WHEN value1 THEN result1
WHEN value2 THEN result2
ELSE some_other_result
END
```

For example, if you want to choose the winner of a competition:

```SQL
SELECT customer_id,
CASE customer_id
	WHEN 32 THEN 'Winner'
	WHEN 26 THEN 'Second Place'
	ELSE 'Normal'
END AS competition_results
FROM customer;
```

The general `CASE` syntax is more flexible and lets you do more conditional checks. The `CASE` expression syntax essentially checks for equality to the provided expression.

### COALESCE

The `COALESCE` function accepts an unlimited number of arguments. It returns the first argument that is not null.

If all arguments are null, the `COALESCE` function will return null.

```SQL
COALESCE (arg_1, arg_2, ... arg_n)
```

The `COALESCE` function becomes useful when querying a table that contains null values and substituting them with another value for the purpose of performing operations. For example, replaceing null with 0 before doing calculations.

```SQL
SELECT item, (price - COLAESCE(discount, 0)) AS final_price FROM table
```

### CAST

The `CAST` operator lets you convert from one data type to another.

Keep in mind, not every instance of a data type can be `CAST` to another data type. It must be reasonable to convert the data. For example, the string '5' to the integer 5. 'five' to 5 will not work.

Syntax for `CAST` function:

```SQL
SELECT CAST('5' AS INTEGER)
```

PostgreSQL's `CAST` operator:

```SQL
SELECT '5'::INTEGER
```

Keep in mind that you can use this in a `SELECT` query with a column name instead of a single instance.

For example, if the date column was set up using strings, you could `CAST` that to use timestamps.

```SQL
SELECT CAST(date AS TIMESTAMP) FROM table
```

### NULLIF

The `NULLIF` function takes in 2 arguments and returns `NULL` if both are equal, otherwise it returns the first argument.

```SQL
NULLIF(arg1, arg2)
```

This is very useful in cases where a `NULL` value would cause an error or an unwanted result. For exmaple, to check against something being equal to 0.

```SQL
SELECT (
SUM(CASE WHEN department = 'A' THEN 1 ELSE 0 END) /
NULLIF(SUM(CASE WHEN department = 'B' THEN 1 ELSE 0 END), 0)
) AS department_ratio
FROM depts;
```

### Views

There are often specific combinations of tables and conditions that you find yourself using quite often for a project.

Instead of having to perform the same query over and over again, you can create a view to quickly see this query with a simple call.

```SQL
CREATE VIEW customer_info AS
SELECT first_name, last_name, address FROM customer
INNER JOIN address
ON customer.address_id = address.address_id
```

```SQL
SELECT * FROM customer_info
```

A view is a database object that is essentially a stored query.

A view can be accessed as a virtual table in PostgreSQL.

A view does not store data physically, it simply stores the query.

You can also update and alter existing views.

```SQL
CREATE OR REPLACE VIEW customer_info AS
SELECT first_name, last_name, address, district FROM customer
INNER JOIN address
ON customer.address_id = address.address_id
```

You can rename views.

```SQL
ALTER VIEW customer_info RENAME to c_info
```

You can remove a view like dropping a table.

```SQL
DROP VIEW IF EXISTS customer_info
```

### Importing and Exporting Data

pgAdmin has the ability to import information from external files. Right click on the table and select Import/Export.

Things to keep in mind:

Not every outside data file will work. Variations in formatting, macros, data types, etc. may prevent the Import command from reading the file, at which point you must edit your file to be compatible with SQL.

You must provide the exact file path to your outside file, otherwise the Import command will fail to find the file.

The Import command does not create a table for you. It assumes a table is already created. Currently there isn't an automated way within pgAdmin to create a table directly from a .csv file.
