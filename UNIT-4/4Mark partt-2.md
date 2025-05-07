# Q10: One of the hardest problems in query optimization is accurately estimating the costs of alternative query plans. Optimizers cost query plans using mathematical models that rely heavily on estimating the cardinality (number of tuples). Justify how you would estimate the cost in query optimization.

**What**: Query cost estimation is the process of predicting the computational resources (CPU, I/O, memory, network) required to execute a particular query plan, with cardinality estimation (predicting result sizes) being the most critical factor.

**Why**: Accurate cost estimation is essential because it allows the query optimizer to choose the most efficient execution plan among many alternatives, significantly impacting query performance and system resource utilization.

**When**: Cost estimation occurs during the query optimization phase, before actual execution, whenever a database query is submitted.

**Where**: This estimation happens in the query optimizer component of the database management system.

**How**: Cost estimation relies on several techniques:

1. **Statistics Collection and Maintenance**:
   - Collecting histograms on column value distributions
   - Tracking number of distinct values (NDV) per column
   - Maintaining up-to-date table sizes and row counts
   - Recording index characteristics (height, clustering factor)

2. **Cardinality Estimation Methods**:
   - **Selectivity calculation**: For filter predicates like `col = value`
     - Formula: selectivity = 1 / number of distinct values
     - Estimated rows = table_rows × selectivity
   - **Range queries**: For predicates like `col BETWEEN x AND y`
     - Use histograms to estimate the fraction of values in the range
   - **Compound predicates**: For conditions like `A AND B`
     - Apply the independence assumption: selectivity(A AND B) = selectivity(A) × selectivity(B)
   - **Join cardinality**: For joins between tables
     - Formula: |R ⋈ S| = |R| × |S| / max(NDV(R.join_col), NDV(S.join_col))

3. **Cost Model Components**:
   - **I/O cost**: Number of disk blocks accessed
     - Sequential reads vs. random access costs
   - **CPU cost**: Processing time for operations
   - **Memory usage**: Buffer pool utilization
   - **Network cost**: Data transfer between nodes (distributed systems)

4. **Improving Estimation Accuracy**:
   - Using multi-dimensional histograms for correlated columns
   - Sampling data for complex predicates
   - Learning from execution feedback (adaptive query processing)
   - Maintaining statistics on intermediate results

**Example**: 
Consider estimating the cost of two alternative plans for: 
```sql
SELECT * FROM orders JOIN customers 
ON orders.customer_id = customers.id 
WHERE orders.amount > 1000
```

**Plan 1**: Filter orders first, then join
1. Cardinality estimate:
   - orders table: 1,000,000 rows
   - Selectivity of `amount > 1000`: 0.1 (10%)
   - After filter: 100,000 rows
   - Join with customers (10,000 rows): 100,000 rows
2. Cost components:
   - I/O: Scan orders (5,000 blocks) + index lookup on customers (≈100,000 lookups)
   - CPU: Filter 1M rows + join processing
   - Total cost estimate: 105,000 disk accesses + CPU processing

**Plan 2**: Join first, then filter
1. Cardinality estimate:
   - Join result: 1,000,000 rows
   - After filter: 100,000 rows
2. Cost components:
   - I/O: Scan orders (5,000 blocks) + scan customers (500 blocks)
   - CPU: Join 1M × 10K rows + filter 1M rows
   - Total cost estimate: 5,500 disk accesses + high CPU for join

Based on these estimates, Plan 1 would likely be more efficient for this particular query.

**Diagram**:
```
Query Optimization Cost Estimation Process
┌───────────────────────────────────────────────────────┐
│                Statistics Collection                   │
├───────────────┬───────────────┬───────────────────────┤
│  Histograms   │ Table Stats   │ Index Statistics      │
│  • Value      │ • Row counts  │ • Clustering factor   │
│    distribution│ • Block counts│ • Index height        │
│  • Quantiles  │ • Avg row size│ • Distinct values     │
└───────┬───────┴───────┬───────┴───────────┬───────────┘
        │               │                   │
        ▼               ▼                   ▼
┌───────────────────────────────────────────────────────┐
│                Cardinality Estimation                  │
├───────────────┬───────────────┬───────────────────────┤
│  Selectivity  │  Join Size    │  Group-By Size        │
│  Calculation  │  Estimation   │  Estimation           │
└───────┬───────┴───────┬───────┴───────────┬───────────┘
        │               │                   │
        ▼               ▼                   ▼
┌───────────────────────────────────────────────────────┐
│                    Cost Model                          │
├───────────────┬───────────────┬───────────────────────┤
│   I/O Cost    │   CPU Cost    │   Memory/Network Cost  │
└───────┬───────┴───────┬───────┴───────────┬───────────┘
        │               │                   │
        ▼               ▼                   ▼
┌───────────────────────────────────────────────────────┐
│                   Total Plan Cost                      │
└───────────────────────────────────────────────────────┘
```

