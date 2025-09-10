# 📘 PostgreSQL - Day 2: Data Types & Constraints

"In [**Day-1**](https://github.com/anumcait/postgresql-notes/blob/main/Day-1-Basics.md), We learnt about what is a database is , types of databases, basics of postgresql, how to install and create database with simple creation of table with select query.

Today in **Day-2** we will learn two topics **data types and constraints**, for any database not only postgresql these two are foundations.

These two decide how data is stored into tables and how reliable your database becomes."

---

## What is a Data type
A **Data type** is an attribute that **classifies the kind of data** that a **variable holds**.
For Example: In an employee table, empid holds number or charcter based on requirement, Employee name must be character etc.

## PostgreSQL Data Types:
Postgresql is **powerful** because it supports **rich data types**.

### 1. Numeric Datatypes:
- `INTEGERS` -> Holds whole numbers (Ex: 10, 100, -50)
- `NUMERIC(10,2)` -> Exact decimal numbers, good for money (like invoice total, rate etc.)
- `SERIAL` -> Auto-increment numbers (Used for IDs generation etcc)

📖 Example:
```sql
CREATE TABLE salaries (
  emp_id SERIAL PRIMARY KEY,
  amount NUMERIC(10,2) NOT NULL
);
```
---

### 2. Text Types
- CHAR(n) -> Fixed length string (pads with spaces)
- VARCHAR(n) -> Variable length string, max n, if given VARCHAR(10), only 3 characters are there in data, it won't take spaces
- TEXT -> Unlimited length string

📖 Example:
```sql
CREATE TABLE departments (
  dept_id SERIAL PRIMARY KEY,
  dept_name VARCHAR(50) NOT NULL
);
```
---

### 3. Date/Time types
- DATE -> 2025-09-10
- TIME -> 14:30:00
- TIMESTAMP -> 2025-09-08 14:30:00

📖 Example:
```sql
CREATE TABLE attendance (
  emp_id INT,
  login_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```
---

### 4. Boolean Type
- BOOLEAN -> TRUE / FALSE Used for active status, flags, etc.

📖 Example:
```sql
ALTER TABLE employees
ADD COLUMN is_active BOOLEAN DEFAULT TRUE;
```
---

### 5. JSON / JSONB
- JSON -> Raw JSON text
- JSONB -> Binary JSON (faster for queries)

📖 Example:
```sql
CREATE TABLE audit_logs (
  id SERIAL PRIMARY KEY,
  details JSONB
);
INSERT INTO audit_logs(details)
VALUES ('{"user":"Tom","action":"login","status":"success"});
```
---

# 🔹 PostgreSQL Constraints
Constraints keep our data clean, consistent, and reliable.

### 1. NOT NULL
- Prevents empty values.

📖 Example:
```sql
CREATE TABLE employees (
  emp_id SERIAL PRIMARY KEY,
  name VARCHAR(100) NOT NULL
);
```
---

### 2. UNIQUE
- Ensures no duplicate values.

📖 Example:
```sql
ALTER TABLE employees
ADD constraint unique_email UNIQUE(email);
```
### 3. PRIMARY KEY
- Combines **NOT NULL + UNIQUE**
- Identifies each row uniquely.

📖 Example:
```sql
CREATE TABLE departments (
  dept_id SERIAL PRIMARY KEY,
  dept_name VARCHAR(50) UNIQUE NOT NULL
);
```
* here we cannot use two primary keys for one table, so first column given primary key

---

### 4. FOREIGN KEY
- Links one table to another.
- Ensures referential integrity.

📖 Example:
```sql
CREATE TABLE employee (
  emp_id SERIAL PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  dept_id INT REFERENCES departments(dept_id)
);
```
---

### 5. CHECK
- Validates conditions on values.

📖 Example:
```sql
ALTER TABLE employees
ADD CONSTRAINT check_salary CHECK (salary > 0)
```
---

### 6. DEFAULT
- Assigns a default value if none is given.

📖 Example:
```sql
ALTER TABLE employees
ADD COLUMN created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP;
```
---

Let's see the Real-World Examples (HR Schema)

```sql

-- Departments
CREATE TABLE departments (
    dept_id SERIAL PRIMARY KEY,
    dept_name VARCHAR(50) UNIQUE NOT NULL
);

-- Employees
CREATE TABLE employees (
    emp_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    dept_id INT REFERENCES departments(dept_id),
    salary NUMERIC(10,2) CHECK (salary > 0),
    is_active BOOLEAN DEFAULT TRUE
);

```

💡 Now we have proper relationships: each employee belongs to a department, email is unique, salary must be positive, and status defaults to active.

---

### Let's see this in the DevOps Perspective, How these data types and constriants used.
- Constraints prevent **bad data** before it enters DB.
- In DevOps pipelines:
    - **Migrations** enforce constraints automatically.
    - Example: A CI/CD pipeline fails if migration introduces conflicting constraints
- JSON/JSONB columns are often used in **logging and monitoring** systems.
- DEFAULT + TIMESTAMP is heavily used for **audit trails**.

Practice Excercise
1. Create a `projects` table with:
    - `project_id` (Serial,PK)
    - `name` (Varchar, not null, unique)
    - `start_date` (date, default = today)
    - `budget` (numeric, must be > 0)
2. Link employees to projects with a works_on table.
3. Insert 3 departments, 5 employees, adn assign them to projects
4. Write a query to fetch all employees in **IT department** working on **Project Alpha**.

---

## 🎤 Mock Interview Q&A
**Q1: Difference between CHAR, VARCHAR, and TEXT?**
👉 CHAR = fixed length and spans spaces, VARCHAR = variable with limit, TEXT = unlimited

**Q2: What is a foreign key?**
👉 A reference that ensures data in one table matches valid records in another table.

**Q3: Why do we need constraints in DevOps pipelines?**
👉 To enforce data integrity, avoid garbage data, and fail fast in migrations

---

# 🎯 Recap (Day-2)
- Learned PostgreSQL **data types** (numeric, text, date, boolean, JSON)
- Understood **constraints** (NOT NULL, UNIQUE, PK, FK, CHECK, DEFAILT).
- Built a **mini HR Schema** with Employees & Departments.
- Saw DevOps perspective: constraints + migrations = safe deployments.






