
# 🚀 RayVaan's SQL Learning Documentary

 
> _A complete beginner-to-analyst level SQL walkthrough, featuring real-world queries and hands-on insights._



## 📅 Day-1: Text-Based Retrieval Queries

### 🔍 Queries:
```sql
SELECT * FROM movies;
SELECT COUNT(*) FROM movies;
SELECT DISTINCT industry FROM movies;
SELECT * FROM movies WHERE title LIKE "%THOR%";
SELECT * FROM movies WHERE title LIKE "%america%";
```

### 💡 Takeaways:
- `SELECT`, `FROM`, and `WHERE` are the basic SQL pillars.
- `*` retrieves all columns.
- `DISTINCT` shows unique values.
- `LIKE` with `%` performs pattern-matching text searches.

### 🧠 Exercises:
```sql
-- 1. Print all Marvel Studios movie titles & release years
SELECT title, release_year FROM movies WHERE studio = "Marvel Studios";

-- 2. Find all movies with "Avenger" in title
SELECT * FROM movies WHERE title LIKE "%Avenger%";

-- 3. Year "The Godfather" was released
SELECT release_year FROM movies WHERE title = "The Godfather";

-- 4. Distinct studios in Bollywood
SELECT DISTINCT studio FROM movies WHERE industry = "Bollywood";
```

---

## 📅 Day-2: Numerical Queries — Filtering & Sorting

### 🔢 Queries:
```sql
-- Using BETWEEN, IN, AND, OR
SELECT * FROM movies WHERE imdb_rating BETWEEN 6 AND 8;
SELECT * FROM movies WHERE release_year IN (2019, 2021, 2022);

-- NULL handling
SELECT * FROM movies WHERE imdb_rating IS NULL;
SELECT * FROM movies WHERE imdb_rating IS NOT NULL;

-- ORDER & LIMIT
SELECT * FROM movies WHERE industry="BOLLYWOOD" ORDER BY imdb_rating DESC LIMIT 3;
```

### 💡 Takeaways:
- Use `BETWEEN`, `IN`, `AND`, `OR` for range or discrete filtering.
- `NULL` ≠ empty string. Use `IS NULL` / `IS NOT NULL`.
- `ORDER BY` defaults to ascending (`ASC`); use `DESC` for descending.
- `LIMIT` + `OFFSET` = pagination.

### 🧠 Exercises:
```sql
-- 1. All movies by release_year (latest first)
SELECT * FROM movies ORDER BY release_year DESC;

-- 2. All 2022 movies
SELECT * FROM movies WHERE release_year = 2022;

-- 3. Movies after 2020 with rating > 8
SELECT * FROM movies WHERE release_year > 2020 AND imdb_rating > 8;

-- 4. Studios: Marvel + Hombale Films
SELECT * FROM movies WHERE studio IN ('Marvel studios', 'Hombale Films');

-- 5. All THOR movies by year
SELECT * FROM movies WHERE title LIKE "%THOR%" ORDER BY release_year;
```

---

## 📅 Day-2 Continued: Summary Analytics (MIN, MAX, AVG, GROUP BY)

### 📊 Queries:
```sql
SELECT MIN(imdb_rating), MAX(imdb_rating), ROUND(AVG(imdb_rating), 2) FROM movies WHERE industry = 'Bollywood';

SELECT studio, COUNT(*) AS movie_count FROM movies GROUP BY studio ORDER BY movie_count DESC;
```

### 💡 Takeaways:
- Use `MIN`, `MAX`, `AVG` for summary statistics.
- Combine with `GROUP BY` to get insights across categories.
- Rename columns using `AS`.

### 🧠 Exercises:
```sql
-- 1. Movies released between 2015 and 2022
SELECT COUNT(*) FROM movies WHERE release_year BETWEEN 2015 AND 2022;

-- 2. Yearly movie counts
SELECT release_year, COUNT(*) AS movie_count FROM movies GROUP BY release_year ORDER BY release_year DESC;
```

---

## 📅 Day-3: HAVING Clause & Query Execution Order

### 📌 Query Flow:  
`FROM → WHERE → GROUP BY → HAVING → ORDER BY`

### 🧠 Sample:
```sql
SELECT release_year, COUNT(*) AS movies_count
FROM movies
GROUP BY release_year
HAVING movies_count > 2
ORDER BY movies_count DESC;
```

### 💡 Takeaways:
- Use `HAVING` to filter after aggregation.
- Use `WHERE` to filter before aggregation.

