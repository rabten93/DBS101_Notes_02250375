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
