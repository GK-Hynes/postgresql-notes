# freeCodeCamp PostgreSQL Course

[View course on YouTube](https://www.youtube.com/watch?v=qw--VYLpxG4)

## What is a Database?

A database is a place to store, manipulate an retrieve data.

Postgres is the database engine and SQL is the Structured Query Language for interacting with the data.

### How data is stored

In a relational database, data is stored in tables, consisting of columns and rows, and there are relations between these tables.

SQL lets you manage data in a relational database.

### PostgreSQL

PostgreSQL is a modern, open source, object-relational database management system.

### Create a database

`CREATE DATABASE datbase_name;`

### Connecting to a database

From the command line, use `psql -h HOSTNAME -p PORT -U USERNAME DATABASE_NAME`.

Or just `psql \c DATABASE_NAME`.

### Deleting a database

`DROP DATABASE database_name;`

### Creating a table

```SQL
CREATE TABLE table_name (
  column_name data_type constraints
)
```

[PostgreSQL docs for data types](https://www.postgresql.org/docs/12/datatype.html).

You will want to make essential entries, such as id, first_name, `NOT NULL`. Entries that could be empty, such as email, can be left as nullable.

```SQL
CREATE TABLE person (
  id BIGSERIAL PRIMARY KEY,
  first_name VARCHAR(50) NOT NULL,
  last_name VARCHAR(50) NOT NULL,
  gender VARCHAR(50) NOT NULL,
  date_of_birth DATE NOT NULL,
  email VARCHAR(150)
);
```

### Inserting records into a table

```SQL
INSERT INTO person (
  first_name,
  last_name,
  gender,
  date_of_birth)
VALUES ('Lucy', 'Jones', 'FEMALE, DATE '1990-06-03');
```

### Distinct

Use `DISTINCT` to only display unique values when querying.

```SQL
SELECT DISTINCT country_of_birth FROM person ORDER BY country_of_birth;
```

### OFFSET

You can use the `OFFSET` keyword to skip a certain number of results when performing a query.

### FETCH

The `FETCH` keyword lets you limit the number of rows returned.

```SQL
SELECT * FROM person FETCH FIRST 5 ROW ONLY;
```

### Matching characters

When using the `LIKE` keyword, you can use `%` for one or more of any character or `_` for one instance of any character.

```SQL
SELECT * FROM person WHERE email LIKE '____@gmail.com'
```

### GROUP BY

`GROUP BY` lets you group your results based on a column. For each group you can use an aggregate function, such as `SUM()` to calculate the sum of items or `COUNT()` to get the number of items in the groups.

```SQL
SELECT country_of_birth, COUNT(*) FROM person GROUP BY country_of_birth;
```

### HAVING

`HAVING` can be used in conjunction with `GROUP BY` to filter out rows that do/do not match a specified condition.

```SQL
SELECT country_of_birth, COUNT(*) FROM person GROUP BY country_of_birth HAVING COUNT(*) >= 5 ORDER BY country_of_birth;
```

### COALESCE

The `COALESCE` keyword lets you have a default value in case the value searched for is not present.

```SQL
SELECT COALESCE(email, 'Email not provided') FROM person;
```

### EXTRACT

`EXTRACT` lets you extract a specific value from date/time values.

```SQL
SELECT EXTRACT(YEAR FROM NOW());
```

### ON CONFLICT

The `ON CONFLICT` clause lets you specify an alternative action in case you try to insert a row that would violate a unique constraint.

`ON CONFLICT DO NOTHING` avoids inserting the row.

`ON CONFLICT DO UPDATE` updates the existing row with the row being inserted.

### Creating relationships between tables

To create a relationship between two tables, define a foreign key:

```SQL
create table person (
  id BIGSERIAL NOT NULL PRIMARY KEY,
	first_name VARCHAR(50) NOT NULL,
	last_name VARCHAR(50) NOT NULL,
	gender VARCHAR(7) NOT NULL,
	email VARCHAR(100),
	date_of_birth DATE NOT NULL,
	country_of_birth VARCHAR(50) NOT NULL,
	car_id BIGINT REFERENCES car (id),
	UNIQUE(car_id)
);
```

To delete a record with a foreign key constraint, first update the record to remove the foreign key constraint.

### Exporting query results to CSV

```SQL
\copy (YOUR_QUERY) TO FILE_PATH DELIMITER ',' CSV HEADER;
```

### Extensions

Extensions allow features to be added to PostgreSQL.

To view a list of them, use:

```SQL
SELECT * FROM pg_available_extensions;
```

For example, `plv8` lets you write JavaScript functions, `uuid-ossp` lets you generate universally unique identifiers.

To create an extension:

```SQL
CREATE EXTENSION IF NOT EXISTS 'uuid-ossp'
```

To see a list of available functions, use `/df`

For example, you could use a `UUID` instead of a `BIGSERIAL` to give an entry a unique id.

```SQL
CREATE TABLE person (
  person_id UUID NOT NULL PRIMARY KEY,
	first_name VARCHAR(50) NOT NULL,
	last_name VARCHAR(50) NOT NULL,
	gender VARCHAR(50) NOT NULL,
	email VARCHAR(100),
	date_of_birth DATE NOT NULL,
	country_of_birth VARCHAR(50) NOT NULL
);

INSERT INTO person (person_id, first_name, last_name, gender, email, date_of_birth, country_of_birth)
VALUES (uuid_generate_v4(), 'Fernanda', 'Brabazon', 'Female', 'fernandab87@gmail.com', '1987-10-28', 'Canada');
```
