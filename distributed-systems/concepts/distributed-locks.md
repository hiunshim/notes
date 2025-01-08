We use distributed locks when we need a way to lock a resource like tickets for a short time. Traditional DBs with ACID properties use transaction locks but isn't meant for long-term locking.

Distributed locks are great for locking a resource across systems for a longer time. We can use [[redis]] or [[apache-zookeeper]] to implement them.

They can be set to expire after a certain amount of time so even if our service goes down, they will expire automatically.

Examples:
1. E-Commerce Checkout System: hold high-demand items in a user's cart for 10 mins during checkout to ensure it's not sold out while holding it.
2. Ride-Sharing Matchmaking: manage assignment of drivers to riders.
3. Distributed Cron Jobs: ensures a task is executed by only one server at a time.
4. Online Auction Bidding System: can be used during final moments of bidding 

