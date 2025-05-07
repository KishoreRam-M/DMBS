# Q1: Outline the term RAID and visualize it. Catalogue various terms of RAID and elaborate any 2 RAID levels with visualization.

**What**: RAID (Redundant Array of Independent Disks) is a technology that combines multiple physical hard drives into a single logical unit to improve performance, increase storage capacity, and provide data redundancy.

**Why**: RAID is crucial because it:
- Protects against data loss when hardware fails
- Improves system performance through parallel operations
- Creates larger storage volumes from smaller individual disks
- Ensures business continuity through fault tolerance

**When**: RAID is implemented:
- In enterprise servers requiring 24/7 uptime
- For mission-critical applications where data loss is unacceptable
- In systems requiring higher read/write performance than a single disk can provide
- For data centers and cloud storage infrastructure

**Where**: RAID sits between the physical storage layer (the actual hard drives) and the operating system. It can be implemented through hardware controllers or software solutions.

**How**: RAID works by:
1. Distributing data across multiple disks in specific patterns
2. Creating redundancy through mirroring or parity calculations
3. Processing read/write operations in parallel across multiple disks
4. Managing the array as a single logical volume that appears as one disk to the OS

**Example**: A company needs to store critical customer data and can't afford downtime. They implement RAID 10 with four disks, which combines mirroring for redundancy and striping for performance.

**Common RAID Levels**:
- RAID 0: Striping (performance)
- RAID 1: Mirroring (redundancy)
- RAID 5: Striping with parity (balance of performance and redundancy)
- RAID 6: Striping with double parity (enhanced redundancy)
- RAID 10: Mirror of stripes (performance and redundancy)

**RAID 1 (Mirroring)**:

**What**: Creates an exact duplicate of data across two or more disks.

**How**: When data is written to one disk, the same data is simultaneously written to the mirror disk.

**Diagram**:
```
RAID 1 (Mirroring)
┌───────────┐  ┌───────────┐
│   Disk 1  │  │   Disk 2  │
├───────────┤  ├───────────┤
│  Data A1  │  │  Data A1  │
│  Data A2  │  │  Data A2  │
│  Data A3  │  │  Data A3  │
│  Data A4  │  │  Data A4  │
└───────────┘  └───────────┘
```

**RAID 5 (Striping with Parity)**:

**What**: Distributes data and parity information across all disks in the array.

**How**: Data is striped across disks with parity information rotated among the disks. If one disk fails, the parity information can be used to reconstruct missing data.

**Diagram**:
```
RAID 5 (Striping with Parity)
┌───────────┐  ┌───────────┐  ┌───────────┐
│   Disk 1  │  │   Disk 2  │  │   Disk 3  │
├───────────┤  ├───────────┤  ├───────────┤
│  Data A1  │  │  Data A2  │  │ Parity A  │
│  Data B2  │  │ Parity B  │  │  Data B1  │
│ Parity C  │  │  Data C1  │  │  Data C2  │
└───────────┘  └───────────┘  └───────────┘
```

# Q2: "An array of disks is more likely to fail compared to a single disk. How is it that RAID arrays still manage to provide more data protection compared to a single disk?" Justify with a diagrammatic representation.

**What**: RAID systems provide data protection despite the statistical likelihood of individual disk failures by implementing redundancy strategies that can survive these failures.

**Why**: In any storage system, each physical component has a failure probability. With more components, the likelihood of at least one component failing increases. However, RAID counters this by distributing data in ways that allow recovery when failures occur.

**When**: This protection is crucial when:
- Data availability is a priority
- System uptime requirements are high
- Data integrity cannot be compromised
- Recovery time objectives are strict

**Where**: This redundancy occurs at the physical storage layer but is managed by either hardware RAID controllers or software RAID implementations.

**How**: RAID achieves protection through:
1. Data mirroring - Creating complete copies of data on multiple disks
2. Parity calculations - Storing mathematical checksums to reconstruct lost data
3. Distributing critical information - Ensuring no single point of failure
4. Error detection - Identifying when disks have failed or data is corrupted

**Example**: Consider a RAID 1 mirror with two disks. While the likelihood of a disk failure doubles compared to a single disk, the data remains accessible through the surviving mirror if one disk fails. This provides protection that a single disk cannot offer.

