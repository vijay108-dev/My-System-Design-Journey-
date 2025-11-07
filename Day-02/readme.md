#  Choosing the Right Database in HLD (High-Level Design)

---

## Step 1: Know the Two Main Types of Databases

| Type | Examples | When to Use |
|------|-----------|-------------|
| **SQL (Relational)** | MySQL, PostgreSQL, Oracle, SQL Server | When your data is structured, needs ACID properties, and strong consistency. |
| **NoSQL (Non-Relational)** | MongoDB, Cassandra, DynamoDB, Redis | When data is semi-structured or unstructured, you need scalability, or can tolerate eventual consistency. |

---

##  Step 2: Identify What Your System Needs

When designing a system in **HLD**, ask yourself these questions:

### 1. Do you need strong consistency or can you accept eventual consistency?

- **Strong consistency:** Use **SQL**  
  _(e.g., banking system — must always show the correct balance)_

- **Eventual consistency:** Use **NoSQL**  
  _(e.g., social media likes count — can update later)_

---

### 2. How structured is your data?

- **Fixed schema (tables, columns):** SQL  
- **Flexible or changing schema (JSON, documents):** NoSQL  

---

### 3. Read vs Write Patterns

- **Read-heavy systems** (analytics, dashboards) → SQL or read-optimized databases like **Elasticsearch**  
- **Write-heavy systems** (logging, IoT data, chat) → NoSQL (**MongoDB**, **Cassandra**)  

---

### 4. Do you need horizontal scalability?

- **SQL databases** scale **vertically** (add more power to one machine).  
- **NoSQL databases** scale **horizontally** (add more machines easily).  
  If the system needs to handle **millions of users**, prefer **NoSQL**.  

---

### 5. Transaction Requirements

- **ACID transactions (Atomic, Consistent, Isolated, Durable)** → SQL  
- **BASE (Basically Available, Soft state, Eventually consistent)** → NoSQL  

---

##  Step 3: Common Real-World Choices

| System | Database Used | Reason |
|---------|----------------|--------|
| **Banking / Payment App** | PostgreSQL / Oracle | Strong consistency & transactions |
| **E-Commerce** | Hybrid (MySQL + Redis + MongoDB) | Orders = SQL, Caching = Redis, Catalog = MongoDB |
| **Social Media** | Cassandra / DynamoDB | Massive scale, eventual consistency |
| **Messaging App** | MongoDB / Redis | Fast writes & flexible schema |
| **Analytics System** | BigQuery / ClickHouse / Elasticsearch | Heavy read & aggregation operations |

---

##  In Short

| If you need... | Use... |
|----------------|--------|
| Transactions & structure | **SQL** |
| Scalability & flexibility | **NoSQL** |
| Speed for caching | **Redis** |
| Searching | **Elasticsearch** |
| Analytics on large data | **BigQuery / ClickHouse** |

---

 **Tip:** Many large-scale systems use a **hybrid approach** — SQL for critical data, NoSQL for scalability, and Redis for caching.

#  Load Balancer in High-Level Design (HLD)

---

##  What Is a Load Balancer?

A **Load Balancer (LB)** is a system that distributes incoming network traffic across multiple servers to ensure no single server gets overloaded.

Think of it as a **traffic police officer** directing cars (user requests) to different lanes (servers) to keep everything running smoothly.

---

##  Why We Need a Load Balancer

### Without a Load Balancer:
- One server may get too many requests  
- It might crash or slow down  
- Users will face delays or errors  

### With a Load Balancer:
- Requests are evenly spread  
- System stays fast and reliable  
- If one server fails, traffic automatically shifts to healthy ones  

---

##  How It Works (Step-by-Step)

### 1. User Request Comes In
When a user visits a website (say, **vijayapp.com**), the request first goes to the **Load Balancer** instead of directly to a server.

### 2. Load Balancer Chooses a Server
It checks which backend server is least busy or healthy, and forwards the request to that one.

### 3. Server Processes the Request
The selected server handles the request (like fetching data or rendering a page) and sends the response back to the load balancer.

### 4. Load Balancer Sends Response Back to User
The load balancer forwards the response to the user — so the user never knows which server actually handled it.

---

##  Load Balancing Algorithms

