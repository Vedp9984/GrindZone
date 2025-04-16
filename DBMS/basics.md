# Database Management System (DBMS) - Detailed Notes

## 1. Introduction to DBMS

### What is a DBMS?
A **Database Management System (DBMS)** is software that allows users to create, store, manage, and retrieve data from databases efficiently. It acts as an interface between end-users and the database, ensuring that data is consistently organized and easily accessible.

### Why Use DBMS Over File Systems?
Traditional file systems store data in flat files, which can lead to:
- **Data Redundancy**: Duplication of data
- **Data Inconsistency**: Different versions of the same data
- **Difficulty in Accessing Data**: No query language support
- **Data Isolation**: Scattered data files
- **Concurrency Issues**: Multiple users modifying data simultaneously

A DBMS resolves these issues by providing:
- Centralized control over data
- Data integrity and consistency
- Security and access control
- Efficient query processing using SQL

### Applications of DBMS
- **Banking**: Customer information, account activities
- **Airlines**: Reservations, schedules
- **Universities**: Student records, grades
- **Telecommunications**: Call records, customer data
- **Retail**: Inventory, transactions

---

## 2. Database Models
Database models define how data is logically structured and manipulated.

### 1. Hierarchical Model
- Data is organized in a tree-like structure (parent-child relationship).
- Each child has only one parent, but a parent can have many children.

**Example:**
```
Company
│
├── Department1
│   ├── Employee1
│   └── Employee2
└── Department2
    └── Employee3
```
- Efficient for one-to-many relationships.
- Difficult to handle many-to-many relationships.

### 2. Network Model
- More flexible than the hierarchical model.
- Uses a graph structure: each record can have multiple parent and child records.

**Example:**
```
Projects ←→ Employees
  ↑            ↑
 Departments ←→ Managers
```
- Supports many-to-many relationships.
- More complex to design and maintain.

### 3. Relational Model
- Most widely used model.
- Data is stored in tables (relations) with rows and columns.
- Tables can be related using keys.

**Example:**
```
Employees Table:
+----+--------+----------+
| ID | Name   | Dept_ID  |
+----+--------+----------+
| 1  | Alice  | 10       |
| 2  | Bob    | 20       |

Departments Table:
+--------+-------------+
| Dept_ID| Dept_Name   |
+--------+-------------+
| 10     | HR          |
| 20     | IT          |
```
- Uses SQL for querying data.

### 4. Object-Oriented Model
- Integrates object-oriented programming principles with databases.
- Data is stored as objects, similar to programming languages like Java or C++.

**Example:**
```java
class Employee {
  int id;
  String name;
  Department dept;
}
```
- Useful for complex data types like images, multimedia, etc.

---

## 3. DBMS Architecture

### 1-Tier Architecture
- The database is directly accessible to the user.
- Typically used for development or learning purposes.

### 2-Tier Architecture
- Client (user interface) interacts directly with the database server.
- Used in small-scale applications.

**Example:** A desktop application like MS Access.

### 3-Tier Architecture
- Adds a middle layer between client and server for business logic.
- Enhances security, scalability, and maintainability.

**Layers:**
1. **Presentation Layer (Client)** – User interface
2. **Application Layer (Middle Tier)** – Business logic
3. **Data Layer (DBMS Server)** – Database server

**Example:** Web application (HTML frontend → Application server → MySQL DB)

### DBMS Components
- **DBMS Engine**: Handles data storage, retrieval, and updates.
- **Query Processor**: Parses and executes SQL queries.
- **Metadata Manager**: Manages data about the database structure.
- **Transaction Manager**: Ensures ACID properties.
- **Buffer Manager**: Manages in-memory data buffering.

---

## 4. Data Models

### Levels of Data Abstraction
1. **Physical Level** – How data is stored
2. **Logical Level** – What data is stored and relationships
3. **View Level** – How users interact with data

### ER Model (Entity-Relationship Model)
Used to design the logical structure of databases in a visual format.

#### Key Components:
- **Entity**: A real-world object (e.g., Student, Course)
- **Attribute**: A property of an entity (e.g., name, age)
- **Relationship**: Association between entities (e.g., Enrolled)

#### Types of Attributes:
- **Simple**: Cannot be divided (e.g., age)
- **Composite**: Can be divided (e.g., name → first, last)
- **Derived**: Can be derived from other attributes (e.g., age from DOB)
- **Multi-valued**: Can have multiple values (e.g., phone numbers)

#### ER Diagram Symbols:
- Entity: Rectangle
- Attribute: Oval
- Relationship: Diamond
- Lines to connect

#### Example:
```
Student (Student_ID, Name, Age)
Course (Course_ID, Course_Name)
Enrollment (Student_ID, Course_ID, Grade)
```

### ER Diagram Example:
```
[Student] ──(Enrolls)── [Course]
   │                        │
 [Student_ID]         [Course_ID]
```
- Cardinality: One-to-One, One-to-Many, Many-to-Many

---

