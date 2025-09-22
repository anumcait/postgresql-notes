# Postgres Day-5: 📘 Subqueries & Nested Queries

Until now, we learned advanced queries with GROUP BY, HAVING, and aggregate functions that help in summarizing data.

But in real-world ERP/DevOps reporting, we often need to filter data based on another query’s result, or compare one dataset with another dynamically.

That’s where Subqueries (Nested Queries) come in.

**Today, we cover:**

- What are Subqueries?
- Types (Single-row, Multi-row, Multi-column, Correlated, Nested)
- Subquery usage in SELECT, FROM, WHERE, HAVING
- Real-world use cases
- Mock test queries
- Interview Q&A

### 🔹 1. What is a Subquery?

- A Subquery is a query written inside another query.
- Also called Nested Query or Inner Query.
- Always enclosed in ( ).
- Executes first → result goes to Outer Query.

**📖 Example: Employees earning above average salary**
```sql
SELECT emp_name, salary
FROM employee_master
WHERE salary > (SELECT AVG(salary) FROM employee_master);
```
**👉 Real use case: HR wants to find employees above company average pay.**

