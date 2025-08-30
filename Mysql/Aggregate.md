The Ultimate MySQL Bootcamp: Go from SQL Beginner to Expert

#### Aggregate

- We keep using the **books dataset** so we know the data well.
- Goal: learn **aggregate functions** (average, sum, count, group by).
- These help us analyze data by **grouping** (e.g., books per author, per year, per genre).
- Later, we’ll apply the same ideas to **big/complex data** (like Instagram-style data).
- Aggregates let us answer questions like:

  - Which author has the most books?
  - Average pages per genre?
  - Which user/hashtag gets the most likes?

- Aggregates = **backbone of data analysis** in SQL.
- Expect practice exercises to reinforce concepts.

1. Count:
   SELECT COUNT(_) FROM books;
   SELECT COUNT(author_lname) FROM books;
   SELECT COUNT(DISTINCT author_lname) FROM books;
   SELECT COUNT(_) FROM books WHERE title LIKE '%the%';

2. GROUP BY
   GROUP BY = Put similar rows into buckets, then calculate something for each bucket.
   SELECT roleTitle, COUNT(\*) FROM consort_u_prod.users group by roleTitle;

3. MIN and MAX
   MIN → finds the smallest value in a column.
   MAX → finds the largest value in a column.
   SELECT min(email), max(fullName) FROM consort_u_prod.users;
4. subquery

### What is a subquery?

👉 A **query inside another query**.
It runs **first**, gives a result, and that result is used in the **main query**.

### Example 1 (MAX pages)

```sql
SELECT title, pages
FROM books
WHERE pages = (SELECT MAX(pages) FROM books);
```

- Inside `(SELECT MAX(pages) FROM books)` → finds the biggest page number (e.g., 634).
- Outside query → finds all books that have **pages = 634**.

If 2 books have 634 pages → both are shown ✅

---

### Example 2 (MIN year)

```sql
SELECT title, released_year
FROM books
WHERE released_year = (SELECT MIN(released_year) FROM books);
```

- Inside → finds the **earliest year** (e.g., 1945).
- Outside → shows all books released that year.

---

### Compare with ORDER BY + LIMIT

- `ORDER BY pages DESC LIMIT 1` → only gives **1 row** (even if 2 books tie).
- Subquery → gives **all rows that match the max/min**.

---

👉 **One-line memory:**
**Subquery = “Ask SQL for an answer first, then use that answer in another question.”**

--------=====================================================================================================

5. GROUP BY multiple columns

### GROUP BY with 1 column

👉 Makes groups based on **one thing only**.
Example:

```sql
GROUP BY last_name
```

- All people with same last name go in **one bucket**.
- “Harris” bucket has **Dan + Freida together**.

---

### GROUP BY with 2 columns

👉 Makes groups based on **two things together**.
Example:

```sql
GROUP BY first_name, last_name
```

- Now buckets are **full names**.
- One bucket = Dan Harris, another bucket = Freida Harris.
- So they don’t mix! ✅

---

### GROUP BY with CONCAT (make full name)

👉 You can also **combine columns into one** and group by that:

```sql
GROUP BY CONCAT(first_name, ' ', last_name)
```

- Creates one “full name” column.
- Groups by it, same as using 2 columns.

---

### One-line memory:

**GROUP BY 1 column → big buckets.
GROUP BY 2+ columns → smaller, more exact buckets.**

==========================================================================================

### 6. MIN & MAX with GROUP BY

### Key Idea

- `MIN` and `MAX` don’t just work on the **whole table** → they also work **inside groups**.
- So we can find:

  - **Earliest book** per author → `MIN(released_year)`
  - **Latest book** per author → `MAX(released_year)`
  - **Longest book** per author → `MAX(pages)`

---

### Example

```sql
SELECT author_fname, author_lname,
       COUNT(*) AS books_written,
       MIN(released_year) AS earliest_release,
       MAX(released_year) AS latest_release,
       MAX(pages) AS longest_book
FROM books
GROUP BY author_fname, author_lname;
```

👉 Output = **1 row per author** with:

- Total books
- First book year
- Last book year
- Longest book pages

---

### Why GROUP BY both fname + lname?

- If you group by **last name only**, “Dan Harris” and “Freida Harris” get **merged together**.
- Grouping by **both names** keeps them separate. ✅

---

### One-line memory:

**GROUP BY + MIN/MAX = find earliest, latest, biggest, smallest for each group.**

=========================================================================================================

### 7. Sum

Adds things together
SELECT SUM(pages) FROM books;

SELECT author_lname, COUNT(\*), SUM(pages)
FROM books
GROUP BY author_lname;

=====================================================================================================================

### 8. AVG

# What `AVG` does

- `AVG(column)` = finds the **average value** in that column.

---

# Without GROUP BY

```sql
SELECT AVG(pages) FROM books;
```

- Gives the **average pages of all books** in the table.

---

# With GROUP BY

```sql
SELECT author_fname, author_lname, AVG(pages) AS avg_pages
FROM books
GROUP BY author_fname, author_lname;
```

- Groups books by **author**, then finds the **average pages per author**.

---

# Key takeaway

- **`AVG` alone** → one overall average.
- **`AVG + GROUP BY`** → average per group (author, genre, year, etc.).

---

👉 **Memory trick:**
**AVG = sum ÷ count.**

- No group = 1 bucket for all.
- With group = 1 bucket per group.
