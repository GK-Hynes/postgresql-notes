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

SELECT is the most common statement used. It allows you to retrieve information from a table.

SELECT can be combined with other statements to perform more complex queries.

```SQL
SELECT column_name FROM table_name
```

You can use `*` to select all the columns from a table.

In general, it's not good practice to use `*` if you don't need all columns. It automatically queues everything, increasing traffic between the dataase server and the application, which can slow down the retrieval of results.
