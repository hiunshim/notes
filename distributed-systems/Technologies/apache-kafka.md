Kafka guarantees messages are processed in the order they were received.
1. **High Throughput**: Kafka can handle millions of messages per second, perfect for high-volume bidding periods.
    
2. **Durability**: Kafka persists all messages to disk and can be configured for replication, ensuring we never lose a bid.
    
3. **Partitioning**: We can partition our queue by auctionId, ensuring that all bids for the same auction are processed in order while allowing parallel processing of bids for different auctions.

Not good for millions of topics. Use Redis Pub/Sub for handling chat servers ([[distributed-systems/system-design/chat-app/notes|notes]]).
