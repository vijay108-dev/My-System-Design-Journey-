# Shared Database in HLD (Authentication Use Case)

In **High-Level Design (HLD)**, a **Shared Database** is often used when multiple services need access to **common data**, such as user authentication or session information.

This approach ensures that **every service in the system can verify and authenticate users** using the same trusted data source.

---

## What Is a Shared Database?

A **shared database** means that multiple services or applications **connect to the same database** to read or write shared information.

In authentication systems, it allows all services to use **the same user credentials, tokens, and roles**.


---

##  Why Use a Shared Database for Authentication?

| Reason | Description |
|--------|--------------|
| **Centralized Authentication** | All services use one database to verify user credentials. |
| **Single Source of Truth** | User data (like email, password, tokens) stays consistent across all services. |
| **Easy Session Management** | Session data can be shared among different microservices. |
| **Simplifies Authorization** | Role-based access control (RBAC) can be maintained in one place. |
| **Reduced Duplication** | No need to replicate user tables in every service. |

---

##  Example

Imagine you have:
-  **Auth Service** → Handles login, signup, JWT token generation  
-  **Order Service** → Needs to verify user identity before placing an order  
-  **Chat Service** → Needs to verify user session for sending messages  

All these services can connect to the **same shared database** that stores:

| Table | Purpose |
|--------|----------|
| `users` | Stores user credentials and profile |
| `sessions` | Tracks logged-in sessions and tokens |
| `roles` | Defines permissions for users |

This way, any service can quickly **authenticate or authorize** a user using the same database.

---

##  How It Works (Step-by-Step)

1. **User Logs In**
   - The Auth Service verifies credentials (email/password).
   - It stores a session or token in the shared DB.

2. **Other Services Validate**
   - When a user makes a request to another service (like placing an order),
     that service checks the token/session in the same shared DB.

3. **Consistent Authentication**
   - All services rely on one data source — ensuring consistent user state across the system.

---

##  Advantages

1.**Centralized Authentication Data** — One place for user and session data  
2. **Simplified Validation** — All services can easily check credentials or tokens  
3. **Consistent User Experience** — Login once, valid across all services  
4. **Ease of Maintenance** — Only one authentication schema to update  

---

##  Disadvantages

1. **Tight Coupling** — All services depend on the same database  
2. **Scaling Limitations** — Heavy read/write load on a single DB  
3. **Security Risk** — If the shared DB is compromised, all services are affected  
4. **Complex Transactions** — Requires careful handling of concurrent requests  

---

##  Real-World Example

| System | Approach |
|---------|-----------|
| **E-commerce / SaaS Apps** | Shared DB for users and sessions across all microservices |
| **Legacy Monoliths** | Naturally use a single shared DB for authentication |
| **Modern Systems** | Use dedicated Auth service with shared DB or cache (Redis) |

---

##  Best Practice

If your system is small or medium-scale:
-  A **Shared Database** for authentication is perfectly fine.
-  Keep only **authentication and session tables** shared — not business data.
-  Optionally, use **Redis** to cache sessions for faster token validation.

---

##  In Short

> Shared Database in HLD = One common DB that multiple services use to authenticate users.

 **Simple**  
 **Consistent**  
 **Centralized Authentication**  
 **Tightly Coupled**, but practical for small to mid-scale systems.


##  Messaging Queue in HLD

A **Messaging Queue (MQ)** is a communication mechanism that allows different services or components of a system to interact **asynchronously**.  
It temporarily stores messages sent by a **producer** until they are safely processed by a **consumer**.

---

###  How It Works

1. **Producer (Sender)** → Sends a message to the queue.  
2. **Message Queue (Broker)** → Holds messages in order until processed.  
3. **Consumer (Receiver)** → Retrieves and processes the messages.

This setup helps in **decoupling** the producer and consumer so they don’t depend on each other’s availability.

---

### Example (Real-Life Analogy)

Imagine a food delivery app:

- When a user places an order, the **Order Service** sends an event → “Order Placed”.
- The **Notification**, **Payment**, and **Inventory** services listen to this event.
- The **Message Queue** (e.g., Kafka, RabbitMQ) ensures each service gets the message reliably — even if one of them is temporarily offline.

---

###  Why We Use Messaging Queues

| Purpose | Description |
|----------|-------------|
| **Asynchronous Processing** | Improves system performance by not blocking requests. |
| **Service Decoupling** | Each microservice can work independently. |
| **Scalability** | Consumers can be scaled horizontally based on load. |
| **Reliability** | Messages are persisted until processed successfully. |
| **Load Buffering** | Absorbs traffic spikes and smooths processing. |
| **Fault Tolerance** | If a consumer fails, the message stays in the queue. |

---


**Popular Message Brokers:**  
- Apache Kafka  
- RabbitMQ  
- Amazon SQS  
- Google Pub/Sub  
- Redis Streams  

