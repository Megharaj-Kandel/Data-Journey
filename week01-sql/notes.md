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

---

## Lesson 3: String matching with LIKE

While `=` checks for an exact match, `LIKE` is used for flexible, pattern-based string matching — and it's case-insensitive, unlike `=`.

| Operator     | Meaning                                              | Example |
|--------------|--------------------------------------------------------|---------|
| `=`          | Case-sensitive exact match                              | `col_name = "abc"` |
| `!=` or `<>` | Case-sensitive exact inequality                          | `col_name != "abcd"` |
| `LIKE`       | Case-insensitive exact match                             | `col_name LIKE "ABC"` |
| `NOT LIKE`   | Case-insensitive exact inequality                         | `col_name NOT LIKE "ABCD"` |
| `IN (...)`   | Value exists in a given list                              | `col_name IN ("A", "B", "C")` |
| `NOT IN (...)` | Value does not exist in a given list                   | `col_name NOT IN ("D", "E", "F")` |

### Wildcards (used only with LIKE / NOT LIKE)

| Wildcard | Meaning                                             | Example |
|----------|--------------------------------------------------------|---------|
| `%`      | Matches zero or more characters, anywhere in the string  | `col_name LIKE "%AT%"` → matches "AT", "ATTIC", "CAT", "BATS" |
| `_`      | Matches exactly one character                            | `col_name LIKE "AN_"` → matches "AND", but not "AN" |

### Practice queries

```sql
-- Movies with titles containing "Story" (e.g. Toy Story, Toy Story 2)
SELECT Title FROM movies WHERE Title LIKE '%Story%';

-- Movies NOT directed by anyone with "Bird" in their name
SELECT Title, Director FROM movies WHERE Director NOT LIKE '%Bird%';

-- Directors matching a specific list
SELECT Title, Director FROM movies WHERE Director IN ('John Lasseter', 'Pete Docter');

-- Directors excluded from a specific list
SELECT Title, Director FROM movies WHERE Director NOT IN ('John Lasseter', 'Pete Docter');
```

### Key takeaways

- Use `=` when you need an exact, case-sensitive match.
- Use `LIKE` when you need flexible or partial matching — very common for searching names, titles, or text fields.
- `%` is the most-used wildcard in real-world queries (e.g., searching for any email containing "@gmail.com").
- `IN` is a cleaner shortcut than writing multiple `OR` conditions for the same column.

## Lesson 4:  Filtering and Sorting Query Results

While working through some SQL practice exercises, I learned how to filter, sort, group, and paginate query results using `DISTINCT`, `ORDER BY`, `GROUP BY`, `LIMIT`, and `OFFSET`. Below are my notes with examples, based on a sample table called `north_american_cities`.

## Sample Table: `north_american_cities`

| City | Country | Population | Latitude | Longitude |
| ------ | --------- | ------------ | ---------- | ----------- |
| Guadalajara | Mexico | 1500800 | 20.659699 | -103.349609 |
| Toronto | Canada | 2795060 | 43.653226 | -79.383184 |
| Houston | United States | 2195914 | 29.760427 | -95.369803 |
| New York | United States | 8405837 | 40.712784 | -74.005941 |
| Philadelphia | United States | 1553165 | 39.952584 | -75.165222 |
| Havana | Cuba | 2106146 | 23.05407 | -82.345189 |
| Mexico City | Mexico | 8555500 | 19.432608 | -99.133208 |
| Phoenix | United States | 1513367 | 33.448377 | -112.074037 |
| Los Angeles | United States | 3884307 | 34.052234 | -118.243685 |
| Ecatepec de Morelos | Mexico | 1742000 | 19.601841 | -99.050674 |

---

## 1. DISTINCT — Remove duplicate values

Used to return only unique values from a column, removing repeats.

