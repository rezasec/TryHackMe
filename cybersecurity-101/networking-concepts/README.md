# Part 1 - Networking Concepts 

## OSI Model

The OSI model explains how data flows through a network in seven layers. Each layer has a specific purpose, and understanding this really helped me grasp what’s going on when devices exchange information.

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

## TCP/IP Model

Unlike the OSI model, the TCP/IP model is what we actually use today. It’s simpler and groups some of the OSI layers together.

| TCP/IP Layer   | OSI Equivalent Layers      | Example Protocols                     |
|----------------|----------------------------|---------------------------------------|
| Application    | Application, Presentation, Session | HTTP, FTP, DNS, SMTP               |
| Transport      | Transport                   | TCP, UDP                              |
| Internet       | Network                     | IP, ICMP, IPSec                       |
| Link           | Data Link + Physical        | Ethernet, WiFi                        |

Some books show a fifth layer (Physical), but the overall function stays the same. This model was originally built to survive failures and reroute traffic — something that’s still very relevant.

---

## IP Addresses and Subnets

Every device needs a unique IP address. I revisited IPv4 addresses and how they’re structured into four octets. I also looked at how subnet masks work using CIDR notation (like `/24`), and how addresses are split between host and network portions.

Private IP ranges:

- `10.0.0.0/8`
- `172.16.0.0/12`
- `192.168.0.0/16`

Public IPs are routable on the internet, but private ones are not. To connect out, private addresses rely on NAT at the router level.

I learned how routers forward packets between networks, like a post office deciding the next step to deliver mail.

---

## UDP and TCP

Both of these live at Layer 4 and use **port numbers** to talk to specific processes.

### UDP
- Doesn’t guarantee delivery.
- No connection setup needed.
- Faster and simpler.
- Great for things like streaming or DNS.

### TCP
- Reliable and connection-based.
- Guarantees delivery, order, and integrity.
- Used for web traffic, file transfers, and anything where data accuracy matters.

#### TCP Handshake:
1. SYN
2. SYN-ACK
3. ACK

Once the handshake is done, data can be exchanged safely.

---

## Encapsulation

Encapsulation is how data gets wrapped at each layer with its own header and sometimes trailer. Each layer only needs to care about its job.

Here’s what it looks like as data moves down the stack:

1. **Application Layer**: I enter data, like a form or message.
2. **Transport Layer**: Adds a TCP or UDP header (segment/datagram).
3. **Network Layer**: Adds an IP header (packet).
4. **Link Layer**: Adds MAC addresses and creates a frame.

The receiver reverses this whole process and gets back the original data.

---

## Telnet

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

# Part 2 - Netowrking Essentials

## DHCP (gives network settings)

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

## ARP: Bridging Layer 3 Addressing to Layer 2 Addressing

ARP (Address Resolution Protocol) is what lets devices on the same network translate **IP addresses into MAC addresses**.

What happens when one device wants to talk to another on the same subnet:

- It sends an **ARP Request** (broadcast): “Who has 192.168.66.1?”
- The target replies with an **ARP Reply**: “192.168.66.1 is at (mac address)

This info is then cached so future communication doesn't require asking again.

ARP isn’t encapsulated inside IP or UDP, it rides directly inside an **Ethernet frame**. That’s why it’s considered a Layer 2 protocol but it supports layer 3

---

## ICMP: Troubleshooting Networks

ICMP (Internet Control Message Protocol) is used for **network diagnostics and error messages**.

#### Ping:
- Sends an **Echo Request** (ICMP Type 8)
- Waits for an **Echo Reply** (ICMP Type 0)
- Shows round trip time(RTT), packet loss, and response status

Example: ping 192.168.11.1 -c 4

----

## Routing

For packets to reach the right destination, each router along the path needs to know which direction (link) to forward them. That’s where routing comes in.

### How Routing Works: 

When I send data to a web server, my packets might pass through multiple routers. Each one makes a decision about where to send the packet next based on what it knows about the network.

The goal is to choose the best path, and there’s usually more than one possible route. So routers rely on routing algorithms and protocols to keep track of paths.

### Routing Protocols: 

- **OSPF** (Open Shortest Path First)  
  Builds a full map of the network and calculates the shortest path based on link cost. Common in enterprise networks.

- **EIGRP** (Enhanced Interior Gateway Routing Protocol)  
  Cisco proprietary. Balances multiple metrics like delay and bandwidth. Known for fast convergence.

- **BGP** (Border Gateway Protocol)  
  The main routing protocol of the internet. Used to exchange routing information between ISPs and large networks.

- **RIP** (Routing Information Protocol)  
  Simple and older. Chooses routes based on the fewest hops. Mainly used in small networks.

---

## NAT

NAT (Network Address Translation) helps solve the IPv4 address shortage by letting multiple devices in a private network share a single public IP.

### What NAT Does

Instead of giving every device a public IP, a NAT enabled router translates private IP addresses into one public IP address and keeps track of all active connections.

- Inside the network:  
  `192.168.0.129:15401 → Web Server`

- What the server sees:  
  `212.3.4.5:19273 → Web Server`

The router rewrites the IP and port in the packet headers and stores the mapping in a **NAT table**, so when a reply comes back, it knows which internal device to forward it to.

### Why NAT Matters

- Conserves IPv4 addresses
- Allows large networks to function with one or two public IPs
- Adds a layer of isolation from the public internet

------

# Part 3 - Networking Core Protocols



