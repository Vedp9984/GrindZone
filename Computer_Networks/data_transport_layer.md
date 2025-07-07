
## 6. Data Link Layer (MAC Address, CSMA/CD)

### 🔹 MAC Address
- **Definition**: A Media Access Control (MAC) address is a unique 48-bit hardware identifier assigned to network interfaces.
- **Format**: Expressed as six pairs of hexadecimal digits (e.g., `00:1A:2B:3C:4D:5E`).
- **Static vs Dynamic MACs**:
  - **Static**: Permanently assigned by manufacturer (burned into the NIC)
  - **Dynamic**: Can be changed through software (e.g., MAC spoofing)
- **Example**: When your laptop connects to a Wi-Fi network, the router identifies your device by its MAC address before assigning an IP address.

### 🔹 CSMA/CD (Carrier Sense Multiple Access with Collision Detection)
- **Purpose**: Allows multiple devices to share a common communication channel.
- **How it works**:
  1. **Carrier Sense**: Device checks if the channel is idle before transmitting
  2. **Multiple Access**: All devices have equal access to the medium
  3. **Collision Detection**: Devices detect when two signals collide
- **Example scenario**:
  - Computer A wants to send data and checks if the network is free
  - If free, it starts transmitting
  - If Computer B also starts transmitting simultaneously, a collision occurs
  - Both detect the collision, stop transmitting, and wait a random time before retrying
- **Obsolescence**: Rarely used in modern networks because switched Ethernet provides dedicated collision-free connections.

### 🔹 Framing, Error Detection
- **Ethernet Frame Structure**:
  ```
  +--------+--------+------+------+--------+
  |Preamble|Dest MAC|Source| Data | CRC    |
  |  8B    |  6B    | MAC6B|0-1500B| 4B    |
  +--------+--------+------+------+--------+
  ```
- **Error Detection Methods**:
  - **CRC (Cyclic Redundancy Check)**: Polynomial division of message bits, detects most transmission errors
  - **Checksum**: Simple addition of data chunks (less robust than CRC)
  - **Parity Bit**: Adds a bit to make total number of 1s even or odd (simplest method)
- **Example**: If a frame gets corrupted during transmission (e.g., "10101" becomes "10111"), the receiver calculates a different CRC than what was sent and requests retransmission.

## 7. Transport Layer (TCP Congestion Control) & Brief Overview of All Layers

### 🔹 TCP Congestion Control
- **Purpose**: Prevents network congestion by adjusting transmission rates.
- **Slow Start**:
  - Begins with a small congestion window (cwnd), typically 1 MSS
  - Exponentially increases window size until threshold or packet loss
  - **Example**: Starting at 1 segment, then 2, 4, 8, etc.
- **Congestion Avoidance (AIMD)**:
  - **Additive Increase**: Increases cwnd linearly (by 1 MSS per RTT)
  - **Multiplicative Decrease**: Cuts cwnd in half upon packet loss
  - **Example**: When streaming video, if packets start dropping, TCP halves its sending rate
- **Fast Retransmit & Fast Recovery**:
  - Detects packet loss via duplicate ACKs without waiting for timeout
  - **Example**: If sender receives 3 duplicate ACKs for sequence #100, it immediately retransmits packet #101
- **TCP Variants**:
  - **Tahoe**: Simple but drastic (returns to slow start after any loss)
  - **Reno**: More refined (uses fast recovery)
  - **Cubic**: Modern variant optimized for high-bandwidth networks

