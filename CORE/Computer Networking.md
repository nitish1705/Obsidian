## 1. IP Addressing (IPv4)

IPv4, or Internet Protocol version 4, is the fourth version of the Internet Protocol. It uses 32-bit numerical addresses to uniquely identify devices on a network. IPv4 has been the dominant protocol since its introduction in the 1980s.

### Structure

IPv4 addresses are written in dotted decimal notation, such as **192.168.1.1**. Each address is divided into four octets (8 bits each), separated by periods, with values ranging from 0 to 255.

### IPv4 Header

The IPv4 header consists of several fields that enable data transmission, routing, and control. It has a minimum size of 20 bytes and can extend up to 60 bytes with optional fields included. Below are the key components of an IPv4 header:

- **Version:** Specifies the Internet Protocol version being used, which is 4 for IPv4. This ensures that the packet is interpreted correctly by the receiving devices.
- **Header Length:** Indicates the length of the header in 32-bit words. This field helps in identifying where the actual payload data begins.
- **Type of Service (ToS):** Provides information on how the packet should be handled in terms of priority and quality of service, such as minimizing delay or maximizing throughput.
- **Total Length:** Specifies the total size of the packet, including both the header and the payload data, in bytes. The maximum value is 65,535 bytes.
- **Identification:** A unique identifier assigned to each packet to aid in reassembling fragmented packets back into the original message.
- **Flags:** A set of three bits used to control packet fragmentation, such as whether the packet can be fragmented or whether more fragments are expected.
- **Fragment Offset:** Indicates the position of the current fragment in relation to the entire original data, ensuring fragments are correctly reassembled.
- **Time to Live (TTL):** Specifies the maximum number of hops (routers) a packet can traverse before being discarded. This prevents indefinite looping in the network.
- **Protocol:** Identifies the protocol used in the data portion of the packet, such as TCP (6) or UDP (17), ensuring proper handling at the destination.
- **Header Checksum:** A value used to detect errors in the header. If the checksum doesn't match at a router, the packet is discarded.
- **Source Address:** Specifies the IPv4 address of the device that sent the packet, enabling the recipient to know where the data originated.
- **Destination Address:** Specifies the IPv4 address of the intended recipient device, allowing the packet to reach the correct destination.
- **Options (Optional):** Provides additional functionality, such as specifying routing preferences or timestamps. This field is not commonly used due to its impact on performance.

  

![](https://static.takeuforward.org/content/upload_1755270875501_IPV4)

### Address Classes

- **Class A:** Designed for large networks with millions of devices (Range: 1.0.0.0 – 126.255.255.255). Used by ISPs and multinational companies.
- **Class B:** Suitable for medium-sized networks (Range: 128.0.0.0 – 191.255.255.255). Commonly used by universities and large organizations.
- **Class C:** Used for smaller networks like those in small businesses (Range: 192.0.0.0 – 223.255.255.255).
- **Class D:** Reserved for multicast transmissions (Range: 224.0.0.0 – 239.255.255.255), often used in streaming and conferencing applications.
- **Class E:** Reserved for experimental purposes (Range: 240.0.0.0 – 255.255.255.255).

### Limitations

- **Address Space:** IPv4 provides approximately 4.3 billion unique addresses, which is insufficient for modern global requirements.
- **Lack of Security:** IPv4 does not have built-in encryption or authentication mechanisms, making data transmissions vulnerable.
- **Exhaustion:** The rapid growth of the internet has led to a near-exhaustion of available IPv4 addresses, necessitating alternative solutions like IPv6.

### Applications

- Widely used in LANs, WANs, and early internet infrastructure to assign IP addresses to devices.
- Commonly implemented in home and office networks for identifying and communicating with devices like computers, routers, and printers.

  

---

## 2. IP Addressing (IPv6)

IPv6, or Internet Protocol version 6, is the successor to IPv4. It uses 128-bit addresses to overcome the limitations of IPv4, providing an almost unlimited address pool to support modern internet demands.

### Structure

IPv6 addresses are written in colon-hexadecimal notation, such as **2001:0db8:85a3:0000:0000:8a2e:0370:7334**. These addresses consist of eight 16-bit segments, separated by colons, and can include shorthand notation for consecutive zero segments (e.g., **2001:db8::1**).

### IPv6 Header

The IPv6 header is designed to be simpler and more efficient than the IPv4 header. It has a fixed size of 40 bytes, which reduces processing overhead and enhances routing performance. Below are the key components of an IPv6 header:

- **Version:** Specifies the Internet Protocol version being used, which is 6 for IPv6. This ensures compatibility and correct interpretation by the devices.
- **Traffic Class:** A field that serves a similar purpose to the Type of Service (ToS) field in IPv4. It is used to classify packets for quality of service (QoS) and prioritization purposes.
- **Flow Label:** A unique identifier that allows routers to recognize and handle packets belonging to the same data flow, providing special treatment or optimized routing for certain types of traffic.
- **Payload Length:** Specifies the size of the payload (data) in bytes, excluding the IPv6 header itself. This field ensures accurate interpretation of the packet size.
- **Next Header:** Identifies the type of header that follows the IPv6 header. This could be a transport layer protocol (e.g., TCP or UDP) or an extension header for additional features.
- **Hop Limit:** Replaces the TTL field in IPv4. It specifies the maximum number of hops a packet can traverse before being discarded, preventing infinite loops in the network.
- **Source Address:** Indicates the IPv6 address of the device that originated the packet. This is crucial for reply packets and tracing the source of the communication.
- **Destination Address:** Specifies the IPv6 address of the target device or endpoint, ensuring the packet reaches the correct recipient.

  

![](https://static.takeuforward.org/content/upload_1759900188317_ipv6)

### Features

- **Large Address Space:** IPv6 provides approximately 340 undecillion unique addresses, accommodating global needs for the foreseeable future.
- **Built-in Security:** Includes mandatory support for IPsec, ensuring data encryption, authentication, and integrity during communication.
- **Auto-Configuration:** Devices can automatically generate their own IPv6 addresses using Stateless Address Autoconfiguration (SLAAC).
- **Efficient Routing:** Simplifies routing by reducing the size of routing tables, enhancing the performance of large-scale networks.

### Compatibility

IPv6 is backward-compatible with IPv4 through techniques like **dual-stack** (running both protocols simultaneously) and **tunneling** (encapsulating IPv6 packets within IPv4).

### Applications

- Widely implemented in IoT (Internet of Things) devices to accommodate their growing address needs.
- Supports modern internet infrastructure, large-scale data centers, and next-generation networking technologies.

  

---

## 3. Comparison of IPv4 and IPv6

|Feature|IPv4|IPv6|
|---|---|---|
|Address Length|32-bit|128-bit|
|Address Notation|Dotted decimal (e.g., 192.168.1.1)|Colon-hexadecimal (e.g., 2001:db8::1)|
|Address Space|~4.3 billion addresses|~340 undecillion addresses|
|Security|No native encryption or authentication|Built-in support for IPsec|
|Header Size|20 bytes|40 bytes|
|Fragmentation|Performed by routers|Performed by the source device|
|Routing|Less efficient|More efficient|

---

### Applications

- **IPv4:** Still widely used in legacy systems, private networks, and smaller-scale deployments.
- **IPv6:** Essential for IoT devices, large-scale internet services, and modern network infrastructures.