---

## 📅 Day-3: Calculated Columns & Advanced Logic

### 📅 Built-in Functions:
```sql
SELECT CURDATE();
SELECT YEAR(CURDATE());
```

### 📐 Calculated Columns:
```sql
SELECT *, YEAR(CURDATE()) - birth_year AS age FROM actors;
SELECT *, (revenue - budget) AS profit FROM financials;
```

### 💰 Currency & Unit Conversion:
```sql
-- Convert to INR if USD
SELECT *, IF(currency = 'USD', revenue * 85, revenue) AS revenue_inr FROM financials;

-- Normalize unit to millions
SELECT *,
  CASE
    WHEN unit = 'thousands' THEN revenue / 1000
    WHEN unit = 'billions' THEN revenue * 1000
    ELSE revenue
  END AS revenue_mln
FROM financials;
```

### 🧠 Exercise:
```sql
-- Profit Percentage
SELECT *, (revenue - budget) AS profit,
ROUND((revenue - budget) * 100 / budget, 2) AS profit_pct
FROM financials;
```

---

## 🎓 Coffee break 

- Every SQL query has a purpose — think like an analyst.
- Focus on clean data retrieval *and* real-world business logic.
- Practice with sample datasets to build intuition.

---
## 📚 SQL Joins Mastery: The Art of Combining Tables

---

### 🌟 Why Multiple Tables?

- 🔹 To **save space** and avoid repetition.
- 🔹 To **organize data** cleanly and efficiently.
- 🔹 To **simplify updates** and improve database maintenance.

In SQL, we connect multiple tables using the **JOIN** clause.

---

### 🔗 Understanding SQL Joins

- **INNER JOIN**: Intersection — only common records.
- **LEFT JOIN**: All left table records + matching right records.
- **RIGHT JOIN**: All right table records + matching left records.
- **FULL JOIN**: Everything from both tables (LEFT + RIGHT via UNION).

---

### 1⃣ INNER JOIN (Default JOIN)

```sql
-- View tables individually
SELECT * FROM movies;
SELECT * FROM financials;

-- INNER JOIN
SELECT m.movie_id, title, budget, revenue, currency, unit
FROM movies m
JOIN financials f
ON m.movie_id = f.movie_id;
```

---

### 2⃣ LEFT JOIN

```sql
SELECT m.movie_id, title, budget, revenue, currency, unit
FROM movies m
LEFT JOIN financials f
ON m.movie_id = f.movie_id;
```
*Shows all movies even without matching financials.*

---

### 3⃣ RIGHT JOIN

```sql
SELECT m.movie_id, title, budget, revenue, currency, unit
FROM movies m
RIGHT JOIN financials f
ON m.movie_id = f.movie_id;
```
*Shows all financials even without matching movies.*

---

### 4⃣ FULL JOIN (Using UNION)

```sql
SELECT m.movie_id, title, budget, revenue, currency, unit
FROM movies m
LEFT JOIN financials f ON m.movie_id = f.movie_id

UNION

SELECT f.movie_id, title, budget, revenue, currency, unit
FROM movies m
RIGHT JOIN financials f ON m.movie_id = f.movie_id;
```
*Combines everything from both tables.*

---

### 🛠️ USING() Clause

```sql
SELECT movie_id, title, budget, revenue, currency, unit
FROM movies m
LEFT JOIN financials f
USING (movie_id);
```
*Simplifies JOINs when column names are the same.*

---

### 📌 SQL Joins: Key Takeaways

- JOIN + ON = Merge tables on condition.
- JOIN + ON + AND = Merge on multiple columns.
- Aliases (m, f) make queries shorter.
- INNER is default JOIN.
- LEFT, RIGHT, FULL = OUTER JOIN types.
- FULL JOIN = LEFT JOIN + RIGHT JOIN using UNION.

---

### ✏️ Exercises: SQL Joins in Action

**🔹 Show all Movies with their Language Names**

```sql
SELECT title, name
FROM movies m
INNER JOIN languages l
ON m.language_id = l.language_id;
```

**🔹 Show All Telugu Movies**

```sql
SELECT title
FROM movies m
LEFT JOIN languages l
ON m.language_id = l.language_id
WHERE l.name = "Telugu";
```

**🔹 Show Language and Number of Movies Released**

