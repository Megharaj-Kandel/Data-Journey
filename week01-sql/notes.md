# Week 1 — SQL Basics (SQLBolt)

Notes and practice from SQLBolt Lessons 1–2, covering basic queries and filtering conditions.

---

## Lesson 1: SELECT queries

The `SELECT` statement is used to retrieve data from a table.

```sql
SELECT column1, column2 FROM table_name;
```

- Use `*` to select all columns:
```sql
SELECT * FROM movies;
```
- Select specific columns to keep results clean and relevant:
```sql
SELECT Title, Director FROM movies;
```

**Key takeaway:** Always select only the columns you actually need — pulling everything (`*`) works, but is inefficient on large real-world tables.

---

## Lesson 2: WHERE clause — filtering rows

The `WHERE` clause filters rows based on a condition, so you only get results that match.

```sql
SELECT column1, column2 FROM table_name WHERE condition;
```

### Comparison operators

| Operator | Meaning                  | Example                          |
|----------|---------------------------|-----------------------------------|
| `=`      | Equal to                  | `WHERE Director = 'Pete Docter'`  |
| `!=` or `<>` | Not equal to           | `WHERE Director != 'Pete Docter'` |
| `>`      | Greater than               | `WHERE Year > 2005`               |
| `<`      | Less than                  | `WHERE Year < 2005`               |
| `>=`     | Greater than or equal to   | `WHERE Year >= 2005`              |
| `<=`     | Less than or equal to      | `WHERE Year <= 2005`              |

### Logical operators — combining conditions

| Operator | Meaning                              | Example |
|----------|----------------------------------------|---------|
| `AND`    | Both conditions must be true            | `WHERE Year > 2000 AND Director = 'Brad Bird'` |
| `OR`     | At least one condition must be true     | `WHERE Director = 'Brad Bird' OR Director = 'Pete Docter'` |
| `NOT`    | Reverses a condition                    | `WHERE NOT Director = 'Brad Bird'` |

### Range and set filtering

| Keyword       | Meaning                                | Example |
|---------------|------------------------------------------|---------|
| `BETWEEN`     | Value falls within a range (inclusive)    | `WHERE Year BETWEEN 2000 AND 2010` |
| `NOT BETWEEN` | Value falls outside a range               | `WHERE Year NOT BETWEEN 2000 AND 2010` |
| `IN`          | Value matches any in a list               | `WHERE Director IN ('Brad Bird', 'Pete Docter')` |
| `NOT IN`      | Value matches none in a list              | `WHERE Director NOT IN ('Brad Bird', 'Pete Docter')` |

---

## Practice queries

```sql
-- Movies directed by John Lasseter
SELECT Title, Year FROM movies WHERE Director = 'John Lasseter';

-- Movies released after 2005, sorted logically by year
SELECT Title, Year FROM movies WHERE Year > 2005;

-- Movies NOT directed by Brad Bird
SELECT Title, Director FROM movies WHERE Director != 'Brad Bird';

-- Movies released between 2000 and 2010
SELECT Title, Year FROM movies WHERE Year BETWEEN 2000 AND 2010;

-- Movies by either Brad Bird or Andrew Stanton
SELECT Title, Director FROM movies WHERE Director IN ('Brad Bird', 'Andrew Stanton');
```

---

## Key takeaways
- `SELECT` retrieves data; `WHERE` filters it based on conditions.
- Comparison operators (`=`, `!=`, `>`, `<`) compare individual values.
- `AND`/`OR`/`NOT` combine or reverse multiple conditions.
- `BETWEEN` and `IN` are shortcuts that make filtering ranges or lists cleaner than writing multiple `OR` conditions.