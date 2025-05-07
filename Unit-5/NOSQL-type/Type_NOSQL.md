### 1. **Document-Based Databases**

**Definition:**
Stores data as documents, usually in **JSON**, **BSON**, or **XML** format.

**Examples:** MongoDB, CouchDB

**Optimized for:**

* Semi-structured or hierarchical data
* Applications needing flexible schemas
* Content management systems, blogging platforms

**Advantages:**

* Flexible and dynamic schema
* Easy mapping to programming language objects
* Efficient for reading/writing entire documents

**Trade-offs:**

* Not ideal for complex relational queries (like joins)
* Performance issues with very large documents

---

### 2. **Key-Value Stores**

**Definition:**
Stores data as a simple **key-value pair**, where the key is unique.

**Examples:** Redis, DynamoDB, Riak

**Optimized for:**

* High-speed read/write
* Caching, session management, real-time data

**Advantages:**

* Extremely fast and scalable
* Simple design, easy to use

**Trade-offs:**

* No support for querying by value or complex queries
* Limited data modeling (no relationships)

---

### 3. **Column-Based (Wide Column Stores)**

**Definition:**
Stores data in **columns** instead of rows, grouped into families.

**Examples:** Apache Cassandra, HBase, ScyllaDB

**Optimized for:**

* High write throughput
* Time-series data, event logging, IoT

**Advantages:**

* Efficient for queries over large datasets
* Better compression and performance on large-scale data

**Trade-offs:**

* Not as intuitive as document stores
* Requires good schema design to optimize performance

---

### 4. **Graph Databases**

**Definition:**
Stores data as **nodes** (entities) and **edges** (relationships).

**Examples:** Neo4j, Amazon Neptune, ArangoDB

**Optimized for:**

* Complex relationships and interconnected data
* Social networks, fraud detection, recommendation engines

**Advantages:**

* Efficient for traversing relationships
* Schema-free and expressive query languages (like Cypher)

**Trade-offs:**

* Not ideal for simple, flat data
* Requires understanding of graph theory for modeling

---

### Summary Table:

| Feature               | Document DB           | Key-Value Store    | Column-Based DB     | Graph DB               |
| --------------------- | --------------------- | ------------------ | ------------------- | ---------------------- |
| **Structure**         | JSON-like documents   | Key-value pairs    | Column families     | Nodes and edges        |
| **Schema**            | Flexible              | Schema-less        | Semi-structured     | Schema-less            |
| **Query Flexibility** | Moderate              | Low                | Moderate            | High                   |
| **Performance**       | Fast (doc-level ops)  | Very fast          | Fast for large data | Fast for relationships |
| **Best For**          | CMS, catalogs, apps   | Caching, sessions  | Analytics, logging  | Social, network data   |
| **Limitations**       | Weak on relationships | No complex queries | Complex setup       | Limited for flat data  |

---

### Choosing the Right NoSQL Type:

* **Need high-speed lookup?** → Go for **Key-Value**
* **Need flexible structured data?** → Use **Document DB**
* **Need heavy write/load analytics?** → Use **Column-based**
* **Need to model and query relationships?** → Use **Graph DB**
