# Database Indexing — First-Principles Revision Notes

## 1. What problem does indexing solve?

A database stores records. When we search for a particular record, the database needs a way to find it efficiently.

Without an index, it may examine records one by one until it finds the matches. This is a full table scan (in relational databases) or a collection scan (in MongoDB).

Indexing exists to reduce the work required to find data.

## 2. What is an index?

An index is a separate data structure that helps a database locate records using selected fields.

Think of a book's index: instead of reading every page to find a topic, you look up the topic and jump to the relevant pages.

## 3. How does it work?

Imagine a table of products:

| ID | Name     | Category    |
| -- | -------- | ----------- |
| 1  | Keyboard | Electronics |
| 2  | Chair    | Furniture   |
| 3  | Mouse    | Electronics |
| 4  | Desk     | Furniture   |

You frequently search for products by category.

Without an index, the database may check all four rows.

With an index on `Category`, the database maintains ordered search entries conceptually like these:

### Index on Category

Electronics

Record 1

Electronics

Record 3

Furniture

Record 2

Furniture

Record 4

Simplified illustration: an actual database index uses its own data structure and references to stored records.

When searching for `Electronics`, the database can locate the matching index entries and retrieve their records.

Important: The index stores search keys and record references, not necessarily full copies of the rows.

## 4. How are indexes maintained?

The database maintains the index as data changes.

- Insert: Add index entries for the new record.
- Update: Update affected index entries if indexed values change.
- Delete: Remove the deleted record's index entries.

You generally don't maintain these entries manually.

## 5. Why not index every field?

Indexes improve some reads but introduce costs.

| Benefit                   | Cost                       |
| ------------------------- | -------------------------- |
| Faster supported searches | Additional storage         |
| Faster supported sorting  | Extra work during writes   |
| Efficient range queries   | Index maintenance overhead |

Create indexes based on actual query patterns, not simply because a field exists.

## 6. What types of queries can benefit?

Depending on the index type and database, indexes can support:

- Equality: `category = 'Electronics'`
- Range: `price > 1000`
- Sorting: `ORDER BY price`
- Combined filters: `category = 'Electronics' AND price > 1000`

A suitable index depends on the query and how the database executes it.

## 7. What is the fundamental trade-off?

Without an index, the database may do more work during a read.

With an index, the database does additional work to maintain the index during writes and uses extra storage.

The goal is not to create the maximum number of indexes. It is to choose indexes that improve important queries enough to justify their costs.

## Remember these five points

1. Problem: Finding records can require examining many records.
2. Solution: An index provides a separate path to locate records.
3. Mechanism: Search keys are associated with references to records.
4. Maintenance: The database updates indexes when relevant data changes.
5. Trade-off: Faster supported queries in exchange for storage and write overhead.

One-sentence definition: A database index is a maintained data structure that helps the database locate records efficiently using selected fields.