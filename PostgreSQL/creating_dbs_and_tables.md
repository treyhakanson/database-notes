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


