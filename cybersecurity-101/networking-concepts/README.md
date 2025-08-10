## Part 1 - Networking Concepts 

### OSI Model

The OSI model explains how data flows through a network in seven layers. 

| Layer | Name             | Main Function                                  | Example Protocols           |
|-------|------------------|------------------------------------------------|------------------------------|
| 7     | Application       | Interface for user facing applications         | HTTP, DNS, FTP, SMTP         |
| 6     | Presentation      | Encoding, encryption, compression              | JPEG, PNG, MIME, Unicode     |
| 5     | Session           | Starts and manages communication sessions      | NFS, RPC                     |
| 4     | Transport         | Reliable or fast delivery of data              | TCP, UDP                     |
| 3     | Network           | IP addressing and routing between networks     | IP, ICMP, IPSec              |
| 2     | Data Link         | Sends frames over a local network segment      | Ethernet, WiFi (MAC)         |
| 1     | Physical          | Transmits bits over physical medium            | Cables, wireless signals     |

I used the mnemonic *Please Do Not Throw Spinach Pizza Away* to remember the order from bottom to top.

---

### TCP/IP Model

Unlike the OSI model, the TCP/IP model is what we actually use today. It’s simpler and groups some of the OSI layers together.

| TCP/IP Layer   | OSI Equivalent Layers      | Example Protocols                     |
|----------------|----------------------------|---------------------------------------|
| Application    | Application, Presentation, Session | HTTP, FTP, DNS, SMTP               |
| Transport      | Transport                   | TCP, UDP                              |
| Internet       | Network                     | IP, ICMP, IPSec                       |
| Link           | Data Link + Physical        | Ethernet, WiFi                        |

This model was originally built to survive failures and reroute traffic. 

---

### IP Addresses and Subnets

Every device needs a unique IP address. IPv4 addresses structured into four octets. I looked at how subnet masks work using CIDR notation (like `/24`), and how addresses are split between host and network portions.

Private IP ranges:

- `10.0.0.0/8`
- `172.16.0.0/12`
- `192.168.0.0/16`

Public IPs are routable on the internet, but private ones are not. To connect out, private addresses rely on NAT at the router level.

Routers forward packets between networks, like a post office deciding the next step to deliver mail.

---

### UDP and TCP

Both of these live at Layer 4 and use **port numbers** to talk to specific processes.

#### UDP
- Doesn’t guarantee delivery.
- No connection setup needed.
- Faster and simpler.
- Great for things like streaming or DNS.

#### TCP
- Reliable and connection based.
- Guarantees delivery, order, and integrity.
- Used for web traffic, file transfers, and anything where data accuracy matters.

#### TCP Handshake:
1. SYN
2. SYN-ACK
3. ACK

Once the handshake is done, data can be exchanged safely.

---

### Encapsulation

Encapsulation is how data gets wrapped at each layer with its own header and sometimes trailer. Each layer only needs to care about its job.

Here’s what it looks like as data moves down the stack:

1. **Application Layer**: I enter data, like a form or message.
2. **Transport Layer**: Adds a TCP or UDP header (segment/datagram).
3. **Network Layer**: Adds an IP header (packet).
4. **Link Layer**: Adds MAC addresses and creates a frame.

The receiver reverses this whole process and gets back the original data.

---

### Telnet

Telnet is a simple tool I used to manually connect to open TCP ports and talk to servers.

I tested three services:

- **Echo server (port 7)**: Echoes back whatever I type.
- **Daytime server (port 13)**: Sends the current time.
- **HTTP (port 80)**: I typed raw HTTP requests using:

`telnet MACHINE_IP 80`
`GET / HTTP/1.1`
`Host: telnet.thm`
`(Press Enter twice)`

- You can manually send raw HTTP requests this way to test if a web server is listening.

- Telnet is not secure and shouldn’t be used in production environments. It sends all data, including passwords, in plain text.

---

## Part 2 - Networking Essentials

### DHCP

DHCP (Dynamic Host Configuration Protocol) lets devices automatically get:
- An IP address
- Subnet mask
- Gateway/router
- DNS server

This happens in four steps (DORA):

1. **Discover** – Client asks for a DHCP server
2. **Offer** – Server offers an IP and config
3. **Request** – Client accepts the offer
4. **Acknowledge** – Server confirms and leases the IP

