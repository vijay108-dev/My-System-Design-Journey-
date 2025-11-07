#  Database Cache in HLD

A **cache** is a **high-speed data storage layer** that stores a copy of frequently accessed data from the main database, so that future requests can be served faster.

Think of it as a **shortcut** between your app and the database — instead of going all the way to the database every time, you get the data instantly from the cache.

---

##  Why We Need Cache

| Without Caching | With Caching |
|-----------------|---------------|
| Every user request hits the main database | Requests are served from memory |
| Database becomes slow under load | Response time improves drastically |
| High latency | Low latency and smooth user experience |

---

##  How It Works (Step-by-Step)

1. **User requests data**  
   Example: `/user/123`

2. **Application checks cache**
   - If data is found (**cache hit**) → return immediately  
   - If not found (**cache miss**) → query the database

3. **If cache miss occurs**
   - Fetch data from the database  
   - Store it in the cache  
   - Return it to the user

4. **Cache Expiration / Invalidation**
   - Cached data expires after a set time (TTL – Time To Live)  
   - When DB data changes, cache is updated or invalidated

---

##  Example

Let’s say your app shows a **user profile** page.

| Step | Action | Response Time |
|------|---------|----------------|
| 1 | Fetch from DB (first time) | 120 ms |
| 2 | Store result in cache | - |
| 3 | Next request gets from cache | 5 ms |

 **Speed improved 20x** and **DB load reduced**!

---

##  Types of Caching

| Type | Description | Example |
|------|--------------|----------|
| **Application Cache** | Data cached inside the app’s memory | Node.js memory, Spring Cache |
| **Database Cache** | Query results cached to speed up DB | PostgreSQL Buffer Cache |
| **Distributed Cache** | External cache shared by multiple servers | **Redis**, **Memcached** |
| **CDN Cache** | Static content cached at edge servers | Cloudflare, Akamai |

---

##  Popular Tools

| Tool | Description |
|------|--------------|
| **Redis** | In-memory data structure store (very fast, persistent) |
| **Memcached** | Lightweight key-value memory cache |
| **Hazelcast / Aerospike** | Advanced distributed caching systems |

---

##  Cache Strategies

| Strategy | Description | Use Case |
|-----------|--------------|-----------|
| **Read-Through Cache** | App reads from cache first; if miss → cache loads from DB automatically | Common in web apps |
| **Write-Through Cache** | Writes go to cache and DB simultaneously | When consistency is critical |
| **Write-Back (Write-Behind)** | Writes go to cache first; DB updated later asynchronously | For high write performance |
| **Cache-Aside (Lazy Loading)** | App manages cache manually — fetch, store, refresh | Most common approach (Netflix, Twitter) |

---

##  Advantages

1. Reduces database load  
2. Improves response time dramatically  
3. Helps scale read-heavy systems  
4. Enhances user experience  

---

##  Challenges

1. **Cache Invalidation** – Keeping cache and DB in sync  
2. **Stale Data** – Serving outdated results if not refreshed  
3. **Memory Cost** – Large data in RAM can be expensive  
4. **Cache Stampede** – Too many misses can overload DB  

---

##  Real-World Examples

- **YouTube** caches video metadata (titles, likes) in Redis.  
- **Instagram** caches feed data to avoid hitting DB on every scroll.  
- **Amazon** caches product catalog and price data.

---

##  Simple Architecture Diagram

     ┌──────────────┐
     │    Client     │
     └──────┬───────┘
            │
     ┌──────▼───────┐
     │  Application  │
     └──────┬───────┘
            │
 ┌──────────▼──────────┐
 │       Cache          │
 │ (Redis / Memcached)  │
 └────────┬────────────┘
          │
   ┌──────▼──────┐
   │   Database   │
   └──────────────┘
#  Content Delivery Network (CDN) in HLD

A **Content Delivery Network (CDN)** is a geographically distributed network of servers that deliver web content (like images, videos, CSS, JavaScript, etc.) to users from the nearest possible location — ensuring faster performance and lower latency.

Think of it as a network of **delivery centers** spread across the world, each serving your content from the **closest warehouse** to the user.