---

###  Example in HLD (E-Commerce Flow)

User → API Gateway → Order Service → Message Queue (Kafka Topic)
↳ Payment Service
↳ Notification Service
↳ Inventory Service


 Ensures **asynchronous**, **fault-tolerant**, and **scalable** communication between services.

---

###  Key Terms

| Term | Description |
|------|--------------|
| **Producer** | Service that sends the message. |
| **Consumer** | Service that processes the message. |
| **Broker** | Middleware managing queues (e.g., Kafka, RabbitMQ). |
| **Topic/Queue** | Logical channel for messages. |
| **Acknowledgment** | Confirms successful message processing. |

---

###  Real-World Use Cases

- Sending notifications (Email, SMS)
- Processing background jobs
- Event-driven architecture in microservices
- Logging and analytics pipelines
- Retry mechanisms for failed operations

 ##  Database Sharding in HLD

###  What is Database Sharding?

**Database Sharding** is the process of **splitting a large database into smaller, faster, and more manageable pieces** called **shards**.  
Each shard holds a portion of the total data and runs on a separate database server.

Instead of having one giant database handling all requests, we **divide data horizontally** — each shard contains a subset of rows, not columns.

---

###  How It Works

- **Horizontal Partitioning:** Data is divided across multiple databases (shards) based on a **sharding key** (e.g., user ID, region, or account ID).
- Each shard works as an **independent database**, but all shards together represent the full dataset.
- The **application layer** or a **shard manager** determines which shard to query based on the key.

---

###  Architecture Overview


            ┌───────────────────────────┐
            │        Application         │
            │ (Decides shard location)  │
            └────────────┬──────────────┘
                         │
    ┌────────────────────┼────────────────────┐
    │                    │                    │
┌──────────────┐ ┌──────────────┐ ┌──────────────┐
│ Shard 1 │ │ Shard 2 │ │ Shard 3 │
│ user_id 1-100│ │ user_id101-200│ │ user_id201-300│
└──────────────┘ └──────────────┘ └──────────────┘


Each shard stores a subset of data — this reduces load, increases speed, and improves scalability.

---

###  Why We Use Database Sharding

| Benefit | Description |
|----------|-------------|
| **Scalability** | Handle larger datasets by distributing data across servers. |
| **Performance** | Smaller databases → faster queries and indexing. |
| **Availability** | Failure in one shard doesn’t affect others. |
| **Cost Optimization** | Scale horizontally using cheaper commodity hardware. |
| **Load Distribution** | Balances query load among multiple databases. |

---

###  Real-Life Analogy

Think of a **school database**:
- Instead of keeping all students in one giant table, you can split them into multiple databases based on class or batch.
- Example:
  - `Shard 1`: Class 1–5 students  
  - `Shard 2`: Class 6–10 students  
  - `Shard 3`: Class 11–12 students  

Each shard can be managed and queried independently.

---

###  Sharding Key

The **sharding key** decides how data is distributed.

| Type | Example | Use Case |
|------|----------|----------|
| **Hash-Based Sharding** | Use hash of `user_id` to determine shard | Even data distribution |
| **Range-Based Sharding** | Split users by `user_id` ranges | Sequential data (e.g., by region or date) |
| **Directory-Based Sharding** | Maintain lookup table mapping users → shards | Dynamic control but extra management |

---

###  Challenges in Sharding

| Challenge | Description |
|------------|--------------|
| **Rebalancing** | Adding/removing shards requires data redistribution. |
| **Cross-Shard Queries** | Joins across shards are complex and expensive. |
| **Hotspots** | Uneven sharding keys may cause some shards to overload. |
| **Maintenance Overhead** | More shards → more databases to manage. |

---

###  Example (User-Based Sharding)

Sharding Key: user_id

Shard 1 → user_id 1–1000
Shard 2 → user_id 1001–2000
Shard 3 → user_id 2001–3000

When `user_id = 1560` logs in → application routes query to **Shard 2** only.

---

###  Real-World Use Cases

- **E-commerce apps** (split users or orders by region)
- **Social media platforms** (split user profiles or messages)
- **Financial systems** (split accounts by customer ID)
- **Gaming platforms** (split player data by region or game ID)

---

###  Common Tools / Databases Supporting Sharding

- **MySQL Cluster**
- **MongoDB Sharding**
- **Cassandra**
- **CockroachDB**
- **Vitess**
- **PostgreSQL with Citus**

---

###  Summary

| Aspect | Description |
|--------|--------------|
| **Concept** | Dividing a large database horizontally into smaller pieces. |
| **Goal** | Improve scalability, speed, and reliability. |
| **Key** | Sharding key determines data distribution. |
| **Trade-Off** | More complex maintenance and query handling. |