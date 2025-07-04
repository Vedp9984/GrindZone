# Computer Networks Study Guide

## 1. OSI & TCP/IP Models

### 🔹 OSI Model (7 Layers)
- Layered architecture & purpose
- Functions of each layer (Physical → Application)
- Protocols at each layer (e.g., Ethernet, IP, TCP, HTTP)
- Encapsulation & Decapsulation
- Advantages & limitations of OSI

### 🔹 TCP/IP Model (4 Layers)
- Mapping with OSI model
- Protocols at each layer
- Why TCP/IP is preferred in practice
- Differences between OSI and TCP/IP

## 2. LAN, WAN, MAN

### 🔹 Definitions & Differences
- LAN (Local Area Network): scope, speed, use cases
- MAN (Metropolitan Area Network)
- WAN (Wide Area Network): e.g., the internet

### 🔹 Examples and Technologies
- LAN: Ethernet, Wi-Fi
- WAN: Leased lines, MPLS, satellite, Internet backbones
- MAN: Cable TV networks, local ISP backbones

### 🔹 Devices used
- Router, Switch, Hub, Modem

## 3. IP Addressing (IPv4, IPv6)

### 🔹 IPv4
- Structure: 32-bit address, dotted decimal
- Classes (A, B, C, D, E)
- Subnetting & CIDR
- Private vs Public IPs
- Loopback, Broadcast, Multicast IPs

### 🔹 IPv6
- Structure: 128-bit address
- Why IPv6? (IPv4 exhaustion)
- Address types: Global Unicast, Link-local, Multicast
- Transition mechanisms (Dual stack, Tunneling, NAT64)

### 🔹 Tools to practice:
- ipcalc, subnetting questions, real-world CIDR breakdowns

## 4. Routing Techniques (Unicast, Multicast, Broadcast)

### 🔹 Unicast
- One-to-one communication
- Default in most networks

### 🔹 Broadcast
- One-to-all in local network
- Limited to L2/L3 boundaries (e.g., ARP)

### 🔹 Multicast
- One-to-many (selective)
- IGMP (Internet Group Management Protocol)
- Applications: streaming, conferencing

### 🔹 Anycast
- One-to-nearest (in terms of routing distance)
- Used in CDN, DNS root servers

## 5. Protocols (HTTP/HTTPS, TCP/UDP, DNS)

### 🔹 TCP vs UDP
- Connection-oriented vs connectionless
- Reliability, ordering, congestion control
- Use cases: TCP (HTTP, FTP), UDP (DNS, video streaming)

### 🔹 HTTP/HTTPS
- Statelessness, methods (GET, POST, etc.)
- Status codes (200, 404, 500...)
- HTTPS and SSL/TLS basics (encryption, handshakes)

### 🔹 DNS (Domain Name System)
- Recursive vs iterative queries
- DNS hierarchy: root → TLD → authoritative
- DNS resolution flow
- Common record types: A, AAAA, MX, CNAME, NS

## 6. Data Link Layer (MAC Address, CSMA/CD)

### 🔹 MAC Address
- Unique identifier, format
- Static vs dynamic MACs

### 🔹 CSMA/CD (Carrier Sense Multiple Access with Collision Detection)
- Used in traditional Ethernet
- How collision detection works
- Why it's obsolete in full-duplex switched networks

### 🔹 Framing, Error Detection
- Frame format (Ethernet frame)
- CRC, checksum, parity

## 7. Transport Layer (TCP Congestion Control) & Brief Overview of All Layers

### 🔹 TCP Congestion Control
- Slow Start
- Congestion Avoidance (AIMD)
- Fast Retransmit & Fast Recovery
- TCP Tahoe vs Reno vs Cubic

### 🔹 Summary of All Layers:
- Physical Layer – bits, cables, voltages
- Data Link Layer – frames, MAC, switches
- Network Layer – routing, IP
- Transport Layer – TCP/UDP, reliability
- Session Layer – sessions, APIs (rarely tested)
- Presentation Layer – encryption, compression (SSL, TLS)
- Application Layer – HTTP, FTP, SMTP, DNS

## 🔄 Suggested Study Flow
- OSI vs TCP/IP → foundation
- IP Addressing (Subnetting + IPv4/IPv6)
- TCP vs UDP + Congestion Control
- Routing concepts (Unicast/Multicast)
- Data link mechanisms (CSMA/CD, MAC)
- Application Layer protocols: DNS, HTTP
- Real-world tools: ping, traceroute, dig, netstat, tcpdump, Wireshark
