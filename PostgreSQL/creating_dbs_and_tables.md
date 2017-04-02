# Creating Databases and Tables

These notes pertain to creating databases and tables and manipulating the data inside of them. This information pertains mainly to _wiriting_ to databases and tables, so if you're only concerned with _reading_ from dbs/tables, check out some of the other notes.

## Data Types

PostGreSQL supports the following data types:

- Boolean
- Character
- Number
- Temporal
- Special Types
- Array

data type must be specified when creating a column for a table; this prevents the wrong type of data from being inserted into a column

### Boolean

A standard boolean (true/false) value

PostGreSQL will take the input and convert it to a boolean value:

- 1, yes, y, t, true -> `true`
- 0, no, n, f, false -> `false`

When selecting from a boolean column, PostGreSQL will display `t` for `true`, `f` for `false`, and a space character for `NULL`

### Character

3 types of character fields:

- `char` - single chracter.
- `char(n)` - fixed length string; if a string shorter than the specified length is inserted, spaces will be added as padding. If attempting to insert a longer string, PostGreSQL will issue an error.
- `varchar(n)` - variable length string. Allows for storage of a string up to `n` characters. PostGreSQL will **not** pad spaces for variable length string fields.

### Number

2 distinct types of number, each with sub-types:

- Integers
    - `smallint` - 2 byte, signed integer (-32768, 32767)
    - `int` - 4-byte, signed integer (-214783648, 214783647)
    - `serial` - same as `int`, but PostGreSQL will automatically populate the value (similar to `AUTO_INCREMENT` in db management systems)
- Floating point numbers
    - `float(n)` - a floating point number whose percision is up to at least n bytes, up to 8 bytes.
    - `real` or `float8` - a double precision (8 byte) floating point number
    - `numeric` or `numeric(p,s)` - is a real number with p digits and s numbers after the decimal point. `numeric(p,)` will give as much precision as possible (the extact number)

### Temporal

store data related to date and time:

- `date` stores date
- `time` stores time
- `timestamp` stores date and time
- `interval` stores the difference between timestamps
- `timestamptz` stores timestamp and timezone data

## Primary and Foreign Keys

### Primary Keys

a primary key is a column or group of columns used to identify a row uniquely to a table

they are defined through primary key constraints

a table can have only 1 primary key, and it's good practice to add a primary key to every table

primary keys are typically added on creating a table:

```sql
-- Create queries like this one are discussed more below
-- `PRIMARY KEY` here is a constraint

CREATE TABLE <table> (<col_name> <data_type> PRIMARY KEY, ...);
```

### Foreign Keys

A foreign key is a col or group of cols that uniquely identifies a row in _another_ table

in other words, a foreign key defined in one table represents the primary key of another table

the table containing the foreign key is the `referencing` or `child` table, and the table containing the primary key is the `referenced` or `parent` table

foreign keys are set using foreign key constraints

a foreign key maintains "referential integrity" between parent and child tables

## CREATE Table

basic syntax of the create clause:

```sql
CREATE TABLE <table>
(<col> TYPE <col_constraint>, <table_constraint>)
INHERITS <exisiting_table>;
```

examples of column constraints:

- `NOT NULL` the value of the column cannot be null
- `UNIQUE` the value of the column must be unique
    - can have many `NULL` columns though, because PostGreSQL treats each `NULL` value as unique
        - Standard SQL allows only 1 `NULL`
- `PRIMARY KEY` a combination of the `UNIQUE` and `NOT NULL` constraints
- `CHECK` allows for a check condition to be run on upload
- `REFERENCES` constrains the value of the column that exists in a column in another table
    - used to define foreign keys

table constraints

- `UNIQUE (<col_list>)` forces the value stored in the listed columns to be unique
- `PRIMARY KEY (<col_list>)` define a multi-column primary key
- `CHECK (condition)` condition to check on uploading to the table
- `REFERENCES` to constrain the value stored in the column that must exist in a column in another table





