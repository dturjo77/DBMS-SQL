# Relational Model Notes

These notes provide a comprehensive overview of the Relational Model for Day 5-6, focusing on relations, keys (primary, foreign), integrity constraints, practice exercises, and interview preparation. The content covers studying the relational model, writing 5-10 relational schemas with constraints, and solving LeetCode SQL problems (e.g., "Combine Two Tables"). The notes are detailed, actionable, and designed to support both theoretical understanding and practical application.

---

## Understanding the Relational Model

### 1. What is the Relational Model?

- The **Relational Model** is a framework for organizing and managing data in a database using **relations** (tables).
- Proposed by E.F. Codd in 1970, it forms the basis of most modern Database Management Systems (DBMS) like MySQL, PostgreSQL, and Oracle.
- Data is stored in tables, where each table represents a relation, and relationships between tables are established using keys.

### 2. Key Concepts of the Relational Model

#### a. Relations (Tables)

- A **relation** is a table with rows (tuples) and columns (attributes).

- Each row represents a record, and each column represents an attribute of the entity.

- Example: A `Student` table:

  ```
  Student
  +-----------+----------+--------------+
  | StudentID | Name     | Email        |
  +-----------+----------+--------------+
  | 1         | Alice    | alice@univ.edu |
  | 2         | Bob      | bob@univ.edu |
  +-----------+----------+--------------+
  ```

- **Properties**:

  - **No duplicate rows**: Each tuple is unique (ensured by the primary key).
  - **No ordering**: Rows and columns have no inherent order.
  - **Atomic values**: Each cell contains a single, indivisible value.

#### b. Attributes and Domains

- **Attributes**: The columns of a table (e.g., `StudentID`, `Name`, `Email`).
- **Domain**: The set of allowable values for an attribute (e.g., `StudentID` is an integer, `Email` is a string matching an email format).
- Example: The domain of `Email` might be defined as strings matching the regex `^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$`.

#### c. Keys

Keys are attributes or combinations of attributes that uniquely identify tuples or establish relationships.

- **Primary Key (PK)**:
  - A unique identifier for each tuple in a table.
  - Cannot be NULL and must be unique.
  - Example: `StudentID` in the `Student` table.
- **Candidate Key**:
  - Any attribute or set of attributes that could serve as a primary key (i.e., uniquely identifies tuples).
  - Example: Both `StudentID` and `Email` (if unique) could be candidate keys, but only one is chosen as the primary key.
- **Foreign Key (FK)**:
  - An attribute (or set of attributes) in one table that references the primary key of another table.
  - Establishes and enforces relationships between tables.
  - Example: `StudentID` in an `Enrollment` table referencing `Student.StudentID`.
- **Super Key**:
  - Any set of attributes that uniquely identifies a tuple (includes candidate keys and more).
  - Example: `{StudentID, Name}` is a super key, but not minimal.
- **Composite Key**:
  - A primary key consisting of multiple attributes.
  - Example: In an `Enrollment` table, `{StudentID, CourseID, Semester}` might form the primary key.

#### d. Integrity Constraints

Constraints ensure the accuracy, consistency, and reliability of the database. They are enforced by the DBMS.

- **Entity Integrity**:
  - The primary key of a table cannot be NULL and must be unique.
  - Ensures each tuple is uniquely identifiable.
  - Example: `StudentID` in `Student` must be non-NULL and unique.
- **Referential Integrity**:
  - A foreign key value must either match an existing primary key value in the referenced table or be NULL (if allowed).
  - Ensures relationships between tables remain valid.
  - Example: `StudentID` in `Enrollment` must match a `StudentID` in `Student` or be NULL (if allowed).
- **Domain Integrity**:
  - Values in an attribute must conform to the defined domain (data type, range, format).
  - Example: `Email` must be a valid email string; `Age` must be a positive integer.
- **User-Defined Integrity**:
  - Custom rules specific to the application.
  - Example: `Grade` in `Enrollment` must be one of `{A, B, C, D, F}`.
- **NOT NULL Constraint**:
  - Specifies that an attribute cannot contain NULL values.
  - Example: `Name` in `Student` must be provided.
- **Unique Constraint**:
  - Ensures all values in an attribute (or set of attributes) are unique, but allows NULLs (unlike primary key).
  - Example: `Email` in `Student` might be unique but nullable.

### 3. Relational Schema Notation