DHCP uses **UDP**, with the server listening on port **67** and the client sending from port **68**. This saves time, avoids IP conflicts, and makes network access seamless on WiFi and mobile networks.

---

### ARP: Bridging Layer 3 Addressing to Layer 2 Addressing

ARP (Address Resolution Protocol) is what lets devices on the same network translate **IP addresses into MAC addresses**.

What happens when one device wants to talk to another on the same subnet:

- It sends an **ARP Request** (broadcast): “Who has 192.168.66.1?”
- The target replies with an **ARP Reply**: “192.168.66.1 is at (mac address)

This info is then cached so future communication doesn't require asking again.

ARP isn’t encapsulated inside IP or UDP, it rides directly inside an **Ethernet frame**. That’s why it’s considered a Layer 2 protocol but it supports layer 3

---

### ICMP: Troubleshooting Networks

ICMP (Internet Control Message Protocol) is used for **network diagnostics and error messages**.

#### Ping:
- Sends an **Echo Request** (ICMP Type 8)
- Waits for an **Echo Reply** (ICMP Type 0)
- Shows round trip time(RTT), packet loss, and response status

Example: ping 192.168.11.1 -c 4

----

### Routing

For packets to reach the right destination, each router along the path needs to know which direction (link) to forward them. That’s where routing comes in.

#### How Routing Works: 

When I send data to a web server, my packets might pass through multiple routers. Each one makes a decision about where to send the packet next based on what it knows about the network.

The goal is to choose the best path, and there’s usually more than one possible route. So routers rely on routing algorithms and protocols to keep track of paths.

#### Routing Protocols: 

- **OSPF** (Open Shortest Path First)  
  Builds a full map of the network and calculates the shortest path based on link cost. Common in enterprise networks.

- **EIGRP** (Enhanced Interior Gateway Routing Protocol)  
  Cisco proprietary. Balances multiple metrics like delay and bandwidth. Known for fast convergence.

- **BGP** (Border Gateway Protocol)  
  The main routing protocol of the internet. Used to exchange routing information between ISPs and large networks.

- **RIP** (Routing Information Protocol)  
  Simple and older. Chooses routes based on the fewest hops. Mainly used in small networks.

---

### NAT

NAT (Network Address Translation) helps solve the IPv4 address shortage by letting multiple devices in a private network share a single public IP.

#### What NAT Does

Instead of giving every device a public IP, a NAT enabled router translates private IP addresses into one public IP address and keeps track of all active connections.

- Inside the network:  
  `192.168.0.129:15401 → Web Server`

- What the server sees:  
  `212.3.4.5:19273 → Web Server`

The router rewrites the IP and port in the packet headers and stores the mapping in a **NAT table**, so when a reply comes back, it knows which internal device to forward it to.

#### Why NAT Matters

- Conserves IPv4 addresses
- Allows large networks to function with one or two public IPs
- Adds a layer of isolation from the public internet

------

## Part 3 - Networking Core Protocols

### DNS

DNS is what lets us use human friendly domain names like `example.com` instead of IP addresses. When I type a URL, my system goes through a DNS lookup process to resolve the domain into an IP address.

There are different record types in DNS, like:

- `A`: maps to an IPv4 address
- `AAAA`: maps to an IPv6 address
- `MX`: specifies the mail server
- `NS`: lists the authoritative nameservers

By running `nslookup` and using tools like Wireshark, I could see this resolution process in action. DNS uses port 53 and works over both UDP and TCP depending on the request.

---

### WHOIS

When someone registers a domain, their contact info is stored in the WHOIS database. Using the `whois` command, I was able to pull up details like:

- Registrar
- Registration and expiration dates
- Registrant’s name and contact (unless hidden with privacy protection)

It’s useful for figuring out who owns a domain and when it was created. WHOIS doesn’t use a specific port like other protocols, but it's important for internet transparency and record-keeping.

---

### HTTP and HTTPS

I already use HTTP/HTTPS all the time without realizing it. These protocols handle how my browser communicates with websites.

Some important HTTP methods I reviewed:

- `GET`: retrieve a resource
- `POST`: send data to a server
- `PUT`: create or overwrite a resource
- `DELETE`: remove a resource

