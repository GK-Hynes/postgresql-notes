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