**Diagram**:
```
Single Disk vs. RAID 1 Comparison

Single Disk:
┌───────────┐
│   Disk 1  │  ← If this disk fails, ALL data is lost
├───────────┤
│  Data A   │
│  Data B   │
│  Data C   │
└───────────┘

RAID 1:
┌───────────┐  ┌───────────┐
│   Disk 1  │  │   Disk 2  │
├───────────┤  ├───────────┤
│  Data A   │  │  Data A   │  ← If Disk 1 fails, data remains
│  Data B   │  │  Data B   │    available on Disk 2
│  Data C   │  │  Data C   │
└───────────┘  └───────────┘

Failure Scenario:
┌───────────┐  ┌───────────┐
│   Disk 1  │  │   Disk 2  │
├───────────┤  ├───────────┤
│    X X    │  │  Data A   │  ← Even with Disk 1 failed,
│    X X    │  │  Data B   │    all data remains accessible
│    X X    │  │  Data C   │
└───────────┘  └───────────┘
```

# Q3: An application generates 7200 IOPS, with 60 percent of them being reads. Calculate the disk load in RAID 1, RAID 5, and RAID 6.

**What**: IOPS (Input/Output Operations Per Second) is the standard measurement for the total number of read and write operations a storage system can handle per second. Different RAID levels handle reads and writes differently, affecting the total disk load.

**Why**: Understanding disk load across different RAID configurations helps system architects select the appropriate RAID level for their specific workload patterns and performance requirements.

**When**: This analysis is essential during system design, capacity planning, and performance tuning of storage systems.

**Where**: This calculation applies to any system where storage performance is critical, including database servers, virtualization hosts, and application servers.

**How**: To calculate disk load:
1. Determine the number of read and write operations
2. Identify how many disk operations each read/write requires based on the RAID level
3. Multiply operations by their respective disk load factors
4. Sum to find the total disk load

**Example**:
Given information:
- 7200 total IOPS
- 60% reads (40% writes)
- Read IOPS = 0.6 × 7200 = 4320 IOPS
- Write IOPS = 0.4 × 7200 = 2880 IOPS

**Calculating Disk Load for RAID 1:**
- For reads: Reads are distributed between 2 disks, so each read requires 1 disk operation
  - Read load = 4320 × 1 = 4320 disk operations
- For writes: Each write must be written to 2 disks
  - Write load = 2880 × 2 = 5760 disk operations
- Total disk load = 4320 + 5760 = 10,080 disk operations per second

**Calculating Disk Load for RAID 5:**
- For reads: Each read requires 1 disk operation
  - Read load = 4320 × 1 = 4320 disk operations
- For writes: Each write requires 4 disk operations (read old data, read old parity, write new data, write new parity)
  - Write load = 2880 × 4 = 11,520 disk operations
- Total disk load = 4320 + 11,520 = 15,840 disk operations per second

**Calculating Disk Load for RAID 6:**
- For reads: Each read requires 1 disk operation
  - Read load = 4320 × 1 = 4320 disk operations
- For writes: Each write requires 6 disk operations (read old data, read two old parities, write new data, write two new parities)
  - Write load = 2880 × 6 = 17,280 disk operations
- Total disk load = 4320 + 17,280 = 21,600 disk operations per second

**Diagram**:
```
RAID Level Disk Operations Comparison
┌───────────────┬───────────┬───────────┬───────────┬───────────┐
│ RAID Level    │ Read Ops  │ Write Ops │ Total Ops │ % of IOPS │
├───────────────┼───────────┼───────────┼───────────┼───────────┤
│ RAID 1        │    4,320  │    5,760  │   10,080  │    140%   │
│ RAID 5        │    4,320  │   11,520  │   15,840  │    220%   │
│ RAID 6        │    4,320  │   17,280  │   21,600  │    300%   │
└───────────────┴───────────┴───────────┴───────────┴───────────┘
```

# Q4: Assume record size is fixed. Each file has records of one particular type only; different files are used for different relations. Project a simple approach to access the record and write a pseudo code for the same.

**What**: Record access refers to the method used to locate and retrieve specific records from a file or database when given a search key or identifier.