Using Wireshark and telnet, I got a look at raw HTTP requests and responses. HTTP uses port 80, while HTTPS which encrypts data uses port 443.

---

### FTP (File Transfer Protocol)

FTP is a protocol for transferring files. It’s faster and more efficient than HTTP for this purpose.

Some core FTP commands:

- `USER` / `PASS`: login credentials
- `RETR`: download a file
- `STOR`: upload a file

The default control connection is on port 21, but data transfers happen over a separate connection. I logged into an FTP server using `ftp`, listed files, and downloaded one using `get`. It was cool to see the exact commands and responses, and I also reviewed the session using Wireshark.

---

### SMTP (Sending Email)

SMTP is the protocol used to send emails between clients and mail servers. It works like handing your message to a digital post office.

Commands I practiced:

- `HELO` / `EHLO`: initiate connection
- `MAIL FROM`: sender
- `RCPT TO`: recipient
- `DATA`: body of the message
- `.` (dot): ends the message

SMTP runs on port 25. I sent a message using `telnet` and saw how each command was structured.

---

### POP3 (Receiving Email)

POP3 is used to retrieve emails from a mail server. It’s more old school and designed for one device setups where the message is downloaded and then deleted from the server.

Commands I worked with:

- `USER`, `PASS`: login
- `STAT`: message count
- `LIST`: list all messages
- `RETR`: retrieve a message
- `DELE`: delete a message
- `QUIT`: exit the session

POP3 runs on TCP port 110. I was able to use `telnet` to log in and retrieve the same email I sent using SMTP earlier.

---

### IMAP (Email Sync)

IMAP is a more modern email protocol compared to POP3. It keeps messages on the server and syncs status (read, unread, moved, deleted) across multiple clients. This is what makes email work so well across devices.

A few useful IMAP commands:

- `LOGIN`: authenticate
- `SELECT inbox`: choose a mailbox
- `FETCH`: get a message
- `MOVE`, `COPY`: organize messages
- `LOGOUT`: end the session

IMAP uses TCP port 143. I tested the protocol using `telnet`, fetched a message, and saw how it maintains consistent state across clients.

---

### Protocols and their default ports:

| **Protocol** | **Transport** | **Port** |
|--------------|---------------|----------|
| TELNET       | TCP           | 23       |
| DNS          | UDP/TCP       | 53       |
| HTTP         | TCP           | 80       |
| HTTPS        | TCP           | 443      |
| FTP          | TCP           | 21       |
| SMTP         | TCP           | 25       |
| POP3         | TCP           | 110      |
| IMAP         | TCP           | 143      |

---

## Part 4 - Networking Secure Protocols 

### Introduction

Many everyday protocols (HTTP, FTP, SMTP, etc.) were never designed with security in mind. That means attackers could easily read or manipulate data like passwords, credit cards, or email content. The solution was secure versions built using **TLS**, **SSH**, and **VPNs** to ensure **confidentiality**, **integrity**, and **authenticity** of data in transit.

---

### TLS (Transport Layer Security)

TLS is the evolution of SSL and is now the standard for encrypting communication between a client and a server. It operates at the transport layer and protects against interception and tampering.

Some key points:
- TLS was developed to address the security flaws in early SSL.
- It's widely used today in protocols like **HTTPS**, **SMTPS**, and **IMAPS**.
- Servers identify themselves using TLS certificates (usually signed by Certificate Authorities).
- Let’s Encrypt provides free TLS certificates.
- Without TLS, we wouldn’t be able to use the internet securely for shopping, banking, or email.

---

### HTTPS

- HTTP runs on port 80 and sends all data in plaintext.
- HTTPS runs on port 443 and encrypts data after a TLS handshake.
- With HTTPS, packet sniffers only see encrypted "Application Data" and not the actual content.
- In Wireshark, we can only decrypt HTTPS if we have the **private key** or **SSL session keys**.
- TLS doesn’t modify TCP/IP or HTTP itself, it just wraps the HTTP traffic in encryption.

---

### SMTPS, POP3S, IMAPS

These are just the secure versions of email protocols:

| Protocol | Insecure Port | Secure Port |
|----------|---------------|-------------|
| SMTP     | 25            | 465, 587    |
| POP3     | 110           | 995         |
| IMAP     | 143           | 993         |

