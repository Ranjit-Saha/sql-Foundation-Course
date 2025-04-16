
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
SELECT *, IF(currency = 'USD', revenue * 77, revenue) AS revenue_inr FROM financials;

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

## 🎓 Final Thoughts

- Every SQL query has a purpose — think like an analyst.
- Focus on clean data retrieval *and* real-world business logic.
- Practice with sample datasets to build intuition.

---

## 🧾 Credits & Signature

> Made with 💡 by **RayVaan**  
> GitHub: [Ranjit-Saha](https://github.com/Ranjit-Saha)  
> LinkedIn: [itsranjitsaha](https://www.linkedin.com/in/itsranjitsaha)

---

