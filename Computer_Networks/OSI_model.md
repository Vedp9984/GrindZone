## 1. OSI & TCP/IP Models

### 🔹 OSI Model (7 Layers)

The Open Systems Interconnection (OSI) model is a conceptual framework that standardizes the functions of a communication system into seven distinct layers.

#### Layered Architecture & Purpose
- Divides network communication into manageable, modular layers
- Each layer handles specific functions and serves the layer above it
- Enables standardization, easier troubleshooting, and modular development

#### Functions of Each Layer

1. **Physical Layer (Layer 1)**
  - Transmits raw bit stream over physical medium
  - Deals with electrical, mechanical, and timing specifications
  - Example: When you plug an Ethernet cable, the electrical signals traveling through it operate at this layer

2. **Data Link Layer (Layer 2)**
  - Provides node-to-node data transfer
  - Error detection and correction
  - MAC addressing and frame management
  - Example: Ethernet frames containing MAC addresses to ensure data reaches the correct device on a local network

3. **Network Layer (Layer 3)**
  - Handles routing between different networks
  - Logical addressing (IP addresses)
  - Packet forwarding
  - Example: IP routing decisions that determine how packets travel from a device in one network to another network

4. **Transport Layer (Layer 4)**
  - End-to-end communication control
  - Connection-oriented (TCP) or connectionless (UDP) services
  - Flow control and error recovery
  - Example: TCP ensuring all packets of a file transfer arrive in order and requesting retransmission if any are lost

5. **Session Layer (Layer 5)**
  - Establishes, manages, and terminates sessions
  - Dialog control and synchronization
  - Example: NetBIOS, establishing a session for file sharing between computers

6. **Presentation Layer (Layer 6)**
  - Data translation and encryption
  - Character code conversion, data compression
  - Example: Converting an EBCDIC-encoded text file to ASCII, or encrypting data with SSL/TLS

7. **Application Layer (Layer 7)**
  - User interface and application-specific services
  - Provides network services to applications
  - Example: HTTP requests when browsing websites, SMTP for sending emails

#### Key Protocols at Each Layer

- **Physical**: Ethernet, USB, Bluetooth, DSL, ISDN
- **Data Link**: Ethernet, PPP, HDLC, Frame Relay
- **Network**: IP, ICMP, IGMP, IPsec
- **Transport**: TCP, UDP, SCTP
- **Session**: NetBIOS, RPC, PPTP
- **Presentation**: SSL/TLS, JPEG, MPEG, ASCII, EBCDIC
- **Application**: HTTP, SMTP, FTP, DNS, DHCP, SSH

#### Encapsulation & Decapsulation

**Encapsulation (Sender)**:
1. Data starts at Application layer
2. Each layer adds its header (sometimes trailer) to the data
3. Example: HTTP data → TCP header added → IP header added → Ethernet header and trailer added

**Decapsulation (Receiver)**:
1. Physical layer receives bits
2. Each layer strips off its corresponding header/trailer
3. Processed data is passed up to the next layer
4. Example: Ethernet frame → IP packet extracted → TCP segment extracted → HTTP data extracted

#### Advantages & Limitations

**Advantages**:
- Standardized approach to network functionality
- Modular design allows independent layer development
- Excellent teaching tool for understanding networking concepts
- Simplifies troubleshooting by isolating network functions

**Limitations**:
- Strict layering can be inefficient
- Some functions overlap between layers
- Not fully implemented in real-world networking
- Developed before widespread internet adoption

### 🔹 TCP/IP Model (4 Layers)

The TCP/IP model is the networking model used in today's internet and most modern networks.

#### Mapping with OSI Model

1. **Network Access/Link Layer** ≈ OSI Physical + Data Link
2. **Internet Layer** ≈ OSI Network
3. **Transport Layer** ≈ OSI Transport
4. **Application Layer** ≈ OSI Session + Presentation + Application

#### Protocols at Each Layer

1. **Network Access/Link Layer**
  - Ethernet, Wi-Fi (802.11), PPP, ARP
  - Example: Ethernet frames that contain MAC addresses to route data on a local network

2. **Internet Layer**
  - IP (IPv4, IPv6), ICMP, IGMP
  - Example: IP packets routing data across different networks on the internet

3. **Transport Layer**
  - TCP, UDP
  - Example: TCP ensuring a web page loads completely by tracking all packets

4. **Application Layer**
  - HTTP, HTTPS, FTP, SMTP, DNS, SSH, Telnet
  - Example: HTTP request/response when accessing a website

#### Why TCP/IP is Preferred in Practice

- Developed specifically for the internet
- Simpler model with fewer layers
- Protocols were implemented before the model (practical development)
- Widely adopted and proven effective in real-world applications
- Flexible implementation allows for efficiency
- Supports both connection-oriented and connectionless communication

#### Differences Between OSI and TCP/IP

| Aspect | OSI Model | TCP/IP Model |
|--------|-----------|--------------|
| **Number of Layers** | 7 layers | 4 layers |
| **Development** | Theoretical, then protocols | Protocols first, then model |
| **Flexibility** | Strict boundaries | Flexible boundaries |
| **Usage** | Reference model, education | Practical implementation |
| **Approach** | Generic model for all networks | Specifically designed for internet |
| **Protocol Independence** | Separate protocols from model | Built around specific protocols |
| **Adoption** | Limited practical implementation | Universal implementation |

**Example in Action**: When you access a website like GitHub:
1. Your browser creates HTTP request data (Application layer)
2. TCP adds reliability with sequence numbers (Transport layer)
3. IP adds addressing to route across networks (Internet layer)
4. Ethernet frames the data for transmission (Network Access layer)
5. The request traverses the internet, reaching GitHub's servers
6. The process reverses, with each layer extracting what it needs
7. GitHub's server processes the request and sends a response
