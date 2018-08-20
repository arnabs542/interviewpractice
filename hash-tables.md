#### Hash Tables

A **hash table** is a data structure which organizes data using a **hash function**  in order to support quick insertion and search.

There are two different kinds of hash tables: **hash set** and **hash map**.

* The **hash set** is one of the implementations of a **set** data structure to store **no repeated values**.
* The **hash map** is one of the implementations of a **map** data structure to store **\(key, value\)**_** **_pairs.

A good hash function is critical for $$\small O(1)$$ insertion, retrieval, and deletion. A poor hash function will result in many collisions, and depending on how those are resolved \(e.g. chaining\), the time complexity may go up to $$\small O(n)$$.

