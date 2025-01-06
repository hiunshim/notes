Messaging pattern where publishers categorize messages into classes that are received by subscribers. In a typical non-pub-sub messaging pattern, a publisher sends a message directly to subscribers.

Compared with synchronous messaging patterns like RPC and point-to-point messaging patterns, pub/sub provides the highest level of decoupling.

**Filtering**: The process of selecting messages for reception and processing.
1. Topic-based: messages are published to "topics" and subscribers in a topic-based system will receive messages published to the topics.
2. Content-based: messages are only delivered to a subscriber if the attributes or content of those messages matches constraints defined by the subscriber.

**Loose coupling**: publishers do not need to know the existence of the subscribers.

**Limitations**
Redundant subscribers can help assure message delivery, but it still does not guarantee deliver. Tighter coupling outside of the pub/sub system must be enforced to assure delivery.