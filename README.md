Unit I: Introduction to Database Systems
1.1 View of Data (Data Abstraction)
Data abstraction is one of the most fundamental concepts in database systems. It refers to the process of hiding the complexity of how data is stored and showing only what is relevant to the user. There are three levels of data abstraction:
Physical Level — This is the lowest level. It describes how data is actually stored in the computer — in terms of bits, files, and storage structures. End users never see this level.
Logical Level — This is the middle level. It describes what data is stored and the relationships between them. For example, a student record might have fields like Student_ID, Name, and Department. Programmers and database designers work at this level.
View Level — This is the highest level, closest to the end user. It shows only a portion of the database relevant to a specific user. For example, a teacher might only see student grades, while a finance office sees fee records.
This layered approach allows changes at one level without affecting others — a property known as data independence.

1.2 Purpose of Database Systems
Before database systems existed, data was stored in file-based systems — simple flat files managed by individual applications. This caused many problems:
	• Data redundancy — the same data was duplicated across multiple files
	• Data inconsistency — different copies of the same data could have different values
	• Difficulty in accessing data — you needed custom programs just to retrieve information
	• Security and integrity issues — no central control over who could access or modify data
Database systems were developed to solve all of these problems. A Database Management System (DBMS) is software that manages a database, providing tools to store, retrieve, and manipulate data efficiently, securely, and consistently.

1.3 History of Database Systems
The evolution of database systems reflects the growing complexity of data management needs:
Era	Model	Description
1950s–60s	File Systems	Data stored in flat files; no relationships between data
1960s	Hierarchical Databases	Data organized in a tree structure (parent-child relationships)
1970s	Network Databases	More flexible than hierarchical; data linked in a graph structure
1970s–Present	Relational Databases	Data stored in tables with relationships; uses SQL
Modern Era	NoSQL, Cloud, Big Data	Handles unstructured, distributed, and large-scale data
The relational model, introduced by E.F. Codd in 1970, revolutionized how we think about data storage and querying, and remains the dominant model today.

1.4 Database-System Applications
Databases are embedded in nearly every aspect of modern life. Common applications include:
	• Banking Systems — managing accounts, transactions, and loan records
	• Airline Reservations — booking seats, managing schedules and passenger data
	• Universities — student registration, grade management, course enrollment
	• Online Shopping — product listings, order management, payment processing
	• Social Media Platforms — user profiles, posts, connections, and activity logs
	• Healthcare Systems — patient records, prescriptions, appointment scheduling
	• Telecommunication — billing records, call data, subscription management
	• Government Records — citizen data, tax records, public services
The common thread across all these applications is the need to store, retrieve, and manage large amounts of structured data reliably and efficiently.

1.5 Database Languages
Database systems use specialized languages to interact with data. There are four main types:
DDL – Data Definition Language
Used to define and modify the structure of the database.
Examples: CREATE TABLE, ALTER TABLE, DROP TABLE
DML – Data Manipulation Language
Used to insert, update, delete, and retrieve data.
Examples: INSERT INTO, UPDATE, DELETE FROM, SELECT
DCL – Data Control Language
Used to control access permissions to the database.
Examples: GRANT, REVOKE
TCL – Transaction Control Language
Used to manage transactions — groups of operations that must succeed or fail together.
Examples: COMMIT, ROLLBACK, SAVEPOINT
Together, these languages provide complete control over a database — from building its structure to managing who can use it and how.

1.6 Database Design
Good database design is critical to building a system that is efficient, scalable, and free of errors. The design process happens in three phases:
Conceptual Design
The highest-level design phase. A developer creates an Entity-Relationship (ER) diagram to map out entities (like Student, Course) and the relationships between them. This phase focuses on what needs to be stored, not how.
Logical Design
Converts the conceptual model into actual tables and relationships. Each entity becomes a table, and relationships are expressed through keys. This is where the schema is formally defined.
Physical Design
Determines how the database will be physically stored — including file formats, indexes, storage allocation, and performance optimization settings.

1.7 Database Engine
The database engine is the core software component of a DBMS. It is responsible for:
	• Storing and retrieving data from disk
	• Processing queries written in SQL
	• Ensuring transactions follow the ACID properties
	• Managing concurrency so multiple users can access data simultaneously without conflict
The ACID properties that the engine enforces are:
Atomicity — A transaction is "all or nothing." If one step fails, the entire transaction is rolled back. Example: In a bank transfer, if debiting Account A succeeds but crediting Account B fails, the whole transaction is cancelled and Account A's balance is restored.
Consistency — A transaction must always leave the database in a valid state. Example: If a rule states that an account balance cannot go below zero, any transaction attempting to violate this rule will be rejected.
Isolation — Concurrent transactions do not interfere with each other. Each transaction runs as if it is the only one executing. Example: If two students simultaneously try to register for the last seat in a course, isolation ensures only one succeeds.
Durability — Once a transaction is committed, its changes are permanent — even in the event of a power failure or system crash. The DBMS uses logs and backups to ensure this.

1.8 Database and Application Architecture
Database systems are organized into architectural tiers that separate different responsibilities:
One-Tier Architecture
The application and database reside on the same machine. Simple and used in personal or development settings. Example: A desktop application with a local database.
Two-Tier Architecture
The client application communicates directly with a database server over a network. Example: A banking teller application connecting to a central database server.
Three-Tier Architecture
Separates the system into three layers:
	1. Presentation Layer (UI) — what the user sees
	2. Application Layer (Business Logic) — processes user requests
	3. Database Layer — stores and retrieves data
Example: An online shopping website where the browser is the UI, a web server handles logic, and a database server stores product and order data. This is the most common architecture for modern web applications.

1.9 Database Users and Administrators
Different people interact with a database system in different ways:
Database Administrator (DBA)
The DBA is responsible for managing and maintaining the entire database system. Their duties include setting up the database, tuning performance, managing user access, performing backups, and ensuring security. The DBA has the highest level of authority over the database.
Application Programmers
These are developers who write software that interacts with the database. They use programming languages like Python, Java, or PHP, combined with SQL, to build applications for end users.
Sophisticated Users (SQL Experts)
These users interact directly with the database using SQL queries. They might be data analysts or researchers who need to extract specific information without going through a pre-built application.
Naïve/End Users
These are everyday users who interact with the database indirectly through applications. A student registering for a course online, or a customer placing an order — they use the database without knowing it.



Unit II: Database Design
Introduction
If Unit I was about understanding what databases are and why they matter, Unit II is about learning how to actually build one properly. Database design is the process of creating a structured blueprint for how data will be organized, stored, and related. A poorly designed database leads to redundancy, inconsistency, and retrieval problems — which is exactly what databases are supposed to prevent. This unit walks through the entire design process, from understanding requirements all the way to creating a working relational schema.

The Database Design Process
Good database design follows a clear, step-by-step methodology. Skipping any step typically results in a database that is inefficient or difficult to maintain. The five major phases are:
Step 1 — Requirements Collection and Analysis
Before any design work begins, designers must fully understand what the database needs to do. This involves identifying what data users need, understanding the business rules that govern that data, and determining how different pieces of data relate to each other. Stakeholders — the people who will actually use the system — are consulted throughout this phase. For example, designing a university database would require understanding what information is needed about students, courses, instructors, and enrollments, and how those entities interact.
Step 2 — Conceptual Database Design
This phase uses the Entity-Relationship (ER) model to create a high-level visual representation of the data. The conceptual design is completely independent of any specific database management system — it focuses purely on entities, their attributes, and the relationships between them. The output of this phase is an ER Diagram (ERD), which serves as the blueprint for everything that follows.
Step 3 — Logical Database Design
The conceptual ER model is now transformed into an actual relational schema — meaning tables, columns, and keys that can be implemented in a real DBMS. Activities include defining tables for each entity, assigning primary keys to uniquely identify records, creating foreign keys to establish relationships between tables, and applying normalization to eliminate redundancy.
Step 4 — Physical Database Design
This phase determines how the database will actually be stored on disk. It involves decisions about file organization, indexing strategies, and access paths. The physical design is guided by performance requirements and is highly dependent on the specific DBMS being used.
Step 5 — Database Implementation
The final phase involves writing the actual SQL code to create the database, loading initial data, and making the system available to users. This is where the design becomes a real, functional database.

The Relational Model
Before diving into the ER model, it is important to understand the foundation of how data is stored in modern databases — the relational model. The relational model organizes data into tables (also called relations), where:
	• A Relation (Table) is a structured set of data with rows and columns
	• A Tuple (Row) is a single record in the table — for example, one student's data
	• An Attribute (Column) is a specific property being recorded — for example, Name or Major
	• A Domain is the set of allowed values for an attribute — for example, the Major attribute might only allow values like "IT", "CS", or "EE"
Properties of a well-formed relation:
	• The order of rows does not matter
	• Every row must be unique — no two identical records
	• Each attribute value must be atomic (a single, indivisible value)
	• Every attribute has a defined domain

The Entity-Relationship (ER) Model
The ER model is the primary tool used during conceptual database design. It represents the real world using three main building blocks: entities, attributes, and relationships.
Entities
An entity is any real-world object or concept that needs to be represented in the database. Examples include a specific student, a department, a project, or a customer. Entities of the same kind are grouped into an entity type — for instance, all students belong to the STUDENT entity type.
There are two kinds of entity types:
Strong Entity — An entity that has its own key attribute and can exist independently. Drawn as a single rectangle in an ER diagram. Example: CUSTOMER.
Weak Entity — An entity that does not have its own key attribute and depends on another entity for its identification. It must always be associated with a strong (owner/identifying) entity through an identifying relationship. Drawn as a double rectangle. Example: LOAN depends on BANK-BRANCH to exist.
Attributes
Attributes are the properties that describe an entity. There are several types:
Simple (Atomic) Attribute — Holds a single, indivisible value. Example: SSN, Sex. Each entity has exactly one value for this attribute.
Composite Attribute — Made up of multiple sub-attributes. Example: Address can be broken into Street, City, District, and Country. Name can be split into FirstName, MiddleName, and LastName.
Multivalued Attribute — Can hold more than one value for a single entity. Example: A person may have multiple phone numbers, or a student may have multiple previous degrees. Denoted with curly braces: {PhoneNumber}.
Derived Attribute — Its value is calculated or derived from another attribute rather than stored directly. Example: Age can be derived from DateOfBirth; Years of Experience can be derived from JoiningDate. Shown with a dashed oval in the ER diagram.
Key Attribute — The attribute that uniquely identifies each entity within an entity type. Example: StudentID uniquely identifies each student. Shown with an underlined oval in the ER diagram. An entity type may have more than one possible key — these are called candidate keys.
ER Diagram Symbols
Symbol	Meaning
Rectangle	Entity
Double Rectangle	Weak Entity
Diamond	Relationship
Double Diamond	Identifying Relationship (for Weak Entity)
Oval	Attribute
Underlined Oval	Key Attribute
Double Oval	Multivalued Attribute
Clustered Ovals	Composite Attribute
Dashed Oval	Derived Attribute
Double Line	Total Participation