\`\`\`sql
SELECT DISTINCT Country FROM north_american_cities;
\`\`\`

**Result:** Mexico, Canada, United States, Cuba (each listed once, no matter how many cities belong to it).

---

## 2. ORDER BY — Sort results

Sorts rows in ascending (`ASC`, default) or descending (`DESC`) order.

**Example — order US cities by latitude, north to south:**

\`\`\`sql
SELECT City, Latitude
FROM north_american_cities
WHERE Country = 'United States'
ORDER BY Latitude DESC;
\`\`\`

North to south means highest latitude first, so `DESC` is used.

---

## 3. WHERE + ORDER BY — Filtering combined with sorting

**Example — cities west of Chicago (longitude -87.6298), ordered west to east:**

\`\`\`sql
SELECT City, Country, Longitude
FROM north_american_cities
WHERE Longitude < -87.6298
ORDER BY Longitude ASC;
\`\`\`

Since longitude values get more negative moving west, `Longitude < -87.6298` filters to cities west of Chicago, and sorting `ASC` lists the furthest-west city first.

---

## 4. GROUP BY — Group rows for aggregation

Combines rows sharing the same value in a column so aggregate functions (`AVG`, `SUM`, `COUNT`, etc.) can summarize each group.

**Example — average population per country:**

\`\`\`sql
SELECT Country, AVG(Population) AS average_population
FROM north_american_cities
GROUP BY Country
ORDER BY average_population DESC;
\`\`\`

---

## 5. LIMIT and OFFSET — Restrict and paginate results

- `LIMIT` restricts how many rows are returned.
- `OFFSET` skips a number of rows before returning results.

**Example — two largest cities in Mexico:**

\`\`\`sql
SELECT City, Country, Population
FROM north_american_cities
WHERE Country = 'Mexico'
ORDER BY Population DESC
LIMIT 2;
\`\`\`

**Example — third and fourth largest cities in the United States:**

\`\`\`sql
SELECT City, Country, Population
FROM north_american_cities
WHERE Country = 'United States'
ORDER BY Population DESC
LIMIT 2 OFFSET 2;
\`\`\`

**Pagination formula:**

\`\`\`
OFFSET = (page_number - 1) × page_size
LIMIT = page_size
\`\`\`

**Nth and (N+1)th largest formula:**

\`\`\`
OFFSET = N - 1
LIMIT = 2
\`\`\`

---

## Quick Reference Table

| Clause | Purpose |
| -------- | --------- |
| `DISTINCT` | Removes duplicate values from results |
| `ORDER BY` | Sorts results in ascending or descending order |
| `GROUP BY` | Groups rows for use with aggregate functions |
| `LIMIT` | Restricts the number of rows returned |
| `OFFSET` | Skips a number of rows before returning results |

---

### Key Takeaway

These clauses are commonly combined in real-world queries — filtering data with `WHERE`, grouping it with `GROUP BY`, sorting it with Limit and offset

# Lesson 5 — SQL JOINs (INNER, LEFT, RIGHT)

This lesson covers how to combine data from two tables using `JOIN`, and the difference between `INNER JOIN`, `LEFT JOIN`, and `RIGHT JOIN`.

---

## What is a JOIN?

A `JOIN` combines rows from two (or more) tables based on a related column between them. Instead of querying tables separately, a join lets you pull matching data together into a single result — like connecting a customer to their orders, or a student to their class.

---

## Real-Life Example: Customers & Orders

Imagine a simple online store database.

**Customers**

| CustomerID | Name |
| --- | --- |
| 1 | Ram |
| 2 | Sita |
| 3 | Hari |
| 4 | Gita |

**Orders**

| OrderID | Item | CustomerID |
| --- | --- | --- |
| 101 | Laptop | 1 |
| 102 | Phone | 1 |
| 103 | Headphones | 2 |
| 104 | Charger | 99 |

Notice two important mismatches:

- **Hari** and **Gita** (CustomerID 3 and 4) have never placed an order.
- Order 104 (Charger) belongs to CustomerID **99**, which doesn't exist in the Customers table — maybe a data entry mistake, or a deleted account.

This kind of mismatch is exactly what makes JOIN types behave differently, and it's realistic — in real databases, not every customer has ordered something, and not every order cleanly matches a customer.

---

## 1. INNER JOIN

Returns only rows where there's a **match in both tables**. Anything unmatched on either side is dropped completely.

```sql
SELECT Name, Item
FROM Customers
JOIN Orders
    ON Customers.CustomerID = Orders.CustomerID;
```

**Result:**

| Name | Item |
| --- | --- |
| Ram | Laptop |
| Ram | Phone |
| Sita | Headphones |

Hari and Gita are gone (no orders). The mystery order 104 is gone too (no matching customer). Only *confirmed pairs* survive.

**Use INNER JOIN when:** you only care about customers who have actually ordered something.

---

## 2. LEFT JOIN

Returns **every row from the left table**, plus matches from the right table. If there's no match, the right table's columns come back as `NULL`.

```sql
SELECT Name, Item
FROM Customers
LEFT JOIN Orders
    ON Customers.CustomerID = Orders.CustomerID;
