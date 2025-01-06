functional
- post item for auction (start price, end date)
- bid on an item, accept if highest
- view auction and highest bid
- 10 mil concurrent auctions

non-functional
- strong bid consistency
- fault tolerant. can't drop bids
- real-time max bid display
- scale to 10 mil auctions

entities
1. user
2. auction
3. item
4. bid

apis
POST /auctions -> Auction
{
	item,
	startPrice,
	startTime,
	endTime
}

POST /auctions/:auctionId/bids -> Bid
{
	price
}

GET /auctions/:auctionId -> Auction & Item


high-level
- maintain current highest bid in the auction table so we don't query all bids every few seconds
- consider sse or websockets as alternative to polling (mention it during the design to show awareness)

 deep-dive
 - optimistic concurrency control - bid service starts a transaction, reads current highest bid from auction table, inserts new bid and update current highest bid atomically. if the current highest bid has changed since reading, the transaction fails and we retry. does NOT lock

|**Concurrency Control**|**Scenario**|**Mechanism**|**Example**|
|---|---|---|---|
|**Optimistic Concurrency**|High scalability with retries|Validate inventory availability at checkout; fail transaction if item is already sold.|E-commerce flash sales (Amazon Lightning Deals).|
|**Pessimistic Concurrency**|High consistency with locked inventory|Lock inventory temporarily during reservation or checkout.|Ticketing systems (concerts, events, airline bookings).|
|**Queue-Based System**|Fairness with controlled user load|Users placed in a queue; items reserved sequentially based on order in queue.|Nike SNKRS app, online lotteries for sneakers.|
|**Lottery/Randomized**|Avoids race conditions, suitable for limited stock|Pre-register users; randomly select a subset to purchase; reserve inventory only for winners.|Nvidia GPUs, PS5 pre-orders, or high-end sneakers.|

real-time updates
- pub/sub and SSE

scale DB
- shard DB using auction ID
- consistent hashing to distribute data evenly
- shouldn't need query across shards since bids and auctions are on the same shard
- hot auctions unlikely because more bids means more people will be priced out and there will be less bids later on