Relationships and Relationship Types
A relationship connects two or more entities that have a meaningful association. For example, an employee works for a department, or a student enrolls in a course. Relationships of the same type are grouped into a relationship type.
The degree of a relationship type refers to how many entity types participate in it. Most relationships are binary (degree 2), involving exactly two entity types. When an entity type is related to itself, it is called a recursive relationship. The SUPERVISION relationship in a company database is a classic example — an EMPLOYEE supervises another EMPLOYEE, with one playing the role of "Supervisor" and the other playing "Supervisee."
Relationships can also have attributes of their own. For example, in a WORKS_ON relationship between an EMPLOYEE and a PROJECT, the attribute "Hours" describes how many hours per week the employee works on that project — information that belongs to the relationship rather than to either entity alone.

Mapping Cardinalities (Relationship Constraints)
Cardinality defines how many instances of one entity can be associated with instances of another through a relationship. There are four types:
One-to-One (1:1) — Each entity in A is associated with at most one entity in B, and vice versa. Example: One employee manages one department, and one department is managed by one employee (MANAGES relationship).
One-to-Many (1:N) — One entity in A can be associated with many entities in B, but each entity in B is associated with only one entity in A. Example: One department employs many employees, but each employee works for only one department (WORKS_FOR relationship).
Many-to-One (N:1) — The reverse of 1:N. Many entities in A relate to one entity in B.
Many-to-Many (M:N) — Entities in A can relate to many entities in B, and vice versa. Example: An employee can work on many projects, and a project can have many employees working on it (WORKS_ON relationship).

Participation Constraints
Alongside cardinality, participation constraints define whether an entity's existence depends on its involvement in a relationship.
Total Participation — Every entity in the entity type must participate in at least one instance of the relationship. This is sometimes called an existence dependency. Shown with a double line in the ER diagram. Example: Every employee must work for a department — so EMPLOYEE has total participation in WORKS_FOR.
Partial Participation — Some entities may not participate in any relationship instance. Shown with a single line. Example: Not every employee is a manager, so EMPLOYEE has partial participation in MANAGES.
A more precise way to express these constraints is the (min, max) notation, which specifies the minimum and maximum number of relationship instances an entity can participate in. For example:
	• Employee in MANAGES: (0,1) — an employee manages at most one department, but may manage none
	• Department in MANAGES: (1,1) — every department must have exactly one manager

Keys in Relational Databases
Keys are fundamental to identifying records and establishing relationships between tables.
Primary Key — An attribute (or set of attributes) that uniquely identifies each record in a table. It must be unique and cannot be NULL. Example: StudentID in a Student table.
Candidate Key — Any attribute or combination of attributes that could serve as the primary key. A table may have multiple candidate keys, and the designer chooses one as the primary key. Example: Both StudentID and Email could uniquely identify a student.
Composite Key — A primary key made up of two or more attributes combined. Used when no single attribute is sufficient for unique identification. Example: (StudentID, CourseID) together form the primary key of an Enrollment table.
Foreign Key — An attribute in one table that references the primary key of another table, establishing a link between the two. Foreign keys are the mechanism that enforces relationships in a relational database.

From ER Model to Relational Schema
Once the ER diagram is complete, the logical design phase converts it into actual database tables. The general rules are:
	• Each strong entity type becomes a table, with its attributes becoming columns and its key attribute becoming the primary key
	• Each weak entity type also becomes a table, but its primary key is a combination of its own partial key and the primary key of its owner (identifying) entity
	• Each relationship type may become a separate table (especially for M:N relationships), containing the primary keys of both participating entities as foreign keys
	• Multivalued attributes are converted into separate tables

The Extended ER (EER) Model
The standard ER model has one significant limitation — it does not natively support the concept of specialization or generalization, which are important when certain entities share some attributes but differ in others. For example, EMPLOYEE might be specialized into MANAGER and TECHNICIAN, each with their own additional attributes.
The Extended Entity-Relationship (EER) Model addresses this by incorporating set-subset relationships and specialization/generalization hierarchies. These concepts allow designers to model inheritance-like structures in their database schemas.

A Complete Example: The COMPANY Database
To bring all these concepts together, consider a COMPANY database with the following requirements:
	• The company is organized into DEPARTMENTs, each with a name, number, and a manager (an employee) with a start date
	• Each department controls one or more PROJECTs, each with a name, number, and location
	• Each EMPLOYEE has a social security number, address, salary, sex, and birth date; they work for one department and may work on multiple projects (with hours tracked per project); each employee may have a direct supervisor
	• Each employee may have DEPENDENTs (family members), each with a name, sex, birth date, and relationship type
From these requirements, four entity types emerge: EMPLOYEE, DEPARTMENT, PROJECT, and DEPENDENT. DEPENDENT is a weak entity because it cannot be identified without knowing which EMPLOYEE it belongs to. The relationship types include WORKS_FOR (N:1), MANAGES (1:1), WORKS_ON (M:N), CONTROLS (1:N), SUPERVISION (recursive 1:N), and DEPENDENTS_OF (identifying relationship for the weak entity).


Unit III: Introduction to SQL
Introduction
If Unit II was about designing the blueprint of a database, Unit III is about bringing that blueprint to life and actually working with the data inside it. SQL — Structured Query Language — is the standard language used to communicate with relational databases. It is the tool that allows users to create database structures, insert and retrieve data, update records, and delete information. Nearly every major database system in the world — MySQL, PostgreSQL, Oracle, Microsoft SQL Server — uses SQL as its primary language. Understanding SQL is therefore one of the most practically valuable skills in the entire course.

3.1 Overview of SQL Query Language
SQL was originally developed at IBM in the 1970s, based on E.F. Codd's relational model. It was standardized by ANSI in 1986 and has been updated multiple times since. Despite its age, SQL remains the dominant language for relational databases today.
SQL is not a traditional programming language — it is a declarative language, meaning you describe what you want, not how to get it. You tell the database "give me all students with a GPA above 3.5" and the database engine figures out how to retrieve that information efficiently.
SQL is made up of several sub-languages that were introduced in Unit I:
	• DDL (Data Definition Language) — for creating and modifying the structure of database objects like tables
	• DML (Data Manipulation Language) — for inserting, updating, deleting, and querying data
	• DCL (Data Control Language) — for managing access permissions
	• TCL (Transaction Control Language) — for managing transactions
This unit focuses primarily on DDL and DML.

3.2 SQL Data Definition
The DDL component of SQL allows designers to define the schema of a database — that is, the structure of tables, the data types of attributes, and the constraints that govern the data.
Creating a Table
The CREATE TABLE statement defines a new table and its columns:
sql
CREATE TABLE Student (
    StudentID   INT,
    Name        VARCHAR(50),
    Major       VARCHAR(30),
    GPA         DECIMAL(3,2),
    PRIMARY KEY (StudentID)
);
Each column is defined with a name and a data type. Common SQL data types include:
	• INT or INTEGER — whole numbers
	• DECIMAL(p,d) or NUMERIC(p,d) — numbers with decimal places; p is total digits, d is digits after the decimal point
	• VARCHAR(n) — variable-length character strings up to n characters
	• CHAR(n) — fixed-length character strings of exactly n characters
	• DATE — stores date values (year, month, day)
	• BOOLEAN — stores true/false values
Integrity Constraints
Constraints enforce rules on the data to maintain accuracy and consistency:
NOT NULL — ensures a column cannot be left empty:
sql
Name VARCHAR(50) NOT NULL
UNIQUE — ensures all values in a column are distinct:
sql
Email VARCHAR(100) UNIQUE
PRIMARY KEY — uniquely identifies each row; implies both NOT NULL and UNIQUE:
sql
PRIMARY KEY (StudentID)
FOREIGN KEY — links a column to the primary key of another table, enforcing referential integrity:
sql
FOREIGN KEY (DeptID) REFERENCES Department(DeptID)
CHECK — enforces a condition on the values in a column:
sql
GPA DECIMAL(3,2) CHECK (GPA >= 0.0 AND GPA <= 4.0)
DEFAULT — assigns a default value when none is provided:
sql
Major VARCHAR(30) DEFAULT 'Undecided'
Modifying and Removing Tables
To add a new column to an existing table:
sql
ALTER TABLE Student ADD Email VARCHAR(100);
To remove a column:
sql
ALTER TABLE Student DROP COLUMN Email;
To delete an entire table and all its data permanently:
sql
DROP TABLE Student;
To remove all data from a table but keep its structure:
sql
TRUNCATE TABLE Student;

3.3 Basic Structure of SQL Queries
The most fundamental SQL operation is the query — retrieving data from one or more tables. Every SQL query is built around three clauses: SELECT, FROM, and WHERE.
sql
SELECT attribute_list
FROM   table_name
WHERE  condition;
SELECT Clause
The SELECT clause specifies which columns to retrieve. To retrieve all columns, use the asterisk wildcard:
sql
SELECT * FROM Student;
To retrieve specific columns only:
sql
SELECT Name, GPA FROM Student;
To eliminate duplicate rows from the result, use DISTINCT:
sql
SELECT DISTINCT Major FROM Student;
You can also perform arithmetic in the SELECT clause. For example, to see an employee's salary with a 10% raise:
sql
SELECT Name, Salary * 1.10 AS IncreasedSalary FROM Employee;
The AS keyword renames a column in the output — this is called an alias.
FROM Clause
The FROM clause specifies which table (or tables) to retrieve data from:
sql
SELECT Name FROM Student;
When multiple tables are listed in the FROM clause, SQL generates the Cartesian product of those tables (every combination of rows). This is then filtered using the WHERE clause.
WHERE Clause
The WHERE clause filters rows based on a condition. Only rows where the condition is true are included in the result:
sql
SELECT Name, GPA
FROM   Student
WHERE  Major = 'CS' AND GPA > 3.0;
Comparison operators available: =, <> (not equal), <, >, <=, >=
Logical operators: AND, OR, NOT
Joining Tables
When data from multiple tables is needed, a join combines them based on a matching condition. The most common type is the natural join or inner join:
sql
SELECT Student.Name, Course.CourseName
FROM   Student, Enrollment, Course
WHERE  Student.StudentID = Enrollment.StudentID
AND    Enrollment.CourseID = Course.CourseID;
This retrieves student names alongside the course names they are enrolled in, by linking the three tables through their shared key columns.
ORDER BY Clause
Results can be sorted using ORDER BY. The default is ascending (ASC); use DESC for descending:
sql
SELECT Name, GPA
FROM   Student
ORDER BY GPA DESC;

3.4 Additional Basic Operations
LIKE Operator (String Matching)
The LIKE operator is used for pattern matching in strings. Two wildcards are used:
	• % matches any sequence of characters (including none)
	• _ matches exactly one character
