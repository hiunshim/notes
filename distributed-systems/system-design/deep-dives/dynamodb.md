### Secondary Indexes
DynamoDB supports two types:
1. **Global Secondary Index (GSI)** - an index with a partition and optional sort key that differs from the table's partition key. GSIs allow you to query items based on attributes other than the table's partition key.
2. **Local Secondary Index (LSI)** - an index with the same partition key as the table's primary key but a different sort key. LSIs enable range queries and sorting within a partition, providing an alternative to GSIs when querying items with the same partition key.