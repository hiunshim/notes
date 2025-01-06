- functional
	- post comments on live video feed
	- view comments on live
	- view old comments
- non-functional
	- millions of users
	- availability > consistency
	- low latency comment broadcast (~200ms)
	- reliability

**Pagination**
Offset vs. Cursor
- Offset: `GET /comments/:liveVideoId?offset=0&pagesize=10`
	- challenging in fast-moving, high-volume comment feed
	- database must count through all rows for each query
	- unstable when comment is added or deleted while the user is scrolling
- Cursor: `GET /comments/:liveVideoId?offset=0&pagesize=10`
	- db doesn't need to count
	- stable when comments are added are deleted
	- still requires db query for each new page of results


