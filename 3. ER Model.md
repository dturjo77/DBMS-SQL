#Entity-Relationship (ER) Model Notes

These notes provide a comprehensive overview of the Entity-Relationship (ER) model for Day 3-4, focusing on ER diagrams, entities, attributes, relationships, and cardinality. The content is aligned with the recommended activities of learning ER modeling, creating ER diagrams (ERDs) for sample systems (hospital, library) using tools like Lucidchart or Draw.io, and converting ERDs to relational schemas.

---

## Understanding the ER Model

### 1. What is the ER Model?

- The **Entity-Relationship (ER) Model** is a conceptual modeling approach used to design databases by representing real-world objects and their relationships.
- It provides a visual way to describe the structure of a database using **ER diagrams (ERDs)**.
- The ER model is used during the **conceptual design phase** to create a high-level blueprint before implementing a database in a DBMS.

### 2. Components of the ER Model

#### a. Entities

- An **entity** is a real-world object or concept about which data is stored (e.g., Student, Book, Patient).
- Entities are represented as **rectangles** in ERDs.
- **Types**:
  - **Strong Entity**: Has a primary key and does not depend on other entities (e.g., `Student` with `StudentID`).
  - **Weak Entity**: Depends on another entity (its owner) and does not have a unique primary key (e.g., `Dependent` of an `Employee`).

#### b. Attributes

- **Attributes** are properties or characteristics of an entity (e.g., `StudentID`, `Name`, `Email` for a `Student`).
- Represented as **ovals** connected to their entity in ERDs.
- **Types**:
  - **Simple**: Cannot be divided (e.g., `Age`).
  - **Composite**: Can be divided into subparts (e.g., `Name` = `FirstName` + `LastName`).
  - **Single-valued**: Has one value per entity (e.g., `StudentID`).
  - **Multi-valued**: Can have multiple values (e.g., `PhoneNumber`, denoted with a double oval).
  - **Derived**: Computed from other attributes (e.g., `Age` derived from `BirthDate`, denoted with a dashed oval).
  - **Key Attribute**: Uniquely identifies an entity (e.g., `StudentID`, underlined in ERDs).

#### c. Relationships

- A **relationship** describes how entities are associated (e.g., a `Student` enrolls in a `Course`).
- Represented as **diamonds** in ERDs, connecting related entities.
- **Types**:
  - **Binary**: Involves two entities (e.g., `Student`–`Course`).
  - **Ternary**: Involves three entities (e.g., `Student`–`Course`–`Professor`).
  - **Recursive**: An entity relates to itself (e.g., `Employee` supervises another `Employee`).

#### d. Cardinality

- **Cardinality** specifies the number of instances of one entity that can or must be associated with each instance of another entity in a relationship.
- Represented by constraints on the edges connecting entities to relationships.
- **Types**:
  - **One-to-One (1:1)**: One instance of entity A relates to exactly one instance of entity B (e.g., `Person`–`Passport`).
  - **One-to-Many (1:N)**: One instance of entity A relates to multiple instances of entity B (e.g., `Department`–`Employee`).
  - **Many-to-One (N:1)**: Multiple instances of entity A relate to one instance of entity B (e.g., `Employee`–`Department`).
  - **Many-to-Many (M:N)**: Multiple instances of entity A relate to multiple instances of entity B (e.g., `Student`–`Course`).
- **Participation Constraints**:
  - **Total Participation**: Every instance of an entity must participate in the relationship (denoted by a double line).
  - **Partial Participation**: Some instances may not participate (denoted by a single line).

### 3. ER Diagram Notations

- **Chen Notation** (commonly used):
  - Entities: Rectangles.
  - Attributes: Ovals (key attributes underlined).
  - Relationships: Diamonds.
  - Cardinality: Labels (e.g., 1:N) or crow’s foot notation.
- **Crow’s Foot Notation**:
  - Uses symbols to indicate cardinality (e.g., a crow’s foot for "many").
  - More compact and widely used in modern tools.

### 4. Tools for Creating ERDs

- **Lucidchart**: A cloud-based diagramming tool with ERD templates and collaboration features.
- **Draw.io (diagrams.net)**: A free, browser-based tool integrated with cloud storage (Google Drive, OneDrive).
- Both tools support drag-and-drop interfaces to create entities, attributes, and relationships with proper notations.

---

## ERD Design and Conversion to Relational Schemas

### 1. Steps to Create an ERD

