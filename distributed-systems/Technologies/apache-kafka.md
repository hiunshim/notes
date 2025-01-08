Kafka is a distributed streaming platform that can be used as a queue.

Kafka guarantees messages are processed in the order they were received.
1. **High Throughput**: Kafka can handle millions of messages per second, perfect for high-volume bidding periods.
    
2. **Durability**: Kafka persists all messages to disk and can be configured for replication, ensuring we never lose a bid.
    
3. **Partitioning**: We can partition our queue by auctionId, ensuring that all bids for the same auction are processed in order while allowing parallel processing of bids for different auctions.

Not good for millions of topics. Use Redis Pub/Sub for handling chat servers ([[distributed-systems/system-design/chat-app/notes|notes]]).

Most queues are FIFO, but Kafka allows more complex ordering guarantees like specified priority or time.

Although Kafka is a popular pub/sub system that is highly scalable and fault-tolerant, it has a hard time adapting to scenarios where dynamic subscription and unsubscription based on user interactions are needed.