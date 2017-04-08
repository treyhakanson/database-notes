# Window Functions

## Overview

window functions operate on the result set of a query, after `WHERE` clauses and all standard aggregation. Thus, they operate on a _window_ of data. By default this is unrestricted: the entire result set, but it can be restricted to provide more useful results. Keep in mind that window function act over rows in a result set.

## Functions

`OVER` - used to specify a partition
`ROW_NUMBER` - used to specify row number; must be used with an `OVER` clause