```

**Result:**

| Name | Item |
| --- | --- |
| Ram | Laptop |
| Ram | Phone |
| Sita | Headphones |
| Hari | NULL |
| Gita | NULL |

Every customer shows up — even Hari and Gita, who never ordered anything. `Customers` is the left table, so `LEFT JOIN` guarantees it's fully preserved.

The mystery order (CustomerID 99) is missing here, because it belongs to `Orders` — the unprotected side.

**Use LEFT JOIN when:** you want every customer, including ones with zero orders (e.g., "find customers who haven't ordered anything yet" — those NULL rows are your answer).

---

## 3. RIGHT JOIN

The mirror image of `LEFT JOIN`. Returns **every row from the right table**, plus matches from the left table. Unmatched left-side rows become `NULL`.

```sql
SELECT Name, Item
FROM Customers
RIGHT JOIN Orders
    ON Customers.CustomerID = Orders.CustomerID;
```

**Result:**

| Name | Item |
|---|---|
| Ram | Laptop


# 📘 Lesson 6: SQL Aggregate Functions

Today's lesson covers **Aggregate Functions** — these take a bunch of rows and squash them into **one summary value**, like a total, an average, or a count.

---

## Sample Table

**`orders`**

| order_id | customer | amount | quantity |
|----------|----------|--------|----------|
| 1        | Sebika   | 500    | 2        |
| 2        | Ram      | 1200   | 5        |
| 3        | Sita     | 750    | 3        |
| 4        | Hari     | 300    | 1        |
| 5        | Sebika   | 900    | 4        |

---

### 1️⃣ COUNT() — "How many?"

```sql
SELECT COUNT(*) AS total_orders
FROM orders;
```

**Result:** `5`

> `COUNT(*)` counts every row. `COUNT(column_name)` only counts rows where that column isn't NULL.

---

### 2️⃣ SUM() — "What's the total?"

```sql
SELECT SUM(amount) AS total_revenue
FROM orders;
```

**Result:** `3650`

You can combine it with `WHERE` to sum a specific subset:

```sql
SELECT SUM(amount) AS sebika_total
FROM orders
WHERE customer = 'Sebika';
```

**Result:** `1400` (500 + 900)

---

### 3️⃣ AVG() — "What's the average?"

```sql
SELECT AVG(amount) AS average_order_value
FROM orders;
```

**Result:** `730`

> AVG automatically ignores NULL values — it doesn't treat them as 0.

---

### 4️⃣ MIN() — "What's the smallest?"

```sql
SELECT MIN(amount) AS cheapest_order
FROM orders;
```

**Result:** `300`

---

### 5️⃣ MAX() — "What's the biggest?"

```sql
SELECT MAX(amount) AS biggest_order
FROM orders;
```

**Result:** `1200`

---

## 🔗 Combining Aggregates with GROUP BY

`GROUP BY` splits the table into groups (like "per customer") so aggregates calculate separately for each group instead of the whole table.

```sql
SELECT customer, SUM(amount) AS total_spent, COUNT(*) AS num_orders
FROM orders
GROUP BY customer;
```

**Result:**

| customer | total_spent | num_orders |
|----------|--------------|-------------|
| Sebika   | 1400         | 2           |
| Ram      | 1200         | 1           |
| Sita     | 750          | 1           |
| Hari     | 300          | 1           |

---

## 🎯 Filtering Groups with HAVING

`WHERE` can't filter aggregate results — that's what `HAVING` is for.

```sql
SELECT customer, SUM(amount) AS total_spent
FROM orders
GROUP BY customer
HAVING SUM(amount) > 500;
```

**Result:**

| customer | total_spent |
|----------|--------------|
| Sebika   | 1400         |
| Ram      | 1200         |
| Sita     | 750          |

> 🔑 **WHERE** filters rows *before* grouping. **HAVING** filters groups *after* aggregation.

---

## 🧠 Quick Recap

| Function  | What it Does                          |
|-----------|-----------------------------------------|
| COUNT()   | Counts rows (or non-NULL values)        |
| SUM()     | Adds up values                          |
| AVG()     | Calculates the average                  |
| MIN()     | Finds the smallest value                |
| MAX()     | Finds the largest value                 |
| GROUP BY  | Groups rows before aggregating          |
| HAVING    | Filters groups after aggregation        |

---

## ✅ What I Learned Today

- How to summarize data using aggregate functions
- How to group results with `GROUP BY`
- The difference between `WHERE` and `HAVING`

*Next up: Subqueries and nested SELECT statements 🚀*
