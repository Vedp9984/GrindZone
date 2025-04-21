# Types of Keys in Database Management Systems (DBMS)

## Introduction

Keys are an essential component of relational database design, helping to establish relationships between tables, enforce data integrity, and ensure efficient data retrieval. This document provides a comprehensive overview of the different types of keys used in DBMS with examples.

## 1. Primary Key

A primary key is a column (or combination of columns) that uniquely identifies each row in a table. It ensures entity integrity and cannot contain NULL values.

**Characteristics:**
- Must be unique for each record
- Cannot be NULL
- Should be immutable (rarely changed)
- Only one primary key per table

**Example:**
```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    Email VARCHAR(100)
);
```

In this example, `EmployeeID` is the primary key as it uniquely identifies each employee.

**Real-world usage:**
| EmployeeID | FirstName | LastName | Email               |
|------------|-----------|----------|---------------------|
| 1          | John      | Doe      | john.doe@email.com  |
| 2          | Jane      | Smith    | jane.smith@email.com|
| 3          | Robert    | Johnson  | robert.j@email.com  |

## 2. Composite Key

A composite key is a primary key composed of multiple columns that together uniquely identify each record.

**Example:**
```sql
CREATE TABLE OrderItems (
    OrderID INT,
    ProductID INT,
    Quantity INT,
    UnitPrice DECIMAL(10,2),
    PRIMARY KEY (OrderID, ProductID)
);
```

In this example, neither `OrderID` nor `ProductID` alone can uniquely identify a record, but their combination can.

**Real-world usage:**
| OrderID | ProductID | Quantity | UnitPrice |
|---------|-----------|----------|-----------|
| 1001    | 5         | 2        | 10.99     |
| 1001    | 8         | 1        | 24.99     |
| 1002    | 5         | 3        | 10.99     |

## 3. Foreign Key

A foreign key is a column (or combination of columns) that refers to the primary key of another table. It establishes a relationship between tables and enforces referential integrity.

**Example:**
```sql
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    CustomerID INT,
    OrderDate DATE,
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);
```

In this example, `CustomerID` in the Orders table is a foreign key that references the `CustomerID` primary key in the Customers table.

**Real-world usage:**

Customers Table:
| CustomerID | CustomerName   | Email                 |
|------------|----------------|------------------------|
| 101        | Alice Cooper   | alice@example.com     |
| 102        | Bob Williams   | bob@example.com       |

Orders Table:
| OrderID | CustomerID | OrderDate  |
|---------|------------|------------|
| 1001    | 101        | 2025-02-15 |
| 1002    | 102        | 2025-02-16 |
| 1003    | 101        | 2025-03-01 |

## 4. Candidate Key

A candidate key is a column or combination of columns that could potentially serve as the primary key. A table can have multiple candidate keys, but only one is chosen as the primary key.

**Example:**
In a Students table, both `StudentID` and `Email` could uniquely identify each student, making them candidate keys.

```sql
CREATE TABLE Students (
    StudentID INT,
    Email VARCHAR(100),
    FirstName VARCHAR(50),
    LastName VARCHAR(50)
);
```

Both `StudentID` and `Email` are candidate keys as they can uniquely identify each student.

**Real-world usage:**
| StudentID | Email               | FirstName | LastName |
|-----------|---------------------|-----------|----------|
| 1001      | john@university.edu | John      | Smith    |
| 1002      | mary@university.edu | Mary      | Johnson  |

## 5. Super Key

A super key is a set of one or more columns that can uniquely identify a row in a table. A super key may contain additional columns that are not necessary for unique identification.

**Example:**
In the Students table, `{StudentID, FirstName, LastName}` is a super key because it contains `StudentID` which alone can uniquely identify a row.

**Real-world usage:**
In the Students table shown earlier, the combinations {StudentID, Email}, {StudentID, FirstName}, and {StudentID, FirstName, LastName} are all super keys because they contain StudentID, which uniquely identifies each row.

## 6. Alternate Key

An alternate key is a candidate key that is not selected as the primary key. It can still be used to uniquely identify rows.

**Example:**
In the Students table, if `StudentID` is chosen as the primary key, then `Email` becomes an alternate key.

```sql
CREATE TABLE Students (
    StudentID INT PRIMARY KEY,
    Email VARCHAR(100) UNIQUE,
    FirstName VARCHAR(50),
    LastName VARCHAR(50)
);
```