**Why**: Efficient record access is crucial for database performance, especially in systems where rapid data retrieval is required. With fixed-size records, we can use direct addressing for fast access.

**When**: This approach is used in database management systems, particularly when:
- Records have fixed sizes
- Fast access to individual records is required
- The system has a predictable data structure

**Where**: This record access method is implemented at the file system or database engine level, sitting between the physical storage and the application layer.

**How**: With fixed-size records, we can calculate the exact position of any record using:
1. Record position = (Record ID - 1) × Record Size + Header Size
2. Seek to that position in the file
3. Read the record data of the predetermined size

**Example**: In a student record system with fixed-size records of 100 bytes each, to access student ID 45, we would seek to position (45-1) × 100 = 4400 bytes from the start of the file.

**Pseudo Code**:
```
function accessRecord(fileName, recordID, recordSize):
    // Open the file for reading
    file = open(fileName, "read")
    
    // Calculate the position of the record in the file
    // Assuming first record is at ID=1
    headerSize = 64  // Bytes reserved for file header
    position = headerSize + (recordID - 1) * recordSize
    
    // Seek to the calculated position
    file.seek(position)
    
    // Read the record data
    recordData = file.read(recordSize)
    
    // Check if record exists or is marked as deleted
    if recordData is empty or recordData.isDeleted():
        return "Record not found or deleted"
    
    // Close the file
    file.close()
    
    // Return the record data
    return recordData

function updateRecord(fileName, recordID, recordSize, newData):
    // Open the file for writing
    file = open(fileName, "write")
    
    // Calculate position
    headerSize = 64
    position = headerSize + (recordID - 1) * recordSize
    
    // Seek to position
    file.seek(position)
    
    // Write the new data
    file.write(newData)
    
    // Close the file
    file.close()
    
    return "Record updated successfully"
```

**Diagram**:
```
Fixed-Size Record Access
┌───────────────────────────────────────────────────────┐
│                    File Structure                     │
├─────────────┬─────────────┬─────────────┬─────────────┤
│ File Header │  Record #1  │  Record #2  │  Record #3  │
│  (64 bytes) │ (100 bytes) │ (100 bytes) │ (100 bytes) │
└─────────────┴─────────────┴─────────────┴─────────────┘
               ↑             ↑             ↑
               |             |             |
        Offset=64      Offset=164    Offset=264

To access Record #2:
Position = 64 + (2-1) * 100 = 164 bytes from start
```

# Q5: State hashing. Consider the following list of elements: 9, 19, 29, 39, 49, 59, 69. Insert the elements into a hash table of size 10.

**What**: Hashing is a technique that maps data elements to fixed-size array locations (buckets) using a hash function, enabling fast data access, insertion, and deletion operations.

**Why**: Hashing provides near-constant time access to data regardless of dataset size, dramatically improving search performance compared to linear or binary search methods.

**When**: Hashing is used when:
- Fast data lookup is required
- The dataset is large but memory access needs to be efficient
- Direct mapping between keys and values is needed

**Where**: Hashing is implemented in:
- Hash tables and hash maps in programming languages
- Database indexing
- Caching systems
- Password storage (with cryptographic hash functions)
- File integrity verification

**How**: Hashing works by:
1. Applying a hash function to the key to generate an index
2. Storing the value at that index in the hash table
3. Handling collisions when multiple keys map to the same index

For the given elements (9, 19, 29, 39, 49, 59, 69) and a hash table of size 10:
1. Hash function: h(k) = k % 10 (remainder when divided by 10)
2. Each value maps to index: h(key)
3. Collisions must be handled when multiple elements map to the same index

**Example**: Let's insert the given elements using a hash function h(k) = k % 10:

- h(9) = 9 % 10 = 9 → Place 9 at index 9
- h(19) = 19 % 10 = 9 → Collision! Need collision resolution
- h(29) = 29 % 10 = 9 → Collision!
- h(39) = 39 % 10 = 9 → Collision!
- h(49) = 49 % 10 = 9 → Collision!
- h(59) = 59 % 10 = 9 → Collision!
- h(69) = 69 % 10 = 9 → Collision!

All elements hash to position 9, causing severe collision. We can use chaining (linked lists at each index) to resolve these collisions.

