For celebrities, read the feed when using the app.
For regular users, use async workers to insert the feed into the feed table.

Use feed cache to track which posts are older when querying for more feed using timestamp.

Use redundant post cache with read-through LRU eviction policy to cache hot posts.

![[distributed-systems/system-design/newsfeed/design.png]]
