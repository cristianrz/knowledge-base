% Theory

# Five stages of hacking

1.  Reconaissance
2.  Scanning and enumeration
3.  Gaining access
4.  Maintaining access
5.  Covering tracks

# Physical/Social

- Google street view
- hunter.io
- googling
    - filetype/ext
- theharvester (emails, subdomains)
- haveibeenpwned
- bluto: brute force against have i been pwned
- crt.sh: *.domain.com
- wappalyzer: technologies on a website

# Vulnerability scanners

- Nessus
    - Pro edition
- nikto
- burp suite (for web apps)
    - Pro edition (400USD)

# Routing

- process:
    1.  find all the entries in the list that match subnet mask and network
    2.  use the one that has the longest subnet mask
    3.  if none of them matches, use the default rule (0.0.0.0)
- routers change MAC addresses but they don't change IPs
- FF:FF:FF:FF:FF:FF is the broadcast MAC address
- routers do not forward packets with broadcast MAC

# Layers

- Layer 1: cable
- layer 2: MAC addresses
    - 48 bit
- layer 3: IP addresses

# IPv4 packet

* Version: 4 for IPv4
* Internet Header Length: size of the IPv4 header
* Differentiated Services Code Point: type of service
* Explicit Congestion Notifiction: end-to-end notification of network congestion
* Total length: entire packet size in bytes, including header and data
* Identification: uniquely identifying the group of fragments of a single IP datagram
* Flags:
	* bit 0: Reserved; must be zero.
	* bit 1: Don't Fragment (DF)
	* bit 2: More Fragments (MF)
* Fragment offset: This field specifies the offset of a particular fragment relative to the beginning of the original unfragmented IP datagram in units of eight-byte blocks. The first fragment has an offset of zero. 
* Time to Live: field is used as a hop count—when the datagram arrives at a router, the router decrements the TTL field by one. When the TTL field hits zero, the router discards the packet and typically sends an ICMP time exceeded message to the sender.
* Protocol: [list of protocol numbers](https://en.wikipedia.org/wiki/List_of_IP_protocol_numbers)
* Header checksum
* Source address
* Destination address
* Options: The options field is not often used. Packets containing some options may be considered as dangerous by some routers and be blocked.
* Payload

# TCP header

* Source port
* Destination port
* Sequence number
* Acknowledgement number
* Data offset: specifies the size of the TCP header in 32-bit words.
* Reserved: For future use and should be set to zero.
* Flags
* Window size: The size of the receive window, which specifies the number of window size units[c] that the sender of this segment is currently willing to receive.
* Checksum: The 16-bit checksum field is used for error-checking of the TCP header, the payload and an IP pseudo-header. The pseudo-header consists of the source IP address, the destination IP address, the protocol number for the TCP protocol (6) and the length of the TCP headers and payload (in bytes).
* Urgent pointer
* Options

# TCP flags

- NS (1 bit): ECN-nonce - concealment protection[a]
- CWR (1 bit): Congestion window reduced (CWR) flag is set by the sending host to indicate that it received a TCP segment with the ECE flag set and had responded in congestion control mechanism.[b]
- ECE (1 bit): ECN-Echo has a dual role, depending on the value of the SYN flag. It indicates:
	- If the SYN flag is set (1), that the TCP peer is ECN capable.
	- If the SYN flag is clear (0), that a packet with Congestion Experienced flag set (ECN=11) in the IP header was received during normal transmission.[b] This serves as an indication of network congestion (or impending congestion) to the TCP sender.
- URG (1 bit): Indicates that the Urgent pointer field is significant
- ACK (1 bit): Indicates that the Acknowledgment field is significant. All packets after the initial SYN packet sent by the client should have this flag set.
- PSH (1 bit): Push function. Asks to push the buffered data to the receiving application.
- RST (1 bit): Reset the connection
- SYN (1 bit): Synchronize sequence numbers. Only the first packet sent from each end should have this flag set. Some other flags and fields change meaning based on this flag, and some are only valid when it is set, and others when it is clear.
- FIN (1 bit): Last packet from sender

# Information gathering

- Find e-mail addresses:
	- [[hunter.io]]
	- [[phonebook.cz]]
	- https://clearbit.com/
- Find subdomains
	- https://crt.sh/

# Scanning and enumeration

```bash
nikto -h http://example.com
```

```bash
dirbuster 
```
