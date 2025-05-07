| **Aspect**                 | **Indexing**                                          | **Hashing**                                         |
| -------------------------- | ----------------------------------------------------- | --------------------------------------------------- |
| **What it is**             | Ordered structure (e.g., B-tree) for faster searching | Uses a hash function to map keys to fixed locations |
| **Used for**               | Range queries, alphabetical or sorted searches        | Exact match lookups using IDs or unique keys        |
| **Search type**            | Partial match, range search (e.g., names A–D)         | Exact match only (e.g., ID = 29384)                 |
| **Ordering**               | Maintains order (alphabetical/date-wise)              | No ordering of data                                 |
| **Speed**                  | Fast (logarithmic time, e.g., O(log n))               | Very fast (constant time, e.g., O(1))               |
| **Use case in catalog**    | Find all authors starting with "M"                    | Find author using unique ID like #29384             |
| **Data structure**         | Tree-based (e.g., B-tree, B+ tree)                    | Hash table with buckets                             |
| **Supports range queries** | Yes (e.g., "A to F")                                  | No                                                  |
| **Collision handling**     | Not needed                                            | Required (chaining or open addressing)              |
| **Flexibility**            | More flexible for different types of queries          | Limited to key-based lookups                        |
