# Network Fundamentals

In this README, I go over how computer networks work. From understanding what networking is to exploring routers, switches, VPNs, firewalls, and how they all connect.

## What is Networking?

Networking is how computers talk to each other and exchange data. Whether it’s browsing the web, using email, or sharing files, networking makes it all happen. Devices connect together to form networks, and these networks can talk to each other to create something even bigger(internet).

---

## Intro to LAN

A LAN (Local Area Network) is a private network that connects computers in a small area, like a home or office. It’s fast and great for sharing files, printers, or internet access between devices in the same location.

---

## OSI Model

The OSI model breaks networking into 7 layers, each with its own job:

1. **Physical** – Cables, signals, hardware.
2. **Data Link** – Frames and MAC addresses.
3. **Network** – IP addresses and routing.
4. **Transport** – TCP/UDP and port numbers.
5. **Session** – Handles connections.
6. **Presentation** – Encryption, formatting.
7. **Application** – What the user interacts with. EX= web browsers/email

I also looked at how data moves down (encapsulation) and up (decapsulation) through these layers.

The Application Layer is the one we interact with directly. Apps like browsers and email clients sit here. DNS also belongs to this layer. DNS translates website names into IP addresses. GUIs (Graphical User Interfaces) make it easy for us to use this layer.

---

## Packets & Frames

Data is broken into packets and frames before being sent over a network.

- A **packet** exists at Layer 3 (Network). It includes things like source/destination IP addresses and a TTL (Time to Live) value.
- A **frame** exists at Layer 2 (Data Link). It wraps the packet and includes MAC addresses for delivery across the local network.

Its like putting an envelope (packet) into a bigger envelope (frame) before mailing it. When the frame is opened at the destination, the packet is still inside.

### Headers

Every packet and frame has a **header**. This is like an extra info like source/destination addresses, ports, TTL, sequence numbers, etc. These headers tell networking devices how to handle the data.

---

## TCP/IP = The Three Way Handshake

TCP (Transmission Control Protocol) ensures data is sent reliably. It sets up a connection using a Three Way Handshake

1. **SYN** – Client says: “Here’s my sequence number, let’s sync.”
2. **SYN/ACK** – Server replies: “Here’s mine, and I got yours.”
3. **ACK** – Client confirms and starts sending data.

Each piece of data has a sequence number, and the receiver sends an acknowledgement number to confirm.

TCP guarantees data delivery but is slower because of all these checks. It’s used for things like web browsing and email.

---

## UDP/IP

UDP (User Datagram Protocol) skips the handshake. It just sends data fast and simple. No guarantees the data will arrive or arrive in order. It’s used for things like video calls, gaming, or streaming where speed matters more than reliability.

UDP headers are simpler and lighter than TCP. No sequence numbers, no acknowledgements just source/destination IP, ports, and the data.

---

## Ports 101

Ports are how devices know where to send data within a system. Think of them like doors to specific applications.

- Port numbers range from 0 to 65535.
- **Common ports**:
  - 21 – FTP
  - 22 – SSH
  - 80 – HTTP
  - 443 – HTTPS(secure version)
  - 445 – SMB
  - 3389 – RDP

---

## Extending Your Network

### Port Forwarding

Port forwarding lets users outside a local network (like the internet) access services inside it. Without it, a web server in your home/office would only be available to your local network.

With port forwarding:
- Router sends traffic on a specific public port (like 80) to the correct *nternal IP and port* like 192.168.1.10:80. 
- This makes internal service like a website accessible from the internet.

### Firewalls 

Firewalls control what traffic is allowed in and out of a network. They can block traffic based on IP addresses, ports, or protocols.

- **Stateful firewalls** – Track entire connections. Smarter, but use more resources.
- **Stateless firewalls** – Check each packet against rules. Faster, but dumber.

I practiced blocking malicious traffic on port 80 based on source IP.

---

## VPN Basics

VPN (Virtual Private Network) connects two networks over the internet securely.

- It encrypts data, so nobody like ISPs or hackers on public WiFi can snoop.
- It creates a tunnel between locations like office A and office B, making devices on both sides feel like they’re on the same network.

### VPN Types

- **PPP** – Authenticates and encrypts but isn’t routable.
- **PPTP** – Sends PPP over the internet. Easy but weak encryption.
- **IPSec** – Stronger encryption and more secure but harder to set up.

---

## LAN Networking Devices

### MAC vs IP Address

- **MAC Address**: address that identifies a device on the local network. Stays the same.
- **IP Address**: address that can change based on the network. Used to route data across networks.

### Routers

Routers connect different networks together and figure out the best path for data. They work at Layer 3 Network Layer and handle routing between devices on separate networks.

They also manage stuff like:
- Port forwarding
- Firewall rules
- NAT = Network Address Translation

### Switches

Switches connect multiple devices on the **same** network using Ethernet cables. They come in two types:

- **Layer 2 switches** – Operate at the Data Link layer. Use MAC addresses to send data to the right device.
- **Layer 3 switches** – More advanced. Can also route packets using IP addresses like a router. 

### VLANs

VLANs (Virtual LANs) let you split one physical network into multiple virtual ones. Devices in different VLANs can be isolated from each other even if they’re connected to the same switch. Great for security for example separating the Accounting and Sales teams. 

---