1. **Identify Entities**: List the main objects in the system (e.g., for a hospital: `Patient`, `Doctor`, `Appointment`).
2. **Define Attributes**: Assign properties to each entity, including key attributes.
3. **Determine Relationships**: Identify how entities are related and specify cardinality.
4. **Add Constraints**: Include primary keys, foreign keys, and participation constraints.
5. **Draw the ERD**: Use a tool to visually represent the model, ensuring clarity and correctness.
6. **Validate**: Check that the ERD meets the system’s requirements and supports all necessary operations.

### 2. Practice: ERDs for Sample Systems

Below are ERDs for a **hospital** and **library** system, designed using Chen notation for clarity.

#### a. Hospital System ERD

**Entities and Attributes**:

- **Patient**:
  - `PatientID` (key, unique)
  - `Name` (composite: `FirstName`, `LastName`)
  - `DOB` (Date of Birth)
  - `Phone` (multi-valued)
- **Doctor**:
  - `DoctorID` (key, unique)
  - `Name` (composite: `FirstName`, `LastName`)
  - `Specialty`
- **Appointment**:
  - `AppointmentID` (key, unique)
  - `Date`
  - `Time`
  - `Diagnosis` (derived from visit outcome)

**Relationships**:

- `Patient`–`Makes`–`Appointment`: A patient can have multiple appointments (1:N).
- `Doctor`–`Attends`–`Appointment`: A doctor can attend multiple appointments, but each appointment has one doctor (1:N).

**Cardinality**:

- `Patient` to `Appointment`: 1:N (one patient can have many appointments).
- `Doctor` to `Appointment`: 1:N (one doctor can have many appointments).
- **Total Participation**: Every `Appointment` must involve one `Patient` and one `Doctor`.

**Text-Based ERD** (Chen Notation Approximation):

```
[Patient] --(Makes)--> [Appointment] <--(Attends)-- [Doctor]
   |                        |                        |
   | PatientID (PK)        | AppointmentID (PK)     | DoctorID (PK)
   | Name                  | Date                   | Name
   | DOB                   | Time                   | Specialty
   | Phone (multi-valued)  | Diagnosis (derived)
```

#### b. Library System ERD

**Entities and Attributes**:

- **Book**:
  - `ISBN` (key, unique)
  - `Title`
  - `Author`
  - `PublicationYear`
- **Member**:
  - `MemberID` (key, unique)
  - `Name` (composite: `FirstName`, `LastName`)
  - `Email`
- **Loan**:
  - `LoanID` (key, unique)
  - `BorrowDate`
  - `ReturnDate`
  - `Fine` (derived, based on overdue days)

**Relationships**:

- `Member`–`Borrows`–`Loan`: A member can have multiple loans (1:N).
- `Book`–`InvolvedIn`–`Loan`: A book can be part of multiple loans (1:N).

**Cardinality**:

- `Member` to `Loan`: 1:N (one member can have many loans).
- `Book` to `Loan`: 1:N (one book can be borrowed in many loans).
- **Partial Participation**: A `Member` or `Book` may not be involved in any `Loan`.

**Text-Based ERD** (Chen Notation Approximation):

```
[Member] --(Borrows)--> [Loan] <--(InvolvedIn)-- [Book]
   |                        |                        |
   | MemberID (PK)         | LoanID (PK)            | ISBN (PK)
   | Name                  | BorrowDate             | Title
   | Email                 | ReturnDate             | Author
                           | Fine (derived)         | PublicationYear
```

### 3. Converting ERDs to Relational Schemas

To convert an ERD to a **relational schema**, follow these steps:

1. **Map Strong Entities**:
   - Create a table for each strong entity.
   - Include all attributes, with the key attribute as the primary key.
2. **Map Weak Entities**:
   - Create a table with attributes and include the primary key of the owner entity as a foreign key.
   - The primary key is a composite of the weak entity’s partial key and the owner’s primary key.
3. **Map Relationships**:
   - **1:1 or 1:N Relationships**: Add a foreign key in the table on the “many” side referencing the “one” side’s primary key.
   - **M:N Relationships**: Create a separate table with foreign keys referencing both entities’ primary keys. The combination of foreign keys forms the primary key.
4. **Map Multi-valued Attributes**:
   - Create a separate table with the entity’s primary key and the multi-valued attribute.
5. **Map Composite Attributes**:
   - Include each component as a separate column in the entity’s table.
6. **Map Derived Attributes**:
   - Typically not stored in the schema but computed when needed.

#### a. Hospital System Relational Schema

**Tables**:

- `Patient`:
  - `PatientID` (Primary Key, Integer)
  - `FirstName` (Varchar)
  - `LastName` (Varchar)
  - `DOB` (Date)
- `PatientPhone` (for multi-valued `Phone`):
  - `PatientID` (Foreign Key referencing `Patient`, Integer)
  - `Phone` (Varchar)
  - Primary Key: (`PatientID`, `Phone`)