```sql
SELECT l.name, COUNT(m.movie_id) AS no_movies
FROM languages l
LEFT JOIN movies m USING (language_id)
GROUP BY language_id
ORDER BY no_movies DESC;
```

---

### 📊 CROSS JOIN: Cartesian Product

```sql
SELECT * FROM items CROSS JOIN variants;

SELECT *,
    CONCAT(name, " - ", variant_name) AS full_name,
    (price + variant_price) AS full_price
FROM items
CROSS JOIN variants;
```

**Takeaways:**

- **CONCAT()** = Combine strings.
- CROSS JOIN = When no common columns exist.

---

### 📊 Analytics Within Tables: Calculating Profit

**💰 Profit Calculation**

```sql
SELECT m.movie_id, title, budget, revenue, currency, unit,
       (revenue - budget) AS profit
FROM movies m
JOIN financials f
ON m.movie_id = f.movie_id;
```

**💰 Bollywood Movies Only**

```sql
SELECT m.movie_id, title, budget, revenue, currency, unit,
       (revenue - budget) AS profit
FROM movies m
JOIN financials f
ON m.movie_id = f.movie_id
WHERE industry = "Bollywood"
ORDER BY profit DESC;
```

**💰 Adjusted Profit in Million Units**

```sql
SELECT m.movie_id, title, budget, revenue, currency, unit,
       CASE
           WHEN unit = "thousands" THEN ROUND((revenue - budget) / 1000, 2)
           WHEN unit = "billions" THEN ROUND((revenue - budget) * 1000, 2)
           ELSE ROUND((revenue - budget), 2)
       END AS profit_mln
FROM movies m
JOIN financials f
ON m.movie_id = f.movie_id
WHERE industry = "Bollywood"
ORDER BY profit_mln DESC;
```

**Key Lessons:**

- Analyze tables to find business insights.
- ORDER BY to find Top/Bottom performers.
- Master JOINs for powerful analytics!

---

### 💪 Joining More Than Two Tables

**🎬 Movies and Actors**

```sql
SELECT m.title, a.name
FROM movies m
JOIN movie_actor ma ON m.movie_id = ma.movie_id
JOIN actors a ON a.actor_id = ma.actor_id;
```

**🎬 Movies and All Actors (Grouped)**

```sql
SELECT m.title, GROUP_CONCAT(a.name) AS actors
FROM movies m
JOIN movie_actor ma ON m.movie_id = ma.movie_id
JOIN actors a ON a.actor_id = ma.actor_id
GROUP BY m.movie_id;
```

**👤 Actors and Their Movies**

```sql
SELECT a.name, GROUP_CONCAT(m.title SEPARATOR " | ") AS movies
FROM actors a
JOIN movie_actor ma ON a.actor_id = ma.actor_id
JOIN movies m ON m.movie_id = ma.movie_id
GROUP BY a.actor_id;
```

**👤 Actors with Movies Count**

```sql
SELECT a.name, GROUP_CONCAT(m.title SEPARATOR " | ") AS movies,
       COUNT(m.title) AS movies_count
FROM actors a
JOIN movie_actor ma ON a.actor_id = ma.actor_id
JOIN movies m ON m.movie_id = ma.movie_id
GROUP BY a.actor_id
ORDER BY movies_count DESC;
```

**Tips:**

- Break down complex joins into smaller steps.
- Use **GROUP_CONCAT()** to combine rows into a single text field.
- ER diagrams make JOIN planning easier.

---

### ✏️ Final Exercise: Hindi Movies Revenue Report

```sql
SELECT title, revenue, currency, unit,
       CASE
           WHEN unit = "thousands" THEN ROUND(revenue / 1000, 2)
           WHEN unit = "billions" THEN ROUND(revenue * 1000, 2)
           ELSE revenue
       END AS revenue_mln
FROM movies m
JOIN financials f ON m.movie_id = f.movie_id
JOIN languages l ON m.language_id = l.language_id
WHERE l.name = "Hindi"
ORDER BY revenue_mln DESC;
```

---

### 🚀 Final Note:

**Mastering Joins = Mastering SQL Analytics!**

Unlock the real power of databases by becoming fluent in joining tables and crafting insights like a true Data Wizard 🧙‍♂️🌟.


---
## 🧾 Credits & Signature

> Made with 💡 by **RayVaan**  
> GitHub: [Ranjit-Saha](https://github.com/Ranjit-Saha)  
> LinkedIn: [itsranjitsaha](https://www.linkedin.com/in/itsranjitsaha)

---

