---
layout: docu
railroad: expressions/star.js
title: Star Expression
---

## Examples

Select all columns present in the `FROM` clause:

```sql
SELECT * FROM table_name;
```

Count the number of rows in a table:

```sql
SELECT count(*) FROM table_name;
```

DuckDB offers a shorthand for `count(*)` expressions where the `*` may be omitted:

```sql
SELECT count() FROM table_name;
```

Select all columns from the table called `table_name`:

```sql
SELECT table_name.*
FROM table_name
JOIN other_table_name USING (id);
```

Select all columns except the city column from the addresses table:

```sql
SELECT * EXCLUDE (city)
FROM addresses;
```

Select all columns from the addresses table, but replace city with `lower(city)`:

```sql
SELECT * REPLACE (lower(city) AS city)
FROM addresses;
```

Select all columns matching the given expression:

```sql
SELECT COLUMNS(c -> c LIKE '%num%')
FROM addresses;
```

Select all columns matching the given regex from the table:

```sql
SELECT COLUMNS('number\d+')
FROM addresses;
```

## Syntax

<div id="rrdiagram"></div>

## Star Expression

The `*` expression can be used in a `SELECT` statement to select all columns that are projected in the `FROM` clause.

```sql
SELECT *
FROM tbl;
```

The `*` expression can be modified using the `EXCLUDE` and `REPLACE`.

### `EXCLUDE` Clause

`EXCLUDE` allows us to exclude specific columns from the `*` expression.

```sql
SELECT * EXCLUDE (col)
FROM tbl;
```

### `REPLACE` Clause

`REPLACE` allows us to replace specific columns with different expressions.

```sql
SELECT * REPLACE (col / 1000 AS col)
FROM tbl;
```

## `COLUMNS` Expression

The `COLUMNS` expression can be used to execute the same expression on multiple columns. For example:

```sql
CREATE TABLE numbers (id INTEGER, number INTEGER);
INSERT INTO numbers VALUES (1, 10), (2, 20), (3, NULL);
SELECT min(COLUMNS(*)), count(COLUMNS(*)) FROM numbers;
```


| id | number | id | number |
|---:|-------:|---:|-------:|
| 1  | 10     | 3  | 2      |

The `*` expression in the `COLUMNS` statement can also contain `EXCLUDE` or `REPLACE`, similar to regular star expressions.

```sql
SELECT
    min(COLUMNS(* REPLACE (number + id AS number))),
    count(COLUMNS(* EXCLUDE (number)))
FROM numbers;
```


| id | min(number := (number + id)) | id |
|---:|-----------------------------:|---:|
| 1  | 11                           | 3  |

`COLUMNS` expressions can also be combined, as long as the `COLUMNS` contains the same (star) expression:

```sql
SELECT COLUMNS(*) + COLUMNS(*) FROM numbers;
```


| id | number |
|---:|-------:|
| 2  | 20     |
| 4  | 40     |
| 6  | NULL   |

`COLUMNS` expressions can also be used in `WHERE` clauses. The conditions are applied to all columns and are combined using the logical `AND` operator.

```sql
SELECT *
FROM (
    SELECT 0 AS x, 1 AS y, 2 AS z
    UNION ALL
    SELECT 1 AS x, 2 AS y, 3 AS z
    UNION ALL
    SELECT 2 AS x, 3 AS y, 4 AS z
)
WHERE COLUMNS(*) > 1; -- equivalent to: x > 1 AND y > 1 AND z > 1
```


| x | y | z |
|--:|--:|--:|
| 2 | 3 | 4 |

## `COLUMNS` Regular Expression

`COLUMNS` supports passing a regex in as a string constant:

```sql
SELECT COLUMNS('(id|numbers?)') FROM numbers;
```


| id | number |
|---:|-------:|
| 1  | 10     |
| 2  | 20     |
| 3  | NULL   |

The matches of capture groups can be used to rename columns selected by a regular expression:

```sql
SELECT COLUMNS('(\w{2}).*') AS '\1' FROM numbers;
```


| id |  nu  |
|---:|-----:|
| 1  | 10   |
| 2  | 20   |
| 3  | NULL |

The capture groups are one-indexed; `\0` is the original column name.

## `COLUMNS` Lambda Function

`COLUMNS` also supports passing in a lambda function. The lambda function will be evaluated for all columns present in the `FROM` clause, and only columns that match the lambda function will be returned. This allows the execution of arbitrary expressions in order to select columns.

```sql
SELECT COLUMNS(c -> c LIKE '%num%') FROM numbers;
```


| number |
|--------|
| 10     |
| 20     |
| NULL   |

## `STRUCT.*`

The `*` expression can also be used to retrieve all keys from a struct as separate columns.
This is particularly useful when a prior operation creates a struct of unknown shape, or if a query must handle any potential struct keys.
See the [`STRUCT` data type]({% link docs/1.0/sql/data_types/struct.md %}) and [nested functions]({% link docs/1.0/sql/functions/nested.md %}) pages for more details on working with structs.

For example:

```sql
SELECT st.* FROM (SELECT {'x': 1, 'y': 2, 'z': 3} AS st);
```


| x | y | z |
|--:|--:|--:|
| 1 | 2 | 3 |