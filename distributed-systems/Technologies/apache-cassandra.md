LSM tree
1. Commit log - write ahead log to ensure durability of writes
2. Memtable - in-memory sorted data structure
3. SSTable - sorting table. immutable file on disk flushed from the memtable


### When to use Cassandra
- Availability over consistency
- high write throughput
- flexible schemas or schemas with many sparse columns (wide-column)
- have clear access patterns for an application or use-case that the schema can revolve around

Limitations
- not good for system that needs strict consistency
- not good for advanced query patterns such as multi-table JOINs and adhoc aggregations