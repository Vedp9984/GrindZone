
# 📘 SQL (Structured Query Language) - Complete Notes

---
## 🧱 1. SQL Categories

### ✅ DDL (Data Definition Language)
Used to define or modify the structure of database objects like tables.

| Command | Description |
|---------|-------------|
| `CREATE` | Creates a new table, view, or other object |
| `ALTER`  | Modifies an existing database object |
| `DROP`   | Deletes database objects |

#### 🔹 Example:
```sql
CREATE TABLE Students (
  ID INT PRIMARY KEY,
  Name VARCHAR(50),
  Age INT
);

ALTER TABLE Students ADD Email VARCHAR(100);

DROP TABLE Students;
```

---

### ✅ DML (Data Manipulation Language)
Used for manipulating data stored in the database.

| Command | Description |
|---------|-------------|
| `SELECT` | Retrieves data |
| `INSERT` | Adds new data |
| `UPDATE` | Modifies existing data |
| `DELETE` | Removes data |

#### 🔹 Example:
```sql
INSERT INTO Students (ID, Name, Age) VALUES (1, 'Ved', 21);

SELECT * FROM Students;

UPDATE Students SET Age = 22 WHERE ID = 1;

DELETE FROM Students WHERE ID = 1;
```

---

### ✅ DCL (Data Control Language)
Used to control access to data.

| Command | Description |
|---------|-------------|
| `GRANT`  | Gives user access privileges |
| `REVOKE` | Removes access privileges |

#### 🔹 Example:
```sql
GRANT SELECT, INSERT ON Students TO user_name;

REVOKE INSERT ON Students FROM user_name;
```

---

### ✅ TCL (Transaction Control Language)
Used to manage transactions in SQL.

| Command    | Description                          |
|------------|--------------------------------------|
| `COMMIT`   | Saves the changes made by the transaction |
| `ROLLBACK` | Reverts changes if something goes wrong |
| `SAVEPOINT`| Sets a point to which you can roll back |

#### 🔹 Example:
```sql
BEGIN;

INSERT INTO Students VALUES (2, 'Amit', 23);
SAVEPOINT sp1;

UPDATE Students SET Age = 24 WHERE ID = 2;

ROLLBACK TO sp1;

COMMIT;
```

---

## 🔗 2. SQL Joins

Joins are used to combine rows from two or more tables based on related columns.

### 🔹 INNER JOIN
Returns only matching rows from both tables.

```sql
SELECT A.Name, B.Course
FROM Students A
INNER JOIN Enrollments B ON A.ID = B.StudentID;
```

### 🔹 LEFT JOIN
Returns all rows from the left table and matched rows from the right table.

```sql
SELECT A.Name, B.Course
FROM Students A
LEFT JOIN Enrollments B ON A.ID = B.StudentID;
```

### 🔹 RIGHT JOIN
Returns all rows from the right table and matched rows from the left table.

```sql
SELECT A.Name, B.Course
FROM Students A
RIGHT JOIN Enrollments B ON A.ID = B.StudentID;
```

### 🔹 FULL JOIN
Returns all rows when there is a match in either table.

```sql
SELECT A.Name, B.Course
FROM Students A
FULL OUTER JOIN Enrollments B ON A.ID = B.StudentID;
```

---

## 🔍 3. Subqueries

A subquery is a query nested inside another query.

### 🔹 Example:
Get names of students who are older than the average age.

```sql
SELECT Name FROM Students
WHERE Age > (SELECT AVG(Age) FROM Students);
```

---

## 👁️ 4. Views

A view is a virtual table based on the result of a SELECT query.

### 🔹 Example:
```sql
CREATE VIEW YoungStudents AS
SELECT Name, Age FROM Students
WHERE Age < 25;

SELECT * FROM YoungStudents;
```

---

## ⚡ 5. Indexes

Indexes are used to speed up the search queries.

### 🔹 Example:
```sql
CREATE INDEX idx_name ON Students(Name);
```

👉 Note: Indexes speed up `SELECT` queries but may slow down `INSERT`, `UPDATE`, and `DELETE`.

---

## 📌 Summary Table

| Category | Commands |
|----------|----------|
| DDL | CREATE, ALTER, DROP |
| DML | SELECT, INSERT, UPDATE, DELETE |
| DCL | GRANT, REVOKE |
| TCL | COMMIT, ROLLBACK, SAVEPOINT |

---

## 🔄 6. Logical Query Processing Order

SQL is not executed in the order you write it, but in a specific logical order:

| Step | Clause | What Happens Here |
|------|--------|-------------------|
| 1 | FROM | Selects the table(s) and handles joins |
| 2 | WHERE | Filters rows based on conditions |
| 3 | GROUP BY | Groups rows (if used) |
| 4 | HAVING | Filters groups (used after GROUP BY) |
| 5 | SELECT | Picks specific columns or computed values |
| 6 | ORDER BY | Sorts the result |
| 7 | LIMIT | Limits the number of returned rows (optional) |

---

🧠 **Pro Tip for Students:**
- Practice queries on a real SQL editor like **MySQL Workbench**, **SQLite**, or **PostgreSQL**.
- Use `EXPLAIN` keyword to understand how SQL engine executes your query.