**Diagram**:
```
Hash Table with Chaining Collision Resolution
┌───┐   ┌───┐   ┌───┐   ┌───┐   ┌───┐   ┌───┐   ┌───┐   ┌───┐   ┌───┐   ┌───────────────────────────────────┐
│ 0 │   │ 1 │   │ 2 │   │ 3 │   │ 4 │   │ 5 │   │ 6 │   │ 7 │   │ 8 │   │ 9 │→[9]→[19]→[29]→[39]→[49]→[59]→[69]
└───┘   └───┘   └───┘   └───┘   └───┘   └───┘   └───┘   └───┘   └───┘   └───────────────────────────────────┘

Alternative Solution: Linear Probing
┌───┐   ┌───┐   ┌───┐   ┌───┐   ┌───┐   ┌───┐   ┌───┐   ┌───┐   ┌───┐   ┌───┐
│   │   │   │   │   │   │ 39│   │ 49│   │ 59│   │ 69│   │   │   │   │   │ 9 │
└───┘   └───┘   └───┘   └───┘   └───┘   └───┘   └───┘   └───┘   └───┘   └───┘
  0       1       2       3       4       5       6       7       8       9
                        ↑       ↑       ↑       ↑               ↑
                        |       |       |       |               |
                       19      29      39      49              original
                     probed   probed  probed  probed           position
```

# Q6: Compare and contrast static and dynamic hashing.

**What**: 
- Static hashing uses a fixed-size hash table with a predetermined hash function that doesn't change as the data grows.
- Dynamic hashing uses variable-sized hash tables that can expand or contract based on the data volume, adapting the hash function accordingly.

**Why**: 
- Static hashing is simpler but suffers from performance degradation when the load factor increases.
- Dynamic hashing maintains performance regardless of data size but requires more complex algorithms and overhead.

**When**:
- Static hashing is suitable for applications where the data size is known and relatively stable.
- Dynamic hashing is ideal for applications with unpredictable or rapidly growing data volumes.

**Where**:
- Static hashing is found in simpler database implementations and applications with fixed data requirements.
- Dynamic hashing is used in modern database systems, file systems, and large-scale data storage solutions.

**How**:
- Static hashing:
  1. Uses a fixed number of buckets
  2. Handles overflow with techniques like chaining or open addressing
  3. Performance degrades as the load factor increases

- Dynamic hashing:
  1. Adjusts the hash table size by splitting or merging buckets
  2. Modifies the hash function when resizing
  3. Maintains a relatively consistent performance regardless of table size

**Example**:
- Static hashing: A hash table with 1,000 buckets using h(k) = k % 1000 will always map to the same 1,000 locations.
- Dynamic hashing: Extendible hashing starts with a small table that doubles in size when a threshold is reached, adjusting how keys are mapped.

**Diagram**:
```
Static Hashing
┌───────────────────────────────────────────────────────┐
│                     Hash Table                        │
├───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┤
│ 0 │ 1 │ 2 │ 3 │ 4 │ 5 │ 6 │ 7 │ 8 │ 9 │10 │11 │...│N │
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
  ↑   ↑   ↑   ↑                               ↑
  │   │   │   └──────┐                        │
  │   │   │          │                        │
 h(k₁)│   │        h(k₄)                     h(kₙ)
      │   │
     h(k₂)│
         h(k₃)
         
Overflow Handling with Chaining
┌───┐                        
│ 9 │→[9]→[19]→[29]→[39]→[49]...   As items increase, chains get longer
└───┘                              and performance decreases

Dynamic Hashing (Extendible Hashing)
Initial state:
┌─────┐
│  0  │→[9,19]
├─────┤
│  1  │→[29,39]
└─────┘

After splitting due to overflow:
┌─────┐
│  00 │→[9]
├─────┤
│  01 │→[19]
├─────┤
│  10 │→[29]
├─────┤
│  11 │→[39]
└─────┘
```

# Q7: Relational Database Queries

**What**: Relational database queries are structured requests to retrieve or manipulate data stored in related tables. The given database contains three tables: employee, works, and company.

**Why**: SQL queries allow us to extract meaningful information from related data by specifying conditions and joining tables based on common attributes.

**When**: These queries are used whenever specific information needs to be retrieved from a database, especially when the information spans multiple related tables.

