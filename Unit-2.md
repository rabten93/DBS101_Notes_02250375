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