sql
-- Find all students whose name starts with 'S'
SELECT Name FROM Student WHERE Name LIKE 'S%';
-- Find students whose name is exactly 5 characters
SELECT Name FROM Student WHERE Name LIKE '_____';
-- Find students whose name contains 'ang'
SELECT Name FROM Student WHERE Name LIKE '%ang%';
BETWEEN Operator
Used to filter values within a range (inclusive of both endpoints):
sql
SELECT Name, Salary
FROM   Employee
WHERE  Salary BETWEEN 20000 AND 50000;
IN Operator
Checks whether a value matches any value in a list:
sql
SELECT Name FROM Student
WHERE  Major IN ('CS', 'IT', 'EE');
Renaming (AS)
Aliases can be applied to both columns and tables to make queries more readable:
sql
SELECT S.Name, S.GPA
FROM   Student AS S
WHERE  S.GPA > 3.5;
String Operations
SQL supports several built-in string functions:
sql
-- Convert to uppercase
SELECT UPPER(Name) FROM Student;
-- Get length of a string
SELECT LENGTH(Name) FROM Student;
-- Extract a substring
SELECT SUBSTRING(Name, 1, 3) FROM Student;

3.5 Set Operations
SQL supports set operations that combine the results of two or more queries. For these to work, both queries must return the same number of columns with compatible data types.
UNION — combines results from two queries and removes duplicates:
sql
SELECT Name FROM Student
UNION
SELECT Name FROM Instructor;
UNION ALL — combines results and keeps all duplicates:
sql
SELECT Name FROM Student
UNION ALL
SELECT Name FROM Instructor;
INTERSECT — returns only rows that appear in both result sets:
sql
SELECT CourseID FROM Enrollment_2024
INTERSECT
SELECT CourseID FROM Enrollment_2025;
EXCEPT (or MINUS) — returns rows that appear in the first result set but not the second:
sql
SELECT StudentID FROM Student
EXCEPT
SELECT StudentID FROM Enrollment;
-- Returns students who are not enrolled in any course

3.6 Null Values
NULL in SQL represents a missing, unknown, or not-applicable value. It is not the same as zero or an empty string — it literally means the value is absent. Understanding how NULL behaves is critical because it can lead to unexpected results if not handled carefully.
NULL in Arithmetic
Any arithmetic operation involving NULL produces NULL:
	• 5 + NULL = NULL
	• NULL * 10 = NULL
NULL in Comparisons
Any comparison with NULL produces UNKNOWN (not TRUE or FALSE):
	• NULL = NULL evaluates to UNKNOWN, not TRUE
To check for NULL values, SQL provides the IS NULL and IS NOT NULL operators:
sql
-- Find students with no declared major
SELECT Name FROM Student WHERE Major IS NULL;
-- Find students who have a declared major
SELECT Name FROM Student WHERE Major IS NOT NULL;
Three-Valued Logic
Because NULL comparisons result in UNKNOWN, SQL operates on three-valued logic: TRUE, FALSE, and UNKNOWN. In a WHERE clause, only rows where the condition evaluates to TRUE are returned — rows where the condition is UNKNOWN (due to NULL involvement) are excluded.

3.7 Aggregate Functions
Aggregate functions perform calculations across a set of rows and return a single summary value. They are used in the SELECT clause and are typically combined with the GROUP BY clause.
The five main aggregate functions are:
	• COUNT() — counts the number of rows or non-NULL values
	• SUM() — calculates the total of numeric values
	• AVG() — calculates the average of numeric values
	• MAX() — returns the largest value
	• MIN() — returns the smallest value
sql
-- Count total number of students
SELECT COUNT(*) FROM Student;
-- Count students with a declared major (ignores NULLs)
SELECT COUNT(Major) FROM Student;
-- Average GPA of all students
SELECT AVG(GPA) FROM Student;
-- Highest and lowest salary
SELECT MAX(Salary), MIN(Salary) FROM Employee;
-- Total salary expenditure
SELECT SUM(Salary) FROM Employee;
GROUP BY Clause
GROUP BY groups rows that share a common value in a specified column, then applies aggregate functions to each group separately:
sql
-- Count students in each major
SELECT Major, COUNT(*) AS StudentCount
FROM   Student
GROUP BY Major;
-- Average GPA per department
SELECT Major, AVG(GPA) AS AvgGPA
FROM   Student
GROUP BY Major;
HAVING Clause
HAVING filters groups after aggregation has been applied — it is the GROUP BY equivalent of WHERE. You cannot use WHERE to filter on aggregate results; HAVING is used for that purpose:
sql
-- Show only majors with more than 10 students
SELECT Major, COUNT(*) AS StudentCount
FROM   Student
GROUP BY Major
HAVING COUNT(*) > 10;
The order of clauses in a full SQL query is:
SELECT → FROM → WHERE → GROUP BY → HAVING → ORDER BY

3.8 Nested Subqueries
A subquery (also called a nested query or inner query) is a complete SQL query placed inside another query. The outer query uses the result of the inner query as part of its condition. Subqueries are powerful tools for answering complex questions that cannot be expressed in a single simple query.
Subquery in WHERE with IN
sql
-- Find students enrolled in the 'Databases' course
SELECT Name
FROM   Student
WHERE  StudentID IN (
    SELECT StudentID
    FROM   Enrollment
    WHERE  CourseID = (
        SELECT CourseID
        FROM   Course
        WHERE  CourseName = 'Databases'
    )
);
Subquery with Comparison Operators
When a subquery is guaranteed to return exactly one value, it can be used directly with comparison operators:
sql
-- Find employees earning more than the average salary
SELECT Name, Salary
FROM   Employee
WHERE  Salary > (SELECT AVG(Salary) FROM Employee);
EXISTS and NOT EXISTS
EXISTS returns TRUE if the subquery produces at least one row. It is used to check for the existence of related records:
sql
-- Find students who are enrolled in at least one course
SELECT Name
FROM   Student S
WHERE  EXISTS (
    SELECT *
    FROM   Enrollment E
    WHERE  E.StudentID = S.StudentID
);
NOT EXISTS is the opposite — it finds rows where no matching record exists in the subquery:
sql
-- Find students not enrolled in any course
SELECT Name
FROM   Student S
WHERE  NOT EXISTS (
    SELECT *
    FROM   Enrollment E
    WHERE  E.StudentID = S.StudentID
);
ALL and SOME/ANY
	• ALL — the condition must be true for every value returned by the subquery
	• SOME or ANY — the condition must be true for at least one value
sql
-- Find employees earning more than ALL employees in Department 3
SELECT Name FROM Employee
WHERE Salary > ALL (
    SELECT Salary FROM Employee WHERE DeptID = 3
);

3.9 Modification of the Database
SQL provides three DML statements for modifying data: INSERT, UPDATE, and DELETE.
INSERT — Adding New Records
To insert a single row with values for all columns (in table order):
sql
INSERT INTO Student
VALUES (101, 'Sonam Wangchuk', 'CS', 3.8);
To insert a row specifying only certain columns (safer and clearer):
sql
INSERT INTO Student (StudentID, Name, Major)
VALUES (102, 'Karma Dema', 'IT');
To insert multiple rows at once:
sql
INSERT INTO Student (StudentID, Name, Major, GPA) VALUES
(103, 'Tashi Dorji', 'EE', 3.2),
(104, 'Pema Zangmo', 'CS', 3.9),
(105, 'Ugyen Wangdi', 'IT', 2.8);
To insert data from another table:
sql
INSERT INTO GraduateStudent (StudentID, Name, GPA)
SELECT StudentID, Name, GPA
FROM   Student
WHERE  GPA >= 3.5;
UPDATE — Modifying Existing Records
The UPDATE statement modifies existing data. Always use a WHERE clause with UPDATE — without it, every row in the table will be changed:
sql
-- Update the major of one specific student
UPDATE Student
SET    Major = 'EE'
WHERE  StudentID = 101;
Updating multiple columns at once:
sql
UPDATE Employee
SET    Salary = Salary * 1.10,
       DeptID = 2
WHERE  EmployeeID = 205;
Updating based on a subquery:
sql
-- Give a 5% raise to all employees who work on Project 1
UPDATE Employee
SET    Salary = Salary * 1.05
WHERE  EmployeeID IN (
    SELECT EmployeeID FROM WorksOn WHERE ProjectID = 1
);
DELETE — Removing Records
The DELETE statement removes rows from a table. Like UPDATE, always use a WHERE clause unless you intentionally want to delete all rows:
sql
-- Delete one specific student
DELETE FROM Student
WHERE  StudentID = 101;
Delete based on a condition:
sql
-- Delete all students with GPA below 1.0
DELETE FROM Student
WHERE  GPA < 1.0;
Delete based on a subquery:
sql
-- Delete enrollments for a course that has been cancelled
DELETE FROM Enrollment
WHERE  CourseID IN (
    SELECT CourseID FROM Course WHERE Status = 'Cancelled'
);
Delete all rows (use with caution — this empties the table):
sql
DELETE FROM Student;

Putting It All Together — A Complete Example
Consider a university database with tables for Student, Course, and Enrollment. A complex query might look like this:
sql
-- Find the name and GPA of students enrolled in more than 3 courses,
-- sorted by GPA from highest to lowest
SELECT S.Name, S.GPA, COUNT(E.CourseID) AS CourseCount
FROM   Student S, Enrollment E
WHERE  S.StudentID = E.StudentID
GROUP BY S.StudentID, S.Name, S.GPA
HAVING COUNT(E.CourseID) > 3
ORDER BY S.GPA DESC;
This single query demonstrates SELECT with aliases, a join across two tables, filtering with WHERE, grouping with GROUP BY, post-aggregation filtering with HAVING, and sorting with ORDER BY — all the key building blocks of SQL working together.


Unit IV: Intermediate and Advanced SQL
Introduction
Unit III covered the fundamentals of SQL — creating tables, writing basic queries, and modifying data. Unit IV builds on that foundation and explores the more powerful, sophisticated features of SQL that are used in real-world database applications. These advanced features are what separate a beginner SQL user from a professional database developer. Topics like views, triggers, stored procedures, and authorization are the building blocks of secure, maintainable, and high-performance database systems used in industry every day.

