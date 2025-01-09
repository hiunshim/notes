Partition Key - hashed to determine where the item is stored.
Sort Key (optional) - forms a composite primary key when combined with partition key. enables efficient range queries.
$$Primary key = {partition key}:{sort key}$$

Consistent hashing for partition keys.
B-trees for sort keys.
### Secondary Indexes
1. Global Secondary Index (GSI) - index with partition key and optional sort key that differs from the table's partition key. Allows us to query items based on attributes other than the partition key. (if table is partitioned with chat_id and sort key timestamp, GSI on user_id lets us query all messages sent by a user)
2. Local Secondary Index (LSI) - index with the same partition key as the table's primary key but a different sort key. Enables range queries and sorting within a partition, providing an alternative to GSIs when querying items with the same partition key. (if we use user_id as LSI, we can query all messages sent by a user in a given chat)

### DAX
Dynamo comes with built-in in-memory cache called DynamoDB Accelerator (DAX). Can cache item or query.

### When to use Dynamo
Can be justified for almost any persistence layer needs.

**Limitations**
1. Cost: expensive with high-volume workloads.
2. Complex Query patterns: queries can get complex if the system needs multiple joins or multi-table transactions.
3. Data modeling constraints: If GSIs and LSIs are frequently used, relational DB like [[postgres]] might be a better fit.
4. Vendor lock-in: AWS

DynamoDB supports ACID properties and transactions so old SQL vs. NoSQL is old news.
