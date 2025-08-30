https://javarevisited.blogspot.com/2022/12/12-database-sql-index-interview.html#
https://advancedsqlpuzzles.com/
https://platform.stratascratch.com/coding?code_type=3&difficulties=1

These are suitable for entry-level SQL developers:
What's a primary key?
What's a foreign key?
What SQL keyword can be added to a query to return only unique results?
What's the difference between a JOIN and a UNION?
What's the difference between an INNER JOIN and a LEFT OUTER JOIN?
How would you store a comma-delimited string of text in a relational database?
These would be more for mid-level or senior SQL developers:
Is it appropriate to store financial data in a column with data type FLOAT? Why or why not?
What is the difference between a clustered index and a non-clustered index? How many clustered indexes can each table have?
What does a database cursor do? Is it a good idea to use them? Why or why not?
What does a database trigger do? Is it a good idea to use them? Why or why not?

I prefer not to ask concepts and instead them solve a live coding challenge, but if I had to ask SQL concepts I'd ask the following,

- Question: What is a DISTINCT clause? Answer: Used to return only unique (different) values
- Question: What is the difference between a UNION and UNION ALL? Answer: Union extracts the rows that are being specified in the query while Union All extracts all the rows including the duplicates (repeated values) from both the queries.
- Question: How do Window Functions work? Answer: Window Functions execute calculations at the row level with a related set of values. Compared to using aggregate functions like SUM(), window functions don’t collapse the output of the rows into a single value. Rather, the output is returned for every row.
- Question: What is the difference between a primary and a foreign key? Answer: Primary keys are either a column or a set of columns in a table that hold uniquely identifiable rows in the table. While a foreign key could be a column or a set of columns in a table whose values correspond to the values of the primary key in another table.
- Question: What is the difference between LEAD and LAG? Answer: LEAD will give you the rows AFTER the row you are finding a value for. LAG will give you the rows BEFORE the row you are finding a value for

1. Difference between Primary Keys and Foreign Keys
2. The different join types. Specifically the difference between INNER JOINs (aka JOIN) and OUTER JOINs (i.e.LEFT 3. 3. OUTER JOIN aka LEFT JOIN or RIGHT OUTER JOIN aka RIGHT JOIN). Sometimes FULL OUTER JOINs are asked about too, but this is rare.
3. Difference between a Clustered Index and a Nonclustered Index
4. The purpose of a GROUP BY clause and when to use a HAVING clause vs a WHERE clause
5. Window Functions (classic ones are ROW_NUMBER() and RANK())
6. The purpose of a DISTINCT clause
7. The purpose of a TOP or LIMIT clause (depending on the database system, but they mean the same thing)
8. Less likely, but also possible that you can be asked about the UNION and UNION ALL clauses, their purposes, and differences between each other

9. The different join types
   Which JOIN should you use?
   Use INNER JOIN when you only need matching data.
   Use LEFT JOIN when you want all from the first table, even if no match.
   Use RIGHT JOIN when you want all from the second table, even if no match.
   Use FULL JOIN (via UNION) when you want everything from both tables.
10. When Should You Avoid JOIN?
    If you have a small dataset, subqueries might work fine.
    If your database is large, JOIN is better because subqueries can be slow.
11. Self-Referencing Foreign Key in MySQL & Sequelize
    A self-referencing foreign key is when a table has a foreign key that refers to its own primary key. This is useful in hierarchical relationships like:
    User table (where a user can refer to another user as a friend, manager, or parent).
12. diff b/w turncate and delete
    delete is a data manupulation language(DML), it is used to delete specific rows from table, we delete rows with where clause, while turncate
    is data defination language(DDL), it is used to delete all rows from table and it faster then delete, turncate delted records parmently bu delete can be restored.
13. diff dateTime and timestamps: ### **Key Differences**
    | Feature | DATETIME | TIMESTAMP |
    |--------------- |----------------------|--------------------|
    | **Storage** | 8 bytes | 4 bytes (smaller) |
    | **Time Zone** | Does NOT change | Converts based on time zone |
    | **Range** | `1000-01-01` to `9999-12-31` | `1970-01-01` to `2038-01-19` |
    | **Auto Update** | ❌ No automatic update | ✅ Can auto-update on row change |
    | **Best Use Case**| Event scheduling (Fixed times) | Logging, tracking last modified times |

14. Why do we use the MySQL window function?
    The SQL window function operates independently on each row. It is used for complex calculations and analyses, such as ranking or aggregating data based on specific criteria.