Here, `Email` is an alternate key with a UNIQUE constraint.

## 7. Surrogate Key

A surrogate key is an artificially created key, typically an auto-incrementing integer, used as a primary key when no natural key exists or when the natural key is complex or subject to change.

**Example:**
```sql
CREATE TABLE Products (
    ProductID INT IDENTITY(1,1) PRIMARY KEY,
    ProductName VARCHAR(100),
    SKU VARCHAR(20),
    Description TEXT
);
```

Here, `ProductID` is a surrogate key automatically generated by the DBMS.

**Real-world usage:**
| ProductID | ProductName          | SKU       | Description                   |
|-----------|----------------------|-----------|-------------------------------|
| 1         | Wireless Mouse       | MS-WL-101 | Bluetooth wireless mouse      |
| 2         | Mechanical Keyboard  | KB-MC-202 | RGB mechanical keyboard       |

## 8. Natural Key

A natural key is a key formed of real-world attributes that can uniquely identify a record, like social security numbers, ISBN numbers, or email addresses.

**Example:**
```sql
CREATE TABLE Books (
    ISBN VARCHAR(13) PRIMARY KEY,
    Title VARCHAR(200),
    Author VARCHAR(100),
    PublicationYear INT
);
```

Here, `ISBN` is a natural key as it's an inherent attribute of the book.

**Real-world usage:**
| ISBN           | Title                     | Author           | PublicationYear |
|----------------|---------------------------|------------------|-----------------|
| 9780141439518  | Pride and Prejudice       | Jane Austen      | 1813           |
| 9780061120084  | To Kill a Mockingbird     | Harper Lee       | 1960           |

## 9. Unique Key

A unique key constraint ensures that all values in a column or combination of columns are unique. Unlike a primary key, a unique key can allow NULL values (though only one NULL).

**Example:**
```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    Email VARCHAR(100) UNIQUE,
    Phone VARCHAR(15) UNIQUE,
    FirstName VARCHAR(50),
    LastName VARCHAR(50)
);
```

Here, both `Email` and `Phone` have unique constraints.

**Real-world usage:**
| EmployeeID | Email               | Phone          | FirstName | LastName |
|------------|---------------------|----------------|-----------|----------|
| 1          | john@company.com    | 555-1234       | John      | Doe      |
| 2          | sarah@company.com   | 555-5678       | Sarah     | Johnson  |

## 10. Compound Key

A compound key is similar to a composite key but refers more generally to any key that consists of multiple columns. The term is often used interchangeably with composite key.

**Example:**
```sql
CREATE TABLE CourseEnrollments (
    StudentID INT,
    CourseID INT,
    EnrollmentDate DATE,
    Grade CHAR(1),
    PRIMARY KEY (StudentID, CourseID)
);
```

Here, the combination of `StudentID` and `CourseID` forms a compound key.

## 11. Secondary Key

A secondary key is an attribute or combination of attributes used to retrieve data, but not necessarily for unique identification. It typically forms the basis for an index to improve query performance.

**Example:**
```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    DepartmentID INT
);

CREATE INDEX idx_department ON Employees(DepartmentID);
```

Here, `DepartmentID` is a secondary key used to quickly find all employees in a specific department.

## Summary

The use of appropriate keys in database design is crucial for maintaining data integrity, establishing relationships between tables, and ensuring efficient data retrieval. The choice of keys depends on the specific requirements of the database and the nature of the data being stored.

| Key Type | Description | Can Contain Duplicates | Can Contain NULLs |
|----------|-------------|------------------------|-------------------|
| Primary Key | Uniquely identifies each record | No | No |
| Composite Key | Multiple columns that together form a primary key | No | No |
| Foreign Key | References a primary key in another table | Yes | Typically Yes |
| Candidate Key | Column(s) that could serve as a primary key | No | No |
| Super Key | Set of columns that includes a candidate key | No | Depends |
| Alternate Key | Candidate key not chosen as primary key | No | No |
| Surrogate Key | Artificially created key (often auto-increment) | No | No |
| Natural Key | Key based on real-world attributes | No | No |
| Unique Key | Ensures unique values but not used as primary key | No | Usually one NULL allowed |
| Compound Key | Multiple columns forming a key | No | No |
| Secondary Key | Used for retrieval but not unique identification | Yes | Yes |