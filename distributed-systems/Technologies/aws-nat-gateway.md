**Network Address Translation**
Makes requests to the external services and appear as if the request originates from a small set of IPs ([elastic IPs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html)).

characteristics
- operates at the network layer (layer 3 [[osi-model]])
- does not process HTTP/HTTPS headers of application-layer data
- stateless - only performs IP address and port translation