**Where**: These queries are executed within database management systems (DBMS) like MySQL, PostgreSQL, Oracle, or SQL Server.

**How**: SQL queries use SELECT statements with:
1. Column selection (SELECT clause)
2. Table specification (FROM clause)
3. Conditions (WHERE clause)
4. Table relationships (JOIN operations)

**Example**:

**(a) Find the names of all employees who live in the city 'Miami'**
```sql
SELECT person_name
FROM employee
WHERE city = 'Miami';
```

**(b) Find the names of all employees whose salary is greater than $100,000**
```sql
SELECT person_name
FROM works
WHERE salary > 100000;
```

**(c) Find the names of all employees who live in 'Miami' and whose salary is greater than $100,000**
```sql
SELECT e.person_name
FROM employee e
JOIN works w ON e.person_name = w.person_name
WHERE e.city = 'Miami' AND w.salary > 100000;
```

**(d) Find the names of all employees who work for "First Bank Corporation"**
```sql
SELECT person_name
FROM works
WHERE company_name = 'First Bank Corporation';
```

**Diagram**:
```
Database Schema and Query Visualization

Schema:
┌───────────────────┐     ┌───────────────────────┐     ┌───────────────────┐
│     employee      │     │         works         │     │      company      │
├───────────────────┤     ├───────────────────────┤     ├───────────────────┤
│ person_name (PK)  │────>│ person_name (PK/FK)   │     │ company_name (PK) │
│ street            │     │ company_name (PK/FK) ─┼────>│ city              │
│ city              │     │ salary                │     │                   │
└───────────────────┘     └───────────────────────┘     └───────────────────┘

Query (c) execution flow:
┌───────────────────┐                  ┌───────────────────────┐
│     employee      │                  │         works         │
├───────────────────┤                  ├───────────────────────┤
│ person_name: John │──────JOIN───────>│ person_name: John     │
│ street: Oak Ave   │ e.person_name =  │ company_name: ABC Corp│
│ city: Miami       │ w.person_name    │ salary: 120000        │
└───────────────────┘                  └───────────────────────┘
        ↓                                        ↓
     Filter by                               Filter by
   city = 'Miami'                        salary > 100000
        ↓                                        ↓
        └────────────INTERSECTION────────────────┘
                            ↓
                   Result: John (meets both conditions)
```

# Q8: A database table T1 has 2000 records and occupies 80 disk blocks. Another table T2 has 400 records and occupies 20 disk blocks. The join condition must be evaluated for every pair of records. Memory can hold only 1 block from each table at a time. No index is available. Using nested-loop join, with the best choice for the outer loop, find the number of block accesses required.

**What**: Nested-loop join is a basic algorithm that joins two tables by comparing each record in one table with every record in the other table. The number of disk block accesses is a measure of the algorithm's I/O cost.

**Why**: Understanding block access requirements helps database administrators optimize query performance by choosing appropriate join algorithms and allocating sufficient memory.

**When**: This analysis is crucial when designing database queries, especially in systems with limited memory where disk I/O becomes a major performance bottleneck.

**Where**: This type of analysis is used in database management systems to optimize query execution plans.

**How**: To calculate the number of block accesses for a nested-loop join:
1. Choose the smaller table for the outer loop to minimize block reads
2. For each block in the outer table, read all blocks of the inner table
3. Add the number of times each table is accessed

Given information:
- T1: 2000 records in 80 blocks
- T2: 400 records in 20 blocks
- Memory can hold only 1 block from each table
- No indexes available
- Join requires all pair-wise record comparisons

**Example**:
Since T2 is smaller (20 blocks vs. 80 blocks), we choose T2 as the outer table:

1. Read each block of T2 once: 20 block accesses
2. For each block of T2, read all blocks of T1: 20 × 80 = 1600 block accesses
3. Total block accesses = 20 + 1600 = 1620 block accesses

