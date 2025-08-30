### ✅ CHECK Constraints

1. **What it is**:
   A rule you can add to a column (or table) that must be **true** for every inserted/updated row.

2. **How it works**:

   - You define `CHECK (condition)` when creating a table.
   - If the condition = `TRUE` → row is valid.
   - If the condition = `FALSE` → insert/update fails.

3. **Examples**:

   - `age INT CHECK (age > 0)` → age cannot be negative.
   - `CHECK (reverse(word) = word)` → only allow palindromes.
   - `CHECK (username NOT LIKE '% %')` → no spaces in username.

4. **Error**:
   If a value breaks the rule → MySQL rejects the row with a “check constraint violated” error.

5. **Flexibility**:
   You can use numbers, strings, operators (`>`, `<`, `BETWEEN`, etc.), and even functions (`REVERSE()`, `LENGTH()`, etc.).

---

👉 In short: **CHECK lets you enforce custom rules on your data so only valid rows are saved.**

=======================================================================================

### 🏷 Naming CHECK Constraints (Summary)

1. **Default behavior**

   - MySQL auto-generates names like:
     `table_name_check_1`, `table_name_check_2`, …
   - Example: `palindromes_check_1 is violated`.

2. **Custom naming (better)**

   - Use `CONSTRAINT constraint_name` before the `CHECK`.
   - Example:

     ```sql
     CREATE TABLE users2 (
       username VARCHAR(20) NOT NULL,
       age INT,
       CONSTRAINT age_not_negative CHECK (age >= 0)
     );
     ```

   - Now error says: `Check constraint 'age_not_negative' is violated`.

3. **Advantages**

   - Easier to **read errors** (you know which rule failed).
   - Easier to **drop/alter** constraints later (using their name).

4. **Example with palindrome rule**

   ```sql
   CREATE TABLE palindromes2 (
     word VARCHAR(100),
     CONSTRAINT word_is_palindrome CHECK (REVERSE(word) = word)
   );
   ```

   - Inserting `"mom"` → ✅ works.
   - Inserting `"mama"` → ❌ error: `word_is_palindrome violated`.

---

👉 In short: **Naming constraints makes errors clearer and maintenance easier** than relying on MySQL’s generic `check_1, check_2`.

=====================================================================================

### 🔑 Two Main Ideas Here

1. **UNIQUE Constraint with Multiple Columns**

   - You can tell SQL:
     "This combination of columns must be unique, even if each column alone is not unique."
   - Example:

     - Company name can repeat.
     - Address can repeat.
     - But **same company at same address** → ❌ Not allowed.

   ```sql
   CREATE TABLE companies (
       name VARCHAR(100) NOT NULL,
       address VARCHAR(200) NOT NULL,
       CONSTRAINT unique_name_address UNIQUE (name, address)
   );
   ```

   ✅ Allowed:

   - (Blackbird Auto, 123 Spruce)
   - (Luigi’s Pies, 123 Spruce)
     ❌ Not Allowed:
   - (Luigi’s Pies, 123 Spruce) again

---

2. **Check Constraints with Multiple Columns**

   - A `CHECK` can compare **values across columns**.
   - Example: Houses — you must sell for **at least what you paid**.

   ```sql
   CREATE TABLE houses (
       purchase_price INT NOT NULL,
       sale_price INT NOT NULL,
       CONSTRAINT sale_ge_purchase
           CHECK (sale_price >= purchase_price)
   );
   ```

   ✅ Allowed: (100, 200)
   ❌ Not Allowed: (300, 250) → check fails

---

### 🌟 Why This Is Useful

- **UNIQUE across multiple columns** → helps prevent duplicate records when no single column is unique.
- **CHECK across multiple columns** → enforces logical business rules.

👉 So you’re not limited to “one column = unique” or “check one column”; you can combine columns for stronger rules.

---

=========================================================================================

- **ALTER TABLE** is used to modify an existing table (add, drop, rename, change type, add/remove constraints, etc.).

- **Syntax (add column):**

  ```sql
  ALTER TABLE table_name
  ADD [COLUMN] column_name datatype [constraints] [DEFAULT value];
  ```

- If you add a column:

  - Existing rows get **NULL** if the column allows nulls.
  - If `NOT NULL` → SQL inserts a default (empty string for text, 0 for numbers).
  - You can also define your own `DEFAULT`.

**Examples:**

```sql
-- Add phone column (nullable)
ALTER TABLE companies
ADD COLUMN phone VARCHAR(15);

-- Add employee_count column (not null, default 1)
ALTER TABLE companies
ADD COLUMN employee_count INT NOT NULL DEFAULT 1;
```

👉 Null vs. default behavior is the key thing to remember.

==========================================================================================

- Use **ALTER TABLE** to remove columns.

- **Syntax:**

  ```sql
  ALTER TABLE table_name
  DROP [COLUMN] column_name;
  ```

- The word **COLUMN** is optional (but clearer).

- Once dropped → the column and its data are **permanently gone** (like dropping a table).

- Works even if the column had values (MySQL doesn’t care).

**Examples:**

```sql
-- Drop phone column
ALTER TABLE companies DROP COLUMN phone;

-- Drop employee_count column
ALTER TABLE companies DROP COLUMN employee_count;
```

👉 Dropping is destructive. There’s no undo unless you restore from backup.

===============================================================================

### ✅ Add Constraint

- You can add new rules to an existing table.
- **Syntax (check example):**

  ```sql
  ALTER TABLE table_name
  ADD CONSTRAINT constraint_name CHECK (condition);
  ```

- Example: purchase price must be ≥ 0

  ```sql
  ALTER TABLE houses
  ADD CONSTRAINT positive_price CHECK (purchase_price >= 0);
  ```

---

### ❌ Drop Constraint

- Remove an existing constraint by its name.
- **Syntax:**

  ```sql
  ALTER TABLE table_name
  DROP CONSTRAINT constraint_name;
  ```

- Example:

  ```sql
  ALTER TABLE houses
  DROP CONSTRAINT positive_price;
  ```

---

👉 Notes:

- You **must know the constraint name** (better to use custom names).
- Works for **CHECK, UNIQUE, PRIMARY KEY, FOREIGN KEY** constraints.
- Use docs if syntax is confusing — `ALTER TABLE` is like a Swiss army knife.

====================================================================================

### 🔄 Rename a Table

1. **Method 1 (short form):**

   ```sql
   RENAME TABLE old_name TO new_name;
   ```

2. **Method 2 (with ALTER):**

   ```sql
   ALTER TABLE old_name RENAME TO new_name;
   ```

✅ Both do the same thing.

---

### 🔄 Rename a Column

- **Syntax:**

  ```sql
  ALTER TABLE table_name
  RENAME COLUMN old_column TO new_column;
  ```

⚠️ Note: `COLUMN` keyword is **required** here (unlike ADD/DROP where it’s optional).

---

👉 Example:

```sql
-- Rename table
RENAME TABLE companies TO suppliers;

-- Rename column
ALTER TABLE companies
RENAME COLUMN name TO company_name;
```

---
