## 📘 Advanced Queries (Functions, Operators & Expressions, GROUP BY, HAVING, Aggregate Functions)  

In [Day-3](Day-3-Joins%26Relationships.md) we learned the basics of creating tables, inserting data using data types, constraints, and relationships and joins.

But, in real-world usecases like reporting, data analytics (ERP), We rarely look at one row, instead we need to **calculate, transform, or manipulate data** while querying.

That's where today's topic comes in picture.
Today, we cover
- Functions
- Operators & Expressions
- Aggregate Functions
- GROUP BY
- HAVING
These are the real backbone of reporting queries.

---

### 🔹 1. Functions in PostgreSQL

Functions are pre-built operations that simplify data handling.

**Common Functions**

- `Lower(text)` → Converts to lower case
- `Upper(text)` → Converts to UPPER CASE
- `Length(text)` → Returns length of a string
- `NOW()` → Current timestamp
- `AGE(date1,date2)` → Finds difference in dates

**📖 Example:**

```sql

SELECT emp_name, LENGTH(emp_name) AS name_length,
        UPPER(department) AS dept_upper,
        AGE(NOW(), joining_date) AS experience
FROM employee_master;

``` 
**👉 Real use case:**

- HR wants to calculate employee experience directly from DB.

- ERP wants standard uppercase department codes.

### 🔹 2. Operators & Expressions

Operators are symbols to perform calculation

**Arithmetic Operators:**

- `+` (addition)
- `-` (subtraction)
- `*` (multiplication)
- `/' (division)
- `%` (modulus)

**📖 Example:**

```sql

SELECT emp_id, basic_salary, basic_salary * 0.10 AS bonus
FROM employee_master;

``` 

👉 Real use case: Calculate employee bonuses dynamically.

**Comparison Operators:**

`=` , `!=` , `<` , `>`, `<=` , `>=`

**📖 Example:**

```sql

SELECT emp_id, salary
FROM employee_master
WHERE salary > 5000;

``` 
👉 Real use case: Filter employees eligible for higher tax bracket.

---

### 🔹 3. Aggregate Functions

Aggregate functions summarize **multiple rows** into a single result.

- count(*) → Number of rows
- sum(column) → Total
- AVG(column) → Average
- MIN(column) → Lowest
- MAX(column) → Highest

**📖 Example:**

```sql

SELECT COUNT(*) AS total_employees,
       AVG(salary) AS avg_salary,
       MAX(salary) AS highest_salary
FROM employee_master;

``` 
  
👉 Real use case: HR summary dashboard for workforce analytics.

### 🔹 4. GROUP BY

Group rows and applies aggregate functions

**📖 Example:** Employees per department

```sql

SELECT department, COUNT(*) AS total_employees
FROM employee_master
GROUP BY department;

``` 
👉 Real use case: HR headcount per department.

### 🔹 5. HAVING Clause

Filters results **after Grouping** (while `WHERE` filters rows after grouping)

**📖 Example:** Employees per department

```sql

SELECT department, COUNT(*) AS total_employees
FROM employee_master
GROUP BY department
HAVING COUNT(*) > 5;

``` 
👉 Real use case: Only show large teams in HR analytics.

### 🎯 Mock Test

**Write a query to count employees in each department.**
```sql
SELECT department_id, COUNT(employee_id) AS employee_count
FROM employees
GROUP BY department_id;
```
👉 Groups by department and counts employees in each.

**Find total salary paid in each department.**
```sql
SELECT department_id, SUM(salary) AS total_salary
FROM employees
GROUP BY department_id;
```
👉 Uses SUM() aggregate function per department.

**Show departments where employee count > 10.**
```sql
SELECT department_id, COUNT(employee_id) AS employee_count
FROM employees
GROUP BY department_id
HAVING COUNT(employee_id) > 10;
```
👉 HAVING is used to filter after grouping.

**Show maximum salary in each department and filter only those where max salary > 80,000.**
```sql
SELECT department_id, MAX(salary) AS max_salary
FROM employees
GROUP BY department_id
HAVING MAX(salary) > 80000;
```
👉 Returns only departments with high salary brackets.

**Calculate average salary per job role.**
```sql

SELECT job_role, AVG(salary) AS avg_salary
FROM employees
GROUP BY job_role;

```
👉 Average is calculated role-wise.

**Write a query to calculate experience in years for each employee.**
```sql
SELECT employee_id, first_name, last_name,
       EXTRACT(YEAR FROM AGE(CURRENT_DATE, hire_date)) AS experience_years
FROM employees;
```
👉 AGE() finds the difference between current date and hire date.
👉 EXTRACT(YEAR FROM …) gives experience in years.


### 🎤 Interview Questions & Answers

**Q1. Difference between WHERE and HAVING?**

**WHERE:** filters rows before aggregation.

**HAVING:** filters groups after aggregation.

👉 Example:

```sql

SELECT department_id, AVG(salary)
FROM employees
WHERE salary > 30000   -- filters rows first
GROUP BY department_id
HAVING AVG(salary) > 50000; -- filters aggregated groups
```

**Q2. What is the difference between COUNT(*) and COUNT(column)?**

**COUNT(*):** counts all rows (including NULLs).

**COUNT(column)**: counts only non-NULL values in that column.

**Q3. Can you use aggregate functions without GROUP BY?**

Yes. If no GROUP BY is specified, the aggregate applies to the entire result set.

```sql
SELECT AVG(salary) FROM employees;  -- whole table avg
```

**Q4. Explain the difference between DISTINCT and GROUP BY.**

**DISTINCT:** removes duplicates, no aggregates.

**GROUP BY:** groups rows and can be used with aggregates.

👉 Example:
```sql
SELECT DISTINCT department_id FROM employees;   -- just unique depts
SELECT department_id, COUNT(*) FROM employees GROUP BY department_id;  -- counts per dept
```

**Q5. Can HAVING be used without GROUP BY?**

Yes. It acts like WHERE, but less common.
```sql
SELECT AVG(salary) 
FROM employees 
HAVING AVG(salary) > 50000;
```

**Q6. Give real-world ERP/DevOps use cases of aggregates.**

Scenario: You want to see the total number of leave applications filed by employees per department, but only departments with more than 10 applications.
```sql
SELECT department_id, COUNT(*) AS leave_count
FROM leave_applications
GROUP BY department_id
HAVING COUNT(*) > 10;
```

**Q7. Difference between INNER JOIN and using GROUP BY for aggregations?**

**INNER JOIN:** combines rows from multiple tables.

**GROUP BY:** aggregates rows into summary values.

👉 You often use them together for reporting.

**Q8. Performance Question**

**What’s faster: WHERE or HAVING?**

**WHERE is faster** since it reduces rows before grouping.

HAVING works after grouping, so **it processes more data**.


### 🔑 Key Takeaways

**Functions** simplify string, numeric, and date handling.

**Operators** help in calculations and filtering.

**Aggregate functions** provide summaries.

**GROUP BY **organizes rows into groups.

**HAVING** filters after aggregation.

👉 Real DevOps + ERP reports always use a mix of these.

