1. Distributed vs Centralized Database Architecture

Definition:
A centralized database stores all data on a single server, while a distributed database spreads data across multiple servers in different locations.

Example:
A centralized system is like one big library in a single city. A distributed system is like having smaller libraries in multiple cities for faster, more reliable access.

Pros of Distributed:

Faster access for local users

Fault tolerance: system keeps running even if one server fails


Cons:

Synchronization is hard

Data consistency is harder to maintain



---

2. CAP Theorem in Distributed Systems

Definition:
The CAP Theorem states that a distributed system can only guarantee two out of three: Consistency, Availability, and Partition Tolerance.

Example:
Pizza chain: You can’t guarantee the same menu (consistency), always deliver (availability), and work during internet failures (partition tolerance) all at once.


---

3. Access Control Mechanisms in Databases

Definition:
Access control ensures that only authorized users can access or modify certain parts of the database. It includes authentication, authorization, and auditing.

Example:
Like a school:

Authentication = student ID to enter

Authorization = only staff enter the staff room

Auditing = security logs to track access



---

4. Document-Based vs Key-Value NoSQL Databases

Definition:

Key-Value Store: Stores data as key-value pairs. Fast and simple.

Document-Based DB: Stores data as JSON-like documents with nested structures.


Example:

Use key-value for fast lookups (e.g., session tokens)

Use document-based for complex data like user profiles or products



---

5. Distributed Query Processing Challenges

Definition:
Distributed query processing involves executing queries across multiple nodes or servers, rather than on a single database instance.

Challenges:

Network delays

Data consistency issues

High cost of data movement


Solution:
Optimize query paths, cache results, and group related data on the same nodes.


---

6. Query Optimization in Distributed Databases

Definition:
Query optimization is the process of choosing the most efficient way to execute a query, especially when data is distributed.

In Distributed Systems:

Optimize for data location

Use parallel processing

Minimize network traffic



---

7. Data Fragmentation

Definition:
Data fragmentation splits a dataset into smaller parts (fragments) and stores them across multiple locations.

Pros:

Faster local access

Balanced storage load


Cons:

Complex joins across fragments

Performance overhead during distributed queries



---

8. SQL Injection: Causes and Prevention

Definition:
SQL Injection is a security vulnerability where attackers insert malicious SQL code into queries to access or manipulate data.

Causes:

Using user input directly in SQL queries


Prevention:

Parameterized queries

Input validation

Escaping dangerous characters



---

9. Ensuring Data Consistency in Distributed Databases

Definition:
Data consistency means all users see the same data, even with concurrent updates from different locations.

Solutions:

Consensus algorithms (Raft, Paxos)

Synchronous replication

Versioning and conflict resolution



---

10. Best NoSQL Model for High Availability and Fault Tolerance

Definition:
A highly available and fault-tolerant system ensures users can access data even when some parts fail.

Best Options:

Cassandra: Highly available, peer-to-peer model

MongoDB: Easy replication and automatic failover



---

11. Improving Query Performance in Distributed Databases

Definition:
Improving query performance means reducing response time and resource usage during data retrieval.

Techniques:

Localizing data

Running queries in parallel

Creating indexes

Caching results



---

12. Defending Against SQL Injection

Definition:
Defending against SQL injection involves preventing attackers from injecting malicious SQL commands through user inputs.

Strategies:

Always use parameterized queries

Validate and sanitize all inputs

Use WAFs

Periodic security audits



---

13. Modeling User Profiles in Document-Based NoSQL Databases

Definition:
Modeling in NoSQL involves storing complex, nested user data using documents (e.g., JSON).

Approach:

Use embedded documents for quick access to related data

Use references when data grows large or is shared across documents



---

14. Flow Control Mechanisms in Databases

Definition:
Flow control in databases includes managing user access, transactions, and system load to ensure safe and efficient operations.

Mechanisms:

Access control (authentication, authorization)

Transaction control (ensures atomicity and consistency)

Rate limiting (prevents overload)

Auditing (logs user actions)
