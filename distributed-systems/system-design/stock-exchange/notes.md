High-level design
- SSE for price updates
- AWS NAT Gateway for order dispatch
- abstracted trade processor
![[high-level-design.png]]

### Deep Dives
**scaling live price updates** - how do we route symbol price updates to the symbol service servers connected to users who care about those symbol updates?
- Pub/Sub: users subscribe to symbol service and the service ensures it is subscribed to symbol price updates via Redis for all the symbols the users care about.
- Flow
	1. user subscribes via symbol service server. the server tracks *Symbol -> Set\<userID>* mapping so it adds entry for each symbol the user is subscribed to. (maintained in memory and user makes a post request again upon disconnecting)
	2. symbol service manages subscripts to Redis pub/sub.
	3. symbol price processor publishes price update to the channel and symbol service server consumes the event and fans out the price update to all users.
	4. if a user disconnects of unsubscribes, the symbol service server removes them from the Set\<userID> mapping. If it has no subscribers, it can unsubscribe from the Redis channel for that symbol.
	
**how does the system track of order updates**
Persistent key-value storage like [RocksDB](https://rocksdb.org/) to make externalOrderId to userId mapping.
1. order services created orderId to userId mapping after an order is submitted
2. trade processor looks up the userId after the trade processes with the orderId
3. trade processor users the userId to update the order in the order db

**How does the system manage order consistency**
1. tell client if the write to order db failed
2. mark the order status as failed if the order via exchange failed
3. if processing the order failed after exchange submission, have a clean-up job that goes through the pending orders to see if the order went through and update the status accordingly