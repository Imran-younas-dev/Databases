### **UNION vs UNION ALL**

1. **UNION**

   - Combines results from **two or more SELECT queries**.
   - It **removes duplicates** (only unique rows are shown).
   - Extra step = sorting + removing duplicates → can be a little slower.

   ✅ Example:

   ```sql
   SELECT name FROM employees
   UNION
   SELECT name FROM managers;
   ```

   → If the same `name` appears in both, it will show only **once**.

---

2. **UNION ALL**

   - Combines results from **two or more SELECT queries**.
   - It **keeps duplicates** (shows everything as it is).
   - Faster than UNION (no extra work to remove duplicates).

   ✅ Example:

   ```sql
   SELECT name FROM employees
   UNION ALL
   SELECT name FROM managers;
   ```

   → If the same `name` appears in both, it will show **multiple times**.

---

### **Quick Difference**

| Feature             | UNION                                     | UNION ALL                                           |
| ------------------- | ----------------------------------------- | --------------------------------------------------- |
| Removes Duplicates? | ✅ Yes                                    | ❌ No                                               |
| Performance         | Slower (because it checks for duplicates) | Faster                                              |
| When to Use         | When you only want **unique** results     | When you want **all results**, including duplicates |

---

💡 **Shortcut to remember:**

- **UNION** = Unique.
- **UNION ALL** = All.
