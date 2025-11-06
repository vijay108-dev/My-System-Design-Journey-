 1.First— What is HLD?

HLD (High-Level Design) explains the overall structure of the system —
how components interact, what protocols are used, and how data flows between them.

It focuses on:

System architecture

Communication between services

APIs, load balancers, databases, etc.

So HLD is about how one system talks to another system — over a network.

 Now, where does networking fit?

When two systems (like a client and a server) talk, the communication follows the OSI model —
which has 7 layers (from top to bottom):

Application

Presentation

Session

Transport

Network

Data Link

Physical

2. Why HLD focuses on Application Layer and Transport Layer

Let’s understand this clearly 👇

 1. Application Layer — Defines WHAT is communicated

At this layer, your HLD decides how your system components talk logically.

It focuses on:

Protocols like HTTP, HTTPS, SMTP, FTP, WebSocket, gRPC

APIs, endpoints, data formats (JSON, XML)

Client–server interactions (request/response)

 Example:
In a Swiggy-like system,

User Service talks to Restaurant Service using REST API over HTTP.

HLD mentions: “Communication will happen over HTTPS for security.”

So, HLD defines the rules of communication — that’s why it focuses on the Application layer.

 2. Transport Layer — Defines HOW it’s communicated

Once the application knows what to send,
the transport layer ensures how the data will travel reliably.

It deals with:

Protocols: TCP (reliable), UDP (faster, less reliable)

Ports: e.g., HTTP → port 80, HTTPS → port 443

Connection types: persistent, stateless, etc.

 Example:
In your design, the backend API might use TCP (reliable communication).
A live chat feature might use UDP (faster, real-time).

So, HLD mentions which transport protocol the system will use and why.

3. Why not lower layers (Network, Data Link, Physical)?

Because:

Those layers deal with hardware, routing, and signal transmission.

That’s the job of network engineers or infrastructure, not the system designer.

In HLD, we assume the network exists — we only care about how our services communicate logically.

🔍 Summary
Layer	Role in HLD	Example
Application	Defines what is sent and how systems talk	HTTP APIs, JSON data, REST, gRPC
Transport	Defines how data is delivered reliably	TCP for reliability, UDP for speed
Lower Layers	Managed by hardware/network infra	Routers, switches, cables — not HLD focus

💡 In short:

HLD focuses on Application and Transport layers because they define how different parts of your software communicate — what data they exchange, and how reliably it’s transferred over the network.

# 🌐 DNS in HLD

##  What is DNS?
**DNS (Domain Name System)** is like the Internet’s phonebook.  
It converts **domain names** (like `www.google.com`) into **IP addresses** (like `142.250.190.14`) so that computers can find each other.

---

##  DNS in High-Level Design (HLD)

In HLD, DNS helps the **client reach the correct server** by resolving the domain name before the request goes to the backend.

## 🧾 In Short:
DNS tells the client **where** to send the request.