4.1 Join Expressions
In Unit III, joins were introduced informally by listing multiple tables in the FROM clause and linking them in the WHERE clause. SQL provides more explicit and powerful join syntax that gives developers finer control over how tables are combined.
Inner Join
An inner join returns only the rows where there is a match in both tables. Rows with no match in either table are excluded:
sql
SELECT Student.Name, Course.CourseName
FROM   Student
INNER JOIN Enrollment ON Student.StudentID = Enrollment.StudentID
INNER JOIN Course ON Enrollment.CourseID = Course.CourseID;
Natural Join
A natural join automatically joins tables based on columns that share the same name and data type. It eliminates the need to specify the join condition explicitly:
sql
SELECT Name, CourseName
FROM   Student NATURAL JOIN Enrollment NATURAL JOIN Course;
However, natural joins can be dangerous — if two tables accidentally share a column name that was not intended as a join key, the query will produce incorrect results. It is generally safer to use explicit join conditions.
Left Outer Join
Returns all rows from the left table, and matched rows from the right table. Where there is no match, NULL values are filled in for the right table's columns. This is useful when you want to include records even if they have no related records in the other table:
sql
-- Show all students, including those not enrolled in any course
SELECT Student.Name, Enrollment.CourseID
FROM   Student
LEFT OUTER JOIN Enrollment ON Student.StudentID = Enrollment.StudentID;
Right Outer Join
Returns all rows from the right table and matched rows from the left table. NULLs fill in where there is no match on the left side:
sql
SELECT Student.Name, Course.CourseName
FROM   Enrollment
RIGHT OUTER JOIN Course ON Enrollment.CourseID = Course.CourseID;
Full Outer Join
Returns all rows from both tables. Where there is no match on either side, NULLs are filled in:
sql
SELECT Student.Name, Course.CourseName
FROM   Student
FULL OUTER JOIN Enrollment ON Student.StudentID = Enrollment.StudentID;
Cross Join
Returns the Cartesian product of both tables — every possible combination of rows. This is rarely used intentionally but is the default when no join condition is specified:
sql
SELECT Student.Name, Course.CourseName
FROM   Student CROSS JOIN Course;
Self Join
A table can be joined with itself, which is useful for recursive or hierarchical relationships. For example, finding each employee's supervisor from the same Employee table:
sql
SELECT E.Name AS Employee, S.Name AS Supervisor
FROM   Employee E
JOIN   Employee S ON E.SupervisorID = S.EmployeeID;

4.2 Views
A view is a virtual table defined by a stored SQL query. It does not store data itself — every time the view is accessed, the underlying query is executed to generate the result. Views are one of the most important features in SQL for several reasons: they simplify complex queries, provide a security layer by restricting what data users can see, and present data in a format that is easy for specific users to work with.
Creating a View
sql
CREATE VIEW HighGPA AS
SELECT StudentID, Name, GPA
FROM   Student
WHERE  GPA >= 3.5;
Once created, this view can be queried exactly like a regular table:
sql
SELECT * FROM HighGPA;
SELECT Name FROM HighGPA WHERE GPA = 4.0;
Views for Simplifying Complex Queries
Instead of writing a long join every time, a view can encapsulate it:
sql
CREATE VIEW StudentCourses AS
SELECT S.Name, C.CourseName, C.Credits
FROM   Student S
JOIN   Enrollment E ON S.StudentID = E.StudentID
JOIN   Course C ON E.CourseID = C.CourseID;
Users can then simply query StudentCourses without needing to understand the underlying join logic.
Views for Security
A view can expose only certain columns or rows of a table, hiding sensitive information:
sql
-- Create a view that hides salary information
CREATE VIEW PublicEmployeeInfo AS
SELECT EmployeeID, Name, Department
FROM   Employee;
Users granted access to this view cannot see the Salary column even though it exists in the base table.
Updatable Views
In some cases, data can be inserted, updated, or deleted through a view. A view is generally updatable if it is based on a single table, contains no aggregate functions, has no GROUP BY or HAVING clause, and does not use DISTINCT. When these conditions are met, modifications through the view are reflected in the underlying base table.
WITH CHECK OPTION
When a view is created with WITH CHECK OPTION, any INSERT or UPDATE through the view must satisfy the view's WHERE condition. This prevents users from inserting rows that would then be invisible in the view:
sql
CREATE VIEW HighGPA AS
SELECT StudentID, Name, GPA
FROM   Student
WHERE  GPA >= 3.5
WITH CHECK OPTION;
Dropping a View
sql
DROP VIEW HighGPA;
Dropping a view does not affect the underlying base tables or their data.
Materialized Views
Unlike regular views, a materialized view actually stores the result of the query physically on disk. This dramatically improves query performance for complex, frequently-executed queries, but requires the stored data to be refreshed periodically to stay current. Materialized views are particularly valuable in data warehousing and reporting systems.

4.3 Transactions
A transaction is a logical unit of work consisting of one or more SQL statements that must all succeed or all fail together. Transactions were introduced briefly in Unit I through the ACID properties. In this unit, the focus is on how transactions are controlled using SQL commands.
Transaction Control Commands
COMMIT — permanently saves all changes made during the current transaction:
sql
COMMIT;
ROLLBACK — undoes all changes made since the last COMMIT, restoring the database to its previous state:
sql
ROLLBACK;
SAVEPOINT — creates a named checkpoint within a transaction, allowing partial rollbacks:
sql
SAVEPOINT after_insert;
-- If something goes wrong later:
ROLLBACK TO after_insert;
A Transaction Example
sql
-- Transfer Nu. 5000 from Account A to Account B
BEGIN TRANSACTION;
UPDATE Account SET Balance = Balance - 5000 WHERE AccountID = 'A';
UPDATE Account SET Balance = Balance + 5000 WHERE AccountID = 'B';
-- If both updates succeeded:
COMMIT;
-- If any error occurred:
-- ROLLBACK;
Without transaction control, if the system crashed between the two UPDATE statements, Account A would lose Nu. 5000 without Account B receiving it — a serious data integrity failure. The transaction ensures this cannot happen.
Transaction Isolation Levels
When multiple transactions execute concurrently, they can interfere with each other in several ways. SQL defines four isolation levels that control how much one transaction is affected by others:
	• READ UNCOMMITTED — a transaction can read data that another transaction has modified but not yet committed (dirty reads possible)
	• READ COMMITTED — a transaction can only read committed data (default in most systems)
	• REPEATABLE READ — guarantees that if a row is read twice within the same transaction, it will have the same value both times
	• SERIALIZABLE — the highest isolation level; transactions execute as if they were running one after another, preventing all concurrency anomalies
sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;

4.4 Integrity Constraints
Integrity constraints are rules enforced by the database system to ensure data remains accurate, consistent, and meaningful. They were introduced in Unit III as part of DDL; this section explores them in greater depth.
Constraints on a Single Relation
These constraints apply to individual tables:
sql
CREATE TABLE Employee (
    EmployeeID  INT         PRIMARY KEY,
    Name        VARCHAR(50) NOT NULL,
    Salary      DECIMAL(10,2) CHECK (Salary > 0),
    Email       VARCHAR(100) UNIQUE,
    DeptID      INT         DEFAULT 1
);
Referential Integrity (Foreign Key Constraints)
Referential integrity ensures that a value in one table corresponds to a valid value in another table. The foreign key constraint enforces this:
sql
CREATE TABLE Enrollment (
    StudentID   INT,
    CourseID    INT,
    PRIMARY KEY (StudentID, CourseID),
    FOREIGN KEY (StudentID) REFERENCES Student(StudentID),
    FOREIGN KEY (CourseID)  REFERENCES Course(CourseID)
);
When a foreign key constraint is defined, the database prevents:
	• Inserting a row with a foreign key value that does not exist in the referenced table
	• Deleting a row from the referenced table if dependent rows exist elsewhere
Referential Integrity Actions
SQL allows you to specify what happens when a referenced row is deleted or updated:
sql
FOREIGN KEY (StudentID) REFERENCES Student(StudentID)
    ON DELETE CASCADE
    ON UPDATE CASCADE
	• CASCADE — automatically deletes or updates dependent rows
	• SET NULL — sets the foreign key to NULL when the referenced row is deleted
	• SET DEFAULT — sets the foreign key to its default value
	• RESTRICT or NO ACTION — prevents the deletion or update if dependent rows exist
Assertions
An assertion is a database-wide constraint that is not limited to a single table. It is checked whenever any relevant data changes:
sql
CREATE ASSERTION SalaryCheck
CHECK (NOT EXISTS (
    SELECT * FROM Employee E, Department D
    WHERE  E.DeptID = D.DeptID
    AND    E.Salary > D.MaxAllowedSalary
));
Assertions are powerful but computationally expensive, so many database systems have limited support for them.

4.5 SQL Data Types and Schemas
Built-in Data Types
Beyond the basic types introduced in Unit III, SQL supports several specialized data types:
	• DATE — stores a date: '2025-06-15'
	• TIME — stores a time: '14:30:00'
	• TIMESTAMP — stores both date and time: '2025-06-15 14:30:00'
	• INTERVAL — represents a duration: INTERVAL '3' MONTH
	• BLOB (Binary Large Object) — stores large binary data like images or files
	• CLOB (Character Large Object) — stores large text data like documents
	• BOOLEAN — stores TRUE or FALSE
User-Defined Types
SQL allows the creation of custom data types to improve semantic clarity and consistency:
sql
CREATE TYPE CurrencyAmount AS NUMERIC(12,2) FINAL;
Once defined, this type can be used in table definitions:
sql
CREATE TABLE Budget (
    DeptID      INT,
    Amount      CurrencyAmount
);
Domains
A domain is similar to a user-defined type but allows constraint specification:
sql
CREATE DOMAIN GradeType AS CHAR(2)
CHECK (VALUE IN ('A', 'B', 'C', 'D', 'F'));
Schemas and Catalogs
In large database systems, objects are organized into hierarchies. A schema is a named collection of database objects (tables, views, etc.) that belong to a particular user or application:
sql
CREATE SCHEMA UniversityDB;
A catalog is a higher-level container that holds multiple schemas. This hierarchy — catalog → schema → table — helps manage large, complex database environments with many users and applications.

4.6 Index Definition in SQL
An index is a data structure that the database creates and maintains to speed up data retrieval. Without an index, the database must scan every row in a table to find matching records — an operation that becomes extremely slow as tables grow larger. An index on a commonly-searched column allows the database to locate records almost instantly, similar to how an index in a textbook helps you jump directly to a topic.
Creating an Index
sql
-- Create an index on the Name column of the Student table
CREATE INDEX idx_student_name ON Student(Name);
-- Create a composite index on two columns
CREATE INDEX idx_enrollment ON Enrollment(StudentID, CourseID);
Unique Index
A unique index both speeds up queries and enforces uniqueness:
sql
CREATE UNIQUE INDEX idx_email ON Student(Email);
Dropping an Index
sql
DROP INDEX idx_student_name;
When to Use Indexes
Indexes improve performance on columns that are frequently used in WHERE clauses, JOIN conditions, or ORDER BY clauses. However, indexes are not free — they consume storage space and slow down INSERT, UPDATE, and DELETE operations because the index must be updated along with the data. The database designer must balance read performance against write performance when deciding where to place indexes.

4.7 Authorization
Authorization controls who can do what within a database. In a real system with many users — students, teachers, administrators, accountants — different people should have access to different data and different operations. SQL's authorization system allows administrators to grant specific privileges to specific users.
Privileges
SQL defines several types of privileges that can be granted on database objects:
	• SELECT — permission to read data
	• INSERT — permission to add new rows
	• UPDATE — permission to modify existing rows
	• DELETE — permission to remove rows
	• REFERENCES — permission to create foreign keys referencing a table
	• ALL PRIVILEGES — grants all of the above