- `Doctor`:
  - `DoctorID` (Primary Key, Integer)
  - `FirstName` (Varchar)
  - `LastName` (Varchar)
  - `Specialty` (Varchar)
- `Appointment`:
  - `AppointmentID` (Primary Key, Integer)
  - `PatientID` (Foreign Key referencing `Patient`, Integer)
  - `DoctorID` (Foreign Key referencing `Doctor`, Integer)
  - `Date` (Date)
  - `Time` (Time)
  - (`Diagnosis` is derived, not stored)

**Explanation**:

- `Patient` and `Doctor` are strong entities, mapped directly to tables.
- `Phone` (multi-valued) requires a separate `PatientPhone` table.
- `Makes` and `Attends` (1:N) are represented by foreign keys `PatientID` and `DoctorID` in `Appointment`.

#### b. Library System Relational Schema

**Tables**:

- `Book`:
  - `ISBN` (Primary Key, Varchar)
  - `Title` (Varchar)
  - `Author` (Varchar)
  - `PublicationYear` (Integer)
- `Member`:
  - `MemberID` (Primary Key, Integer)
  - `FirstName` (Varchar)
  - `LastName` (Varchar)
  - `Email` (Varchar)
- `Loan`:
  - `LoanID` (Primary Key, Integer)
  - `MemberID` (Foreign Key referencing `Member`, Integer)
  - `ISBN` (Foreign Key referencing `Book`, Varchar)
  - `BorrowDate` (Date)
  - `ReturnDate` (Date)
  - (`Fine` is derived, not stored)

**Explanation**:

- `Book` and `Member` are strong entities, mapped to tables.
- `Loan` represents the M:N relationship between `Member` and `Book`, so it includes foreign keys `MemberID` and `ISBN`.
- No multi-valued or composite attributes require additional tables.

### 4. Additional Practice: E-Commerce System ERD

To reinforce learning, let’s design an ERD for an e-commerce system (as an extra example).

**Entities and Attributes**:

- **Customer**:
  - `CustomerID` (key, unique)
  - `Name` (composite: `FirstName`, `LastName`)
  - `Email`
- **Product**:
  - `ProductID` (key, unique)
  - `Name`
  - `Price`
- **Order**:
  - `OrderID` (key, unique)
  - `OrderDate`
  - `TotalAmount` (derived)

**Relationships**:

- `Customer`–`Places`–`Order`: A customer can place multiple orders (1:N).
- `Order`–`Contains`–`Product`: An order can contain multiple products, and a product can be in multiple orders (M:N).

**Cardinality**:

- `Customer` to `Order`: 1:N.
- `Order` to `Product`: M:N (requires a separate table for the relationship).

**Text-Based ERD**:

```
[Customer] --(Places)--> [Order] --(Contains)--> [Product]
   |                        |                        |
   | CustomerID (PK)       | OrderID (PK)           | ProductID (PK)
   | Name                  | OrderDate              | Name
   | Email                 | TotalAmount (derived)  | Price
```

**Relational Schema**:

- `Customer`:
  - `CustomerID` (Primary Key, Integer)
  - `FirstName` (Varchar)
  - `LastName` (Varchar)
  - `Email` (Varchar)
- `Product`:
  - `ProductID` (Primary Key, Integer)
  - `Name` (Varchar)
  - `Price` (Decimal)
- `Order`:
  - `OrderID` (Primary Key, Integer)
  - `CustomerID` (Foreign Key referencing `Customer`, Integer)
  - `OrderDate` (Date)
- `OrderDetail` (for M:N `Contains` relationship):
  - `OrderID` (Foreign Key referencing `Order`, Integer)
  - `ProductID` (Foreign Key referencing `Product`, Integer)
  - `Quantity` (Integer)
  - Primary Key: (`OrderID`, `ProductID`)

---

## Key Takeaways

- The **ER Model** is a powerful tool for conceptual database design, using entities, attributes, relationships, and cardinality.
- **ERDs** visually represent the database structure, making it easier to communicate and validate designs.
- **Entities** (strong/weak), **attributes** (simple, composite, multi-valued, derived), and **relationships** (1:1, 1:N, M:N) form the core of the model.
- **Cardinality** and **participation constraints** define how entities interact.
- Converting ERDs to **relational schemas** involves mapping entities, relationships, and attributes to tables, foreign keys, and constraints.
- Tools like **Lucidchart** or **Draw.io** simplify ERD creation with templates and intuitive interfaces.

## Recommended Next Steps

- Practice creating ERDs for other systems (e.g., airline reservation, social media platform).
- Learn SQL to implement and query the relational schemas.
- Study normalization to refine relational schemas and eliminate redundancy.
