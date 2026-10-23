# System Design Learning Roadmap

A structured 8-week roadmap to learn System Design fundamentals, distributed systems, database architecture, networking, scalability, and real-world system design problems.

## Table of Contents

- [Overview](#overview)
- [Week 1 — Scalability, Performance & Traffic Management](#week-1--scalability-performance--traffic-management)
- [Week 2 — Databases, Data Modeling & Storage](#week-2--databases-data-modeling--storage)
- [Week 3 — Networking, APIs & Communication](#week-3--networking-apis--communication)
- [Week 4 — Distributed Systems](#week-4--distributed-systems)
- [Week 5 — APIs, Architecture Patterns & Microservices](#week-5--apis-architecture-patterns--microservices)
- [Week 6 — Social Media & Content Platform Design](#week-6--social-media--content-platform-design)
- [Week 7 — Dropbox, Typeahead, Rate Limiting & Messenger](#week-7--dropbox-typeahead-rate-limiting--messenger)
- [Week 8 — Web Crawler, Yelp, Uber & Ticketmaster](#week-8--web-crawler-yelp-uber--ticketmaster)
- [Daily Learning Routine](#daily-learning-routine)
- [Topic Mastery Checklist](#topic-mastery-checklist)

## Overview

This roadmap focuses on understanding how real-world systems work, how architectural decisions are made, and how to handle scalability, performance, consistency, concurrency, and failures.

### Learning Goals

- Understand scalability, performance, and traffic management.
- Learn database design, indexing, partitioning, sharding, and replication.
- Understand networking, HTTP, REST, gRPC, GraphQL, and real-time communication.
- Learn distributed systems fundamentals and consistency models.
- Design maintainable architectures using appropriate patterns.
- Practice designing real-world systems through interview-style problems.
- Develop the ability to explain architecture, trade-offs, and failure scenarios clearly.

---

# Week 1 — Scalability, Performance & Traffic Management

## 1. Scalability

**What is Scalability?**

Scalability is a system's ability to maintain acceptable performance as users, requests, or data volume increase.

### Types of Scaling

**Vertical Scaling (Scale Up)**

Increasing the CPU, RAM, or storage capacity of an existing server.

- Simple to implement.
- Limited by hardware capacity.
- May require downtime depending on the infrastructure.

**Horizontal Scaling (Scale Out)**

Adding more servers or application instances to distribute workloads.

- Supports greater traffic capacity.
- Improves resilience when designed properly.
- Introduces coordination and distributed-state challenges.

**Example:** If your HPC Lab website grows from 100 users to 100,000 users, a single Node.js server may no longer be sufficient. Multiple application instances, a load balancer, and caching may become necessary.

### Topics to Learn

- Scalability vs. performance.
- Latency vs. throughput.
- Stateless vs. stateful applications.
- Bottleneck identification.
- Auto-scaling mechanisms.

### Resources

- [Scaling and Its Types — Saud Anwar](https://medium.com/)
- [How to Scale a System from 0 to 10 Million+ Users](https://www.google.com/search?q=How+to+Scale+a+System+from+0+to+10+million+users)

## 2. API Gateway

An API Gateway acts as an entry point between clients and backend services.

```text
                 Web / Mobile Client
                         |
                         v
                    API Gateway
             Routing | Authentication
               Rate Limiting | Policies
                         |
          +--------------+--------------+
          |              |              |
          v              v              v
      Auth Service  Content Service  Academic Service
```

**Example:** In your microservices project:

- `/users` requests are routed to the Auth Service.
- `/departments` requests are routed to the Academic Service.

### Topics to Learn

- Request routing.
- Authentication and authorization.
- Rate limiting.
- Timeout and error handling.
- Gateway bottlenecks.
- Single point of failure and high availability.

**Note:** An API Gateway is not mandatory in every architecture.

## 3. Load Balancing

Load balancing distributes incoming traffic across multiple servers.

### Load Balancing Algorithms

- **Round Robin:** Selects servers sequentially.
- **Weighted Round Robin:** Distributes traffic according to server weights.
- **Least Connections:** Selects a server with fewer active connections.
- **IP Hash:** Uses client IP information to select a server.

### Topics to Learn

- Health checks.
- Layer 4 vs. Layer 7 load balancing.
- Session persistence and its limitations.
- Connection draining.
- Load balancer high availability.

**Practice:** If three application servers receive 30,000 requests, how would you distribute the traffic?

The exact distribution depends on the algorithm, server capacity, request cost, and connection behavior.

## 4. Caching

Caching stores frequently accessed data temporarily to reduce latency and backend workload.

### Types of Caching

- Browser cache.
- CDN cache.
- Application cache.
- Redis / Memcached.

### Topics to Learn

- Cache hit vs. cache miss.
- TTL (Time to Live).
- Cache invalidation.
- Cache-aside.
- Write-through.
- Write-behind.
- Cache stampede.
- Cache penetration.
- Cache consistency.

**Example:** Instead of fetching HPC Lab homepage data from MongoDB on every request, cache frequently accessed data in Redis.

When an admin updates the content, the cache must be updated or invalidated appropriately.

## 5. CAP Theorem & PACELC

CAP Theorem describes trade-offs in distributed data systems during network partitions.

- **Consistency:** Every successful read returns the latest successful write, according to the system's consistency guarantee.
- **Availability:** Every request received by a non-failing node gets a response, though it may not contain the latest data.
- **Partition Tolerance:** The system continues operating despite communication failures between nodes, subject to its design.

During a network partition, a distributed system must make trade-offs between consistency and availability.

CAP does not mean that an entire system permanently chooses only C or A.

**PACELC:**

- **If Partition (P):** Trade-off between Availability (A) and Consistency (C).
- **Else (E):** Trade-off between Latency (L) and Consistency (C).

### Topics to Learn

- Strong consistency.
- Eventual consistency.
- CAP trade-offs.
- PACELC.
- Consistency requirements for different workloads.

**Example:**

- Banking transactions: Strong correctness guarantees are critical.
- Social media like counts: Eventual consistency may be acceptable.

## 6. Bloom Filter

A Bloom Filter is a memory-efficient probabilistic data structure used to test whether an item may exist in a set.

It can return:

- **Definitely not present:** The item is absent.
- **Possibly present:** The item may exist.

### Topics to Learn

- Hash functions.
- Bit arrays.
- False positives.
- False positive probability.
- Memory efficiency.
- Standard Bloom Filter deletion limitations.

**Example:** A crawler can use a Bloom Filter to quickly check whether a URL may have been visited.

A Bloom Filter is not a replacement for exact storage or database verification.

## Week 1 Practice

- How would you scale a website with 100,000 concurrent users?
- What happens if Redis goes down?
- How does traffic shift when a server becomes unhealthy?
- Do banking systems and social media require the same consistency guarantees?
- How would you handle Bloom Filter false positives?

---

# Week 2 — Databases, Data Modeling & Storage

**Goal:** Understand where to store data, how to optimize queries, and how to scale database infrastructure.

## 1. SQL vs. NoSQL

| Aspect | SQL | NoSQL |
|---|---|---|
| Data Model | Tables, rows, columns | Documents, key-value, graph, wide-column |
| Schema | Typically structured schema | Model-dependent flexibility |
| Relationships | JOINs and relational constraints | Embedding, references, or other approaches |
| Examples | PostgreSQL, MySQL | MongoDB, Redis, Cassandra |
| Common Use Cases | Transactions, relational data | Flexible documents, caching, distributed workloads |

Database selection should depend on:

- Access patterns.
- Consistency requirements.
- Transaction requirements.
- Query complexity.
- Expected scale.
- Operational complexity and cost.

## 2. Data Modeling

Data modeling defines how application data is structured and related.

**Example: Mobile Banking System**

```text
Users
-----
user_id (PK)
name
phone

Wallets
-------
wallet_id (PK)
user_id (FK)
balance

Transactions
------------
transaction_id (PK)
sender_id
receiver_id
amount
status
created_at
```

Relationships:

- User → Wallet: One-to-one, if the business rules enforce it.
- User → Transactions: One-to-many.
- Wallet → Transactions: One-to-many, depending on the transaction model.

### Topics to Learn

- Entity and attributes.
- Primary key and foreign key.
- One-to-one, one-to-many, many-to-many.
- Normalization and denormalization.
- Constraints and unique keys.
- Read-heavy vs. write-heavy data modeling.
- Data retention.
- Audit history.

**Important:** A money transfer requires more than updating wallet balances. Consider transaction records, atomicity, duplicate request prevention, and failure recovery.

## 3. Database Indexing

An index is an additional data structure that helps retrieve data efficiently.

### Topics to Learn

- B-tree indexes.
- Composite indexes.
- Column ordering in composite indexes.
- Unique indexes.
- Selectivity.
- Query execution plans.
- `EXPLAIN`.
- Read/write trade-offs.

**Example:**

```sql
CREATE UNIQUE INDEX idx_users_phone
ON users(phone);
```

Use indexes based on actual query patterns. Unnecessary indexes increase storage requirements and write overhead.

## 4. Partitioning

Partitioning divides a large table or dataset into smaller logical parts.

### Types

- **Horizontal Partitioning:** Divides rows.
- **Vertical Partitioning:** Divides columns or features.
- **Range Partitioning:** Divides data by ranges.
- **Hash Partitioning:** Divides data using a hash function.
- **List Partitioning:** Divides data by predefined categories.

**Example:** Partitioning transactions by month can improve management and query efficiency for time-bounded workloads.

Partitioning benefits depend on query patterns, partition pruning, and database implementation.

## 5. Sharding

Sharding distributes data across multiple database nodes.

```text
             Application
                  |
                  v
          Shard Router / Key
                  |
         +--------+--------+
         |        |        |
         v        v        v
      Shard A  Shard B  Shard C
      Users    Users    Users
```

### Topics to Learn

- Shard key selection.
- Hot shards.
- Uneven distribution.
- Cross-shard queries.
- Resharding.
- Sharding vs. partitioning.
- Operational complexity.
- When sharding is unnecessary.

A good shard key should distribute data and traffic reasonably evenly while supporting common access patterns.

## 6. Replication

Replication maintains multiple copies of data across database nodes.

### Topics to Learn

- Primary/leader replication.
- Secondary/follower replication.
- Synchronous replication.
- Asynchronous replication.
- Read replicas.
- Replication lag.
- Failover.
- Split-brain prevention.

**Sharding vs. Replication:**

- Sharding distributes different subsets of data.
- Replication creates additional copies of data.
- A system can use both simultaneously.

## Week 2 Practice

- Why might PostgreSQL be suitable for banking?
- How would you model data in MongoDB?
- What would you investigate if a database query became slow?
- What are your options when a database reaches 10 TB?
- How would you handle primary database failure?
- What happens when a read replica returns stale data?

---

# Week 3 — Networking, APIs & Communication

**Goal:** Understand how requests travel from clients to servers and how services communicate.

## 1. HTTP & HTTPS

HTTP is an application-layer protocol for client-server communication.

HTTPS uses TLS to protect HTTP communication.

### Topics to Learn

- Request and response.
- HTTP methods: `GET`, `POST`, `PUT`, `PATCH`, `DELETE`.
- Status codes: `200`, `201`, `400`, `401`, `403`, `404`, `429`, `500`, `503`.
- Headers and body.
- Cookies.
- Statelessness.
- TLS.
- Connection reuse.
- HTTP/2 and HTTP/3 fundamentals.

**Example:** `POST /transactions` initiates a money transfer request.

The API design should define error responses and whether retries are safe.

## 2. DNS

DNS resolves domain names to information such as IP addresses.

```text
Browser
   |
   v
DNS Resolver
   |
   v
Authoritative DNS
   |
   v
DNS Record / IP Address
```

### Topics to Learn

- A and AAAA records.
- CNAME records.
- MX records.
- DNS resolver.
- Authoritative nameserver.
- DNS caching and TTL.
- DNS failover.
- DNS and CDN relationship.

DNS resolution does not complete the web request. Connection establishment, TLS negotiation, and HTTP communication may follow.

## 3. REST API

REST is an architectural style for designing resource-oriented APIs.

### Examples

```http
GET    /users/123
POST   /users
PATCH  /users/123
DELETE /users/123
```

### Topics to Learn

- Resource naming.
- Stateless requests.
- HTTP status codes.
- Pagination.
- Filtering and sorting.
- API versioning.
- Authentication and authorization.
- Idempotency.
- Safe retries.
- Rate limiting.
- Error response format.

## 4. gRPC

gRPC is a remote procedure call framework commonly used for service-to-service communication.

It commonly uses Protocol Buffers for service contracts and serialization.

### Topics to Learn

- `.proto` contracts.
- Code generation.
- Unary RPC.
- Streaming RPC.
- HTTP/2.
- Deadlines and timeouts.
- Retry policies.
- Service-to-service communication.

gRPC can provide strongly typed contracts and streaming capabilities. REST may be simpler for public APIs and browser-oriented integrations.

## 5. GraphQL

GraphQL allows clients to request specific fields from a schema.

### Topics to Learn

- Schema and types.
- Queries and mutations.
- Resolvers.
- Over-fetching and under-fetching.
- N+1 query problem.
- DataLoader.
- Query complexity limits.
- Caching.
- Authorization.

GraphQL does not automatically improve performance. Resolver implementation and database query planning remain important.

## 6. Forward Proxy & Reverse Proxy

### Forward Proxy

Acts on behalf of clients for outbound requests.

Common use cases:

- Corporate filtering.
- Egress control.
- Access policies.

### Reverse Proxy

Sits in front of backend servers.

Common use cases:

- TLS termination.
- Request routing.
- Caching.
- Load balancing.

Examples: Nginx and other reverse proxy solutions.

## 7. WebSockets & Long Polling

### WebSockets

Provides a persistent connection that allows both client and server to exchange messages.

Common use cases:

- Chat.
- Live updates.
- Real-time collaboration.

### Long Polling

The client sends a request, and the server holds it until data becomes available or a timeout occurs. The client then sends another request.

### Topics to Learn

- Polling vs. long polling vs. WebSockets.
- Connection management.
- Heartbeats.
- Reconnection.
- Multi-server connection routing.
- Redis Pub/Sub or message brokers.
- Slow clients.
- Backpressure.

### Resource

- [A Guide to Async Patterns in API Design](https://www.google.com/search?q=A+Guide+to+Async+Patterns+in+API+Design)

## Week 3 Practice

- Explain the request lifecycle from browser to backend.
- REST or WebSockets for a chat application?
- When would you choose REST over gRPC?
- What problems can incorrect HTTP status codes cause?
- How should a WebSocket client reconnect after a server restart?

---

# Week 4 — Distributed Systems

**Goal:** Understand network failures, coordination, consistency, and partial failures.

## 1. Distributed Systems & Fault Tolerance

A distributed system consists of independent machines or processes that communicate over a network.

### Topics to Learn

- Partial failure.
- Network latency.
- Packet loss.
- Timeout vs. failure.
- Retry.
- Exponential backoff.
- Jitter.
- Replication.
- Redundancy.
- Failover.
- Observability: logs, metrics, traces.

One server failing does not necessarily mean the entire system must fail. Good architecture limits the impact of failures.

## 2. Consistency Models

Consistency models describe how and when data becomes visible across replicas.

### Topics to Learn

- Strong consistency.
- Linearizability.
- Eventual consistency.
- Read-your-writes consistency.
- Causal consistency.
- Monotonic reads.

**Example:**

- Profile photo updates may require read-your-writes behavior.
- Social media like counts may tolerate eventual consistency.

## 3. Quorum

Quorum strategies define how many replicas must acknowledge operations.

Assume a system has five replicas:

- `N = 5`: Total replicas.
- `W = 3`: Write acknowledgements.
- `R = 3`: Replicas contacted for a read.

A common overlap condition is:

```text
R + W > N
```

Here:

```text
3 + 3 > 5
```

This ensures overlap between the read and write replica sets under the assumed replica-set model.

However, overlap alone does not guarantee latest-value reads or strong consistency. The replication protocol, versioning, conflict resolution, and read/write semantics matter.

### Topics to Learn

- Read quorum.
- Write quorum.
- Replica failure.
- Consistency vs. availability.
- Quorum limitations.

## 4. Leader/Follower Architecture

A leader handles writes while followers receive replicated updates.

### Topics to Learn

- Leader election.
- Replication logs.
- Failover.
- Failure detection.
- Split brain.
- Fencing.
- Stale leader risks.
- Multi-leader vs. leader/follower replication.

**Example:** When a primary database fails, a follower may be promoted. The system must prevent conflicting writes if the old leader returns.

## 5. Consistent Hashing

Consistent hashing distributes keys across nodes using a hash ring.

### Topics to Learn

- Hash ring.
- Virtual nodes.
- Node addition and removal.
- Data remapping.
- Hot partitions.
- Consistent hashing vs. modulo hashing.

Consistent hashing can reduce remapping when nodes change, but it does not automatically guarantee balanced load.

## 6. Merkle Tree

A Merkle Tree is a hash-based tree used to detect differences between datasets.

### Topics to Learn

- Leaf hashes.
- Parent hashes.
- Root hash.
- Data integrity.
- Replica synchronization.
- Anti-entropy repair.

**Example:** Two replicas can compare Merkle Tree hashes to identify which portions of their datasets differ.

## Week 4 Practice

- What happens if two replicas show different wallet balances?
- How would a new leader be elected after a crash?
- What happens if a retry executes the same transaction twice?
- Why can consistent hashing help when nodes are added?
- Where is eventual consistency acceptable?

---

# Week 5 — APIs, Architecture Patterns & Microservices

**Goal:** Design maintainable, scalable, and failure-tolerant application architectures.

## 1. API Design

A good API should be predictable, secure, and maintainable.

### Topics to Learn

- REST resource design.
- Request validation.
- Authentication vs. authorization.
- Pagination and filtering.
- Idempotency keys.
- API versioning.
- Error handling.
- Backward compatibility.
- Synchronous vs. asynchronous APIs.

**Example:** Retrying a payment request should not charge the customer twice.

Possible mechanisms:

- Idempotency keys.
- Durable request records.
- Database uniqueness constraints.
- Atomic transaction strategy.
- Payment reconciliation.

## 2. Architectural Design Patterns

### Publisher-Subscriber (Pub/Sub)

Publishers produce events, and subscribers consume them.

```text
Order Service
     |
     v
Message Broker / Event Bus
     |
     +-------------------+
     |                   |
     v                   v
Email Service      Inventory Service
     |
     v
Analytics Consumer
```

### Topics to Learn

- Producers and consumers.
- Kafka vs. RabbitMQ fundamentals.
- At-least-once delivery.
- Duplicate events.
- Idempotent consumers.
- Dead-letter queues (DLQ).
- Event ordering.
- Retries.
- Eventual consistency.

### Circuit Breaker

Prevents repeated calls to a failing dependency.

**States:**

- **Closed:** Requests flow normally.
- **Open:** Requests are rejected quickly or handled through a fallback.
- **Half-open:** Limited test requests check whether the dependency has recovered.

**Example:** If the Recommendation Service is unavailable, the homepage can still display content without recommendations.

### Topics to Learn

- Timeouts.
- Failure thresholds.
- Recovery policy.
- Fallbacks.
- Retry storms.

### Sharding Pattern

Study sharding from an architectural perspective.

Focus on:

- Data routing.
- Partition keys.
- Cross-shard operations.
- Rebalancing.
- Operational complexity.

### Static Content Hosting

Static assets such as HTML, CSS, JavaScript bundles, images, and videos can be served through object storage and CDNs.

### Topics to Learn

- Object storage vs. application servers.
- CDN edge caching.
- Cache-Control headers.
- Versioned asset filenames.
- Cache invalidation.
- Signed URLs.
- Private asset access.

**Example:** Host the HPC Lab frontend separately from its backend API to reduce unnecessary application-server workload for static assets.

## 3. Monolith vs. Microservices

| Aspect | Monolith | Microservices |
|---|---|---|
| Deployment | Usually deployed together | Independently deployable services |
| Complexity | Lower initial complexity | Higher operational complexity |
| Scaling | Application or module level | Individual services |
| Communication | Often in-process | Network/API/message-based |
| Data | May use a shared database | Service-owned data is common |
| Failure | Process-level impact is possible | Partial failures must be managed |

### Microservices Topics

- Service boundary design.
- Database per service.
- Data ownership.
- API Gateway.
- Service discovery.
- Synchronous vs. asynchronous communication.
- Distributed tracing.
- Saga pattern.
- Distributed transactions.
- Circuit breakers.
- Retries and timeouts.
- Eventual consistency.
- Deployment and monitoring.
- Configuration management.

**Interview Insight:** Microservices are not always the best solution. A modular monolith is often a better starting point for small teams and straightforward CRUD applications.

## Week 5 Practice

- When would you split a monolith into microservices?
- An order was created, but the email failed. What would you do?
- How would you isolate the failure of one service?
- How should consumers handle duplicate messages?
- What if two services need coordinated database updates?

---

# Week 6 — Social Media & Content Platform Design

This week introduces complete system design problems.

## System Design: 8-Step Process

1. **Requirements:** Identify functional and non-functional requirements.
2. **Scale Estimation:** Estimate users, requests/sec, storage, and bandwidth.
3. **API Design:** Define primary endpoints and request/response structures.
4. **Data Model:** Identify entities, keys, indexes, and relationships.
5. **High-Level Architecture:** Design clients, load balancers, services, databases, and caches.
6. **Core Data Flow:** Explain read and write workflows step by step.
7. **Scaling & Failure:** Identify hot keys, replication, retries, and failover.
8. **Trade-offs:** Explain design choices and alternatives.

## 1. Social Media Platform

### Functional Requirements

- User registration and profiles.
- Follow/friend relationships.
- Posts, likes, and comments.
- News feed.
- Notifications.
- Image/video uploads.

### Logical Architecture

```text
Web / Mobile Client
        |
        v
Load Balancer + API Gateway
        |
        +------------------------------+
        |              |               |
        v              v               v
 User/Social       Post/Feed       Notification
   Service          Service           Service
        |
        v
    Media Service

Databases | Cache | Queue | Object Storage | CDN
```

This is a logical architecture. Each component does not necessarily need to be a separate microservice.

## 2. Facebook News Feed

The primary challenge is retrieving and ranking relevant posts efficiently.

### Topics to Learn

- Fan-out on write.
- Fan-out on read.
- Hybrid fan-out.
- Feed ranking.
- Feed cache.
- High-follower accounts.
- Hot-key problems.
- Pagination.
- Feed freshness.

**Fan-out on write:** Generate feed candidates when a post is published.

- Faster feed reads.
- Higher write amplification.

**Fan-out on read:** Collect posts when a user opens the feed.

- Simpler write path.
- More computation during reads.

A hybrid strategy can balance these trade-offs.

## 3. Instagram

### Features

- Photo/video upload.
- Image processing.
- Thumbnails.
- Object storage.
- CDN delivery.
- Follow system.
- Feed generation.
- Likes and comments.
- Notifications.

### Topics to Learn

- Presigned upload URLs.
- Background media processing.
- Metadata DB vs. media storage.
- CDN caching.
- Signed access.
- Large file uploads.
- Retries and resumable uploads.
- Feed and media delivery integration.

Store media binaries in object storage and keep metadata and object references in the database.

## 4. Twitter/X Search

### Features

- Post creation.
- Text search.
- Hashtag search.
- Recent and relevant results.
- Search index updates.

### Topics to Learn

- Inverted indexes.
- Search engine vs. relational database indexes.
- Asynchronous indexing.
- Eventual consistency.
- Ranking.
- Pagination.
- Index rebuilding.
- Stale search results.

The primary database is the source of truth. The search index is typically a derived representation.

## 5. YouTube

### Features

- Video upload.
- Transcoding.
- Video metadata.
- Streaming playback.
- Search and recommendations.
- View counts and analytics.

### Topics to Learn

- Object storage.
- Transcoding pipelines.
- Background job queues.
- Adaptive bitrate streaming.
- HLS/DASH fundamentals.
- CDN edge caching.
- Popular video traffic management.
- Eventual consistency in analytics.

Video transcoding should generally happen asynchronously rather than blocking the upload request until completion.

## 6. Quora

### Features

- Questions and answers.
- Topic following.
- Voting.
- Comments.
- Search and recommendations.

### Topics to Learn

- Relational data modeling.
- Vote uniqueness.
- Duplicate prevention.
- Ranking.
- Caching.
- Search indexing.
- Moderation workflows.
- Read-heavy query optimization.

## 7. Stack Overflow

### Features

- Questions, answers, and comments.
- Tags.
- Voting and reputation.
- Search.
- Accepted answers.
- Moderation.

### Topics to Learn

- Question-answer relationships.
- Many-to-many tag mapping.
- Unique vote constraints.
- Reputation update consistency.
- Search indexing.
- Read-heavy workloads.
- Cache invalidation.

## Week 6 Practice

For every system, document:

1. Five functional requirements.
2. Five non-functional requirements.
3. Main APIs.
4. Data model.
5. Architecture diagram.
6. One write flow and one read flow.
7. One scaling bottleneck.
8. Three failure scenarios.

---

# Week 7 — Dropbox, Typeahead, Rate Limiting & Messenger

## 1. Dropbox — File Storage & Sync

### Features

- File upload/download.
- Folder and file metadata.
- Sharing permissions.
- File versioning.
- Device synchronization.
- Large file uploads.

### Logical Architecture

```text
Client
  |
  v
Upload API / Metadata Service
  |
  +--------------------+
  |                    |
  v                    v
Object Storage     Metadata DB
(File Blobs)       + Sync Events
                       |
                       v
                Other Devices
                Sync Changes
```

### Topics to Learn

- Metadata database vs. object storage.
- Chunked uploads.
- Resumable uploads.
- Checksums.
- Deduplication trade-offs.
- Versioning.
- Conflict resolution.
- Sync cursor/change log.
- Access control.
- Sharing links.

## 2. Typeahead Suggestions

Typeahead displays suggestions while a user types into a search box.

**Example:** Typing `sys` may return:

- system design
- system design interview
- system architecture

### Topics to Learn

- Prefix search.
- Trie.
- Precomputed suggestions.
- Search indexes.
- Caching.
- Debouncing.
- Top-K ranking.
- Hot prefixes.
- High query volume.

Prefix-oriented indexes and caching can reduce unnecessary database scans.

## 3. API Rate Limiting

Rate limiting controls how many requests a client can send within a defined period.

### Topics to Learn

- Fixed Window Counter.
- Sliding Window Log.
- Sliding Window Counter.
- Token Bucket.
- Leaky Bucket.
- Distributed rate limiting.
- Redis atomic operations.
- Per-user limits.
- Per-IP limits.
- Per-API-key limits.
- HTTP 429.
- `Retry-After`.

### Request Flow

```text
Incoming API Request
         |
         v
    Rate Limiter
         |
     +---+---+
     |       |
     v       v
  Allowed  Rejected
     |       |
     v       v
 Forward   HTTP 429
 to API
```

**Example:** Allow 60 requests per user per minute.

With multiple application servers, rate-limit state may need to be shared consistently.

## 4. Messenger

### Features

- One-to-one and group chat.
- Message delivery.
- Online/offline status.
- Read receipts.
- Message history.
- Push notifications.
- Media attachments.

### Topics to Learn

- WebSockets.
- Message brokers.
- Conversation IDs.
- Message sequencing.
- Durable message storage.
- Delivery acknowledgements.
- At-least-once delivery.
- Deduplication.
- Offline message handling.
- Connection gateways.
- Presence service.
- Multi-device synchronization.

### Message Flow

1. Client sends a message through a WebSocket connection.
2. Server validates user authentication and conversation authorization.
3. Message is saved to durable storage.
4. Server attempts delivery if the recipient is online.
5. If offline, the system retains the message for later delivery.
6. Delivery and read receipts are recorded according to their defined semantics.

**Important:** A WebSocket connection alone does not guarantee reliable message delivery. Persistence, acknowledgements, and retry policies are required.

## Week 7 Practice

- How would you upload a 5 GB file?
- How would you return search suggestions within 100 ms?
- How would you enforce shared rate limits across multiple servers?
- How would you store messages for offline recipients?
- How would you identify duplicate messages?

---

# Week 8 — Web Crawler, Yelp, Uber & Ticketmaster

This week focuses on scheduling, graph traversal, geospatial search, real-time location, concurrency, and correctness.

## 1. Web Crawler

A web crawler automatically visits webpages, collects content, and supplies data for search indexing.

### Logical Architecture

```text
Seed URLs
    |
    v
URL Frontier / Priority Queue
    |
    v
Scheduler
    |
    v
Fetch Workers
    |
    v
Parse HTML -> Extract Links
    |
    v
Deduplication
    |
    v
Storage -> Search Index
```

### Topics to Learn

- URL frontier.
- BFS fundamentals.
- Priority scheduling.
- Visited URL tracking.
- Bloom Filter.
- Canonical URLs.
- Duplicate content detection.
- Per-domain rate limiting.
- `robots.txt`.
- Crawl policies.
- Retry and backoff.
- Distributed workers.
- Fault-tolerant queues.
- Checkpointing.

**Interview Challenge:** Prevent unnecessary duplicate crawling while ensuring that worker crashes do not cause queued work to be lost.

## 2. Yelp — Proximity Search

Find restaurants, hospitals, shops, or services near a user's location.

### Features

- Nearby places.
- Radius search.
- Distance calculation.
- Category filters.
- Rating filters.
- Sorting.
- Location updates.

### Topics to Learn

- Latitude and longitude.
- Haversine distance.
- Geospatial indexes.
- Geohash.
- Quadtree.
- Spatial partitioning.
- Bounding-box filtering.
- Nearby results pagination.
- Location updates.
- Cache design.

**Example:** A user in Khilkhet searches for restaurants within 3 km.

The system can retrieve candidate places using a spatial index and then apply exact distance and business filters.

Do not rely solely on matching geohash prefixes. Verify actual distance and account for neighboring cells and boundary cases.

## 3. Uber — Ride Hailing

Ride-hailing systems combine location tracking, matching, pricing, concurrency, and real-time communication.

### Main Components

- Rider Service.
- Driver Service.
- Location Service.
- Matching Service.
- Trip Service.
- Pricing Service.
- Payment Service.
- Notification Service.

### Logical Architecture

```text
Rider App                    Driver App
    |                            |
    v                            v
Ride Request                Live Location
    |                            |
    +-------------+--------------+
                  |
                  v
        Location & Matching
                  |
                  v
         Nearby Eligible Drivers
                  |
                  v
            Driver Assignment
                  |
                  v
              Trip Service
                  |
                  v
Requested -> Accepted -> In Progress -> Completed
                  |
                  v
              Payment
                  |
                  v
            Notifications
```

### Topics to Learn

**Location Tracking**

- GPS updates.
- WebSockets and streaming approaches.
- Geospatial indexes.
- Update frequency.
- Storage costs.

**Driver Matching**

- Nearby eligible drivers.
- Driver availability.
- Matching timeout.
- Assignment concurrency control.
- Preventing incompatible duplicate assignments.

**Trip Lifecycle**

- State machine.
- Valid state transitions.
- Duplicate request prevention.
- Retry and idempotency.
- Trip history.

**Payment**

- Fare calculation.
- Payment authorization.
- Payment capture.
- Payment failure recovery.
- Reconciliation.

**Critical Interview Question:**

Two riders request the same driver simultaneously. How would you atomically assign the driver to one ride and reject or rematch the other request?

Consider database transactions, conditional updates, uniqueness constraints, and assignment state transitions.

## 4. Ticketmaster — Ticket Booking

Ticket booking is a high-contention inventory management problem.

A limited number of seats are available, while thousands of users may try to book simultaneously.

### Features

- Event and seat listing.
- Seat availability.
- Temporary reservation.
- Payment.
- Booking confirmation.
- Cancellation and refund.

### Booking Flow

```text
User Selects a Seat
         |
         v
Inventory Service
Atomic Seat Hold
         |
         v
Temporary Hold + Expiry
         |
         v
Payment Processing
         |
         v
Confirm Booking / Release Hold
```

### Topics to Learn

- Inventory consistency.
- Atomic conditional updates.
- Row-level locking.
- Optimistic concurrency control.
- Temporary holds.
- Hold expiration.
- Idempotency keys.
- Payment coordination.
- Queueing.
- Admission control.
- Hot rows.
- Popular-event traffic.
- Failure recovery.
- Reconciliation.

**Critical Interview Question:**

If 100 users attempt to pay for the same seat simultaneously, how do you ensure that at most one user gets the seat?

### Possible Techniques

- Database transactions.
- Row-level locking.
- Atomic conditional updates.
- Unique constraints.
- Temporary reservations.
- Idempotent payment workflows.

A Redis lock alone is not sufficient to guarantee inventory correctness. Durable database constraints and failure semantics must also be considered.

## Week 8 Practice

- What happens if a crawler worker crashes?
- How would you retrieve nearby search results efficiently?
- What if two rides assign the same driver?
- What happens if payment succeeds but booking confirmation fails?
- How would you prevent bookings after inventory is exhausted?

---

# Daily Learning Routine

Follow a consistent learning process instead of only reading topic names.

## Topic Mastery Checklist

Track each topic using the following checklist:

- [ ] **Definition:** What is the topic?
- [ ] **Problem:** What problem does it solve?
- [ ] **Mechanism:** How does it work internally?
- [ ] **Architecture:** Can I draw its architecture?
- [ ] **Data Flow:** Can I explain the complete request flow?
- [ ] **Trade-offs:** Do I understand its advantages, disadvantages, and alternatives?
- [ ] **Failure:** Can I handle timeouts, crashes, duplicates, and retries?
- [ ] **Interview:** Can I explain it in my own words within 2–5 minutes?

## Daily 2-Hour Routine

| Time | Activity |
|---|---|
| 20 minutes | Revise previous topics |
| 35 minutes | Learn a new concept and its internal mechanisms |
| 25 minutes | Draw architecture diagrams and data flows |
| 25 minutes | Analyze failure scenarios and trade-offs |
| 15 minutes | Practice an interview-style explanation |

## Weekly Revision

- Reserve at least one day each week for revising previous topics.
- Conduct at least one complete mock interview every week.
- If time is limited, study one new topic and revise one previous topic daily.
- Maintain notes for design decisions, alternatives, and lessons learned.
- Practice explaining concepts without relying on notes.

---

# Final Learning Strategy

For every system design problem:

1. Clarify the requirements before choosing technologies.
2. Estimate scale instead of guessing infrastructure needs.
3. Define APIs and data models before drawing detailed architecture.
4. Explain write and read flows separately.
5. Identify bottlenecks and failure scenarios.
6. Discuss consistency, concurrency, and idempotency where relevant.
7. Explain trade-offs and justify architectural decisions.
8. Start simple and introduce complexity only when requirements justify it.

**The goal is not to memorize architectures. The goal is to understand why a design works, where it fails, and how to improve it.**
