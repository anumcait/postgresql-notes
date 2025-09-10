# 📘 PostgreSQL - Day 1: Database Fundamentals & Postgresql basics
## In this we will cover
- What is a Database?
- Types of Databases
- What is an RDBMS?
- Introduction to SQL
- Why PostgreSQL?
- Installing PostgreSQL
- Basic `psql` usage
- Creating first database and table

---
## 🧠 1. What is a Database?
A **Database** is a structured collection of data that can be easily accessed, managed and updated.
### ✅ Real-life example:
Think of a **library**
- Library = Database
- Books = Data
- Book Shelves = Tables
- Catalog System = Database engine

### 🔍 Explanation:
The **catalog system** in a real-world helps you :
- **Locate** books (Search by title, author, topic)
- **Organize** books (Which shelf, which section)
- **Maintain** Info (new books, removed books, borrow/return status)

Just like that, the **Database Engine** in a database:
- **Locates** data using indexes and queries
- **Organizes** data into structured formats ( tables, schemas)
- **Maintains** consistency, Integrity, and rules (constraints, relationships)
- **Process** SQL Commands (like SELECT, INSERT, UPDATE, DELETE etc.)

---

## 🧩 2. Types of Databases
### 🗃️ Relational Databases (RDBMS)
- Stores data in **tables** (rows and columns)
- Use **SQL** to manage data
- Example : PostgreSQL, Mysql, SQLite, Oracle

### 🧱 NoSQL Databases
- Schema less, flexible structure
- Good for unstructured or semi-structured data
- Example: MongoDB, Firebase, Redis

### ⚙️ Others
- Graph Databases (Neo4J)
- Time-series Databases (InfluxDB)
- Object-orinted Databases (db4o, ObejctDB)

---

## 🧱 What is an RDBMS?
**RDBMS** = *Relational Database Management System*
- Software used to create, manage, and interact with relational databases
- Data is stored in **tables**
- Tables can be related to each other using keys (Primary key, Foreign key)

### 🧾 Example Table: `employee`

| id | name    | age |
|----|---------|-----|
| 1  | Alice   | 20  |
| 2  | Bob     | 22  |

---

## 💬 4. What is SQL?

**SQL** = Structured Query Language
It is used to perform CRUD Operations:
- **C**: `CREATE` — create database objects (tables, etc.)
- **R**: `SELECT` — read data
- **U**: `UPDATE` — update data
- **D**: `DELETE` — delete data

**Example:**

```sql
SELECT name FROM students WHERE age > 20;
```
## 🐘 5. Why PostgreSQL?

- ✅ Open-source, stable, and powerful

- ✅ Supports advanced features: JSON, arrays, window functions

- ✅ Extensible via plugins (e.g.,PostGIS for geodata)

- ✅ Follows SQL Standards closely

- ✅ Used by major companies: Apple, Instagram, Reddit, etc.

### 🛠️ 6. Installing PostgreSQL

Official Documentation : https://www.postgresql.org/docs/

**✅ Windows / macOS**

Download installer from: https://www.postgresql.org/download/

Installer includes:
```
PostgreSQL Server

pgAdmin (GUI)

StackBuilder (optional)
```
**✅ Linux (Ubuntu/Debian)**
```
sudo apt update
sudo apt install postgresql postgresql-contrib
```

## **💻 7. Using the psql CLI**

**▶️ Start PostgreSQL CLI (Linux/Mac)**

```
sudo -U postgres psql
```
**▶️ Useful psql Commands**

| Command  | Description |
|--------- |-------------|
| \q       | Quit psql   |
| \l       | List Databases|
| \c dbname| connect to a database|
| \dt      | List tables in current database|
| \du      | List users|
| \h       | Get help on SQL Commands|

### 🧪 8. Create Your First Database
**▶️ Step 1: Create a database**

``
CREATE DATABASE hr;
``

**▶️ Step 2: Connect to the new database**

``
\c hr
``

**▶️ Step 3: Create a table**

``
CREATE TABLE employee (
  id SERIAL PRIMARY KEY,
  name TEXT NOT NULL,
  age INT
);
``

**▶️ Step 4: Insert data**
```
INSERT INTO employee (name, age) VALUES ('Alice', 20);

INSERT INTO employee (name, age) VALUES ('Bob', 22);
```

**▶️ Step 5: Query the table**

`
SELECT * FROM employee;
`

### ✅ Output:
| id | name    | age |
|----|---------|-----|
| 1  | Alice   | 20  |
| 2  | Bob     | 22  |

