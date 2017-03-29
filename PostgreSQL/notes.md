# PostgreSQL - Relational Database

## Overview

PostgreSQL is a popular, open-source database built on SQL (Structured Query Language)

### Common Commands

```sql
CREATE DATABASE <dbname> -- Create a db
DROP DATABASE <dbname> -- delete a db
VIEW DATABASES -- view all dbs
```

## Creating and Restoring Databases

### Basic Creation and Restoration

can create a database from pgAdmin by right-clicking "Databases" and selecting create, or by entering the following SQL query:

```sql
CREATE DATABASE <dbname>
```

Delete a database by right-clicking the database and selecting "Delete/Drop Database", or by using the following SQL Query:

```sql
-- Drop database
DROP DATABASE <dbname>

-- Check if database exists before dropping to avoid errors
DROP DATABASE IF EXISTS <dbname>
```

Can easily restore dbs from `.backup` files or `.tar`s, etc. Just right click a database and select "Restore"

### Restoring only Table Schemas

Sometimes it's useful to restore ONLY the table schemas (names, datatypes) and their relationships, but not any of the data.

To do this, just restore as usual, but under "Restore Options #1" select "Only Schema" and under "Restore Options #2" select "Clean Before Restore"

## Statements/Clauses

### SELECT

queries for data from tables, can be combined with other clauses to create powerful, specific queries

basic syntax:

```sql
SELECT <col_1>, <col_2>, ... FROM <table>;
```

`*` is select all (not best practice in real-world scenarios since tables can be quite large)

SQL is technically case-insensitive

end lines with semi-colons

can write multi-line queries; whitespace will be ignored, and line won't end until a semicolon is hit

the order the columns are selected in will be the order the matching rows are output in

### DISTINCT

query for only unique values, not duplicates:

```sql
SELECT DISTINCT <col> FROM <table>;
```

### WHERE

allows for querying specific rows from a table based on a restriction:

```sql
SELECT <cols> FROM <table> WHERE <conditions>;
```

basic conditional operators in PostgreSQL:

```
=   equal
>   greater than
<   less than
>=  greater than or equal to
<=  less than or equal to
<>  not equal
!=  not equal (alternate) 
AND logical and
OR  logical or
```

### COUNT

a function that returns the number of rows that match a specific condition of a query:

```sql
SELECT COUNT(<col>) FROM <table>;
```

the `COUNT` function ignores `NULL` values in the column being considered. Can use in unison with `DISTINCT` as well:

```sql
SELECT COUNT(DISTINCT <col>) FROM <table>;
-- or
SELECT COUNT(DISTINCT(<col>)) FROM <table>;
```

### LIMIT

can add a `LIMIT` to a query to prevent too many results from coming back:

```sql
SELECT <col_1>, <col_2> FROM <table> LIMIT <num>;
```

### ORDER BY

Can order a table based on a columns value:

```sql
SELECT <col_1>, <col_2> FROM <table> ORDER BY <col> <ASC/DESC>;
-- example:
SELECT food_name FROM food ORDER BY cost DESC;
```

`ASC` - Ascending
`DESC` - Descending

`ASC` is the default if no options is given

can order by multiple columns, which will just be performed sequentially. For example, if the first sort is by first name, and the second by last name, then anyone with the same first name will be ordered by lastname:

```sql
SELECT first_name, last_name FROM people
ORDER BY first_name ASC, last_name ASC;
```

*IMPORTANT*

PostGreSQL allows for sorting by columns that aren't selected, but many other SQL implementations can only sort by selected columns. Therefore, it's best practice to include the columns that are being sorted by.

### BETWEEN/NOT BEWTEEN

`BETWEEN`: syntactic sugar for using `>=`, `<=`, and logical `AND`
`NOT BETWEEN`: syntactic sugar for using `>`, `<`, and logical `OR`

```sql
SELECT <col_1>, <col_2> FROM <table>
WHERE <col> BETWEEN <val_1> AND <val_2>;
-- example:
SELECT name, payment FROM purchase
WHERE payment BETWEEN 8 AND 10;
```

### IN/NOT IN

can check if a value exists in a list of values:

```sql
SELECT <col_1>, <col_2> FROM <table>
WHERE <col> IN (<val_1>, <val_2> ... <val_n>)
```

