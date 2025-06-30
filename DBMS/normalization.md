# 📘 Database Normalization Explained Simply

## ✅ Purpose of Normalization

Normalization is the process of organizing data in a database to:
- Reduce data redundancy (duplicate data)
- Ensure data integrity
- Make the database efficient and easy to maintain

---

## 🔢 1NF (First Normal Form)

### 📌 Rule:
- Each column should contain **atomic (indivisible)** values.
- Each row should be **unique**.

### ❌ Bad Table (Not in 1NF):

| StudentID | Name     | Subjects           |
|-----------|----------|--------------------|
| 1         | Alice    | Math, Science      |
| 2         | Bob      | English, History   |

- Here, `Subjects` column has multiple values → violates 1NF.

### ✅ Converted to 1NF:

| StudentID | Name  | Subject   |
|-----------|-------|-----------|
| 1         | Alice | Math      |
| 1         | Alice | Science   |
| 2         | Bob   | English   |
| 2         | Bob   | History   |

---

## 🔢 2NF (Second Normal Form)

### 📌 Rule:
- Must be in **1NF**
- No **partial dependency** (i.e., non-key attributes must depend on the whole primary key)

### ❌ Bad Table (in 1NF but not 2NF):

**Primary Key: (StudentID, Subject)**

| StudentID | Subject | StudentName |
|-----------|---------|-------------|
| 1         | Math    | Alice       |
| 1         | Science | Alice       |
| 2         | English | Bob         |

- `StudentName` depends only on `StudentID` (part of the key), not the whole `(StudentID, Subject)` → violates 2NF.

### ✅ Converted to 2NF:

**Student Table:**

| StudentID | StudentName |
|-----------|-------------|
| 1         | Alice       |
| 2         | Bob         |

**Enrollment Table:**

| StudentID | Subject   |
|-----------|-----------|
| 1         | Math      |
| 1         | Science   |
| 2         | English   |

---

## 🔢 3NF (Third Normal Form)

### 📌 Rule:
- Must be in **2NF**
- No **transitive dependency** (i.e., non-key → non-key → key)

### ❌ Bad Table (in 2NF but not 3NF):

| StudentID | StudentName | DeptID | DeptName |
|-----------|-------------|--------|----------|
| 1         | Alice       | D1     | CS       |
| 2         | Bob         | D2     | Math     |

- `DeptName` depends on `DeptID`, not directly on `StudentID` → transitive dependency

### ✅ Converted to 3NF:

**Student Table:**

| StudentID | StudentName | DeptID |
|-----------|-------------|--------|
| 1         | Alice       | D1     |
| 2         | Bob         | D2     |

**Department Table:**

| DeptID | DeptName |
|--------|----------|
| D1     | CS       |
| D2     | Math     |

---

## 🔢 BCNF (Boyce-Codd Normal Form)

### 📌 Rule:
- Stronger version of 3NF
- For every functional dependency `A → B`, `A` must be a **super key**

### ❌ Bad Table (in 3NF but not BCNF):

| Teacher | Subject | Dept  |
|---------|---------|-------|
| Alice   | Math    | CS    |
| Alice   | Physics | CS    |

- `Teacher → Dept` ✅
- `Teacher + Subject` is the primary key

But `Teacher` is not a super key → violates BCNF

### ✅ Converted to BCNF:

**TeacherDept Table:**

| Teacher | Dept |
|---------|------|
| Alice   | CS   |

**TeacherSubject Table:**

| Teacher | Subject |
|---------|---------|
| Alice   | Math    |
| Alice   | Physics |

---

## 🔄 Denormalization

### 📌 Definition:
Denormalization is the process of combining normalized tables to **improve read performance**, even if it means adding **some redundancy**.

### ✅ When to use:
- For faster queries (fewer joins)
- In read-heavy systems (e.g., reporting, dashboards)

### 🔁 Example:

Instead of having two separate tables:

**Student Table:**

| StudentID | Name |
|-----------|------|
| 1         | Alice|

**Subject Table:**

| StudentID | Subject |
|-----------|---------|
| 1         | Math    |

Denormalized as:

| StudentID | Name  | Subject |
|-----------|-------|---------|
| 1         | Alice | Math    |

---

## 📝 Summary Table

| Normal Form | Key Rule                                  |
|-------------|--------------------------------------------|
| 1NF         | Atomic values only                        |
| 2NF         | No partial dependency                     |
| 3NF         | No transitive dependency                  |
| BCNF        | Left side of every FD must be super key   |
| Denormalization | Combine tables for performance         |

