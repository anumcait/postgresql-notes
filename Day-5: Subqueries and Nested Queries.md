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

---

### 🔹 2. Types of Subqueries

**a) Single-row Subquery**

Returns one value.

Works with = , < , > , <= , >= , <>.

```sql
SELECT emp_name
FROM employee_master
WHERE salary = (SELECT MAX(salary) FROM employee_master);
```

**👉 Use case: Find the highest-paid employee.**

---

**b) Multiple-row Subquery**

Returns multiple values.

Works with IN , ANY , ALL.
```sql
SELECT emp_name
FROM employee_master
WHERE dept_id IN (SELECT dept_id FROM departments WHERE location = 'Delhi');
```

**👉 Use case: Show employees working in Delhi offices.**

---


**c) Multiple-column Subquery**

Returns more than one column.
```sql
SELECT emp_name, salary
FROM employee_master
WHERE (dept_id, salary) IN
      (SELECT dept_id, MAX(salary) FROM employee_master GROUP BY dept_id);
```

**👉 Use case: Find top earner in each department.**

---

**d) Correlated Subquery**

Inner query depends on outer query row.

Executes repeatedly for each row.

```sql
SELECT e1.emp_name, e1.salary
FROM employee_master e1
WHERE salary > (SELECT AVG(e2.salary)
                FROM employee_master e2
                WHERE e1.dept_id = e2.dept_id);
```

**👉 Use case: Employees earning above average salary within their department.**

---

**e) Nested Subquery**

Subquery inside another subquery.

```sql
SELECT emp_name
FROM employee_master
WHERE dept_id = (SELECT dept_id
                 FROM departments
                 WHERE location_id = 
                       (SELECT location_id FROM locations WHERE city = 'Mumbai'));
```

**👉 Use case: Employees from Mumbai location departments.**

---

**🔹 3. Where Subqueries are Used?**

In SELECT (calculated column)

```sql
SELECT emp_name,
       (SELECT dept_name FROM departments d WHERE d.dept_id = e.dept_id) AS department
FROM employee_master e;
```

In FROM (Inline View)

```sql
SELECT dept_id, AVG(salary)
FROM (SELECT * FROM employee_master WHERE job_role = 'Manager') AS mgrs
GROUP BY dept_id;
```

In WHERE (filtering rows)

```sql
SELECT emp_name
FROM employee_master
WHERE salary > (SELECT AVG(salary) FROM employee_master);
```

In HAVING (filtering groups)

```sql
SELECT dept_id, AVG(salary)
FROM employee_master
GROUP BY dept_id
HAVING AVG(salary) > (SELECT AVG(salary) FROM employee_master);
```

# 🎯 Mock Test

**Find employees who earn more than the average salary of the company.**
```sql
SELECT emp_name, salary
FROM employee_master
WHERE salary > (SELECT AVG(salary) FROM employee_master);
```

**Find employees working in departments located in Delhi.**
```sql
SELECT emp_name
FROM employee_master
WHERE dept_id IN (SELECT dept_id FROM departments WHERE location = 'Delhi');
```

**Show the highest paid employee in each department.**
```sql
SELECT emp_name, salary
FROM employee_master
WHERE (dept_id, salary) IN
      (SELECT dept_id, MAX(salary) FROM employee_master GROUP BY dept_id);
```

**List employees whose salary is above the average of their department.**
```sql
SELECT e1.emp_name, e1.salary
FROM employee_master e1
WHERE salary > (SELECT AVG(e2.salary)
                FROM employee_master e2
                WHERE e1.dept_id = e2.dept_id);
```

**Find department of employee ‘Rahul’.**

```sql
SELECT dept_name
FROM departments
WHERE dept_id = (SELECT dept_id FROM employee_master WHERE emp_name = 'Rahul');
```


# 🎤 Interview Questions & Answers

**Q1. Difference between Subquery and JOIN?**

Subquery: executes inner query first → passes result.

JOIN: combines data from multiple tables directly.

👉 Subqueries are easier to read, JOINs are usually faster.

**Q2. Can we use ORDER BY in subquery?**

Yes, but usually only with LIMIT/TOP.

**Q3. Difference between Correlated and Non-correlated Subquery?**

Correlated: depends on outer query, runs multiple times.

Non-correlated: independent, runs once.

**Q4. When to use IN, ANY, ALL?**

IN → match from list.

ANY → match condition with any value.

ALL → match condition with all values.

**Q5. Real-world DevOps use case?**

Check servers consuming above avg CPU in cluster:
```sql
SELECT server_id, cpu_usage
FROM servers s1
WHERE cpu_usage > (SELECT AVG(s2.cpu_usage) FROM servers s2 WHERE s1.cluster_id = s2.cluster_id);
```
# 🔑 Key Takeaways

- Subquery = query inside another query.
- Types: Single-row, Multi-row, Multi-column, Correlated, Nested.
- Can be used in SELECT, FROM, WHERE, HAVING.
- Subqueries are powerful but can be slower than JOINs.
- Useful in ERP reports, DevOps monitoring, audits, and analytics.

---