A relational schema defines the structure of a table, including attributes, data types, and constraints. Notation example:

```
TableName(Attribute1 DataType [Constraints], Attribute2 DataType [Constraints], ...)
```

Example:

```
Student(StudentID Integer PRIMARY KEY, Name Varchar(50) NOT NULL, Email Varchar(100) UNIQUE)
```

---

## Practice and Interview Preparation

### 1. Practice: Writing Relational Schemas with Constraints

Below are **8 relational schemas** for different systems, each with appropriate constraints to demonstrate the application of primary keys, foreign keys, and integrity constraints.

#### Schema 1: University System

- **Student**:

  ```
  Student(StudentID Integer PRIMARY KEY, Name Varchar(50) NOT NULL, Email Varchar(100) UNIQUE, Major Varchar(50))
  ```

- **Course**:

  ```
  Course(CourseID Varchar(10) PRIMARY KEY, Title Varchar(100) NOT NULL, Credits Integer CHECK (Credits > 0))
  ```

- **Enrollment**:

  ```
  Enrollment(StudentID Integer, CourseID Varchar(10), Semester Varchar(20), Grade Varchar(2) CHECK (Grade IN ('A', 'B', 'C', 'D', 'F')),
             PRIMARY KEY (StudentID, CourseID, Semester),
             FOREIGN KEY (StudentID) REFERENCES Student(StudentID),
             FOREIGN KEY (CourseID) REFERENCES Course(CourseID))
  ```

#### Schema 2: E-Commerce System

- **Customer**:

  ```
  Customer(CustomerID Integer PRIMARY KEY, FirstName Varchar(50) NOT NULL, LastName Varchar(50) NOT NULL, Email Varchar(100) UNIQUE)
  ```

- **Product**:

  ```
  Product(ProductID Integer PRIMARY KEY, Name Varchar(100) NOT NULL, Price Decimal(10,2) CHECK (Price >= 0), Stock Integer CHECK (Stock >= 0))
  ```

- **Order**:

  ```
  Order(OrderID Integer PRIMARY KEY, CustomerID Integer, OrderDate Date NOT NULL,
        FOREIGN KEY (CustomerID) REFERENCES Customer(CustomerID))
  ```

- **OrderDetail**:

  ```
  OrderDetail(OrderID Integer, ProductID Integer, Quantity Integer CHECK (Quantity > 0),
              PRIMARY KEY (OrderID, ProductID),
              FOREIGN KEY (OrderID) REFERENCES Order(OrderID),
              FOREIGN KEY (ProductID) REFERENCES Product(ProductID))
  ```

#### Schema 3: Hospital System

- **Patient**:

  ```
  Patient(PatientID Integer PRIMARY KEY, Name Varchar(100) NOT NULL, DOB Date, Gender Varchar(10) CHECK (Gender IN ('M', 'F', 'Other')))
  ```

- **Doctor**:

  ```
  Doctor(DoctorID Integer PRIMARY KEY, Name Varchar(100) NOT NULL, Specialty Varchar(50))
  ```

- **Appointment**:

  ```
  Appointment(AppointmentID Integer PRIMARY KEY, PatientID Integer, DoctorID Integer, AppointmentDate Date NOT NULL,
              FOREIGN KEY (PatientID) REFERENCES Patient(PatientID),
              FOREIGN KEY (DoctorID) REFERENCES Doctor(DoctorID))
  ```

#### Schema 4: Library System

- **Book**:

  ```
  Book(ISBN Varchar(13) PRIMARY KEY, Title Varchar(200) NOT NULL, Author Varchar(100), PublicationYear Integer CHECK (PublicationYear > 1800))
  ```

- **Member**:

  ```
  Member(MemberID Integer PRIMARY KEY, Name Varchar(100) NOT NULL, Email Varchar(100) UNIQUE)
  ```

- **Loan**:

  ```
  Loan(LoanID Integer PRIMARY KEY, MemberID Integer, ISBN Varchar(13), BorrowDate Date NOT NULL, ReturnDate Date,
       FOREIGN KEY (MemberID) REFERENCES Member(MemberID),
       FOREIGN KEY (ISBN) REFERENCES Book(ISBN))
  ```

#### Schema 5: Airline Reservation System

- **Flight**:

  ```
  Flight(FlightID Integer PRIMARY KEY, FlightNumber Varchar(10) NOT NULL, DepartureCity Varchar(50), ArrivalCity Varchar(50), DepartureTime Datetime)
  ```