could also replace the `val`s in the above example with another SQL query using a `SELECT` statement (a sub-query)

### LIKE/NOT LIKE/ILIKE

Used to find data based on pattern matching; similar to a regex

Supported in most SQL systems

Wildcard characters:
- `%` matches an unlimited number of any character
- `_` matches any 1 character

```sql
-- matches 'Hello World', 'Hello There', etc.
...
LIKE 'Hello%';

-- matches 'He', 'Hey', 'Her', etc.
...
LIKE 'He_';
```

can use any number of wildcard characters, at any position in the string

`ILIKE` is PostGreSQL specific, and provides case-insensitive pattern matching

### Aggregate Function (MIN, MAX, AVG, SUM)

aggregate functions combine a large amount of data into a single value

`COUNT` is also an aggregate function

syntax is the same as `COUNT`:

```sql
SELECT <agg func>(<col>) FROM <table>;
```

`ROUND` can be used to round a value to n number of decimal places. example:

```sql
ROUND(<val>, <num decimal places>);
```

### GROUP BY

without an aggregate function, `GROUP BY` basically acts like `DISTINCT`, returning only unique values

```sql
SELECT <col_1>, <col_2> FROM <table> GROUP BY <col>;
```

some SQL engines will not allow the above, and will actually require an aggregate function to be used with `GROUP BY`

when used in conjunction with an aggregate function, can form groups around values. For example, can sum all payments made by a specific customer:

```sql
SELECT customer_id, SUM(amount)
FROM payment
GROUP BY customer_id;
```

in summary, the above is sorting the result set by `customer_id`, and summing the `amount` paid by each customer.

similarly to `ORDER BY`, PostgreSQL can group by a column that isn't selected, but most SQL engines cannot. So, it's best practice to select the row being grouped by

### HAVING

`HAVING` clause is used in conjunction with the `GROUP BY` clause to filter group rows that do not satisfy a particular condition

```sql
SELECT <col_1>, <agg_func(col_2)>
FROM <table>
GROUP BY <col_1>
HAVING condition;
```

the `HAVING` clause sets the condition for group rows created by the `GROUP BY` clause after the `GROUP BY` clause applies while the `WHERE` clause set the condition for individual rows before `GROUP BY` clause applies
    - basically, the `HAVING` clause applies to all rows made by the `GROUP BY`, whereas the `WHERE` clause affects what rows are grouped
    - also, aggregate functions cannot be used with `WHERE`

here's a more complex example involving both `WHERE` and `HAVING`:

```sql
SELECT rating, ROUND(AVG(rental_rate), 2)
FROM film
WHERE rating in ('R', 'G', 'PG')
GROUP BY rating
HAVING AVG(rental_rate) < 3;
```

### AS

allows renaming of columns, table selections, aggregate functions, etc. with an alias

```sql
-- column
SELECT <col> AS <col_name> FROM <table>;
-- aggregate function
SELECT <agg func(col_1)> AS <col_name> ... ;
```

can use `AS` implicitly just by placing a space between the column's name and its alias:

```sql
SELECT <col> <col_name> FROM <table>;
```

### JOIN

joins relate data across multiple tables

#### Types of JOIN Clauses

![Types of SQL JOIN Clauses](https://www.codeproject.com/KB/database/Visual_SQL_Joins/Visual_SQL_JOINS_orig.jpg)

Types of `JOIN` Clauses:

- LEFT JOIN
- RIGHT JOIN
- INNER JOIN
- OUTER JOIN
- FULL OUTER JOIN

#### INNER JOIN

the most basic type of `JOIN`

an `INNER JOIN` clause returns rows in `A` table that have corresponding rows in `B` table

let's say we have a table `A` with keys `key1` and `key2`, and a table `B` with keys `key3`, `key4`, and `key5` where `B.key5` corresponds to `A.key1`:

```sql
SELECT A.key1 A.key2, B.key3, B.key4
FROM A
INNER JOIN B ON A.key1 = B.key3;
```

`A` is the main table, and `B` is the tabled being joined to

specifying the table when selecting (`A.key`) is only mandatory if the `key` appears in both tables

`INNER` can be dropped; `JOIN` by itself will default to an `INNER JOIN`

