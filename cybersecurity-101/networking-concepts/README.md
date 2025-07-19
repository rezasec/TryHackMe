# Part 1 - Networking Concepts 

## Introduction

The module started by setting the stage for what I’d be learning: the OSI and TCP/IP models, how IP addresses and subnets work, the differences between TCP and UDP, and how to connect to open ports using tools like Telnet. All of it ties into understanding how devices communicate on a network.

---

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

## Task 7 — Telnet

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
