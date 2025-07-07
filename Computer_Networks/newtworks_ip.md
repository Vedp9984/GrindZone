
### 🔹 Network Types - Definitions & Differences

- **LAN (Local Area Network)**
  - **Scope**: Limited geographical area (single building, office, or campus)
  - **Speed**: High bandwidth (100Mbps-10Gbps)
  - **Latency**: Low (1-10ms)
  - **Use cases**: Home networks, office connections, school computer labs
  - **Control**: Privately owned and managed

- **MAN (Metropolitan Area Network)**
  - **Scope**: City-wide coverage (connects multiple LANs)
  - **Speed**: Medium to high bandwidth (10-100+ Mbps)
  - **Latency**: Medium (10-50ms)
  - **Use cases**: University campuses, city government networks
  - **Control**: Usually operated by larger organizations or consortiums

- **WAN (Wide Area Network)**
  - **Scope**: Large geographical areas (countries, continents, global)
  - **Speed**: Typically lower bandwidth than LANs (1-100+ Mbps)
  - **Latency**: Higher (50-300+ ms)
  - **Use cases**: Global business communications, internet connectivity
  - **Control**: Often relies on public telecommunications infrastructure

### 🔹 Examples and Technologies

- **LAN Technologies**
  - **Ethernet**: IEEE 802.3 standard, speeds from 10Mbps to 400Gbps
  - **Wi-Fi**: IEEE 802.11 standards (a/b/g/n/ac/ax), wireless connectivity
  - **Token Ring**: Older technology with deterministic access method
  - **Characteristics**: High reliability, low maintenance cost, easy troubleshooting

- **MAN Technologies**
  - **Cable TV networks**: Shared infrastructure using cable modems
  - **Local ISP backbones**: Fiber optic rings connecting neighborhoods
  - **WiMAX**: IEEE 802.16 for wireless metropolitan coverage
  - **SONET/SDH**: Optical carrier networks for telecommunications

- **WAN Technologies**
  - **Leased lines**: Dedicated point-to-point connections (T1/E1, T3/E3)
  - **MPLS (Multiprotocol Label Switching)**: Traffic routing technique using labels
  - **Satellite**: Global coverage, higher latency
  - **Internet backbones**: Core routers and high-capacity links between ISPs
  - **Cellular networks**: 4G/5G infrastructure for mobile data

### 🔹 Network Devices

- **Router**
  - **Function**: Connects different networks, forwards packets based on IP addresses
  - **OSI Layer**: Network layer (Layer 3)
  - **Key features**: Routing tables, NAT, firewall capabilities, QoS

- **Switch**
  - **Function**: Connects devices within the same network, forwards frames based on MAC addresses
  - **OSI Layer**: Data Link layer (Layer 2)
  - **Types**: Unmanaged, managed, PoE, multilayer
  - **Features**: VLAN support, STP, link aggregation

- **Hub**
  - **Function**: Simple connection point for devices, broadcasts all data to all ports
  - **OSI Layer**: Physical layer (Layer 1)
  - **Limitations**: Creates collision domains, low security, largely obsolete

- **Modem**
  - **Function**: Modulates/demodulates signals between digital and analog
  - **Types**: DSL, cable, fiber, dial-up
  - **Role**: Connects LAN to WAN (typically the internet)

## 3. IP Addressing (IPv4, IPv6)

### 🔹 IPv4

- **Structure**
  - 32-bit address (4 bytes)
  - Dotted decimal notation (e.g., 192.168.1.1)
  - 4 octets separated by periods
  - Each octet range: 0-255

- **Address Classes**
  - **Class A**: 0.0.0.0 to 127.255.255.255 (Large networks, /8 prefix)
  - **Class B**: 128.0.0.0 to 191.255.255.255 (Medium networks, /16 prefix)
  - **Class C**: 192.0.0.0 to 223.255.255.255 (Small networks, /24 prefix)
  - **Class D**: 224.0.0.0 to 239.255.255.255 (Multicast)
  - **Class E**: 240.0.0.0 to 255.255.255.255 (Experimental/Reserved)

- **Subnetting & CIDR**
  - **Subnet mask**: Divides IP into network and host portions (e.g., 255.255.255.0)
  - **CIDR notation**: Address/prefix length (e.g., 192.168.1.0/24)
  - **Benefits**: Efficient IP allocation, reduced routing table size, improved security
  - **Calculation**: 2^(32-prefix) = number of addresses

- **Special IPs**
  - **Private ranges**:
    - 10.0.0.0/8 (Class A)
    - 172.16.0.0/12 (Class B)
    - 192.168.0.0/16 (Class C)
  - **Loopback**: 127.0.0.0/8 (typically 127.0.0.1)
  - **Broadcast**: xxx.xxx.xxx.255 (for /24 network)
  - **Multicast**: 224.0.0.0 to 239.255.255.255
  - **APIPA**: 169.254.0.0/16 (Automatic Private IP Addressing)

### 🔹 IPv6

- **Structure**
  - 128-bit address (16 bytes)
  - Hexadecimal notation with colons (e.g., 2001:0db8:85a3:0000:0000:8a2e:0370:7334)
  - 8 groups of 4 hexadecimal digits
  - Simplification rules:
    - Leading zeros in a group can be omitted
    - Consecutive zero groups can be replaced with :: (only once per address)

- **Why IPv6?**
  - IPv4 address exhaustion (only ~4.3 billion addresses)
  - Eliminates need for NAT in many cases
  - Better security (IPsec built-in)
  - Improved routing efficiency
  - Auto-configuration capabilities

- **Address Types**
  - **Global Unicast**: Public internet addresses (starts with 2000::/3)
  - **Link-local**: Automatic addresses for local communication (fe80::/10)
  - **Unique Local**: Private network addresses (fd00::/8)
  - **Multicast**: One-to-many delivery (ff00::/8)
  - **Anycast**: Routing to nearest member of a group

- **Transition Mechanisms**
  - **Dual Stack**: Running both IPv4 and IPv6 simultaneously
  - **Tunneling**: Encapsulating IPv6 packets within IPv4 (6to4, Teredo)
  - **NAT64/DNS64**: Translation between IPv6-only and IPv4-only networks
  - **464XLAT**: Translation for IPv4-only applications on IPv6-only networks

### 🔹 Tools to Practice

- **ipcalc**: Command-line IP subnet calculator
- **Subnetting exercises**: Calculate network/broadcast addresses, CIDR notation
- **Real-world CIDR breakdowns**: Analyze enterprise network designs
- **GNS3/Packet Tracer**: Network simulation for practical experience
- **Online calculators**: IPv4/IPv6 subnet calculators (e.g., sipcalc, subnetcalc)
- **Wireshark**: Packet capture analysis to understand IP addressing in action