---

##  Why We Need a CDN

| Without CDN | With CDN |
|--------------|-----------|
| All users fetch data from one origin server → slow | Users get data from nearest server → fast |
| High latency for users far from the server | Low latency globally |
| Server overloads with traffic | Load distributed across CDN edge servers |
| Poor user experience | Smooth, fast loading everywhere |

---

##  How CDN Works (Step-by-Step)

1. **User Requests Content**  
   A user visits `vijayapp.com` → requests images, CSS, JS, etc.

2. **DNS Redirects to Nearest CDN Node**  
   CDN finds the nearest server (based on user’s location).

3. **Edge Server Delivers Content**
   - If content is already cached → instantly delivered (**cache hit**).
   - If not → fetched from origin server, cached, and then delivered (**cache miss**).

4. **Future Requests Served Faster**
   Once cached, all nearby users get the content immediately from that CDN edge server.

---

##  Example

Suppose the main server is in **Mumbai**, but users are in **New York** and **London**.

- Without CDN → every request travels to Mumbai → slow.
- With CDN → users in New York get content from a nearby CDN node in the U.S., London users from a U.K. node → fast and efficient.

---

##  CDN Architecture Overview

    ┌────────────────┐
    │   Origin Server │
    └──────┬─────────┘
           │
 ┌─────────▼─────────┐
 │     CDN Network    │
 │ (Global Edge Nodes)│
 └─────────┬─────────┘
           │
    ┌──────▼──────┐
    │    Users     │
    │ (Worldwide)  │
    └──────────────┘

---

##  Common CDN Providers

| Provider | Description |
|-----------|--------------|
| **Cloudflare** | Free and powerful global CDN, also offers DDoS protection |
| **Akamai** | Enterprise-grade CDN used by Netflix, Adobe |
| **Amazon CloudFront** | AWS’s global CDN integrated with S3 and EC2 |
| **Fastly** | High-performance CDN used by GitHub, Reddit |
| **Google Cloud CDN** | Built into Google Cloud ecosystem |

---

##  What Can Be Cached in a CDN?

| Type | Examples |
|------|-----------|
| **Static Content** | Images, CSS, JS, Fonts, Videos |
| **Dynamic Content (limited)** | HTML pages that rarely change |
| **APIs (Edge Caching)** | Caching API responses close to users |

---

##  Key Features of a CDN

-  **Geo-Distributed Edge Servers** — Serve data from nearest location  
-  **Low Latency** — Reduces round-trip time  
-  **DDoS Protection** — Shields origin from attacks  
-  **Intelligent Caching** — Keeps most-used content in memory  
-  **Cache Invalidation** — Removes or refreshes outdated files  
-  **SSL Termination** — Handles HTTPS certificates at the edge  

---

##  Benefits

1.Faster load times  
2. Reduced bandwidth usage  
3. Improved reliability and uptime  
4. Global scalability  
5. Enhanced security  

---

##  Limitations

1.Dynamic or personalized content may not cache well  
2. Cache invalidation delays can serve outdated data  
3. Extra cost for premium CDN services  
4. Complexity in configuration for large-scale apps  

---

##  Real-World Examples

- **Netflix** uses CDNs (Open Connect) to stream movies quickly to users worldwide.  
- **YouTube** delivers videos through CDN edge servers.  
- **E-commerce sites** use CDNs to serve images and static assets faster during flash sales.

---

##  In Short

**CDN = Global network of edge servers that cache and deliver content closer to users.**

It improves **speed**, **reliability**, and **security** — making it a key component of modern **High-Level System Design (HLD)**.


# 🏗️ Stateful vs Stateless Architecture in HLD

In **High-Level Design (HLD)**, understanding whether a system is **stateful** or **stateless** is crucial — it determines **how requests are handled**, **how scaling works**, and **how failures are managed**.

---

## ⚙️ 1. What Is a Stateful Architecture?

In a **stateful architecture**, the **server remembers the client’s state** between multiple requests.

That means the server keeps track of:
- Login sessions  
- User preferences  
- Shopping cart data  
- Chat history, etc.

