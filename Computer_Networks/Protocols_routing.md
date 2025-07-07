## 4. Routing Techniques (Unicast, Multicast, Broadcast)

### 🔹 Unicast
- **Definition**: One-to-one communication where a packet is sent from a single source to a specific destination
- **Characteristics**:
  - Most common form of data transmission
  - Uses specific IP addresses for source and destination
  - Efficient for point-to-point communications
- **Examples**: Web browsing, email, file transfers
- **Default** in most networks and Internet communications

### 🔹 Broadcast
- **Definition**: One-to-all communication within a network segment
- **Characteristics**:
  - Packets sent to all devices on the local network
  - Uses special broadcast addresses (e.g., 255.255.255.255)
  - Limited by routers (doesn't cross Layer 3 boundaries)
- **Use cases**:
  - Address Resolution Protocol (ARP)
  - DHCP discovery
  - Network announcements
- **Limitations**: Can create unnecessary traffic and processing overhead

### 🔹 Multicast
- **Definition**: One-to-many selective communication
- **Characteristics**:
  - Sends packets to a group of interested receivers
  - Uses Class D IP addresses (224.0.0.0 - 239.255.255.255)
  - More efficient than multiple unicasts
- **Protocols**:
  - IGMP (Internet Group Management Protocol): Manages multicast group memberships
  - PIM (Protocol Independent Multicast): Handles multicast routing
- **Applications**:
  - Video/audio streaming
  - Video conferencing
  - Financial data distribution
  - IPTV

### 🔹 Anycast
- **Definition**: One-to-nearest communication based on routing metrics
- **Characteristics**:
  - Multiple destinations share the same IP address
  - Packets routed to "closest" instance (by routing algorithm)
  - Provides load balancing and fault tolerance
- **Applications**:
  - Content Delivery Networks (CDNs)
  - DNS root servers (13 logical servers, many physical instances)
  - DDoS mitigation services

## 5. Protocols (HTTP/HTTPS, TCP/UDP, DNS)

### 🔹 TCP vs UDP

#### TCP (Transmission Control Protocol)
- **Connection-oriented**: Establishes session before data transfer
- **Reliability features**:
  - Acknowledgments and retransmissions
  - In-order delivery of packets
  - Flow control (sliding window)
  - Congestion control (slow start, congestion avoidance)
- **Overhead**: Higher due to connection management and reliability features
- **Use cases**: Web browsing (HTTP), email (SMTP), file transfers (FTP)

#### UDP (User Datagram Protocol)
- **Connectionless**: No session establishment
- **Characteristics**:
  - No guaranteed delivery
  - No packet ordering
  - No flow or congestion control
  - Lower overhead, faster transmission
- **Use cases**: DNS lookups, video streaming, VoIP, online gaming
- **Benefits**: Lower latency, simpler implementation

### 🔹 HTTP/HTTPS

#### HTTP (Hypertext Transfer Protocol)
- **Stateless protocol**: Each request-response cycle is independent
- **Methods**:
  - GET: Retrieve data
  - POST: Submit data
  - PUT: Update resources
  - DELETE: Remove resources
  - Others: HEAD, OPTIONS, PATCH, etc.
- **Status codes**:
  - 1xx: Informational
  - 2xx: Success (200 OK)
  - 3xx: Redirection
  - 4xx: Client errors (404 Not Found)
  - 5xx: Server errors (500 Internal Server Error)

#### HTTPS (HTTP Secure)
- HTTP over TLS/SSL encryption
- **Security features**:
  - Data encryption
  - Server authentication
  - Data integrity
- **TLS Handshake process**:
  1. Client hello (cipher suites, random number)
  2. Server hello (certificate, cipher selection)
  3. Key exchange
  4. Secure symmetric communication

### 🔹 DNS (Domain Name System)
- **Purpose**: Translates human-readable domain names to IP addresses
- **Query types**:
  - **Recursive**: Resolver takes full responsibility for finding answer
  - **Iterative**: Resolver follows referrals to find answer
- **DNS hierarchy**:
  - Root servers (.)
  - Top-Level Domain servers (com, org, net)
  - Authoritative name servers (example.com)
  - Local DNS resolvers
- **Resolution flow**:
  1. Client queries local resolver
  2. Resolver queries root servers
  3. Root refers to TLD servers
  4. TLD refers to authoritative servers
  5. Authoritative server provides answer
- **Common record types**:
  - **A**: Maps hostname to IPv4 address
  - **AAAA**: Maps hostname to IPv6 address
  - **MX**: Mail exchange records
  - **CNAME**: Canonical name (alias)
  - **NS**: Name server records
  - **PTR**: Reverse DNS lookup
  - **SOA**: Start of Authority
  - **TXT**: Text records (SPF, DKIM)
- **Caching**: Temporary storage of DNS records to improve performance
