### Imagine a Classroom 🏫

- We have **students** with their **marks** in Math.
- Each student belongs to a **section** (A or B).

| Student | Section | Marks |
| ------- | ------- | ----- |
| Ali     | A       | 80    |
| Sara    | A       | 90    |
| John    | B       | 70    |
| Emma    | B       | 60    |

---

### 📌 Case 1: GROUP BY

Teacher says:
👉 “Give me the **average marks of each section**.”

We calculate:

- Section A → (80+90)/2 = 85
- Section B → (70+60)/2 = 65

**Result table** (only 2 rows, one per section):

| Section | AvgMarks |
| ------- | -------- |
| A       | 85       |
| B       | 65       |

➡️ Notice: We lost the individual students! Only **summaries remain**.

---

### 📌 Case 2: Window Function

Teacher says:
👉 “Show me **each student with their section’s average**.”

We calculate the same averages as before, but **attach them to every student row**:

| Student | Section | Marks | AvgMarksInSection |
| ------- | ------- | ----- | ----------------- |
| Ali     | A       | 80    | 85                |
| Sara    | A       | 90    | 85                |
| John    | B       | 70    | 65                |
| Emma    | B       | 60    | 65                |

➡️ We **kept all students** and just added extra info about their group.

- **GROUP BY**: “Summarize groups → one row per group.”
- **Window Function**: “Keep all rows, but also show group stats next to them.”

=================================================================================================================================>

### Problem with `GROUP BY` + `ONLY_FULL_GROUP_BY`

- When `ONLY_FULL_GROUP_BY` mode is ON (default in MySQL),
  **every column in `SELECT` must either:**

  1. Be inside an aggregate function (`SUM()`, `AVG()`, `COUNT()`, etc.), **or**
  2. Appear in the `GROUP BY` list.

👉 Otherwise MySQL says: _“Hey! I don’t know which row’s value to show for this column.”_

---

### Example

```sql
SELECT author_fname, book_id
FROM books
GROUP BY author_fname;
```

❌ Error → because `book_id` is not grouped or aggregated.
MySQL is confused:

- If an author wrote 5 books → which `book_id` should it show?

---

### Solutions

1. **Add column to `GROUP BY`**

```sql
SELECT author_fname, book_id
FROM books
GROUP BY author_fname, book_id;
```

✅ Works, but now every (author + book) is its own group → not really grouping by author anymore.

2. **Use aggregate function**

```sql
SELECT author_fname, COUNT(book_id) AS books_written
FROM books
GROUP BY author_fname;
```

✅ Works, because now `book_id` is inside `COUNT()`.

3. **Use a Window Function (alternative)**

```sql
SELECT author_fname, book_id,
       COUNT(*) OVER(PARTITION BY author_fname) AS books_written
FROM books;
```

✅ Keeps all rows (no collapse like `GROUP BY`), but also shows aggregated info.

---

### 🔑 Key Idea

- With **`GROUP BY`** → other columns must be **aggregated or grouped**.
- With **Window Functions** → you can **keep all rows** and still show group stats.

===============================================================================================================

## 🔹 1. Advantage of Window Functions

Window functions are useful because they let you:

1. **Keep all rows** (no collapsing like `GROUP BY`).
   → Example: show each employee’s salary + department average.

2. **Compare row with group stats** easily.
   → Example: find employees who earn more than their department average.

3. **Do ranking, running totals, moving averages**.
   → Example: assign row numbers, calculate cumulative sales month by month.

👉 With plain `GROUP BY` you can’t do these, because `GROUP BY` removes rows.

---

## 🔹 2. Why `OVER(PARTITION BY …)` is needed?

Think of `OVER(PARTITION BY …)` as telling SQL:
👉 _“Hey, apply this aggregate, but don’t collapse the rows. Instead, attach the result to each row in this partition (group).”_

### Example:

```sql
SELECT emp_id, department, salary,
       AVG(salary) OVER(PARTITION BY department) AS dept_avg,
       MAX(salary) OVER(PARTITION BY department) AS dept_max
FROM employees;
```

- `AVG(salary)` needs `OVER(PARTITION BY department)`
- `MAX(salary)` also needs it → otherwise SQL won’t know the "window of rows" to use.

So yes ✅ when you use aggregate functions as window functions, you **always write `OVER(...)`** to define the "window".
If you don’t partition, then the window = whole table.

---

## 🔹 3. Difference in one line

- `GROUP BY` → collapses rows into buckets, only one row per group.
- `Window Function` → calculates group stats but **keeps all original rows**.