- **Passenger**:

  ```
  Passenger(PassengerID Integer PRIMARY KEY, Name Varchar(100) NOT NULL, PassportNumber Varchar(20) UNIQUE)
  ```

- **Booking**:

  ```
  Booking(BookingID Integer PRIMARY KEY, PassengerID Integer, FlightID Integer, BookingDate Date NOT NULL, SeatNumber Varchar(5),
          FOREIGN KEY (PassengerID) REFERENCES Passenger(PassengerID),
          FOREIGN KEY (FlightID) REFERENCES Flight(FlightID))
  ```

#### Schema 6: Online Forum System

- **User**:

  ```
  User(UserID Integer PRIMARY KEY, Username Varchar(50) UNIQUE NOT NULL, Email Varchar(100) UNIQUE, JoinDate Date)
  ```

- **Post**:

  ```
  Post(PostID Integer PRIMARY KEY, UserID Integer, Title Varchar(200) NOT NULL, Content Text, PostDate Datetime,
       FOREIGN KEY (UserID) REFERENCES User(UserID))
  ```

#### Schema 7: Inventory Management System

- **Warehouse**:

  ```
  Warehouse(WarehouseID Integer PRIMARY KEY, Location Varchar(100) NOT NULL, Capacity Integer CHECK (Capacity > 0))
  ```

- **Item**:

  ```
  Item(ItemID Integer PRIMARY KEY, Name Varchar(100) NOT NULL, WarehouseID Integer, Quantity Integer CHECK (Quantity >= 0),
       FOREIGN KEY (WarehouseID) REFERENCES Warehouse(WarehouseID))
  ```

#### Schema 8: Social Media Platform

- **User**:

  ```
  User(UserID Integer PRIMARY KEY, Username Varchar(50) UNIQUE NOT NULL, Email Varchar(100) UNIQUE, Bio Varchar(200))
  ```

- **Friendship**:

  ```
  Friendship(UserID1 Integer, UserID2 Integer, FriendshipDate Date NOT NULL,
             PRIMARY KEY (UserID1, UserID2),
             FOREIGN KEY (UserID1) REFERENCES User(UserID),
             FOREIGN KEY (UserID2) REFERENCES User(UserID),
             CHECK (UserID1 != UserID2))
  ```

**Notes on Schemas**:

- Each schema includes **primary keys** for unique identification.
- **Foreign keys** establish relationships with referential integrity.
- **CHECK constraints** enforce domain integrity (e.g., positive quantities, valid grades).
- **NOT NULL** and **UNIQUE** constraints ensure required and unique values where appropriate.

### 2. Interview Preparation: LeetCode SQL Problems