GRANT Statement
sql
-- Allow user 'karma' to read the Student table
GRANT SELECT ON Student TO karma;
-- Allow user 'sonam' to insert and update the Course table
GRANT INSERT, UPDATE ON Course TO sonam;
-- Grant all privileges to a user
GRANT ALL PRIVILEGES ON Employee TO admin_user;
WITH GRANT OPTION
The WITH GRANT OPTION clause allows the recipient to further grant the same privilege to other users:
sql
GRANT SELECT ON Student TO karma WITH GRANT OPTION;
Now karma can grant SELECT on Student to other users.
REVOKE Statement
Privileges can be taken back using REVOKE:
sql
-- Remove karma's ability to read the Student table
REVOKE SELECT ON Student FROM karma;
-- Remove all privileges from sonam
REVOKE ALL PRIVILEGES ON Course FROM sonam;
Roles
Managing privileges for individual users becomes impractical in large organizations. Roles solve this by grouping privileges together under a label that can then be assigned to users:
sql
-- Create a role
CREATE ROLE instructor;
-- Grant privileges to the role
GRANT SELECT, INSERT ON Grade TO instructor;
-- Assign the role to users
GRANT instructor TO tsheten;
GRANT instructor TO pema;
Now both tsheten and pema have all the privileges associated with the instructor role. When the role's privileges change, all users with that role are automatically affected.

4.8 Accessing SQL from a Programming Language
Real-world applications rarely interact with databases through a SQL command line. Instead, they use programming languages like Python, Java, or PHP to send SQL queries to the database dynamically and process the results. There are two primary approaches to this.
Embedded SQL
Embedded SQL allows SQL statements to be written directly inside a host programming language. The code is pre-processed before compilation, with SQL statements replaced by function calls. A cursor is used to iterate through query results row by row:
c
EXEC SQL BEGIN DECLARE SECTION;
    int studentID;
    char name[50];
EXEC SQL END DECLARE SECTION;
EXEC SQL SELECT Name INTO :name FROM Student WHERE StudentID = :studentID;
Dynamic SQL and APIs (JDBC/ODBC)
More commonly, applications use an API (Application Programming Interface) to send SQL queries at runtime. The two major standards are:
ODBC (Open Database Connectivity) — a standard API used primarily by C, C++, and Windows applications. It allows programs to connect to any ODBC-compatible database.
JDBC (Java Database Connectivity) — the Java equivalent of ODBC. It provides a standard way for Java applications to connect to any relational database:
java
// Java example using JDBC
Connection conn = DriverManager.getConnection(
    "jdbc:mysql://localhost/universitydb", "user", "password");
String query = "SELECT Name, GPA FROM Student WHERE GPA > ?";
PreparedStatement stmt = conn.prepareStatement(query);
stmt.setDouble(1, 3.5);
ResultSet rs = stmt.executeQuery();
while (rs.next()) {
    System.out.println(rs.getString("Name") + ": " + rs.getDouble("GPA"));
}
Prepared statements (as shown above) are preferred over directly embedding values in query strings, because they protect against SQL injection attacks — a serious security vulnerability where malicious input can manipulate the query logic.
ORM (Object-Relational Mapping)
Modern applications often use ORM frameworks like SQLAlchemy (Python) or Hibernate (Java) that automatically translate between object-oriented code and SQL, reducing the need to write raw SQL for routine operations.

4.9 Functions and Stored Procedures
Functions and stored procedures allow SQL logic to be packaged, named, and reused. They are stored in the database itself and can be called whenever needed, making applications more efficient and easier to maintain.
Functions
A function performs a calculation and returns a value. It can be used inside SQL queries:
sql
-- Create a function to calculate letter grade from GPA
CREATE FUNCTION GetLetterGrade(gpa DECIMAL(3,2))
RETURNS VARCHAR(2)
DETERMINISTIC
BEGIN
    IF gpa >= 3.7 THEN RETURN 'A';
    ELSEIF gpa >= 3.0 THEN RETURN 'B';
    ELSEIF gpa >= 2.0 THEN RETURN 'C';
    ELSE RETURN 'F';
    END IF;
END;
Using the function in a query:
sql
SELECT Name, GPA, GetLetterGrade(GPA) AS Grade FROM Student;
Stored Procedures
A stored procedure is a more general block of SQL code that can perform multiple operations, accept input parameters, return output parameters, and execute complex logic. Unlike functions, procedures do not have to return a value:
sql
CREATE PROCEDURE EnrollStudent(
    IN p_StudentID INT,
    IN p_CourseID  INT,
    OUT p_Message  VARCHAR(100)
)
BEGIN
    -- Check if student exists
    IF NOT EXISTS (SELECT 1 FROM Student WHERE StudentID = p_StudentID) THEN
        SET p_Message = 'Student not found';
    -- Check if course exists
    ELSEIF NOT EXISTS (SELECT 1 FROM Course WHERE CourseID = p_CourseID) THEN
        SET p_Message = 'Course not found';
    ELSE
        INSERT INTO Enrollment(StudentID, CourseID) VALUES (p_StudentID, p_CourseID);
        SET p_Message = 'Enrollment successful';
    END IF;
END;
Calling the procedure:
sql
CALL EnrollStudent(101, 205, @result);
SELECT @result;
Advantages of Stored Procedures
	• Performance — compiled and cached by the database; faster than sending individual SQL statements
	• Reusability — logic is written once and reused across multiple applications
	• Security — users can be given access to execute a procedure without having direct access to the underlying tables
	• Reduced network traffic — a single procedure call replaces multiple round trips between the application and database

4.10 Triggers
A trigger is a special type of stored procedure that executes automatically in response to a specific event on a table — an INSERT, UPDATE, or DELETE. Triggers are used to enforce business rules, maintain audit logs, and synchronize data automatically without requiring the application to handle these tasks explicitly.
Trigger Timing
	• BEFORE trigger — executes before the triggering event occurs; can be used to validate or modify data before it is saved
	• AFTER trigger — executes after the triggering event; used for logging, notifications, or updating related tables
Creating a Trigger
sql
-- Automatically log salary changes in an audit table
CREATE TRIGGER LogSalaryChange
AFTER UPDATE ON Employee
FOR EACH ROW
BEGIN
    IF OLD.Salary <> NEW.Salary THEN
        INSERT INTO SalaryAudit (
            EmployeeID, OldSalary, NewSalary, ChangeDate
        )
        VALUES (
            NEW.EmployeeID, OLD.Salary, NEW.Salary, NOW()
        );
    END IF;
END;
In trigger logic, OLD refers to the row's values before the change, and NEW refers to the values after the change.
Another Example — Enforcing a Business Rule
sql
-- Prevent deletion of a student who has active enrollments
CREATE TRIGGER PreventStudentDelete
BEFORE DELETE ON Student
FOR EACH ROW
BEGIN
    IF EXISTS (SELECT 1 FROM Enrollment WHERE StudentID = OLD.StudentID) THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Cannot delete student with active enrollments';
    END IF;
END;
Dropping a Trigger
sql
DROP TRIGGER LogSalaryChange;
When to Use Triggers
Triggers are powerful but should be used carefully. Overuse of triggers can make a database hard to debug and maintain, because the automatic execution happens "behind the scenes" and may not be obvious to developers. Best practices suggest using triggers only for database-level concerns (auditing, referential integrity enforcement) and handling application logic in the application layer.

4.11 Recursive Queries
Some data is naturally hierarchical or recursive — organizational charts, category trees, bill-of-materials structures. Querying such data in standard SQL is difficult because the depth of the hierarchy is unknown. SQL provides recursive queries using the WITH RECURSIVE clause (also called Common Table Expressions or CTEs) to handle this elegantly.
Common Table Expression (CTE)
A CTE is a named temporary result set defined at the beginning of a query:
sql
WITH DeptCTE AS (
    SELECT DeptID, DeptName FROM Department WHERE ParentDeptID IS NULL
)
SELECT * FROM DeptCTE;
Recursive CTE
A recursive CTE references itself. It consists of two parts: the base case (the starting point) and the recursive case (the step that extends the result):
sql
-- Find all employees in a management hierarchy starting from a given manager
WITH RECURSIVE EmployeeHierarchy AS (
    -- Base case: start with the top-level manager
    SELECT EmployeeID, Name, SupervisorID, 1 AS Level
    FROM   Employee
    WHERE  SupervisorID IS NULL
    UNION ALL
    -- Recursive case: find employees supervised by those already found
    SELECT E.EmployeeID, E.Name, E.SupervisorID, EH.Level + 1
    FROM   Employee E
    JOIN   EmployeeHierarchy EH ON E.SupervisorID = EH.EmployeeID
)
SELECT * FROM EmployeeHierarchy ORDER BY Level;
This query starts with the top manager, then finds everyone they supervise, then finds everyone those people supervise, and so on — traversing the entire hierarchy regardless of how deep it goes.

4.12 Advanced Aggregation Features
Beyond the basic aggregate functions covered in Unit III, SQL provides advanced aggregation features that are essential for analytical and reporting queries.
ROLLUP
ROLLUP generates subtotals and grand totals automatically. It creates multiple levels of aggregation within a single query:
sql
SELECT Department, Major, COUNT(*) AS StudentCount
FROM   Student
GROUP BY ROLLUP(Department, Major);
This produces counts grouped by (Department, Major), then subtotals by Department alone, then a grand total for all students.
CUBE
CUBE is similar to ROLLUP but generates all possible combinations of groupings:
sql
SELECT Department, Major, Gender, COUNT(*) AS StudentCount
FROM   Student
GROUP BY CUBE(Department, Major, Gender);
This generates subtotals for every possible combination: by Department alone, by Major alone, by Gender alone, by Department and Major, by Department and Gender, and so on — useful for multidimensional analysis.
GROUPING SETS
GROUPING SETS allows you to specify exactly which groupings you want, without computing all combinations:
sql
SELECT Department, Major, COUNT(*) AS StudentCount
FROM   Student
GROUP BY GROUPING SETS (
    (Department, Major),
    (Department),
    ()
);
The empty set () produces the overall grand total.
Window Functions (OVER Clause)
Window functions perform calculations across a set of rows related to the current row, without collapsing rows the way GROUP BY does. This makes them ideal for ranking, running totals, and moving averages:
sql
-- Rank students by GPA within each major
SELECT Name, Major, GPA,
       RANK() OVER (PARTITION BY Major ORDER BY GPA DESC) AS RankInMajor
FROM   Student;
Common window functions:
	• RANK() — assigns ranks; ties get the same rank, with gaps after tied ranks
	• DENSE_RANK() — like RANK but no gaps after ties
	• ROW_NUMBER() — assigns a unique sequential number to each row
	• SUM() OVER() — running total
	• AVG() OVER() — moving average
sql
-- Calculate running total of salary by hire date
SELECT Name, Salary, HireDate,
       SUM(Salary) OVER (ORDER BY HireDate) AS RunningTotal
