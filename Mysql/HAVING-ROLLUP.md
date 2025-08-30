1. **GROUP BY** → puts rows into buckets (groups) based on the column(s) you choose.

   - Example: group all reviews by `title`.

2. **Each bucket = 1 row in the result.**

   - Inside that row, you can use aggregate functions like `COUNT`, `AVG`, `MAX`, etc.

3. **HAVING** → works **after grouping**. It checks each bucket’s result and decides whether to keep or drop that bucket.

---

✅ Example:

```sql
SELECT title, COUNT(*) AS review_count
FROM full_reviews
GROUP BY title
HAVING COUNT(*) > 1;
```

- Step 1: Group reviews by `title` (bucket for each title).
- Step 2: Count how many reviews in each bucket.
- Step 3: Keep only those buckets where `review_count > 1`.

---

⚡ So your understanding is correct:
👉 `HAVING` = filter on groups (buckets) after `GROUP BY`.

====================================================

### 🔹 How ROLLUP Works

- **Step 1:** Do a normal `GROUP BY` → make groups (buckets) and apply your aggregate functions (like `COUNT`, `SUM`, `AVG`).
- **Step 2:** Then `WITH ROLLUP` automatically **adds extra rows** → showing _subtotals_ and a _grand total_.
- **Step 3:** If you use multiple aggregate functions, **each one gets applied to those rollup rows too**.

---

### 🔹 Example with multiple aggregates

```sql
SELECT genre,
       COUNT(*) AS total_reviews,
       AVG(rating) AS avg_rating
FROM full_reviews
GROUP BY genre WITH ROLLUP;
```

👉 Result:

- For each `genre`: you get `COUNT` and `AVG`.
- Extra row (`genre = NULL`):

  - `COUNT` = total reviews (all genres).
  - `AVG` = overall average rating.

So yes — **each aggregate function runs again on the rollup rows**.

---

✅ Your understanding is correct:

- ROLLUP = final total(s) **per aggregate function**, added to your grouped results.
