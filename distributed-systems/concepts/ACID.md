- Atomicity: All or nothing (balance transfer should be net zero)
- Consistency: data integrity (enforces rules like "balance can't go negative")
- Isolation: determine how transactions interact with data (read uncommitted, read committed, repeatable read, serializable)
- Durability: guarantee write to disk

Isolation Levels
read committed: only sees data that was committed before the query began.
repeatable read: same query within a transaction returns the same result. postgresql's implementation even prevents phantom reads (new rows appearing that match the query).
serializable: makes transactions behave as if they were executed one after another in sequence.