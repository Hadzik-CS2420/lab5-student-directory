# Module 5 Assignment: Student Directory

## Overview

In this assignment you will build a student directory using two hash table implementations from scratch, practicing every core concept from Module 5. Student names are the keys and student IDs are the values — the same scenario you worked with in CT 11 and CT 12.

- **Hash functions** — converting string keys to array indices using a custom hash algorithm
- **Modulo mapping** — mapping hash values into a fixed-size bucket array
- **Separate chaining (open hashing)** — resolving collisions with linked lists at each bucket
- **Linear probing (closed hashing)** — resolving collisions by scanning forward in the array
- **Tombstone deletion** — marking slots as deleted so probing sequences aren't broken
- **Load factor** — tracking how full the table is (`size / capacity`)
- **Resizing & rehashing** — growing the table and reinserting all entries when the load factor is too high

## What You Need To Do

Your work goes in **two files**:
- **`src/ChainingHashTable.cpp`** — TODOs 1–8 (60 points)
- **`src/ProbingHashTable.cpp`** — TODOs 9–13 (40 points)

The headers, main.cpp, and tests are provided — **do not modify them**.

Open each `.cpp` file and follow every `TODO` comment. Each section tells you exactly what to implement and names the Code-Together to review: CT 11 for chaining, CT 12 for probing.

### Checklist

| TODO | Method | Points | Review |
|------|--------|--------|--------|
| **Chaining Hash Table** | | **60** | |
| 1 | Constructor — allocate bucket array | 4 | CT 11 |
| 2 | Destructor — clean up all chains | 5 | CT 11 |
| 3 | `hash()` — custom hash function | 7 | CT 11 |
| 4 | `insert()` — add/update key-value pair | 7 | CT 11 |
| 5 | `search()` — find value by key | 8 | CT 11 |
| 6 | `remove()` — delete a key from its chain | 9 | CT 11 |
| 7 | `load_factor()` and `resize()` — grow & rehash | 14 | CT 11 |
| 8 | `print()` — display each bucket's chain | 6 | CT 11 |
| **Probing Hash Table** | | **40** | |
| 9 | Constructor — allocate slot array with status flags | 3 | CT 12 |
| 10 | `insert()` — linear probing to find open slot | 9 | CT 12 |
| 11 | `search()` — linear probing to find key | 9 | CT 12 |
| 12 | `remove()` — tombstone deletion | 14 | CT 12 |
| 13 | `resize()` — grow & rehash (skip tombstones) | 5 | CT 12 |

## Expected Output (when all TODOs are complete)

```
=== Module 5 Lab: Student Directory ===

--- Part 1: Chaining Hash Table ---
Empty directory: size=0, capacity=7

Registering students...
  insert("Alice", 1001)
  insert("Bob", 1002)
  insert("Carol", 1003)
  insert("Dave", 1004)
  insert("Eve", 1005)

Directory contents:
  [0]: ...
  [1]: ...
  ...

Looking up students...
  search("Carol") = 1003
  search("Zara") = not found

Updating student ID...
  insert("Alice", 9999)
  search("Alice") = 9999

Removing "Bob"...
  Directory size: 4

Resize test (load factor trigger)...
  Before: capacity=7, load_factor=0.57
  After more inserts: capacity=17, load_factor=0.47

--- Part 2: Probing Hash Table ---
Empty directory: size=0, capacity=7

Registering students...
  insert("Frank", 2001)
  insert("Grace", 2002)
  insert("Heidi", 2003)
  insert("Ivan", 2004)

Looking up students...
  search("Heidi") = 2003
  search("Zara") = not found

Removing "Grace" (tombstone)...
  search("Heidi") = 2003  (still works past tombstone)
  size: 3

Resize test...
  Before: capacity=7
  After more inserts: capacity=17  (tombstones cleared)

=== Lab 5 Complete ===
```

## Grading (100 points)

Run `python3 tests/scorecard.py` from the top of the repo to see this table
filled in with your own score. Each test is scored on its own, so a section can
be partly right.

| Category | Points | What is tested |
|---|---|---|
| Build | 0 | Project must compile (tests won't run otherwise) |
| **Chaining Hash Table** | **60** | |
| Constructor | 4 | Correct capacity, size 0, buckets initialized |
| Destructor | 5 | No memory leaks (tested implicitly) |
| `hash()` | 7 | Consistent hashing, good distribution across buckets |
| `insert()` | 7 | New keys added, duplicate keys update value, size updates |
| `search()` | 8 | Finds existing keys, returns indicator for missing keys |
| `remove()` | 9 | Removes from head/middle/tail of chain, no-op when missing |
| `load_factor()` / `resize()` | 14 | Correct ratio, doubles-ish capacity, all entries rehashed |
| `print()` | 6 | Correct format showing each bucket's chain |
| **Probing Hash Table** | **40** | |
| Constructor | 3 | Provided -- correct capacity, all slots marked empty |
| `insert()` | 9 | Linear probing finds open slot, updates duplicates, handles collisions |
| `search()` | 9 | Probes past collisions, stops at empty, finds existing keys |
| `remove()` | 14 | Sets tombstone flag, search still works after, insert reuses the tombstone |
| `resize()` | 5 | Grows table, rehashes occupied slots, skips tombstones |
| **Total** | **100** | |

## Tips for Success

1. **Work through the TODOs in order** — ChainingHashTable first (TODOs 1–8), then ProbingHashTable (TODOs 9–13)
2. **Review Code-Togethers 11 and 12** — each TODO names the one to revisit
3. **Draw the bucket array** — sketch what happens when two keys hash to the same index, for both chaining and probing
4. **Tombstones are the tricky part** — when probing, you must skip past `DELETED` slots during search but can reuse them during insert
5. **Use prime capacities** — the starter code uses 7 as the initial capacity; resize picks the next prime to reduce clustering
6. **Test frequently** — run the tests after each TODO to catch bugs early
