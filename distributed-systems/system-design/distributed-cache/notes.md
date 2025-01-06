**Hot keys**
create copies of hot keys. when a key gets hot, create multiple copies with different suffixes and distribute then via consistent hashing. this approach works best for read-heavy  with minimal writes.

**Summary**
data structures
1. hash table for key-value pairs
2. linked list for LRU eviction
scaling
1. asynchronous replication (default for Redis)
2. consistent hashing for sharding and routing
3. random suffixes for distributing hot key writes across nodes
4. write batching and connection polling for decreasing network latency/overhead

![[distributed-systems/system-design/distributed-cache/design.png]]