FROM   Employee;
GROUPING Function
When using ROLLUP or CUBE, it can be unclear whether a NULL in the result represents an actual NULL value in the data or a subtotal row generated by the aggregation. The GROUPING() function returns 1 for subtotal rows and 0 for regular data rows:
sql
SELECT Department,
       GROUPING(Department) AS IsDeptSubtotal,
       COUNT(*) AS StudentCount
FROM   Student
GROUP BY ROLLUP(Department);


Unit 5: Relational Database Design
5.1 Features of Good Relational Designs
A good relational design stores each fact in exactly one place. The core idea is that a relation should model one coherent concept — student data in one table, department data in another.
The four main goals of good design:
Minimal Redundancy — the same fact should not be repeated across multiple rows. If 100 students belong to the same department, the department office should not appear 100 times.
No Update Anomaly — if a fact is stored in many rows, updating it requires changing every single row. Missing even one creates inconsistency.
No Insertion Anomaly — you should be able to add a new fact without being forced to add an unrelated one. For example, you should be able to add a new department even if no students have joined it yet.
No Deletion Anomaly — deleting one fact should not accidentally destroy another. If the last student in a department is removed, the department's details should not disappear with them.
Two structural properties every good decomposition must have:
Lossless-Join Decomposition — when a relation is split into smaller relations, joining them back must always reproduce the exact original data. No extra (spurious) rows should appear.
Dependency Preservation — the integrity rules (functional dependencies) of the original relation should still be enforceable on the decomposed relations individually, without needing expensive joins.

5.2 Decomposition Using Functional Dependencies
Decomposition means splitting a relation into two or more smaller relations. Functional dependencies guide whether the split is safe and whether it improves the design.
Why decompose? When a relation contains redundancy or violates a normal form like BCNF or 3NF, decomposition removes the problem.
Lossless-join condition: A decomposition of R into R1 and R2 is lossless if the common attributes (R1 ∩ R2) functionally determine all attributes of R1, or all attributes of R2. If neither holds, joining produces spurious tuples.
Example:
R(A, B, C) with FD A → B.
Decompose into R1(A, B) and R2(A, C).
The common attribute A determines all of R1, so this decomposition is lossless. The dependency A → B is also preserved inside R1.

5.3 Normal Forms
Normal forms are formal standards for organising relations to reduce redundancy and prevent anomalies. The process of converting a schema to meet these standards is called normalisation.
Normalisation is based on keys, functional dependencies, multivalued dependencies, and join dependencies.

1NF — First Normal Form
A relation is in 1NF if every attribute contains only atomic (indivisible) values. Each cell must hold exactly one value.
Violations:
	• A PhoneNumbers column containing "17111111, 17222222" in one cell.
	• Repeating columns like Phone1, Phone2, Phone3.
Fix: Create one row per value. One row for 17111111, another row for 17222222, both linked to the same StudentID.
1NF is the foundation of the relational model. Without it, operations like SELECT, JOIN, and PROJECT become unreliable.

2NF — Second Normal Form
A relation is in 2NF if it is in 1NF and every non-prime attribute depends on the whole candidate key, not just part of it.
Key terms:
	• Prime attribute — an attribute that is part of a candidate key.
	• Non-prime attribute — an attribute that is not part of any candidate key.
	• Partial dependency — a non-prime attribute depends on only part of a composite key.
Example of a violation:
ENROLLMENT(StudentID, CourseID, StudentName, CourseName, Grade)
	• Composite key: (StudentID, CourseID)
	• StudentName depends only on StudentID — partial dependency.
	• CourseName depends only on CourseID — partial dependency.
Fix — decompose into:
	• STUDENT(StudentID, StudentName)
	• COURSE(CourseID, CourseName)
	• ENROLLMENT(StudentID, CourseID, Grade)
Note: if a relation has a single-attribute key, partial dependency cannot occur, so it is automatically in 2NF once it is in 1NF.

3NF — Third Normal Form
A relation is in 3NF if it is in 2NF and there is no transitive dependency of a non-prime attribute on a candidate key.
Formal definition: for every non-trivial FD X → A, either X is a superkey, or A is a prime attribute.
Transitive dependency: when Key → Non-key A, and Non-key A → Non-key B. So the key determines B only indirectly, through A.
Example of a violation:
EMPLOYEE(EmpID, EmpName, DeptID, DeptName)
	• EmpID → DeptID
	• DeptID → DeptName
	• Therefore EmpID → DeptName transitively.
DeptName is transitively dependent on EmpID, so this is not in 3NF.
Fix — decompose into:
	• EMPLOYEE(EmpID, EmpName, DeptID)
	• DEPARTMENT(DeptID, DeptName)
3NF is the most widely used normal form in practice. It removes the most common redundancy while usually still preserving all dependencies — a practical balance between theory and real-world implementation.

BCNF — Boyce-Codd Normal Form
BCNF is a stronger version of 3NF. A relation is in BCNF if for every non-trivial FD X → Y, X must be a superkey. No exceptions.
The difference from 3NF: in 3NF, A being a prime attribute is an allowed exception. BCNF removes that exception entirely.
BCNF gives stronger redundancy control but may not always preserve all functional dependencies.

Summary Table
Normal Form	Main Condition	Problem It Removes
1NF	Atomic values only	Repeating groups, multi-valued cells
2NF	No partial dependency	Redundancy from composite key part-dependence
3NF	No transitive dependency	Redundancy among non-key attributes
BCNF	Every determinant is a superkey	Stronger redundancy control than 3NF
4NF	No multivalued dependencies	Redundancy from independent multi-valued facts
5NF	No non-trivial join dependencies	Redundancy from join dependencies

5.4 Functional-Dependency Theory
A functional dependency (FD) X → Y means: whenever two rows agree on X, they must also agree on Y. X determines Y.
Key terms:
	• Determinant — the left-hand side of an FD (X).
	• Dependent — the right-hand side (Y).
	• Trivial FD — Y ⊆ X, e.g. AB → A. Always holds and carries no new information.
	• Non-trivial FD — Y is not a subset of X. Carries real meaning.
	• Superkey — a set of attributes that uniquely identifies every tuple in a relation.
	• Candidate key — a minimal superkey. No proper subset of it is also a superkey.
Armstrong's Axioms — sound and complete rules for deriving all implied FDs:
	• Reflexivity: if Y ⊆ X, then X → Y.
	• Augmentation: if X → Y, then XZ → YZ for any Z.
	• Transitivity: if X → Y and Y → Z, then X → Z.
Derived rules:
	• Union: if X → Y and X → Z, then X → YZ.
	• Decomposition: if X → YZ, then X → Y and X → Z.
	• Pseudotransitivity: if X → Y and WY → Z, then WX → Z.
Attribute Closure (X⁺): the set of all attributes that X can determine under a given set of FDs. Used to test if X is a superkey, to check whether a given FD is implied, and to find candidate keys.
Canonical Cover: a minimal, equivalent set of FDs where no attribute is extraneous and no dependency is redundant. Each left-hand side appears at most once.

5.5 Algorithms for Decomposition Using Functional Dependencies
BCNF Decomposition Algorithm:
	1. Find an FD X → Y in R where X is not a superkey.
	2. Decompose R into R1 = X ∪ Y and R2 = R − (Y − X).
	3. Repeat on each resulting relation until all are in BCNF.
BCNF decomposition is always lossless but may not preserve all dependencies.
3NF Synthesis Algorithm:
	1. Compute a canonical cover Fc of the FDs.
	2. For each FD X → Y in Fc, create a relation schema containing X ∪ Y.
	3. If no created relation contains a candidate key of the original relation, add one that does.
	4. Remove any redundant relation that is fully contained in another.
3NF synthesis guarantees lossless join AND dependency preservation — which is why it is often preferred over BCNF in practice.
Property	BCNF	3NF
Lossless join	Always	Always
Dependency preservation	Not always	Always
Redundancy control	Stronger	Slightly weaker

5.6 Decomposition Using Multivalued Dependencies
A multivalued dependency (MVD) X →→ Y means: for a given value of X, the set of Y values is completely independent of all other attributes in the relation.
MVDs appear when a table stores two independent multi-valued facts about the same key.
Example: A Student can have multiple Hobbies and multiple Languages, independent of each other. Storing (Student, Hobby, Language) in one table creates redundant combinations of every hobby paired with every language. This signals: Student →→ Hobby and Student →→ Language.
4NF addresses this: a relation is in 4NF if for every non-trivial MVD X →→ Y, X is a superkey. The fix is to decompose into two separate tables — one for (Student, Hobby) and one for (Student, Language).
Note: every FD X → Y is also an MVD X →→ Y, so BCNF is a prerequisite for 4NF.

5.7 More Normal Forms
5NF / Project-Join Normal Form (PJNF): a relation is in 5NF if every non-trivial join dependency is implied by the candidate keys. Addresses rare cases where a relation can be split into three or more projections losslessly, but the redundancy cannot be explained by FDs or MVDs alone.
Domain-Key Normal Form (DKNF): every constraint is a logical consequence of only domain constraints and key constraints. The theoretical ideal — almost never fully achieved in practice.
In most database design work, 1NF through BCNF are essential. 4NF is important when multi-valued attributes are present. 5NF and DKNF are advanced topics.

5.8 Atomic Domains and First Normal Form
An atomic domain means the database treats each value as indivisible. Whether something is atomic is context-dependent — a full address might be atomic in one system but split into street, city, and postcode in another where each part needs to be queried separately.
1NF supports clear tuple structure, standard relational operations, easier querying, and is the starting point for all higher normal forms.
Common violations to watch for: comma-separated values in one cell, or repeating columns like Phone1, Phone2, Phone3.

5.9 Database Design Process
Database design follows a sequence of stages, though the process is iterative — designers frequently revisit earlier stages as their understanding deepens.
Stage 1 — Requirements Analysis: understand the users, their transactions, reports needed, business rules, constraints, and what data items must be stored.
Stage 2 — Conceptual Design: create a high-level model, usually an Entity-Relationship (ER) diagram, representing entities, relationships, and constraints.
Stage 3 — Logical Design: convert the ER model into a relational schema with attributes, primary keys, foreign keys, and integrity constraints.
Stage 4 — Schema Refinement / Normalisation: apply FDs, MVDs, and normal forms to remove redundancy and anomalies.
Stage 5 — Physical Design: choose indexes, storage structures, and file organisation based on performance requirements.
Stage 6 — Security and Application Design: define access control, views, transactions, and application-level logic.