SQL problems on LeetCode are excellent for practicing relational model concepts and preparing for technical interviews. Below is an analysis of the recommended problem, **"Combine Two Tables" (LeetCode #175)**, along with guidance for solving similar problems.

#### Problem: Combine Two Tables (LeetCode #175)

**Problem Description**:

- Given two tables:

  - `Person`:

    ```
    PersonId Integer PRIMARY KEY
    FirstName Varchar
    LastName Varchar
    ```

  - `Address`:

    ```
    AddressId Integer PRIMARY KEY
    PersonId Integer FOREIGN KEY REFERENCES Person(PersonId)
    City Varchar
    State Varchar
    ```

- Write an SQL query to report the first name, last name, city, and state for each person, including those without an address.

**Solution**:

- Since not every person has an address, use a **LEFT JOIN** to include all records from `Person`, even if there’s no matching record in `Address`.

- SQL Query:

  ```sql
  SELECT p.FirstName, p.LastName, a.City, a.State
  FROM Person p
  LEFT JOIN Address a ON p.PersonId = a.PersonId;
  ```

**Explanation**:

- **LEFT JOIN** ensures all rows from `Person` are included, with `NULL` for `City` and `State` if no matching `Address` exists.
- The `ON` clause links the tables using the foreign key `PersonId`.
- This problem tests understanding of **joins** and handling missing data, which are common in relational databases.

#### Other Recommended LeetCode SQL Problems

To build proficiency, practice these problems that reinforce relational model concepts:

1. **Second Highest Salary (LeetCode #176)**:

   - Tests understanding of aggregates (`MAX`), subqueries, and handling edge cases (e.g., NULL when no second-highest salary exists).

   - Schema: `Employee(Id Integer PRIMARY KEY, Salary Integer)`.

   - Query example:

     ```sql
     SELECT MAX(Salary) AS SecondHighestSalary
     FROM Employee
     WHERE Salary < (SELECT MAX(Salary) FROM Employee);
     ```

2. **Duplicate Emails (LeetCode #182)**:

   - Tests grouping (`GROUP BY`) and filtering (`HAVING`) to identify duplicates.

   - Schema: `Person(Id Integer PRIMARY KEY, Email Varchar)`.

   - Query example:

     ```sql
     SELECT Email
     FROM Person
     GROUP BY Email
     HAVING COUNT(*) > 1;
     ```

3. **Employees Earning More Than Their Managers (LeetCode #181)**:

   - Tests self-joins to compare rows within the same table.

   - Schema: `Employee(Id Integer PRIMARY KEY, Name Varchar, Salary Integer, ManagerId Integer FOREIGN KEY REFERENCES Employee(Id))`.

   - Query example:

     ```sql
     SELECT e1.Name AS Employee
     FROM Employee e1
     JOIN Employee e2 ON e1.ManagerId = e2.Id
     WHERE e1.Salary > e2.Salary;
     ```

4. **Customers Who Never Order (LeetCode #183)**:

   - Tests `LEFT JOIN` with `NULL` checks to find non-matching records.

   - Schema: `Customers(Id Integer PRIMARY KEY, Name Varchar)`, `Orders(Id Integer PRIMARY KEY, CustomerId Integer FOREIGN KEY REFERENCES Customers(Id))`.

   - Query example:

     ```sql
     SELECT c.Name AS Customers
     FROM Customers c
     LEFT JOIN Orders o ON c.Id = o.CustomerId
     WHERE o.CustomerId IS NULL;
     ```

**Tips for Solving SQL Problems**:

- Understand the schema and relationships (primary and foreign keys).
- Identify the type of join needed (INNER, LEFT, RIGHT, FULL) based on whether all records are required.
- Use subqueries or CTEs for complex filtering or aggregation.
- Handle edge cases (e.g., NULL values, empty tables).
- Practice writing clean, readable SQL with proper indentation.

### 3. Additional Practice Suggestions

- **Create and Populate Tables**:

  - Use a DBMS (e.g., MySQL, PostgreSQL) to create the schemas above and insert sample data.

  - Example SQL for creating the `Student` table:

    ```sql
    CREATE TABLE Student (
        StudentID INT PRIMARY KEY,
        Name VARCHAR(50) NOT NULL,
        Email VARCHAR(100) UNIQUE,
        Major VARCHAR(50)
    );
    INSERT INTO Student VALUES (1, 'Alice', 'alice@univ.edu', 'Computer Science');
    ```

- **Write Queries**:

  - Practice queries like finding all students enrolled in a specific course or products with low stock.

  - Example: List students and their grades for a course:

    ```sql
    SELECT s.Name, e.Grade
    FROM Student s
    JOIN Enrollment e ON s.StudentID = e.StudentID
    WHERE e.CourseID = 'CS101';
    ```

- **Test Constraints**:

  - Attempt to insert invalid data (e.g., duplicate `StudentID`, invalid `Grade`) to verify that constraints are enforced.
  - Example: `INSERT INTO Enrollment VALUES (1, 'CS101', 'Fall2025', 'X');` should fail due to the `CHECK` constraint.

---

## Key Takeaways

- The **Relational Model** organizes data into tables (relations) with attributes, using keys to ensure uniqueness and relationships.
- **Primary Keys** ensure entity integrity, while **Foreign Keys** enforce referential integrity.
- **Integrity Constraints** (entity, referential, domain, user-defined) maintain data accuracy and consistency.
- Practice writing **relational schemas** with constraints to solidify understanding of table design.
- Solving **LeetCode SQL problems** builds proficiency in querying relational databases and prepares for interviews.

## Recommended Next Steps

- Learn SQL in depth (SELECT, JOIN, GROUP BY, subqueries, window functions).
- Study normalization (1NF, 2NF, 3NF) to optimize relational schemas.
- Explore indexing and query optimization for performance.
- Practice more LeetCode SQL problems (e.g., "Department Highest Salary", "Rank Scores") to prepare for interviews.