**Diagram**:
```
Nested-Loop Join Block Access Analysis
┌───────────────────┐     ┌───────────────────┐
│       Table T1    │     │      Table T2     │
│  (2000 records)   │     │   (400 records)   │
│    (80 blocks)    │     │    (20 blocks)    │
└───────────────────┘     └───────────────────┘

Memory buffers:
┌─────────────┐     ┌─────────────┐
│  1 block    │     │  1 block    │
│  from T1    │     │  from T2    │
└─────────────┘     └─────────────┘

Access pattern (T2 as outer loop):
┌───────────────────────────────────────────┐
│ 1. Read block 1 of T2    (1 access)       │
│ 2. Read all 80 blocks of T1 (80 accesses) │
│ 3. Read block 2 of T2    (1 access)       │
│ 4. Read all 80 blocks of T1 (80 accesses) │
│ ...                                       │
│ 39. Read block 20 of T2   (1 access)      │
│ 40. Read all 80 blocks of T1 (80 accesses)│
└───────────────────────────────────────────┘

Total: 20 (T2 blocks) + 20×80 (T1 blocks) = 1620 block accesses
```

# Q9: Write the algorithm for executing relational query operations.

**What**: A relational query operation algorithm is a step-by-step procedure that database management systems follow to process and execute SQL queries efficiently.

**Why**: This algorithm ensures that database queries are executed correctly, efficiently, and in the optimal order to minimize resource usage and response time.

**When**: This algorithm is applied every time a query is submitted to a database management system, from simple selections to complex joins and aggregations.

**Where**: This process occurs within the query processing engine of the DBMS, which sits between the query parser and the storage engine.

**How**: The relational query execution algorithm follows these steps:
1. Parse the SQL query into an internal representation
2. Validate the query (check table/column existence, permissions)
3. Convert to relational algebra operations
4. Optimize the query execution plan
5. Execute the plan
6. Assemble and return results

**Example**:
For the query: 
```sql
SELECT employee.name, department.name 
FROM employee JOIN department 
ON employee.dept_id = department.id 
WHERE employee.salary > 50000;
```

The execution algorithm would:
1. Parse the SQL query into a syntax tree
2. Verify that tables and columns exist
3. Convert to relational algebra operations (selection, projection, join)
4. Optimize by deciding join order and method
5. Execute the optimized plan 
6. Return the result set

**Detailed Algorithm**:
```
Algorithm ExecuteRelationalQuery(query):
    // Phase 1: Query Parsing
    syntaxTree = ParseSQL(query)
    if syntaxTree has errors:
        return error
    
    // Phase 2: Query Validation
    validationResult = ValidateQuery(syntaxTree)
    if validationResult has errors:
        return error
    
    // Phase 3: Conversion to Relational Algebra
    relationalAlgebraTree = ConvertToRelationalAlgebra(syntaxTree)
    
    // Phase 4: Query Optimization
    optimizedPlan = OptimizeQueryPlan(relationalAlgebraTree)
    
    // Phase 5: Plan Execution
    for each operation in optimizedPlan:
        if operation is a table scan:
            Scan the table and retrieve records
        else if operation is a selection:
            Apply the selection condition to filter records
        else if operation is a projection:
            Extract only the requested columns
        else if operation is a join:
            Join tables based on the specified conditions
        else if operation is an aggregation:
            Group records and compute aggregate functions
        else if operation is a sort:
            Sort the results based on the ORDER BY clause
        
    // Phase 6: Result Assembly
    resultSet = AssembleResults(intermediate results)
    
    return resultSet
```

**Diagram**:
```
Query Execution Flow
┌───────────────┐
│  SQL Query    │
└───────┬───────┘
        ▼
┌───────────────┐
│  SQL Parser   │
└───────┬───────┘
        ▼
┌───────────────┐
│  Syntax Tree  │
└───────┬───────┘
        ▼
┌───────────────┐         ┌───────────────┐
│   Validator   │◄────────┤ Schema Catalog │
└───────┬───────┘         └───────────────┘
        ▼
┌───────────────┐
│ Rel. Algebra  │
│  Converter    │
└───────┬───────┘
        ▼
┌───────────────┐         ┌───────────────┐
│   Optimizer   │◄────────┤  Statistics   │
└───────┬───────┘         └───────────────┘
        ▼
┌───────────────┐
│ Execution Plan│
└───────┬───────┘
        ▼
┌───────────────┐         ┌───────────────┐
│Plan Execution │◄────────┤ Data Storage  │
└───────┬───────┘         └───────────────┘
        ▼
┌───────────────┐
│  Result Set   │
└───────────────┘
```