Every request depends on the **previous one**, so the **same client must interact with the same server** each time.

---

### 🧠 Example
Imagine an **online shopping website**:
- You log in → server stores your session data (e.g., in memory).
- You add items to cart → server updates that session.
- If you switch to another server → cart appears empty because the new server doesn’t know your previous state.


---

### 🧩 Characteristics

| Feature | Description |
|----------|--------------|
| **Session Management** | Server stores user session in memory |
| **Dependency** | Each request depends on previous requests |
| **Scaling** | Harder to scale horizontally |
| **Resilience** | If server crashes, state is lost |
| **Example Systems** | Traditional web apps, chat servers, multiplayer games |

---

### ⚠️ Challenges
- Hard to scale (requires **session stickiness**)  
- Difficult to load balance effectively  
- Failover handling is complex  
- High memory usage on each server  

---

## ⚙️ 2. What Is a Stateless Architecture?

In a **stateless architecture**, the server **does not store any client information** between requests.  
Each request is **independent** and **self-contained** — it carries all the information needed to process it.

The server treats every request as **new** and doesn’t care what happened before.

---

### 🧠 Example

APIs like **RESTful APIs** or **Cloud Services**:
- Each HTTP request includes all required details (like user token).
- Any server can handle any request independently.

All responses are valid because **no session memory** is required.

---

### 🧩 Characteristics

| Feature | Description |
|----------|--------------|
| **Session Management** | No server-side session memory |
| **Dependency** | Every request is independent |
| **Scaling** | Easy to scale horizontally |
| **Resilience** | Server crashes don’t affect client |
| **Example Systems** | REST APIs, Microservices, CDN requests |

---

### ✅ Benefits

- Easy to scale horizontally (add/remove servers anytime)
- Ideal for **Auto Scaling** environments (e.g., AWS)
- Fault-tolerant — if one server fails, others take over
- Easy to deploy and maintain

---

### ⚠️ Challenges

- Each request carries extra data (slightly heavier)
- Can be slower if repeated data must be sent every time
- Needs external system for sessions if required (like Redis or DB)

---

## 🧱 Architecture Comparison

| Feature | Stateful | Stateless |
|----------|-----------|------------|
| **Session Storage** | On server memory | On client or shared store |
| **Request Independence** | Dependent | Independent |
| **Scaling** | Difficult | Easy |
| **Load Balancing** | Needs sticky sessions | No stickiness needed |
| **Failure Handling** | Session lost on failure | No impact |
| **Examples** | FTP, Chat apps, Game servers | HTTP, REST API, CDN |

---

## 🧠 Real-World Examples

| Use Case | Architecture | Explanation |
|-----------|---------------|--------------|
| **Netflix API** | Stateless | Each API request is independent |
| **E-commerce Login Session** | Stateful | Needs to remember who is logged in |
| **CDN Edge Servers** | Stateless | Each request handled separately |
| **Online Multiplayer Game** | Stateful | Must remember player state |

---

## 🔄 Transition in Modern Systems

Most modern systems are **moving from stateful → stateless**, to support:
- **Microservices**
- **Auto Scaling**
- **Load balancing**
- **Cloud-native architecture**

Even when “state” is needed, systems **externalize it** using:
- **Redis / Memcached** (for sessions)
- **Databases** (for user data)
- **JWT Tokens** (for authentication)

---

## 🧰 Best Practice in HLD

✅ Design servers as **stateless** wherever possible.  
✅ Use **centralized or distributed data stores** for session/state.  
✅ Keep the **application layer stateless** and **data layer stateful**.

---

## 📘 Summary

| Aspect | Stateful | Stateless |
|--------|-----------|-----------|
| Memory usage | High | Low |
| Horizontal Scaling | Difficult | Easy |
| Fault Tolerance | Low | High |
| Auto Scaling | Complex | Simple |
| Session Storage | Server | Client / Redis |
| Example | Online game | REST API |

---

### 💬 In One Line:
> **Stateful servers remember the past — Stateless servers start fresh every time.**
