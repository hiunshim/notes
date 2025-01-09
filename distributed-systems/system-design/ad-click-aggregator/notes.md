1. click processor server sends click event to a stream like [[apache-kafka]] or [[aws-kinesis]].
2. Stream processor like [[apache-flink]] or [[apache-spark-streaming]] to read the events from the stream and aggregate them in real-time.
3. insert the aggregated data to an OLAP databases like Redshift, Snowflake, or BigQuery.

For Kinesis and Flink, we can shard by AdvertisementID. For OLAP DB, consider sharding by "AdvertiserID" instead so that all of the data for the advertiser is on the same node.

**Hot Shards**
Update the partition key by appending a random number to the AdvertisementID. AdID: 0-N where N is the number of additional partitions for that AdID.

**How to ensure we don't lose clicks**
Streams are already persistent, but we could also use "checkpointing" where it periodically stores to S3, or even dump the raw lick events to a data lake like S3 and aggregate with Spark periodically run a reconciliation job to compare data in the OLAP DB to see if they match.

**Idempotent clicks**
Ad placement service will generate a unique impression ID for each ad. This ID will be sent to the browser and serve as an idempotency key. We can dedup the clicks based on this ID.
- impression: metric that represents the display of an ad. if the ad is shown to the same user multiple times in multiple places, each of these displays are considered a new impression.
We need to dedup before the click goes into a stream. When we get a click, we check if the impression ID exists in a cache and ignore if it does. Impression ID needs to be signed with a secret key to prevent malicious users from generating a whole bunch of unique IDs. That way, we check the signature of the ID, then see if it exists in the cache and ignore it if we cannot verify it.
We would use a distributed cache like Redis Cluster or Memcached and enable cache persistence and have replicas so the cache data is not lost when it goes down.
