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
