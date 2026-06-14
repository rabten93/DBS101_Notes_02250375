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
