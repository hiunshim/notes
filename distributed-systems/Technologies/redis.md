### Overview
Data structures:
- Strings
- Hashes (Objects)
- Lists
- Sets
- Sorted Sets (Priority Queues)
- Bloom Filters
- Geospatial Indexes
- Time Series

Supports communication patterns like [Pub/Sub](https://redis.io/docs/latest/develop/interact/pubsub/) and Streams.

**Choosing how to structure your keys is how you scale Redis.**

### Distributed lock
We can use it as a distributed lock when we need to maintain consistency during updates (ticketmaster) or when we need to make sure multiple people aren't performing an action at the same time (uber).
If the DB can provide consistency, there's no need to rely on a distributed lock since it introduces extra complexity.
When acquiring the lock, we INCR and if the response is 1, we proceed since only we own the lock. If the response is >1, we wait and try again later since it means it's locked.

### Leaderboards
Sorted sets maintain ordered data which is queries in log time.
When searching for a post, we can use sorted sets to maintain top liked posts for a given keyword and periodically remove low-ranked posts.

### Rate limiting
When a request comes in, we increment (INCR) the key. If the response is greater than N, we wait. If it's less than N, we proceed. We expire (EXPIRE) our key so that after time W, the value is reset.

### Proximity search
Supports geospatial indexes GEOADD and GEORADIUS.
```bash
GEOADD key longitude latitude member # Adds "member" to the index at key "key"
GEORADIUS key longitude latitude radius # Searches the index at key "key" at specified position and radius
```
Search is O(N+log(M)) where N=# of elements in the radius, M=# of members in the index.

# Event sourcing
Redis' streams are append-only logs like Kafka topics. Add items to the queue and have a consumer group attached to the stream for the workers to claim and run.

### Hot key issue
- add in-memory cache in our clients
- store the same data in multiple keys and randomize the requests
- add read replica instances and dynamically scale with load