By using TLS, they become **SMTPS**, **POP3S**, and **IMAPS**. The process is the same as HTTPS, TLS wraps the original protocol without changing its structure.

---

### SSH (Secure Shell)

SSH replaced TELNET for secure remote access. It encrypts all data and supports:

- Password, public key, and two factor authentication
- End to end encryption to prevent eavesdropping
- Integrity protection
- Tunneling other protocols
- GUI forwarding over X11

The standard port is **22**, and most SSH clients today use **OpenSSH**.

---

### SFTP vs FTPS

- **SFTP** (SSH File Transfer Protocol) uses port 22 and is part of the SSH suite. It’s simple to set up if SSH is already enabled.
- **FTPS** (File Transfer Protocol Secure) is FTP over TLS, usually on port **990**.
- SFTP is generally easier to use and firewall friendly.
- FTPS needs proper TLS certificates and can be more complex due to separate control and data channels.

---

### VPN (Virtual Private Network)

VPNs let companies securely connect remote users or branches to the main office over the public internet.

Key takeaways:
- Creates a **private encrypted tunnel** over a public network.
- Masks your IP address and can bypass geographic restrictions.
- Your ISP only sees encrypted data, not the content or destination.
- Some VPNs don’t route all traffic, others might leak DNS or IP information.
- Laws around VPNs vary by country, so usage isn’t always legal.

---

## Part 5 - Wireshark: The Basics

## Introduction

Wireshark is an open-source, cross-platform network packet analyser used for inspecting live traffic and saved packet captures (PCAP files). It is one of the most widely used tools for packet analysis, allowing detailed investigation at multiple layers of the OSI model.

- Wireshark is capable of:
  - **Sniffing live traffic**
  - **Inspecting packet capture (PCAP) files**
- Supports a wide range of network protocols
- Provides GUI tools for protocol analysis and packet inspection

---

## The Wireshark Interface

Wireshark’s main interface is divided into three panes:

1. **Packet List Pane** — Displays each captured packet with summary info  
   (Packet No., Time, Source, Destination, Protocol, Length, Info)
2. **Packet Details Pane** — Decodes and displays protocol fields in a tree view
3. **Packet Bytes Pane** — Shows the raw data in hexadecimal and ASCII

---

## Packet Dissection

- **Packet Dissection** - breaks packets down into OSI layers.
- Typical layers seen in an HTTP packet:
  1. **Frame** (Layer 1) — Physical layer details
  2. **Source [MAC]** (Layer 2) — Data Link layer addresses
  3. **Source [IP]** (Layer 3) — Network layer IP addresses
  4. **Protocol** (Layer 4) — Transport layer protocol (TCP/UDP) and ports
  5. **Protocol Errors** — TCP reassembly info
  6. **Application Protocol** (Layer 5+) — HTTP, FTP, SMB
  7. **Application Data** — Application-specific payload

Clicking a detail highlights the corresponding bytes in the Packet Bytes Pane.

---

## Packet Navigation

- **Packet Numbers** — Unique ID for each packet
- **Go to Packet** — Jump to a specific packet by number
- **Find Packets** — Search by:
  - Display filter
  - Hex value
  - String
  - Regex
- **Mark Packets** — Flag packets for later review 
- **Packet Comments** — Add persistent notes within the capture file
- **Export Packets** — Save selected packets to a new file
- **Export Objects** — Extract files from supported protocols ( HTTP, SMB)
- **Time Display Format** — Change between relative, absolute, UTC
- **Expert Info** — Highlights warnings, errors, and notable events in captures

---

## Packet Filtering

Wireshark supports two filter types:

- **Capture Filters** — Apply before capture, limiting what is recorded
- **Display Filters** — Apply after capture, limiting what is displayed

Key filtering tools:

1. **Apply as Filter** — Right-click any field to filter by its value  
2. **Conversation Filter** — Show only packets from a specific conversation  
3. **Colourise Conversation** — Highlight related packets without filtering  
4. **Prepare as Filter** — Build a filter without immediately applying it  
5. **Apply as Column** — Add a chosen field as a visible column  
6. **Follow Stream** — Reconstruct application level data from TCP/UDP streams