5.10 Modelling Temporal Data
Temporal data represents facts associated with time. Standard relational design stores only the current state. Many real applications need to record history too.
Valid time — the period during which a fact is true in the real world. Example: the dates during which a person lived at a particular address.
Transaction time — the period during which a fact was stored in the database. Useful for auditing and recovering database history.
Bitemporal data — when both valid time and transaction time are recorded together.
Snapshot — the state of the data at one specific point in time.
Practical approach: add StartDate and EndDate columns to a relation. For currently valid records, EndDate is set to a far future date or left open-ended.
Example: EmployeeSalary(EmployeeID, Salary, StartDate, EndDate)
Why temporal modelling matters: it enables tracking historical changes, auditing, legal and policy compliance, and answering questions like "What was this employee's salary in March 2023?" rather than only "What is it now?"
Design caution: temporal data complicates keys, constraints, and queries because rows represent facts over intervals. Designers must carefully decide whether time belongs on attributes, entities, relationships, or separate history tables.


Unit 6: Indexing and Query Processing
6.1 Basic Concepts of Indexing
Indexing is a technique used to speed up data retrieval in a database. Instead of scanning every row in a table to find what you need, an index maintains a separate structure that stores key values alongside pointers to the actual records — allowing the database to jump directly to the right location.
Think of it like the index at the back of a textbook. Instead of reading every page to find a topic, you look it up in the index and go straight to the right page.
Why indexing is needed:
	• Reduces search time significantly.
	• Minimises the number of disk I/O operations, which are the most expensive part of query execution.
	• Improves the performance of SELECT queries, especially on large tables.
How it works — simple example:
If you search for a student named Sonam in a table with 10,000 rows:
	• Without an index: the database checks every single row from top to bottom.
	• With an index on Name: the database looks up "Sonam" in the index and goes directly to that row.
An index stores two things: the search key value, and a pointer to where the actual record lives on disk.

6.2 Indexing Structures
Different structures are used to organise an index depending on the size of the data and the types of queries needed.
Single-Level Index — a simple, flat list of key values with pointers to records. Easy to understand but becomes inefficient as the database grows large, because the index itself becomes too big to search quickly.
Multi-Level Index — an index built on top of another index. By adding layers, the search space is reduced at each level, making lookups much faster on large datasets.
B-Tree — a self-balancing tree structure where data is sorted in order (lowest on the left, highest on the right). Key properties:
	• All leaf nodes are at the same level.
	• A node can have more than two children.
	• Height is kept as low as possible and adjusts automatically when data is inserted or deleted.
	• Both internal nodes and leaf nodes store data pointers.
	• Deletion of internal nodes is complex and requires tree restructuring.
B+ Tree — an improved version of the B-Tree and the most important indexing structure in practice. Key differences from B-Tree:
	• Internal nodes store keys only — no data pointers.
	• All data pointers are stored only at the leaf nodes.
	• Leaf nodes are linked together as a linked list, enabling efficient sequential and range access.
	• All leaves are at the same level, keeping the tree balanced.
	• Search is faster because all keys are available at the leaf level.
	• Deletion is simpler because all nodes are found at the leaf level.
B-Tree vs B+ Tree comparison:
Feature	B-Tree	B+ Tree
Data pointers	All nodes	Leaf nodes only
Search speed	Slower (keys not all at leaf)	Faster (all keys at leaf)
Range queries	Not efficient	Efficient (linked leaves)
Deletion	Complex	Simpler
Sequential access	Not possible	Possible via linked list
Height	Larger	Smaller for same data
Most major database systems — MySQL, Oracle, PostgreSQL — use B+ Tree indexing as their default index structure.

6.3 Ordered and Unordered Indices
Ordered Index — records in the index are stored in sorted order based on the key value.
	• Fast for search, especially range queries (e.g. find all students with Age between 18 and 22).
	• Slower for insertions because the sorted order must be maintained every time a new record is added.
Unordered Index (Heap File) — records are stored in no particular order, just added wherever space is available.
	• Very fast for insertions since no ordering needs to be maintained.
	• Slow for searches because the database may need to scan all records to find what it needs.
Comparison:
Feature	Ordered Index	Unordered Index
Search speed	Fast	Slow
Insert speed	Slower	Fast
Range queries	Efficient	Inefficient
The choice depends on the workload. If the system does far more reads than writes, an ordered index is better. If writes dominate, an unordered structure may be preferred.

6.4 Indexing of Temporal and Spatial Data
Standard indexing works well for simple values like names or numbers. But some data types require specialised indexing approaches.
Temporal Data Indexing
Temporal data involves facts associated with time — for example, an employee's salary history, or a patient's medical records over multiple years. A temporal index organises data by time periods or timestamps so that time-based queries can be answered efficiently.
Example: instead of scanning all salary records to find what an employee earned in 2023, a temporal index lets the database jump directly to records within that time range.
Used for: historical analysis, auditing, tracking changes over time.
Spatial Data Indexing
Spatial data represents locations or geometric shapes — for example, GPS coordinates, map regions, or distances between places. Standard B+ Trees cannot handle spatial queries like "find all restaurants within 5km of this location" efficiently.
Specialised structures used for spatial indexing:
	• R-Tree — organises data by bounding rectangles. Each region of space is represented as a rectangular boundary, allowing fast overlap and proximity queries.
	• Quad-Tree — divides space into four quadrants recursively, allowing efficient location-based lookups.
Key distinction:
	• Temporal indexing = time-based access to historical data.
	• Spatial indexing = location-based access to geographic data.

6.5 Indexing for Search
An index does not just speed up simple lookups — it supports several different types of search operations.
Exact match search — finding rows where a column equals a specific value. Example: WHERE Name = 'Sonam'. The index jumps directly to that entry.
Range query — finding rows where a column falls within a range. Example: WHERE Age > 20. An ordered index or B+ Tree handles this efficiently by following the linked leaf nodes.
Pattern matching — finding rows matching a partial value. Example: WHERE Name LIKE 'S%'. An index can help here if the pattern starts from the beginning of the value.
Performance impact:
	• Without an index: O(n) — the database must check every row.
	• With an index: O(log n) — the database navigates a tree structure to the answer.
Important warning: having too many indexes is harmful. Every index must be updated whenever a row is inserted, updated, or deleted. Over-indexing slows down write operations and wastes storage space. Indexes should be created only on columns that are frequently searched or joined.

6.6 Query Processing and Optimisation
Query processing is the full journey an SQL query takes — from the text a user writes to the actual result returned by the database. The DBMS does not simply execute SQL directly; it goes through several stages to understand, translate, optimise, and then execute the query.
The five stages of query processing:
Stage 1 — SQL Query
The user writes an SQL query. Example:
SELECT name, age FROM Student WHERE age > 20;
Stage 2 — Parsing
The DBMS checks whether the query is syntactically correct and makes sense. It verifies:
	• Is the SQL syntax valid?
	• Does the referenced table (Student) exist?
	• Do the referenced columns (name, age) exist in that table?
	• Is the condition logically valid?
If anything is wrong — for example, a missing comma between column names — parsing fails and an error is returned. The query goes no further until it is correct.
Stage 3 — Translation into Relational Algebra
Once the query passes parsing, it is converted into relational algebra — the internal mathematical language the DBMS uses to represent operations. The example query above becomes:
PROJECT(name, age) ← SELECT(age > 20) ← Student
This means: take the Student table, filter rows where age > 20, then return only the name and age columns.
Stage 4 — Optimisation
The same query can often be executed in multiple different ways. The query optimiser evaluates these options and chooses the one with the lowest estimated cost. For the example query, options might include:
	• Plan 1: scan the entire Student table row by row.
	• Plan 2: use an index on the age column to jump directly to relevant rows.
If the table is small, a full scan may be fine. If the table has millions of rows and an index exists, using the index is far cheaper. The optimiser tries to minimise disk access, CPU time, memory usage, and total execution time.
Stage 5 — Execution
The DBMS runs the chosen plan, reads the data, applies all conditions, and returns the final result to the user.

6.6.1 Measures of Query Cost
The cost of executing a query is not just about speed — it is about the resources consumed. The most important measure is disk I/O operations, because reading and writing to disk is far slower than any in-memory operation.
Main cost factors:
	• Disk I/O — the number of disk blocks read or written. This dominates query cost in most real systems.
	• CPU time — the processing required to evaluate conditions, sort data, compute joins, etc.
	• Memory usage — how much RAM the query needs to buffer data during execution.
The core principle of query optimisation is: minimise disk access and you minimise query cost.

6.6.2 Evaluation of Expressions
After a query is translated into relational algebra, the DBMS must evaluate the resulting expression — meaning it must actually carry out the operations (scan, filter, project, join) to produce the result.
Simple example: SELECT * FROM Student WHERE Age > 20
	1. Read the Student table from disk.
	2. Apply the filter: keep only rows where Age > 20.
	3. Output the matching rows.
Join example: SELECT * FROM A JOIN B ON A.id = B.id
	1. Read table A from disk.
	2. Read table B from disk.
	3. Match rows from A and B where the id values are equal.
	4. Output the combined matching rows.
For complex queries involving multiple tables, conditions, and aggregations, the order in which operations are performed matters greatly — doing a filter before a join, for example, reduces the amount of data that needs to be joined and can dramatically cut the cost.

6.6.3 Choice of Evaluation Plans
A query plan is a specific strategy for executing a query. For any given query, multiple plans may be valid. The query optimiser's job is to pick the best one.
Three common join strategies:
Nested Loop Join — for every row in table A, scan all rows in table B and check whether the join condition holds. Simple but expensive for large tables because it results in many disk reads.
Hash Join — build a hash table from one of the two tables (usually the smaller one), then probe the hash table for each row of the other table. Much faster than nested loop for large datasets when memory is available.
Sort-Merge Join — sort both tables on the join attribute, then merge them by scanning through both sorted lists simultaneously. Efficient when the data is already sorted or when both tables are large.
How the optimiser chooses:
The query optimiser estimates the cost of each available plan using statistics about the data — such as the number of rows, the distribution of values, and what indexes exist. It then selects the plan with the lowest estimated cost. This selection happens automatically; the user does not need to specify how the query should be executed.


Unit 7: Transactions, Concurrency Control and Recovery Systems
7.1 Simple Transaction Models
A transaction is a single logical unit of work that accesses or modifies data in a database. It is a sequence of operations — such as reads and writes — that must be executed as a complete, indivisible unit. Either all operations in the transaction succeed, or none of them take effect.
Why transactions matter: in real systems, many things can go wrong — power failures, system crashes, network errors, or two users trying to update the same data at the same time. Transactions provide a structured way to handle all of these situations safely.
Simple example — bank transfer:
Transferring Nu. 5,000 from Account A to Account B involves two steps:
	1. Deduct Nu. 5,000 from Account A.
	2. Add Nu. 5,000 to Account B.
If the system crashes after step 1 but before step 2, Account A has lost money but Account B has not received it. This is a disaster. A transaction ensures both steps happen together or neither happens at all.
Transaction states:
	• Active — the transaction is currently executing.
	• Partially committed — the last operation has been executed but changes are not yet saved permanently.
	• Committed — the transaction has completed successfully and all changes are permanently saved.
	• Failed — the transaction cannot proceed due to an error.
	• Aborted — the transaction has been rolled back and the database is restored to its state before the transaction began.

