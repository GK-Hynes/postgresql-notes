# PostreSQL Notes

Notes on learning PostgreSQL with Jose Portilla's Complete SQL Bootcamp.

## Databases

Databases are systems that allow users to store and organize data.

Spreadsheets:

- Good for one-time analysis
- Work for reasonably sized data sets
- Can be used by untrained people

Databases:

- Data integrity
- Can handle massive amounts of data
- Can quickly combine different data sets
- Steps can be automated
- Can support websites and apps

Compare:

- Tabs = tables
- Columns = columns
- Rows = rows

## PostgreSQL

Free (open source)
Widely used on the internet
Corss-platform

## pgAdmin

pgAdmin is an open source, web-based GUI for managing PostgreSQL.

## SQL Statements

### SELECT

`SELECT` is the most common statement used. It allows you to retrieve information from a table.

`SELECT` can be combined with other statements to perform more complex queries.

```SQL
SELECT column_name FROM table_name
```

You can use `*` to select all the columns from a table.

In general, it's not good practice to use `*` if you don't need all columns. It automatically queues everything, increasing traffic between the dataase server and the application, which can slow down the retrieval of results.

### SELECT DISTINCT

The `DISTINCT` keyword can be used to only return distinct values in a column if the column has duplicate values.

```SQL
SELECT DISTINCT column_name FROM table_name
```

You can add parentheses to the column for clarity. They are necessary when adding more calls, such as `COUNT`.

```SQL
SELECT DISTINCT(column_name) FROM table_name
```

`DISTINCT` is useful for finding unique values in a column.

### COUNT

The `COUNT` function returns the number of input rows that match a specific condition of a query.

You can apply `COUNT` to a specific column or pass `COUNT(*)`. These should produce the same results sicne each column has the same number of rows.

```SQL
SELECT COUNT(column_name) FROM table_name
```

`COUNT` is more useful when combined with other commands, such as `DISTINCT` to get the number of unique values in a column.

```SQL
SELECT COUNT(DISTINCT column_name) FROM table_name
```

### SELECT WHERE

The `WHERE` statement allows you to specify conditions on columns for the rows to be returned.

```SQL
SELECT column1, column2 FROM table_name WHERE conditions
```

The `WHERE` clause appears immediately after the `FROM` clause of the `SELECT` statement.

The conditions filter the rows returned from the `SELECT` statment.

The standard comparison operators apply. Not equal to can be represented by `<>` or `!=`

Logical operators (`AND`, `OR`, `NOT`) allow you to combine multiple comparison operators.


