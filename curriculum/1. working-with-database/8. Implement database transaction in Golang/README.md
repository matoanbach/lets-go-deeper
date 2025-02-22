## ACID Property for transaction

1. Atomicity (A)
- Either all operations successfully or the transaction fails and the db is unchanged
2. Consistency (C)
- The db state must be valid after the transaction. All constraints must be satisfied
3. Isolation (I)
- Concurrent transactions must not affect each other
4. Durability (D)
- Data written by a successful transaction must be recorded in persitent storage