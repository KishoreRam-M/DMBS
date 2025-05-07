### 1. How does the architecture of a distributed database affect data retrieval and storage compared to a centralized system?

* Think of a centralized database like a single big library in one city. Everyone must travel to that library to borrow or return a book. If the library closes (e.g., server crash), no one can access books.
* A distributed database is like having smaller libraries in many cities. People go to their nearest library—faster and more convenient. Even if one library closes, others stay open.

✅ Pros of distributed:

* Faster access for local users
* System keeps running even if a server fails (fault tolerance)

⚠️ But:

* Data must be synchronized (kept up-to-date) across locations
* Harder to manage and ensure all data copies are consistent

---

### 2. What are the trade-offs between consistency, availability, and partition tolerance in distributed systems (CAP Theorem)?

Let’s say you run a pizza delivery chain with 3 goals:

* Every store has the same menu (Consistency)
* Customers always get served (Availability)
* System runs even if one store’s internet is down (Partition Tolerance)

⚠️ CAP Theorem says: You can only achieve 2 out of 3 at the same time.

Examples:

* Banking apps often prioritize Consistency & Partition Tolerance (correct balances > always responding)
* Social media may prefer Availability & Partition Tolerance (show old data instead of nothing)

🧠 Tip: Choose trade-offs based on what your app values more—accuracy or uptime.

---

### 3. How do access control mechanisms help ensure that only authorized users can interact with specific database objects?

Imagine a school:

* Students need ID cards to enter (Authentication)
* Not all students can enter the staff room (Authorization)
* Security cameras record who enters and when (Auditing)

In databases:

* Users must log in (authentication)
* Roles determine what they can see or change (authorization)
* Logs track actions (auditing)

🎯 Result: Keeps your data safe from unauthorized access or misuse.

---

### 4. When is a document-based database better than a key-value store in NoSQL?

🔑 Key-Value store = Locker with a label (key) and a simple item inside (value). Great for:

* Fast lookups
* Simple apps like session storage or caching

📄 Document-based = Filing cabinet with folders (JSON-like documents) that have structured data. Great for:

* User profiles, blog posts, products with details
* Apps needing search/filter (querying fields inside documents)

Choose document-based when you need:

* Flexible schema (no fixed tables)
* Rich queries
* Nested data (e.g., user has name, address, preferences)

---

### 5. Why is distributed query processing more challenging than in centralized databases?

Running a query in one place (centralized) is like asking one chef to cook in one kitchen.

In a distributed database:

* Ingredients are in multiple kitchens
* Chefs must coordinate
* Some kitchens may be far away (network delays)

Problems:

* Delay due to travel (network latency)
* Different kitchens might have different versions of ingredients (data consistency)
* Costly to move ingredients around (data movement)

Solutions:

* Store related data together
* Reduce travel (optimize query paths)
* Cache results to avoid repeated work

---

### 6. How does query optimization differ in distributed databases?

In a regular system (single-node), optimization focuses on:

* CPU, RAM, disk of one machine

In a distributed system:

* Must also think about:

  * Where data lives (avoid moving too much across network)
  * Running multiple query parts in parallel (parallel processing)
  * Network speed and load

Think of it like planning a delivery:

* In one city: focus on shortest path
* Across cities: factor in traffic, fuel, number of trucks (nodes)

---

### 7. What is data fragmentation and how does it affect query performance?

🧩 Data fragmentation = Breaking large datasets into smaller chunks (fragments) and storing them in different places.

Pros:

* Speeds up local access
* Reduces storage load per server

Cons:

* Queries that need data from multiple fragments take longer
* More complex joins

✅ To optimize:

* Place frequently used data together (based on usage patterns)
* Copy key fragments across nodes (replication)
* Smart query planning to reduce fragment access

---

### 8. What causes SQL injection and how to prevent it?

SQL injection happens when:

* User inputs are used directly in SQL queries
* Malicious users can trick the database into running harmful commands

Example:
If input = ' OR 1=1; -- it can bypass login security

🛡️ Prevent it using:

* Parameterized queries (use placeholders like ?)
* Input validation (check format, type, and length)
* Escaping dangerous characters (e.g., quotes)

Tools like PreparedStatement in Java, PDO in PHP, or parameterized queries in Python can help.

---

### 9. How to ensure data consistency in a distributed database with multiple updates?

When different nodes update the same data, conflicts can occur.

🧠 Solutions:

* Consensus algorithms (like Raft or Paxos): nodes agree on changes
* Version control: track who updated what and when
* Synchronous replication: all nodes update at once (slower but safer)

Trade-off: More consistency = less speed and availability

---

### 10. Best NoSQL model for high availability and fault tolerance?

✅ Cassandra or MongoDB are great options.

Why?

* They store multiple copies of data across nodes (replication)
* If one node fails, another responds (failover)
* Easy to add more nodes (horizontal scaling)

Used in: Netflix, Instagram, WhatsApp

Perfect for:

* Messaging apps
* Shopping carts
* Real-time analytics

---

### 11. How to improve slow query performance in a distributed database?

Imagine you run a food delivery app across cities:

* Don’t ask every store for every order
* Keep popular data nearby

✅ Strategies:

* Data localization: put related data on same node
* Query parallelization: split and run on multiple nodes
* Indexing: make searches faster
* Caching: save frequent query results

These cut down on delays and reduce unnecessary data movement.

---

### 12. How to defend against frequent SQL injection attacks?

💡 Golden rule: Never trust user input.

Steps:

* Use parameterized queries always
* Validate inputs (e.g., only allow valid email format)
* Use Web Application Firewalls (WAFs) to block known attack patterns
* Regularly scan your code and database for vulnerabilities

✅ Bonus: Use stored procedures instead of building SQL dynamically

---

### 13. How to model user profiles with relationships in a document-based NoSQL database?

Use JSON-like documents.

Example:

{
"userId": "u123",
"name": "Kishore",
"email": "[kishore@example.com](mailto:kishore@example.com)",
"friends": \[
{"userId": "u456", "name": "Priya"},
{"userId": "u789", "name": "Rahul"}
],
"posts": \[
{"postId": "p1", "content": "Hello world!", "timestamp": "2025-01-01"}
]
}

✅ Embedded relationships = quick access
✅ Referenced relationships = better when relationships grow large

---

### 14. What are flow control mechanisms and how do they protect the database?

Flow control = Managing how users and systems interact with the database safely

It includes:

* Access Control (who can do what)
* Transaction control (ensures all or nothing changes)
* Rate limiting (prevent overload or abuse)
* Role-based access (admins vs regular users)

Imagine a nightclub:

* Bouncer checks ID (authentication)
* VIP area access is restricted (authorization)
* Cameras monitor activity (auditing)

Same logic applies to secure databases.
