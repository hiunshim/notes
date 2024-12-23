1. **DNS Resolution**: DNS converts domain name to IP address (google.com -> 80.80.80.80)
2. **TCP Handshake**: client initiates TCP connection with the server using a three-way handshake.
 - **SYN**: client sends SYN (synchromize) packet to the server to request a connection.
 - **SYN-ACK**: server responds with a SYN-ACK (synchromize-acknowledge) packet to acknowledge the request.
 - **ACK**: client sends ACK (acknowledge) packet to establish the connection.
3. **HTTP Request**: After TCP connection, client sends HTTP GET to request web page. [^1]
4. **Server Processing**: server processes request, retrieves web page, and prepares an HTTP response. [^2]
5. **HTTP Response**: server sends HTTP response back to the client with web page content.
6. **TCP Teardown**: after data transfer, client and server close TCP connection using a four-way handshake.
 - **FIN**: client sends FIN (finish) packet to the server to terminate the connection.
 - **ACK**: server acknowledges the FIN packet with an ACK.
 - **FIN**: servers sends FIN packet to the client to terminate terminate conneciton.
 - **ACK**: client acknowledges the server's FIN packet with an ACK.

**Key Points**
 - Each round trip between client and server adds latency.
 - TCP connection itself represents state that both client and state much maintain. Connection setup process must be repeated for every request.

Questions:
[^1]: is this how Flask template abstracts?
[^2]: how does single-page frontend work then?
