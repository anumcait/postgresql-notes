# PostgreSQL Day-3 : Joins & Relationships

In [Day-2](Day-2-Datatypes&Constraints.md) we learnt how Data types and Constraints keep our database reliable.

But in real-time applications, data never lives in a single table.

One table belongs to its master table or dependant table. To connect this data we need Relationships.

And to query across multiple tables we need JOINS. So now in Day-3 we will learn about these 2 fundamentals.

## 🔹 Relationships in Databases

In postgreSQL, relationships are built using PRIMARY KEY or FOREIGN KEY

**1. One-to-One (1:1):**

- One record in table A relate to one record in table B

  📖 Example : One employee has one ID card

```sql
employee (emp_id PK, name)  
id_card  (card_id PK, emp_id FK UNIQUE)
```

  
**2. One-to-Many (1:N):**

- One record in table A relates to many records in table B.

  📖 Example: One department has many employees
```sql
department (dept_id PK, dept_name)  
employee   (emp_id PK, name, dept_id FK)
```
**3. Many-to-Many (N:N):**
- Many records in table A relates to many records in table B.

  📖 Example: Many-to-Many: Employees can work on multiple projects
```sql
employee (emp_id PK, name)
project (proj_id PK, proj_name)
employee_project (emp_id FK, proj_id FK) --junction table
```
---

## 🔹 Joins in PostgreSQL

### 1. INNER JOIN (Most Commonly Used) 

Returns only matching rows from both tables

📖 Example: List employees with their department names

```sql
SELECT e.emp_id, e.name, d.dept_name
FROM employees e
INNER JOIN department d
ON e.dept_id=d.dept_id;
```

### 2. LEFT JOIN 

Returns all rows from left table, and matching rows from right table

If not match shows NULL
  
  📖 Example: List all employees even if no department is assigned
```sql
SELECT e.emp_id, e.name, d.dept_name
FROM employee e
LEFT JOIN department d
ON e.dept_id = d.dept_id;
```
👉 Good for checking incomplete data (employees without department)

### 3. RIGHT JOIN
Returns all rows from right table. and matching rows from left
  
  📖 Example: List all departments, even if they have no employees
```sql
SELECT e.emp_id, e.name, d.dept_name
FROM employee e
RIGHT JOIN department d on e.dept_id = e.dept_id;
```
### 4. FULL JOIN (FULL OUTER JOIN)
all rows from both sides (employee + department), matched or not. If not matched returns NULL.
  
  📖 Example: Show all employees and departments, matched or not. 
```sql
SELECT e.emp_id, e.name, d.dept_name
FROM employee e
FULL JOIN department d ON e.dept_id = d.dept_id;
```
👉 Be careful with huge data. Sometimes it will crash the system.
### 5. CROSS JOIN (Cartesian product - rarely used)
- Produces Cartesian product of both tables (all combinations).

  📖 Example: For training, pair each employee with every project.
```sql
SELECT e.name, p.proj_name
FROM employee e
CROSS JOIN project p;
```
👉 Be careful with huge data. Sometimes it will crash the system.

---

## 🔹 Realtime Example
- HR System
  - employee linked to department, leaves etc
  - salary linked to employees
      With joins, we can generate reports like:
- All employees in department
- Salaries with department names
- Employee with pending leave approvals etc.

---

## 🛠️ DevOps Perspective on Joins & Relationships

When we design applications, relationships between tables are not just academic – they directly affect migrations, pipelines, monitoring, and troubleshooting.

### 🔹 1. In CI/CD Pipelines (Database Migrations)

In real DevOps workflows, schema changes (new tables, foreign keys) are pushed via migration scripts.

📖 Example:

```npm
npx sequelize-cli migration:generate --name add-department-table

npx sequelize-cli db:migrate
```
If foreign keys are missing, joins will break → your backend APIs or reporting dashboards will fail during integration testing.

That’s why pipelines run DB integration tests to validate queries like JOIN employees e ON e.dept_id = d.dept_id

---

## ✅ Do it Yourself (Hands-on)

1. Create 2 tables: department and employee. Insert sample data.
2. Write queries
    - List employees with department name (INNER JOIN)
    - List all employees, even if no department assigned (LEFT JOIN)
    - List all departments, even if no employees assigned (RIGHT JOIN)
    - Combine employees with departments, showing all (FULL JOIN)
    - Pair each employee with each project (CROSS JOIN)

---

## 📝 Key Takeaways
- Relationships define how tables connect (1:1, 1:N, N:N)
- Joins let us query across multiple tables.
- Define joins are useful for defferent reports.

---

## 🎯 Mock Test – Day-3

**Q1: What is the difference between INNER JOIN and LEFT JOIN?**

**✅ Answer:**

- INNER JOIN : Lists all data when matched in both tables
- LEFT JOIN : Lists all data of Left table and + matching rows from right

**Q2: Which join would you use to find departments without employees?**

**✅ Answer:**: RIGHT JOIN or FULL JOIN (where employee is NULL)

**Q3: Explain Many-to-Many relationship with an ERP example.**

**✅ Answer:** Employees can work on multiple projects, and projects can have multiple employees. This requires a **junction table** like employee_project.

**Q4: What does a CROSS JOIN do?**

**✅ Answer:** Returns Cartesian product - all combinations of rows between two tables

**Q5: Write a Query to display all employees who are not assigned to any department?**

**✅ Answer:**
```sql
SELECT e.emp_id, e.name
FROM employee e
LEFT JOIN department d ON e.dept_id = d.dept_id
WHERE d.dept_id IS NULL;
```