| Algorithm | Description | Use Case |
|------------|--------------|-----------|
| **Round Robin** | Requests are distributed one by one in order | Simple and even distribution |
| **Least Connections** | Sends to the server with the fewest active connections | Good for long-lived connections |
| **IP Hash** | Uses client’s IP to decide which server | Good for session persistence |
| **Weighted Round Robin** | Some servers get more requests based on their power | When servers have unequal capacity |
| **Random** | Chooses a random healthy server | Small systems or testing setups |

---

##  Types of Load Balancers

| Type | Works At | Examples |
|------|-----------|-----------|
| **Layer 4 (Transport Layer)** | Based on IP and TCP/UDP | HAProxy, AWS NLB |
| **Layer 7 (Application Layer)** | Based on content (URL, headers, cookies) | Nginx, AWS ALB, Traefik |

---

##  Features of a Good Load Balancer

- **Health Checks:** Detects when a server goes down and removes it from rotation.  
- **Session Persistence (Sticky Sessions):** Keeps a user on the same server for consistency.  
- **Auto-Scaling Support:** Works with cloud systems that add/remove servers automatically.  
- **SSL Termination:** Handles HTTPS encryption/decryption at the load balancer level.  
- **Caching and Compression:** Improves speed and reduces load on backend servers.  

---

##  In Short

| Concept | Description |
|----------|--------------|
| **Definition** | Distributes traffic across multiple servers |
| **Purpose** | Scalability, fault tolerance, and performance |
| **Common Tools** | Nginx, HAProxy, AWS ELB, Cloudflare LB |
| **Works At** | Layer 4 (Transport) or Layer 7 (Application) |

#  Database Scaling – Master-Slave Architecture

When an application grows, the database often becomes a **bottleneck**.  
To handle more **read** and **write** requests efficiently, we use **database scaling techniques**, and one of the most traditional and effective ways is the **Master–Slave Architecture**.

---

##  What Is Master–Slave Architecture?

In this architecture:

- **Master DB →** Handles all **write operations** (`INSERT`, `UPDATE`, `DELETE`)
- **Slave DBs →** Handle **read operations** (`SELECT`)
- **Replication →** Data written to the master is **copied (replicated)** to the slaves automatically

This setup helps distribute the database load efficiently.

---

##  Why We Need It

| Problem (Without Scaling) | Solution (With Master–Slave) |
|----------------------------|-------------------------------|
| One database handles both reads & writes → becomes slow | Split reads & writes between different DBs |
| High traffic → DB overload | Add multiple slaves for horizontal read scaling |
| Risk of single point of failure | Slaves can act as backups in case master fails |

---

##  How It Works (Step-by-Step)

1. **User sends a request**  
   - If it’s a **read request**, it’s routed to a **slave database**.  
   - If it’s a **write request**, it goes to the **master database**.

2. **Master executes write**  
   - Updates its data.  
   - Sends **replication logs** to all slave databases.

3. **Slaves update themselves**  
   - Each slave updates its data to match the master’s.

4. **Load Balancer (optional)**  
   - Can distribute read requests among multiple slaves for better performance.

---

##  Example Setup

| Role | Type | Operation | Example Queries |
|------|------|------------|----------------|
| Master | Primary DB | Write | `INSERT`, `UPDATE`, `DELETE` |
| Slave 1 | Replica | Read | `SELECT * FROM users;` |
| Slave 2 | Replica | Read | `SELECT * FROM products;` |

---

##  Advantages

1. **High Read Performance** – Distribute reads across slaves  
2. **Backup & Failover Support** – Slaves can become masters if needed  
3. **Scalability** – Add more slaves easily  
4. **Reduced Master Load** – Writes and reads handled separately  

---

##  Challenges

1. **Replication Lag** – Slaves might be slightly behind master  
2. **Write Bottleneck** – Only one master; heavy writes can still slow down  
3. **Failover Complexity** – Switching master during failure must be handled carefully  
4. **Data Consistency Issues** – Read-after-write inconsistencies possible  

---

##  Tools & Technologies

| Category | Examples |
|-----------|-----------|
| **Databases** | MySQL, PostgreSQL, MongoDB (Replica Sets) |
| **Replication Tools** | MySQL Replication, Debezium, AWS RDS Read Replicas |
| **Load Balancers** | HAProxy, ProxySQL, Nginx |

---

##  Real-World Example

- **Instagram** uses a **master-slave setup** to separate heavy photo upload (write) operations from feed read queries.  
- **E-commerce platforms** use it to handle thousands of product reads per second while managing limited writes for inventory updates.

---