---

## Part 6 - Tcpdump: The Basics

# Tcpdump: The Basics

This module introduces **Tcpdump**, a command-line packet capture and analysis tool used for monitoring and inspecting network traffic. Tcpdump can capture live traffic or read from saved `.pcap` files, providing detailed insight into packet-level communication.

---

## Introduction
Tcpdump is a versatile CLI based packet analyzer that:
- Captures live traffic from network interfaces.
- Reads and analyzes `.pcap` files.
- Displays packet details in customizable formats.

It is widely used in network troubleshooting, security analysis, and protocol research.

---

## Basic Packet Capture

Tcpdump can capture live network traffic from a specific interface and optionally save it to a file for later analysis. This is useful for diagnosing issues, investigating suspicious activity, or learning how protocols function.

To capture traffic, use `tcpdump -i <interface>`, replacing `<interface>` with the desired network interface (e.g., `eth0`, `wlan0`). If no interface is specified, Tcpdump defaults to the first available one.

You can save the captured packets into a file for later inspection using `tcpdump -i eth0 -w capture.pcap`. The `-w` option writes the raw packet data to the specified file in `.pcap` format, which can be opened later with Tcpdump or Wireshark.

To read and analyze a saved capture, use `tcpdump -r capture.pcap`. The `-r` option tells Tcpdump to read from a saved file instead of capturing live traffic.

- `-i` selects the capture interface.
- `-w` stores packets in a file for later analysis.
- `-r` reads and inspects previously saved packet captures.
- Capturing without filters records all visible packets, which can be excessive — filters are covered in later tasks.

---

## Filtering Captures
Tcpdump supports powerful filters to capture only the packets you care about, reducing noise and file size. These filters follow the Berkeley Packet Filter (BPF) syntax and can be applied during live captures or when reading from a file.

### Common Filter Types
1. **Host Filters** — Capture traffic to or from a specific host:
   - `tcpdump host 192.168.1.10` (traffic to/from a single IP)
   - `tcpdump src host 192.168.1.10` (only packets from this IP)
   - `tcpdump dst host 192.168.1.10` (only packets to this IP)

2. **Network Filters** — Capture traffic within a subnet:
   - `tcpdump net 192.168.1.0/24`

3. **Port Filters** — Capture traffic on a specific port:
   - `tcpdump port 80` (HTTP)
   - `tcpdump src port 443` (packets from HTTPS)
   - `tcpdump dst port 53` (packets to DNS)

4. **Protocol Filters** — Capture only a specific protocol:
   - `tcpdump icmp` (ping traffic)
   - `tcpdump tcp`
   - `tcpdump udp`

5. **Logical Operators** — Combine filters:
   - `and` — both conditions must match
   - `or` — either condition can match
   - `not` — exclude packets matching a condition  
     Example: `tcpdump tcp and not port 22`

- Filters drastically reduce capture size and focus the analysis.
- Multiple conditions can be combined to target specific scenarios.

---

## Writing and Reading Packet Captures

Tcpdump allows you to **save captured packets** to a file for later analysis or **read existing captures** without running a live capture. This is useful for sharing data, archiving captures, or analyzing traffic offline with other tools like Wireshark.

### Saving Captures to a File
Use the `-w` option to write captured packets to a file in `.pcap` format:
- `tcpdump -w capture.pcap` — Saves all captured packets to `capture.pcap`
- `tcpdump -i eth0 -w web_traffic.pcap` — Captures only from `eth0` and writes to file

### Reading Captures from a File
Use the `-r` option to read packets from a `.pcap` file:
- `tcpdump -r capture.pcap` — Reads and displays all packets from the file
- You can also apply filters while reading:
  - `tcpdump -r capture.pcap port 80` — Shows only HTTP traffic from the saved file

### Combining Options
You can combine capture filters with file writing to save only relevant traffic:
- `tcpdump -i eth0 port 443 -w https_only.pcap` — Saves only HTTPS traffic from `eth0`

- `.pcap` files can be opened in both Tcpdump and Wireshark.
- Use `-w` to capture without printing, which can reduce processing overhead.
- Use `-r` to analyze saved captures offline, applying filters as needed.

---

Part 7 - Nmap: The Basics



