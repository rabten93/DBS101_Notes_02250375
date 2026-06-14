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