# Q11: Using the scenario of an author catalog in a library, briefly narrate indexing and hashing.

**What**: 
- Indexing is an ordered data structure that improves the speed of data retrieval operations by maintaining pointers to records based on key values.
- Hashing is a technique that maps data to fixed locations in a table using a hash function for direct access.

**Why**: Both techniques dramatically improve search efficiency in large datasets like a library's author catalog:
- Indexing allows for range searches and ordered access
- Hashing provides near-constant time access for exact matches

**When**: 
- Indexing is used when searching for ranges of values or when ordering is important
- Hashing is preferred for exact match lookups requiring the fastest possible access time

**Where**: In a library's author catalog:
- Indexes are used for searching by author names alphabetically or by date ranges
- Hash tables might be used for ISBN or unique ID lookups

**How**:
**Indexing in Author Catalog**:
1. Create a B-tree or similar structure with author names as keys
2. Each key contains pointers to all books by that author
3. The index maintains keys in sorted order
4. Searching follows the tree structure to quickly locate an author
5. Range queries (e.g., authors "Adams" to "Anderson") are efficient

**Hashing in Author Catalog**:
1. Apply a hash function to an author ID or ISBN
2. The resulting hash value determines the storage location
3. To retrieve information, hash the ID again and go directly to that location
4. Collisions (when different authors hash to the same location) are resolved using techniques like chaining or open addressing

**Example**:
In a library with 100,000 authors, finding "J.K. Rowling" could require checking thousands of records sequentially. With a B-tree index, we might only need to check 4-5 nodes. With hashing on author ID, we could find her record with just 1 access.

**Diagram**:
```
Library Author Catalog Implementation

B-Tree Index on Author Names:
                        [M-Z]
                       /     \
                  [A-L]       [Q-Z]
                 /     \      /     \
            [A-E]     [F-L] [Q-T]   [U-Z]
            /   \     /   \  /   \   /   \
         [A-B] [C-E]...   ...    ...    ...

Hash Table on Author IDs:
┌───┐
│ 0 │→ [Author #45892: Stephen King, Horror, 1947-...]
├───┤
│ 1 │→ [Author #78341: Maya Angelou, Poetry, 1928-2014...]
├───┤
│ 2 │→ [Author #12458: J.R.R. Tolkien, Fantasy, 1892-1973...]
├───┤
│...│
├───┤
│452│→ [Author #29384: J.K. Rowling, Fantasy, 1965-...] 
├───┤
│...│
└───┘

Usage Comparison:
1. "Find all authors with last names starting with 'S'" 
   - Index: Efficient (navigate to 'S' section and retrieve range)
   - Hash table: Inefficient (must check all entries)

2. "Find author with ID #29384"
   - Index: Moderate speed (multiple node traversals)
   - Hash table: Very fast (direct access to bucket 452)
```

# Q12: Sparse index contains index records for only some search-key values. Identify whether the statement is true or false. Justify with explanation.

**What**: A sparse index is an indexing structure that contains entries for only a subset of the search key values in a file, typically just one entry per data block or group of records.

**Why**: Sparse indices are used to reduce the size of the index while still providing reasonable search performance, offering a balance between index size and lookup speed.

**When**: Sparse indices are used when:
- Storage space for indices is limited
- The data file is sorted on the search key
- Approximate positioning is acceptable before sequential scanning

**Where**: Sparse indices are implemented in database systems and file organizations, particularly for large datasets where a complete (dense) index would be prohibitively large.

**How**: A sparse index works by:
1. Storing index entries only for the first record in each disk block or for specific marker records
2. When searching, finding the largest indexed key value less than or equal to the search key
3. Starting a sequential scan from that position until the desired record is found or passed

