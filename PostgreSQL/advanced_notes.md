# Advanced PostGreSQL Topics

## Overview

more advanced SQL commands to perform more complex queries and manipulations

## Topics

### Timestamps

the `extract` function can be used to extract pieces of a timestamp:

```sql
SELECT extract(<unit> from <timestamp_col>) FROM <table>;
```

units that can be extracted:

- **day** - day of the month
- **dow** - day of the week (0=Sunday -> 6=Saturday)
- **doy** - day of the year (1-365/366, depending on if leap year)
- **epoch** - number of _seconds_ since 1970-01-01 00:00:00 UTC if date value, number of _seconds_ in interval if interval value
- **hour** - hour (0-23)
- **microseconds**
- **millenium**
- **minute** - minute (0-59)
- **month** - number for the month (1-12) if date value, number of months (0-11) if interval value
- **quarter** - quarter (1-4)
- **second** - also includes fractional seconds
- **week** - number of the week of the year based on ISO 8601 (specifies that the year begins on the week containing January 4th)
- **year** - year as 4-digits

arithmetic can be performed on datetime objects as well, check out [PostGreSQL's documentation](https://www.postgresql.org/docs/9.6/static/functions-datetime.html) form more information on adding and subtracting intervals, times, integers, etc.

### Mathematical Functions

a list of all mathematical operators in PostGreSQL can be found [here](http://www.postgresql.org/docs/9.6/static/functions-math.html)

keep in mind that division will return integers, not floats

### String Functions

a list of all string functions in PostGreSQL can be found [here](http://www.postgresql.org/docs/9.6/static/functions-string.html)

strings are not "added together" using `+`, the are concatenated with `||`

### Subqueries

A subquery allows for multiple `SELECT` statements to be used within a single query

can be placed in something like a `WHERE` clause to create more powerful queries

### Self Join

used when the goal is to combine rows with other rows in the same table

requires the use of an alias

example:

```sql
SELECT e1.employee_name
FROM employee AS e1, employee AS e2
WHERE
e1.employee_location = e2.employee_location
AND e2.employee_name = 'John';
```
