OSI model is a representation of network abstractions layers. There are 7 layers:
1. physical
2. data link
3. network
4. transport
5. session
6. presentation
7. application

**Network Layer (3)**
Routing and addressing. Responsible for breaking data in to packets, handling packet forwarding, and providing best-effort delivery to any IP address. No guarantees for delivery.

**Transport Layer (4)**
Communication services. TCP and UDP.
- TCP
    - "Connection-oriented" protocol. Before sending data, connection must be established.
    - Ensures data is delivered correctly in order.
    - Connection takes time to establish, resources to maintain, and badwidth to use.
- UDP
    - "Connectionless" protocol. Doesn't need pror setup to send data to any IP address.
    - Doesn't ensure data delivered correctly in order.
    - Faster than TCP and uses less resources.

**Application Layer
Applcation protocols. DNS, HTTP, Websockets, WebRTC. Builds on top of TCP to provide a layer of abstraction for different types of data typically associated with web apps.