**Example**: In a file of employee records sorted by employee ID, a sparse index might contain entries only for IDs 1000, 2000, 3000, etc. To find employee #1500, we would use the index to locate the entry for #1000, then sequentially scan forward until we find #1500.

**Diagram**:
```
Dense vs. Sparse Index Comparison

Data File (sorted by key):
┌───────┬───────┬───────┬───────┬───────┬───────┬───────┬───────┐
│Block 1│Block 2│Block 3│Block 4│Block 5│Block 6│Block 7│Block 8│
├───────┼───────┼───────┼───────┼───────┼───────┼───────┼───────┤
│ID:100 │ID:225 │ID:340 │ID:470 │ID:520 │ID:650 │ID:780 │ID:890 │
│ID:150 │ID:240 │ID:375 │ID:485 │ID:540 │ID:675 │ID:795 │ID:910 │
│ID:175 │ID:290 │ID:420 │ID:490 │ID:590 │ID:710 │ID:830 │ID:940 │
└───────┴───────┴───────┴───────┴───────┴───────┴───────┴───────┘

Dense Index (one entry per record):
┌───────┬───────┐  ┌───────┬───────┐  ┌───────┬───────┐  ┌───────┬───────┐
│ID:100 │Ptr to │  │ID:340 │Ptr to │  │ID:520 │Ptr to │  │ID:780 │Ptr to │
├───────┼───────┤  ├───────┼───────┤  ├───────┼───────┤  ├───────┼───────┤
│ID:150 │record │  │ID:375 │record │  │ID:540 │record │  │ID:795 │record │
├───────┼───────┤  ├───────┼───────┤  ├───────┼───────┤  ├───────┼───────┤
│ID:175 │       │  │ID:420 │       │  │ID:590 │       │  │ID:830 │       │
├───────┼───────┤  ├───────┼───────┤  ├───────┼───────┤  ├───────┼───────┤
│ID:225 │       │  │ID:470 │       │  │ID:650 │       │  │ID:890 │       │
├───────┼───────┤  ├───────┼───────┤  ├───────┼───────┤  ├───────┼───────┤
│ID:240 │       │  │ID:485 │       │  │ID:675 │       │  │ID:910 │       │
├───────┼───────┤  ├───────┼───────┤  ├───────┼───────┤  ├───────┼───────┤
│ID:290 │       │  │ID:490 │       │  │ID:710 │       │  │ID:940 │       │
└───────┴───────┘  └───────┴───────┘  └───────┴───────┘  └───────┴───────┘

Sparse Index (one entry per block):
┌───────┬───────────┐  ┌───────┬───────────┐  ┌───────┬───────────┐  ┌───────┬───────────┐
│ID:100 │Ptr to Blk1│  │ID:340 │Ptr to Blk3│  │ID:520 │Ptr to Blk5│  │ID:780 │Ptr to Blk7│
└───────┴───────────┘  └───────┴───────────┘  └───────┴───────────┘  └───────┴───────────┘
```

**Statement Evaluation**: The statement "Sparse index contains index records for only some search-key values" is TRUE.

**Justification**: A sparse index intentionally includes only a subset of the search key values from the data file, typically one per data block or at regular intervals. This approach reduces the size of the index while still facilitating faster searches compared to a full file scan. The trade-off is that after using the index to find the appropriate starting point, a sequential scan may be needed to locate the specific record. This makes sparse indices particularly suitable for sorted files where the sequential scan after positioning is relatively efficient.

# Q13: Catalogue the updates in an extendable hash structure with an example.

**What**: Extendable hashing is a dynamic hashing technique that allows a hash table to grow or shrink in response to the database size without requiring rehashing of all records.

**Why**: Extendable hashing provides efficient handling of large, growing datasets by:
- Avoiding performance degradation as data grows
- Eliminating the need for periodic complete file reorganization
- Maintaining near-constant access time despite file growth

**When**: Extendable hashing is used in databases and file systems when:
- The dataset size is unpredictable or constantly growing
- Fast access times must be maintained regardless of file size
- Rehashing the entire structure would be too costly

**Where**: Extendable hashing is implemented in:
- Database management systems
- File systems
- Memory management systems
- Large-scale data storage solutions

**How**: Extendable hashing works through:
1. A directory that stores pointers to data buckets
2. Global and local depth counters that determine bit usage
3. Bucket splitting when overflow occurs
4. Directory doubling when necessary

