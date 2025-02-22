## READ phenomena

### Dirty read

- A transaction reads data written by other concurrent uncommited transaction.

### Non-repeatable read

- A transaction reads the same row twice and sees different value because it has been modified by other committed transaction.

### Phantom read

- A transacttion re-executes a query to find rows that satisfy a condition and sees a different set of rows, due to changes by other committed transaction.

### Serialization anomaly

- The result of a group of concurrent committed transactions is impossible to achieve if we try to run them sequentially in any order without overlapping.

## 4 Standard Isolation Levels

### Level 1
- READ UNCOMMITTED: Can see data written by uncommitted transaction

### Level 2
- READ COMMITTED: Only see data written by committed transaction

### Level 3
- REPEATABLE READ: Same read query always returns same result

### Level 4
- SERIALIZABLE: Can achieve same result if execute transactions serially in some order instead of concurrently.