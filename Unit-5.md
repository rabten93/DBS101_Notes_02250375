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