**Example**: Let's track updates in an extendable hash structure with records having keys 8, 24, 56, 40, 72, 16:

**Diagram**:
```
Initial State (Global Depth=1, Bucket Capacity=2)
Directory:           Buckets:
┌───┐                ┌───────────┐
│ 0 │───────────────>│ Empty     │ Local Depth=1
└───┘                └───────────┘
┌───┐                ┌───────────┐
│ 1 │───────────────>│ Empty     │ Local Depth=1
└───┘                └───────────┘

After inserting 8 (binary 1000, last bit=0):
Directory:           Buckets:
┌───┐                ┌───────────┐
│ 0 │───────────────>│ 8         │ Local Depth=1
└───┘                └───────────┘
┌───┐                ┌───────────┐
│ 1 │───────────────>│ Empty     │ Local Depth=1
└───┘                └───────────┘

After inserting 24 (binary 11000, last bit=0):
Directory:           Buckets:
┌───┐                ┌───────────┐
│ 0 │───────────────>│ 8, 24     │ Local Depth=1
└───┘                └───────────┘
┌───┐                ┌───────────┐
│ 1 │───────────────>│ Empty     │ Local Depth=1
└───┘                └───────────┘

After inserting 56 (binary 111000, last bit=0):
Bucket overflow! Split bucket and increase local depth:
Directory:           Buckets:
┌───┐                ┌───────────┐
│ 0 │─┐              │ 8, 56     │ Local Depth=2 (last 2 bits=00)
└───┘ │              └───────────┘
      │              ┌───────────┐
      └─────────────>│ 24        │ Local Depth=2 (last 2 bits=10)
┌───┐                ┌───────────┐
│ 1 │───────────────>│ Empty     │ Local Depth=1
└───┘                └───────────┘

Need to double directory (Global Depth=2):
Directory:           Buckets:
┌───┐                ┌───────────┐
│ 00│───────────────>│ 8, 56     │ Local Depth=2
└───┘                └───────────┘
┌───┐                ┌───────────┐
│ 01│───────────────>│ Empty     │ Local Depth=1
└───┘                └───────────┘
┌───┐                ┌───────────┐
│ 10│───────────────>│ 24        │ Local Depth=2
└───┘                └───────────┘
┌───┐                ┌───────────┐
│ 11│───────────────>│ Empty     │ Local Depth=1
└───┘                └───────────┘

After inserting 40 (binary 101000, last 2 bits=00):
Bucket overflow! Split bucket again:
Directory:           Buckets:
┌───┐                ┌───────────┐
│ 00│───────────────>│ 8, 40     │ Local Depth=3 (last 3 bits=000)
└───┘                └───────────┘
┌───┐                ┌───────────┐
│ 01│───────────────>│ Empty     │ Local Depth=1
└───┘                └───────────┘
┌───┐                ┌───────────┐
│ 10│───────────────>│ 24        │ Local Depth=2
└───┘                └───────────┘
┌───┐                ┌───────────┐
│ 11│───────────────>│ 56        │ Local Depth=3 (last 3 bits=000)
└───┘                └───────────┘

Double directory again (Global Depth=3):
[Directory expands to 8 entries with appropriate pointers]
```

# Q14: Consider the query: `SELECT LNAME, FNAME FROM EMPLOYEE WHERE SALARY > (SELECT MAX(SALARY) FROM EMPLOYEE WHERE DNO=5);` Write down the inner block and outer block of the query.

**What**: A nested SQL query contains an inner subquery (inner block) embedded within an outer query (outer block). The inner block is evaluated first, and its result is used in the evaluation of the outer block.

**Why**: Nested queries are used when a query requires data that must be calculated from the same or different tables before the main query can be evaluated.

**When**: Nested queries are particularly useful for:
- Comparisons against aggregated values
- Filtering based on the existence of related records
- When processing needs to happen in a specific order

**Where**: Nested queries are processed within the database engine's query processor, which evaluates the inner block first and passes its result to the outer block.

**How**: The query processing follows these steps:
1. Evaluate the inner subquery to completion
2. Use the result of the inner subquery in the condition of the outer query
3. Execute the outer query with the substituted value
4. Return the final result set

**Example**: In the given query, we're finding employees whose salary exceeds the maximum salary in department 5.

**Inner Block**:
```sql
SELECT MAX(SALARY) FROM EMPLOYEE WHERE DNO=5
```
This subquery finds the maximum salary among all employees in department number 5.

