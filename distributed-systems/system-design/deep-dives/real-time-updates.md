### Approaches for Client Updates
- Simple Polling: The Baseline
	- client regularly polls the server for updates
	- simple HTTP request
	- Pros:
		- simple, stateless, no special infrastructure needed
	- Cons:
		- higher latency, more bandwidth, not actually real-time
- Long Polling: The Easy Solution
	- Server holds the request open until new data is available.
	- pros
		- builds on HTTP, easy to implement, no special infrastructure is needed, stateless server-side
	- cons
		- higher latency, more HTTP overhead, resource-intensive with many clients, not suitable for frequent updates, monitoring painful because request is open for a long time, browsers limit the number of concurrent connections per domain
	- Great for infrequent updates, but need to know asap when it does update.
- Server-Sent Events (SSE): The Efficient One-Way Street
	- How it works
		1. Client establishes SSE connection
		2. Server keeps connection open
		3. Server sends messages whenever
		4. Client receives updates in real-time
	- pros
		- browser built-in, automatic reconnection, works over HTTP, more efficient that long polling, simple
	- cons
		- one-way, limited browser support, hard to debug since some proxies and networking equipment doesn't support streaming, browsers limit the number of concurrent connections
- Websocket: The Full-Duplex Champion
	- how it works
		1. client initiates
        2. connection upgrades to websocket protocol
        3. both client and server can send messages
        4. connection stays open until explicitly closed 
    - pros: read and write, lower latency, efficient or frequent messages
    - cons: complex to implement, requires special infrastructure, connections are stateful and makes load balancing and scaling more complex, need to handle reconnection
- WebRTC: The Peer-to-Peer Solution
    - perfect for video/auido calls and some data sharing like document editors.
    - Two methods to work around the security restrictions that clients have:
        1. STUN: "Session Traversal Utilities for NAT" is like "hole punching" which lets peers to repeatedly open ports and share them via the signaling server with peers.
        2. TURN: "Traversal Using Relatys around NAT" is a relay service, a way to bounce reqeust through a central server which can be routed to the peer.
    - How it works
        1. peers discover each other through signaling server
        2. exchange connection info (ICE candidates)
        3. establish direct peer connection using STUN/TURN if needed
        4. stream audio/video or send data directly
    - pros: direct peer communication, low latency, reduced server cost, native audio/video support
    - cons: complicated (> websockets), reuiqres signaling server, NAT/firewall issues, connection setup delay

        
                                                         
                                                         
                                                         
                                                         
