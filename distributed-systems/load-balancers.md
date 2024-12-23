### L3 vs L4
[[networks]]
Two main types of load balancers:
1. layer 4
2. layer 7

**Layer 4**
Operates at the transport layer (TCP/UDP). Makes routing decisions based on IP addresses and ports without looking at the actual content. As if backend server is randomly selected and assumed that TCP connection is made directly between client and server. AWS's Network Load Balance (NLB) is an example.
- Maintains persistent TCP connection between client and server.
- Fast and efficient due to minimal packet inspection.
- Cannot make routing decisions based on application data.
- Typically used when raw performance is the priority.

**Layer 7**
Examines actual content of the request and makes more intelligent routing decisions. AWS's Application Load Balancer (ALB) is an example.
- Terminate incoming connections and create new ones to servers.
- Route based on request content (URL, headers, cookies, etc.)
- More CPU-intensive due to packet inspection.
- Provide more flexibility and features.
- Better suited for HTTP-based traffic.