**Outer Block**:
```sql
SELECT LNAME, FNAME FROM EMPLOYEE WHERE SALARY > [result_of_inner_block]
```
This main query returns the last name and first name of all employees whose salary is greater than the maximum salary found in department 5.

**Diagram**:
```
Query Execution Flow
┌──────────────────────────────────────────────────────┐
│                     Outer Block                      │
│  SELECT LNAME, FNAME FROM EMPLOYEE                   │
│  WHERE SALARY > [?]                                  │
└──────────────────┬───────────────────────────────────┘
                   │ Needs value to compare against
                   ▼
┌──────────────────────────────────────────────────────┐
│                     Inner Block                      │
│  SELECT MAX(SALARY) FROM EMPLOYEE WHERE DNO=5        │
│                                                      │
│  Result: $85,000 (example value)                     │
└──────────────────┬───────────────────────────────────┘
                   │ Returns single value
                   ▼
┌──────────────────────────────────────────────────────┐
│                 Outer Block (Completed)              │
│  SELECT LNAME, FNAME FROM EMPLOYEE                   │
│  WHERE SALARY > $85,000                              │
└──────────────────────────────────────────────────────┘
```

# Q15: State the two-way join and multi-way join with suitable examples.

**What**: 
- A two-way join combines records from exactly two tables based on a related column between them.
- A multi-way join combines records from three or more tables based on related columns across all tables.

**Why**: Joins are essential in relational databases because they allow users to:
- Retrieve related data spread across multiple tables
- Maintain database normalization while getting denormalized views
- Create meaningful reports that combine information from different entities

**When**: Joins are used whenever:
- Information needed for a query spans multiple tables
- Relationships between entities need to be explored
- Data needs to be consolidated from normalized tables

**Where**: Joins are implemented in:
- SQL queries in database management systems
- Relational algebra operations in query processing
- Data integration and ETL (Extract, Transform, Load) processes

**How**:
1. **Two-way Join**: 
   - Matches records from two tables based on a join condition
   - Common types include inner join, left/right outer join, full outer join
   - Uses one join operation with one join condition

2. **Multi-way Join**:
   - Matches records across three or more tables
   - Can be implemented as a sequence of two-way joins
   - Requires multiple join conditions connecting all tables
   - Often follows the structure of foreign key relationships

**Example - Two-way Join**:

Consider retrieving a list of employees with their department names:

```sql
SELECT e.employee_name, d.department_name
FROM employees e
JOIN departments d ON e.department_id = d.department_id;
```

This query joins the employees table with the departments table using the department_id column that appears in both tables.

**Example - Multi-way Join**:

Consider retrieving employees, their departments, and their project assignments:

```sql
SELECT e.employee_name, d.department_name, p.project_name
FROM employees e
JOIN departments d ON e.department_id = d.department_id
JOIN project_assignments pa ON e.employee_id = pa.employee_id
JOIN projects p ON pa.project_id = p.project_id;
```

This query joins four tables: employees, departments, project_assignments (junction table), and projects.

**Diagram**:
```
Two-way Join Example
┌───────────────┐        ┌───────────────┐
│   employees   │        │  departments  │
├───────────────┤        ├───────────────┤
│ employee_id   │        │ department_id │
│ employee_name │        │ department_name│
│ department_id ├────────┤               │
└───────────────┘        └───────────────┘
        JOIN ON employees.department_id = departments.department_id

Multi-way Join Example
┌───────────────┐        ┌───────────────┐
│   employees   │        │  departments  │
├───────────────┤        ├───────────────┤
│ employee_id   │        │ department_id │
│ employee_name │        │ department_name│
│ department_id ├────────┤               │
└───────┬───────┘        └───────────────┘
        │
        │ JOIN ON employees.employee_id = project_assignments.employee_id
        │
┌───────▼───────┐        ┌───────────────┐
│  project_     │        │   projects    │
│  assignments  │        ├───────────────┤
├───────────────┤        │ project_id    │
│ employee_id   │        │ project_name  │
│ project_id    ├────────┤               │
└───────────────┘        └───────────────┘
        JOIN ON project_assignments.project_id = projects.project_id
```

These detailed answers should help you achieve full marks by providing thorough explanations for each question, following the structured format of What, Why, When, Where, How, Example, and including clear diagrams.