7.2 Transaction Atomicity, Isolation and Durability
Every transaction must satisfy four properties, known collectively as the ACID properties. These four properties are what make database transactions reliable.
Atomicity — a transaction is treated as a single indivisible unit. Either all of its operations are completed successfully, or none of them are applied. If a transaction fails halfway through, all changes made so far are undone (rolled back). In the bank transfer example, if the deduction happens but the credit fails, the deduction is reversed.
Consistency — a transaction must bring the database from one valid state to another valid state. All integrity constraints, rules, and relationships must hold before and after the transaction. For example, a transaction cannot leave an account balance as a negative value if that is against the rules.
Isolation — concurrent transactions must execute as if they were running one at a time, in isolation from each other. The intermediate state of a transaction should not be visible to other transactions. This prevents one transaction from reading or modifying data that another transaction is still working on.
Durability — once a transaction is committed, its changes are permanent. Even if the system crashes immediately after a commit, the changes survive. This is typically achieved by writing changes to a log on disk before confirming the commit.

7.3 Transaction Isolation Levels
Full isolation between all transactions is ideal but expensive. In practice, database systems offer different isolation levels that trade off strictness for performance. Lower isolation levels allow more concurrency but introduce certain types of anomalies.
The three main read anomalies:
Dirty read — a transaction reads data that has been modified by another transaction that has not yet committed. If that other transaction is later rolled back, the first transaction has read data that never officially existed.
Non-repeatable read — a transaction reads the same row twice but gets different values because another transaction modified and committed that row between the two reads.
Phantom read — a transaction re-executes a query and finds different rows because another transaction has inserted or deleted rows that match the query condition between the two executions.
The four standard isolation levels (from lowest to highest):
Read Uncommitted — the lowest level. A transaction can read data that has not yet been committed by other transactions. Dirty reads, non-repeatable reads, and phantom reads are all possible. Rarely used in practice but offers maximum concurrency.
Read Committed — a transaction can only read data that has been committed. Dirty reads are prevented. However, non-repeatable reads and phantom reads are still possible. This is the default level in many systems including PostgreSQL and Oracle.
Repeatable Read — a transaction that reads a row will always see the same value for that row throughout its duration. Non-repeatable reads are prevented. Phantom reads are still possible. This is the default in MySQL InnoDB.
Serialisable — the highest level. Transactions are fully isolated from each other, behaving as if they executed one at a time in serial order. All three anomalies are prevented. This is the safest but most expensive level in terms of performance.
Isolation Level	Dirty Read	Non-Repeatable Read	Phantom Read
Read Uncommitted	Possible	Possible	Possible
Read Committed	Prevented	Possible	Possible
Repeatable Read	Prevented	Prevented	Possible
Serialisable	Prevented	Prevented	Prevented

7.4 Serializability
When multiple transactions run concurrently, their operations are interleaved. Serializability is the standard correctness criterion for concurrent execution — it says that a concurrent schedule is correct if its outcome is equivalent to some serial execution of the same transactions (where serial means one transaction fully completes before the next one begins).
Schedule — the order in which the operations of multiple concurrent transactions are executed.
Serial schedule — transactions execute one after another with no interleaving. Always correct but allows no concurrency and is slow.
Serialisable schedule — a concurrent schedule that produces the same result as some serial schedule. This is the goal.
Conflict serializability — the most commonly used form. Two operations conflict if they belong to different transactions, access the same data item, and at least one of them is a write. A schedule is conflict serialisable if it can be transformed into a serial schedule by swapping non-conflicting operations.
Precedence graph (serialisability test):
	• Create one node for each transaction.
	• Draw a directed edge from transaction Ti to Tj if an operation in Ti conflicts with and precedes an operation in Tj.
	• If the graph has no cycle, the schedule is conflict serialisable.
	• If the graph has a cycle, the schedule is not conflict serialisable.
View serializability — a weaker but broader form of serializability. A schedule is view serialisable if it reads and writes the same data values as some serial schedule, even if the order of operations differs. Every conflict serialisable schedule is also view serialisable, but not vice versa.

7.5 Lock-Based Protocols
Locking is the most common mechanism used to enforce isolation and serializability. A lock controls access to a data item, preventing conflicting operations from different transactions from executing simultaneously.
Two types of locks:
Shared lock (S-lock) — used when a transaction wants to read a data item. Multiple transactions can hold a shared lock on the same item at the same time. No transaction can write to an item while others hold a shared lock on it.
Exclusive lock (X-lock) — used when a transaction wants to write a data item. Only one transaction can hold an exclusive lock on an item at a time, and no other transaction can read or write that item while the exclusive lock is held.
Lock compatibility:
	S-lock held	X-lock held
Request S-lock	Compatible	Not compatible
Request X-lock	Not compatible	Not compatible
Two-Phase Locking (2PL) — the most widely used locking protocol for ensuring conflict serializability. It has two phases:
	• Growing phase: a transaction may acquire locks but may not release any.
	• Shrinking phase: a transaction may release locks but may not acquire any new ones.
The point at which a transaction releases its first lock is called the lock point. 2PL guarantees conflict serialisable schedules.
Strict Two-Phase Locking — a variation where all exclusive locks are held until the transaction commits or aborts. This prevents cascading rollbacks and is the most commonly implemented form in practice.

7.6 Deadlock Handling
A deadlock occurs when two or more transactions are each waiting for a lock held by another, creating a cycle of waiting from which none can proceed.
Classic example:
	• Transaction T1 holds a lock on item A and is waiting for a lock on item B.
	• Transaction T2 holds a lock on item B and is waiting for a lock on item A.
	• Neither can proceed. Both are stuck forever.
Two main approaches to handling deadlocks:
Deadlock Prevention — design the system so deadlocks cannot occur in the first place.
	• Wait-Die scheme: if a transaction Ti requests a lock held by Tj, Ti is allowed to wait only if Ti is older than Tj. If Ti is younger, it is killed (rolled back) and will restart later.
	• Wound-Wait scheme: if Ti is older than Tj, Ti forces Tj to roll back (wounds it). If Ti is younger, Ti waits.
	• Both schemes prevent cycles but may cause unnecessary rollbacks.
Deadlock Detection and Recovery — allow deadlocks to occur but detect and resolve them.
	• The system maintains a wait-for graph where nodes are transactions and edges represent waiting relationships.
	• If a cycle is detected in the wait-for graph, a deadlock exists.
	• The system selects a victim transaction — usually the one that has done the least work or holds the fewest locks — and aborts it to break the cycle.
	• The victim transaction is rolled back and can be restarted.
Starvation — a related problem where the same transaction is repeatedly chosen as the victim and never completes. Prevented by ensuring a transaction can only be rolled back a limited number of times before it is guaranteed to proceed.

7.7 Concurrency Control Mechanisms
Beyond locking, several other mechanisms are used to manage concurrent access to data.
Timestamp-Based Protocols — each transaction is assigned a unique timestamp when it begins. The system uses these timestamps to determine the correct order of execution, without using locks.
	• Each data item stores the timestamp of the last transaction that successfully read it (read timestamp) and wrote it (write timestamp).
	• If a transaction tries to read or write data in a way that would violate timestamp order, it is rolled back and restarted with a new timestamp.
	• This approach is deadlock-free but may cause more rollbacks.
Optimistic Concurrency Control — assumes conflicts between transactions are rare. Transactions execute freely without acquiring locks, then at the end go through a validation phase to check if any conflicts occurred.
	• If no conflicts: commit.
	• If conflict detected: roll back and restart.
	• Works well for read-heavy workloads where conflicts are uncommon. Performs poorly when conflicts are frequent.
Multiversion Concurrency Control (MVCC) — instead of locking data, the system maintains multiple versions of each data item. When a transaction reads data, it sees the version that was current when the transaction began, regardless of changes made by other concurrent transactions.
	• Writers create new versions rather than overwriting old ones.
	• Readers never block writers and writers never block readers.
	• Used in PostgreSQL, Oracle, and MySQL InnoDB.
	• Read performance is excellent but storage overhead increases because old versions must eventually be cleaned up (vacuumed).

7.8 Recovery Algorithms
Recovery systems ensure that the database can be restored to a consistent state after a failure — whether that failure is a transaction abort, a system crash, or a disk error.
Types of failures:
	• Transaction failure — a logical error (e.g. division by zero) or a system error forces a transaction to abort.
	• System crash — a power failure or software bug causes the database to crash. Data in memory is lost but disk data survives.
	• Disk failure — physical damage to storage. Requires backup restoration.
The Write-Ahead Log (WAL) — the foundation of most recovery systems. Before any change is written to the actual database, a record of the change is written to a log on disk. This ensures that if a crash occurs, the log can be used to redo or undo operations.
Every log record captures: the transaction ID, the data item affected, the old value (before the change), and the new value (after the change).
UNDO and REDO operations:
	• UNDO — reverses the changes made by an uncommitted transaction. Used when a transaction failed or was aborted. Restores the old value.
	• REDO — re-applies the changes made by a committed transaction that may not have been written to disk before the crash. Restores the new value.
Checkpoints — periodically, the system writes a checkpoint to the log, recording which transactions are currently active and ensuring all committed changes have been flushed to disk. During recovery after a crash, the system only needs to process log records from the most recent checkpoint onwards, rather than the entire log. This significantly reduces recovery time.
ARIES Recovery Algorithm — the most widely used recovery algorithm in modern database systems. It operates in three phases:
	1. Analysis phase — scan the log from the last checkpoint to determine which transactions were active at the time of the crash and which pages were dirty (modified but not written to disk).
	2. Redo phase — replay all logged operations from the point identified in analysis, restoring the database to the exact state it was in just before the crash.
	3. Undo phase — roll back all transactions that were active at the time of the crash and had not yet committed, using the UNDO log records.

7.10 Remote Backup Systems
A remote backup system maintains a copy of the database at a geographically separate location. If the primary site suffers a catastrophic failure — fire, flood, or major hardware failure — the remote backup allows operations to be restored or continued from the secondary site.
How it works:
The primary site continuously or periodically transmits log records to the remote backup site. The backup site applies these logs to its own copy of the database, keeping it as up to date as possible.
Two key design decisions:
Hot standby vs cold standby:
	• Hot standby — the backup site is continuously updated and can take over almost immediately when the primary fails. High cost but minimal downtime.
	• Cold standby — the backup site is updated less frequently (e.g. nightly). Cheaper but recovery takes longer and some recent data may be lost.
Synchronous vs asynchronous replication:
	• Synchronous replication — a transaction is only committed at the primary site after the remote backup confirms it has also received and applied the log record. Guarantees no data loss but adds latency to every commit.
	• Asynchronous replication — the primary site commits without waiting for confirmation from the backup. Lower latency but a small window of data loss is possible if the primary fails before the backup receives the latest logs.
Recovery time objective (RTO) — how quickly the system must be restored after a failure.
Recovery point objective (RPO) — how much data loss is acceptable, measured in time. A hot standby with synchronous replication gives an RPO of near zero.

