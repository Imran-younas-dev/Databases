### 🔹 Clustered Index (default, usually Primary Key)

- By default, most databases (like SQL Server, MySQL InnoDB) create a **clustered index** on the **primary key**.
- That’s why your table rows are physically stored in order of the `id` (or primary key).
- You can have **only one clustered index** per table.

👉 Example:

```sql
CREATE TABLE Customers (
  CustomerID INT PRIMARY KEY,   -- clustered index by default
  Name VARCHAR(100),
  Email VARCHAR(100)
);
```

Here, `CustomerID` is the clustered index.

---

### 🔹 Nonclustered Index (for searching/filtering)

- Used when you want to **search, filter, or sort** on other columns (not the primary key).
- You can make **many nonclustered indexes** on a table.
- You can also create **composite nonclustered indexes** (on multiple columns).

👉 Example:
If your website needs to filter customers by `Name` + `Email`, you can create:

```sql
CREATE NONCLUSTERED INDEX idx_Customers_NameEmail
ON Customers (Name, Email);
```

Now, searching by name/email becomes much faster.

---

### 📌 Refined Understanding (your version, corrected)

- **Clustered index** → already there by default (on primary key like `id`).
- **Nonclustered index** → extra/custom indexes (single or composite) that you create for faster searching/filtering on specific columns (like a website search).

---

⚡ One-liner:
**Clustered index = default “table sorter” (primary key).
Nonclustered index = extra search helpers (can be composite).**
