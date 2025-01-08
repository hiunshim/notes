reservation
1. Implicit Status with Status and Expiration Time: We can do a transaction and make the status as "reserved" when a user clicks on a ticket. We can add another attribute for expiration time and set it now + 10 minutes. When we read, we can implicitly tell if the that "reserved" status is expired or not. This gets messy when viewing the database, so we could simply have a cleanup job that makes it back to "available" if it is expired, and this will not affect our system behavior at all.
2. Distributed Lock with TTL: use Redis to acquire a lock using ticketID with TTL. It will automatically expire. Downside is that when Redis goes down, the user will have a bad experience since booking will fail after they fill out payment info. Still better than previous since if the clean up fails, the DB could show that all tickets are "reserved".

virtual waiting room
event cache read-through
elasticsearch
query result caching with elasticsearch and CDN
