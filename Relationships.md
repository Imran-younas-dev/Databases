### Relationships

Relationships mean connecting tables so real-world messy data makes sense.

# 🔑 3 Types of Relationships in Databases

1. **One-to-One (1:1)**

- Rare.
- Example: `customers` ↔ `customer_details`.
- Each customer has exactly one details row, and vice versa.

2. **One-to-Many (1\:N)**

- **Most common.**
- Example: `books` ↔ `reviews`.
- One book can have many reviews.
- Each review belongs to exactly one book.

3. **Many-to-Many (M\:N)**

- Also common.
- Example: `books` ↔ `authors`.
- A book can have many authors.
- An author can write many books.
- Needs a **join/bridge table** to represent.

---

# ⚡ Takeaway

- **1:1** = one ↔ one (rare).
- **1\:N** = one ↔ many (most common, we’ll focus here).
- **M\:N** = many ↔ many (requires extra table).

---

# Memory trick:

- 1:1 = **exclusive pair**
- 1\:N = **parent → children**
- M\:N = **teamwork (both sides many)**

==================================================================================================================

`Primary Key (PK)`: Unique ID for a row (e.g., customer.id, order.id).
`Foreign Key (FK)`: A column that references PK in another table (e.g., orders.customer_id → customers.id).
`Ensures data integrity`: can’t insert order with non-existing customer.
`What is a CROSS JOIN?`

- It combines every row of table A with every row of table B.
- It doesn’t care about keys, matching, or conditions.
- Just brute force → all possible combinations.
- e.g SELECT \* FROM customers, orders;

`What is INNER JOIN?`

- It returns rows only when there is a match between two tables based on a condition (usually primary key ↔ foreign key).
- Unlike CROSS JOIN, it does not give all combinations, only the meaningful ones.
  e.g Select p.col1, c.col2 from parent_table as p join child_table as c on p.id = c.parentId
- Q: parent always should be in first?

  - No — the parent table does not always have to be written first in a JOIN.
  - The order in the FROM ... JOIN ... clause doesn’t matter logically — the database engine will figure it out.
    ` Inner Joins With Group By` :
    e.g select c.first_name as fn, last_name as ln, count(\*) as tc, sum(o.amount) from customers as c join orders as o on c.id = o.customer_id group by fn, ln order by tc;

`What is LEFT JOIN?`

- It returns all rows from the left table (the first one you write).
- If there is a match in the right table → it shows the data.
- If no match → it still shows the left row, but with NULL for the right side.

`What is RIGHT JOIN?`

- It returns all rows from the right table (the second one you write).
- If there is a match in the left table → it shows the data.
- If no match → it still shows the right row, but with NULL for the left side.
