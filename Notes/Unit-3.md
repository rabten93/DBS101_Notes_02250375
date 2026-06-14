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
