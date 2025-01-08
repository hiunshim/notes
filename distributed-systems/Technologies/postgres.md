write limit: 10K writes per second is at the limit of a well-provisioned single Postgres instance.

1. provides strong ACIRD guarantees
2. handles structured and unstructured data through JSONB support
3. built-in solutions for full-text search (GIN index) and geospatial queries (PostGIS)
4. scale reads through built-in replication
5. excellent tooling and mature ecosystem

use for
- complex relations between data
- strong consistency guarantees
- rich querying capabilities
- mix of structured and unstructured data (JSONB)
- built-int full-text and geospatial

examples
- E-commerce platforms (inventory, orders, user data)
- financial systems (transactions, accounts, audit logs)
- content management systems (posts, comments, users)
- analytics platforms (up to reasonable scale)

don't use when
1. extreme write throughput (use cassandra or redis)
2. global multi-region requirements (postgresql is single-primary) - hard to keep data consistent
	1. consider cockroachDB for global ACID compliance
	2. cassandra for eventual consistency at global scale
	3. dynamodb for managed global tables
3. simple key-value
	1. redis for in-memory performance
	2. dynamodb for managed scalability
	3. cassandra for write-heavy

Performance
1. query
	1. simple indexed lookup: 1000s/sec per core
	2. complex joins: 100s/sec
	3. full-table scans: depends if it fits in memory
2. scale limits
	1. 100M rows for a table
	2. full-text up to tens of millions
	3. complex joins 10M rows
	4. performance drops if it exceeds RAM
3. write throughput limit
	1. simple insert: ~5000/sec per core
	2. update with index modification: ~1000-2000/sec per core
	3. complex transactions (multiple tables/indexes): hundreds/sec
	4. bulk operations: tens of thousands of rows per second
