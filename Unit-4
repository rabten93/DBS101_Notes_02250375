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
