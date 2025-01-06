- functional
	- post an item for auction with starting price and end date
	- bid on an item. bid accepted if higher than current highest bid.
	- view auction, including the highest bid
- non-functional
	- strong consistency for bids - everyone needs to see the same highest bid
	- fault-tolerant and durable since we can't drop any bids
	- display the highest bid in real-time so users know what they're bidding against
	- scale to support 10M concurrent auctions
- entities
	- auction: include information like starting price, end date, and item
	- item: name, description, image
	- bid: amount, user placing the bid, auction being bid on
	- user: represents user who starts an auction or bids on an auction
- api
	- post
		- // POST /auctions -> Auction & Item
		```json
		{
			item: Item,
			startDate: Date,
			endDate: Date,
			startingPrice: number
		}
		```
		- // POST /auctions/:auctionId/bids -> Bid
	- get
		- GET /auctions/:auctionId -> Auction & Item
- high-level
	- post an item for auction with a start price and end date
		- client - api gateway - auction service - db
		- auction (id, itemid, starttime, endtime, creatorid, startingprice)
		- item (id, name, description, imageurl)
	- bid on an item where highest bid is accepted **(this is the most interesting part. focus here)**
		- introduce a new bidding service: client - api gateway - (auction service, bidding service) - db
			- validate bids (is higher than current?)
			- update auction with highest bid
			- store bid history in DB
			- notify relevant parties of bid updates
		- why separate it into its own service?
			- independent scaling: bidding service higher volume than auction. 100X more bids than auctions.
            - isolaton of concerns: bidding logic complicated - race conditions, validations, real-time updates.
            - performance optimization: high-throuput write operation for bid service, auction service for read-heavy workloads.
        - bidding service queries highest bid from the DB. It stores bid with status "accepted" or "rejected" and returns the status to the client. DB bids are stored in a new bids table, linked by auctionID.
            - Why not just store "maxBidPrice"?
                - This violates data integrity: avoid destroying historical data. We permanently lose information. Cannot audit, investigate disputes, analyze patterns of behavior. User will complain their bid should've won. We cannot dispute without proof.
    - view auction, including highest bid
        - two cases: learn about the item to see if they want to buy, or want to place a bid so they need to know the highest bid.
            - First is read-only. Second requires real-time consistency
        - after GET request to /auctions/:auctionId, poll every few seconds for the highest bid
- deep dives
    1. how to ensure strong consistency for bids? (synchronization is essential)
        - bad - row locking with bids query: we want to avoid locking rows. this becomes a bottleneck and increases latency.
        - ok - cache max bid externally: now the problem evolved from consistency within the database to a consistency problem between the DB and cache. redis is single threaded ande supports atomic operations to read and write atomically, but we still need to handle strong consistency with the DB.
            - few options for this:
                - distributed transactions (two-phase commit) to ensure atomicity across redis and db, but this adds complexity and performance overhead
                - redis as the source of truth and write to db asynchronously
                - optimistic concurrency with retry logic to update the cache atomically first, then write the db. if db write failes, roll back the cache update
            - in an interview if distributed transaction is needed, consider if I can work around it to avoid using it.
        - best - cache max bid in the database: we can use Auction table as our cache. 
            1. Lock the auction row for the given auction (just one row)
            2. read max bid from Auction table
            3. write bid to the db with status "accepted" or "rejected"
            4. update max bid in the Auction table if the new bid is higher
            5. commit the transaction
            This only locks a single row and for a short time. To avoid pessimistic locking altogther, use **optimistic concurrency control (OCC)**.
            1. read the auction row and get max bid (referred to as "version" with OCC)
            2. validate if new bid higher than current
            3. try to update row, but only if max bid hasn't changed in the meanwhile
            4. if update succeeds, write the bid record. if fail, retry from step 1.
            This avoids locks entirely while maintaining consistency. will sometimes cause retries when conflicts occur. [[concurrency-control]]
    2. how to ensure system is fault-tolerant and durable?
		Best way is to introduce MQ to the system. benefits:
		- durable storage: write to MQ when bid comes in.
		- buffering against load spikes: queue acts as a buffer, storing the bid temporarily until bid service can process  the load
		- guaranteed ordering: MQ like [[apache-kafka]] guarantees that messages are processed in order they're received.
		flow:
		1. user submits bid through API, then api gateway routes to the producer and producer writes to kafka
		2. kafka acks the write and we tell the user the bid was received
		3. bid service consumes the message and if the bid is valid, write to the db
		4. if bid service fails, bid remains in the kafka to be retried
    3. how to ensure system displays the current highest bid in real-time? [[real-time-updates]]
		- polling is too slow since it's every few seconds. it's also inefficient since every client is poling.
		- ok - long polling for max bid:
			- thundering herd problem [^1]
			- next request latency
			- scaling challenging because each server has own connection
		- best - Server-Sent Events (SSE):
			- need multiple servers to handle all of the SSE connections and coordination become harder.
    1. how to ensure system scales to 10M concurrent auctions?
		- Message queue: partition based on auction ID
		- bid service: stateless for both bid and auction services. horizontally scale and enable auto-scaling.
		- DB: shard by auction ID. all read/write are on the same shard
		- SSE: use Pub/Sub [[real-time-updates]]
			- when bid service receives a bid from the MQ, publish to pub/sub.
			- all other instances of the bid service are subscribed to the same pub/sub channel and receive the message.
			- when the subscriber received the messages and the one of their client connection is subscribed to it, send that connection the new bid data.
- Additional topics with extra time
	1. dynamic auction end times: end hour after highest bid
	2. purchasing: if highest bidder doesn't pay in N time, move to the next bidder
	3. bid history: how to view bid history for the auction real-time

![[design.png]]

Footnotes
[^1]: Thundering herd