#  Relational Model & Relational Algebra

## Relational Model

### 1. What is the Relational Model?
The **Relational Model** is a foundational database model introduced by **E. F. Codd in 1970**. It represents data in the form of **relations (tables)**. Each relation consists of **tuples (rows)** and **attributes (columns)**.

### 2. Basic Terminologies
- **Relation**: A table with rows and columns.
- **Tuple**: A single row in a relation. It represents a single record.
- **Attribute**: A column in a relation. Each attribute has a name and a domain (data type).
- **Domain**: The set of allowable values for an attribute.

**Example:**

| StudentID | Name  | Age | Department |
|-----------|-------|-----|------------|
| 101       | Alice | 20  | CS         |
| 102       | Bob   | 21  | EE         |

### 3. Keys in Relational Model
- **Super Key**: A set of one or more attributes that uniquely identifies a tuple.
- **Candidate Key**: A minimal super key (i.e., no unnecessary attributes).
- **Primary Key**: One candidate key selected to uniquely identify tuples.
- **Foreign Key**: An attribute in one relation that refers to the primary key in another relation.

### 4. Integrity Constraints
- **Entity Integrity**: No primary key value can be NULL.
- **Referential Integrity**: A foreign key must either be NULL or match a primary key value in the referenced relation.
- **Domain Constraint**: Attribute values must lie within a specific domain.
- **Key Constraint**: No two tuples in a relation can have the same value for the primary key.

### ✅ Interview Questions – Relational Model
- What is the difference between a candidate key and a super key?
- Why is referential integrity important?
- Can a relation have multiple primary keys?
- What happens when you delete a row that is referenced by a foreign key?

---

## Relational Algebra

### 1. What is Relational Algebra?
Relational Algebra is a **procedural query language** that operates on relations. It takes one or more relations as input and produces a new relation as output.

It is the theoretical foundation of SQL.

### 2. Basic Operations

#### a. Selection (σ)
Selects tuples that satisfy a given predicate.
```text
σ_condition(Relation)
```
**Example:**
```text
σ_Age > 20(Student)
```

#### b. Projection (π)
Selects specific columns (attributes).
```text
π_columns(Relation)
```
**Example:**
```text
π_Name, Department(Student)
```

#### c. Union ( ∪ )
Combines tuples from two relations, removing duplicates.

**Condition:** Both relations must be **union-compatible** (same attributes and domains).
```text
R ∪ S
```

#### d. Set Difference ( − )
Finds tuples in one relation but not in the other.
```text
R − S
```

#### e. Cartesian Product ( × )
Combines every tuple of one relation with every tuple of another.
```text
R × S
```

#### f. Rename (ρ)
Renames the relation or attributes.
```text
ρ_newName(Relation)
```

### 3. Advanced Operations

#### a. Join (⨝)
Combines related tuples from two relations.

- **Theta Join (θ-join)**: Based on a condition.
- **Equi Join**: Theta join with only equality conditions.
- **Natural Join**: Automatically joins relations based on common attributes.

**Example:**
```text
R ⨝ R.A = S.B S
```

#### b. Division (÷)
Finds tuples in one relation that are associated with **all** tuples in another.

Used for queries like: "Find students who have taken all courses."

###  Interview Questions – Relational Algebra
- **How is relational algebra different from SQL?**
  - Relational algebra is a procedural language (specifies how to get the result), while SQL is a declarative language (specifies what result you want).
  - Relational algebra is a theoretical formal system, while SQL is an implemented query language.
  - Example: Getting students in CS department
    - Relational Algebra: `σ_Department="CS"(Student)`
    - SQL: `SELECT * FROM Student WHERE Department = 'CS'`

- **Explain the difference between natural join and equi-join:**
  - **Natural Join**: Automatically joins tables based on all common attribute names and eliminates duplicate columns.
  - **Equi-Join**: Joins tables based on equality conditions but keeps all columns including duplicates.
  - Example: 
    - For tables Student(ID, Name, DeptID) and Department(DeptID, DeptName)
    - Natural Join: `Student ⋈ Department` joins on DeptID automatically
    - Equi-Join: `Student ⨝Student.DeptID=Department.DeptID Department` requires explicitly specifying the join condition

- **What is the use of the division operator in relational algebra?**
  - Used for "for all" queries when we need to find entities related to every instance of another entity.
  - Example: "Find students who have taken all required courses"
    - If we have Enrolled(StudentID, CourseID) and Required(CourseID)
    - Enrolled ÷ Required gives students who took all required courses

- **Why do we need relational algebra when we already have SQL?**
  - Provides theoretical foundation for database operations
  - Helps in query optimization and understanding execution plans
  - Useful for proving correctness and equivalence of queries
  - Serves as basis for teaching and understanding relational databases

- **What are the properties of union-compatible relations?**
  - Must have the same number of attributes
  - Corresponding attributes must have the same domains (data types)
  - Attribute names can be different
  - Example: 
    - R(ID, Name, Age) and S(StudentNo, StudentName, Years) are union-compatible
    - R(ID, Name) and S(ID, Name, Dept) are not union-compatible

---


