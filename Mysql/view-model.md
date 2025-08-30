### 🔹 Views in SQL

- A **View** = a saved query that acts like a **virtual table**.
- It doesn’t store data, just the query.
- You can query it like a table (`SELECT * FROM view_name`).

---

### 🔹 Why use Views?

- Avoid rewriting long, complex queries (like multiple `JOIN`s).
- Make queries shorter, cleaner, and easier to read.
- Can be used with `WHERE`, `GROUP BY`, `ORDER BY` just like a normal table.

---

### 🔹 Example

Instead of writing a long query every time (joining `reviews`, `series`, `reviewers`),
you can do:

```sql
CREATE VIEW full_reviews AS
SELECT title, release_year, genre, rating, first_name, last_name
FROM reviews
JOIN series ON series.id = reviews.series_id
JOIN reviewers ON reviewers.id = reviews.reviewer_id;
```

Now just use:

```sql
SELECT * FROM full_reviews;
```

---

✅ **Key Idea:** Views don’t add new features, but they save time and make queries simpler by storing reusable queries under a name.
