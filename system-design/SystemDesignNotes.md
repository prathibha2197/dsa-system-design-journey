# System Design 
---

##  Load Balancer

1. Problem: If 1M users access a single server, it may crash.

2. Solution: A Load Balancer distributes traffic across multiple servers.

3. Types:
   - L4 Load Balancer → TCP/UDP level
   - L7 Load Balancer → HTTP/HTTPS level

4. Examples:
   - Nginx
   - AWS ELB (Elastic Load Balancer)

5. Interview Question:
   Why not just use a bigger server?
   → Single point of failure + expensive + limited scaling.

---

##  Database Indexing

1. Index: Book lo index laaga, data fast ga find cheyadaniki

2. B-Tree Index: Default. Range queries ki best. O(log n)

3. Hash Index: = operator ki matrame. O(1) but range queries radu

4. Trade-off:
   Read fast, Write slow. Extra storage padutundi

5. When:
   WHERE, JOIN, ORDER BY lo use chese columns ki pettu

---

## Caching

1. Problem:
   Database ki every request hit chesthe slow avthundi. High traffic lo DB crash avvachu.

2. Solution:
   Cache = Frequently used data ni RAM lo store cheyyadam for fast access.

3. Types:
   - Client-side cache → Browser cache (images, CSS, JS)
   - CDN cache → Cloudflare, Akamai (static content near users)
   - Server-side cache → Redis, Memcached (API/DB results)
   - Database cache → Query cache, buffer cache

4. Cache Patterns:
   - Cache-Aside → App first cache check chesthundi, miss aithe DB nunchi fetch
   - Write-Through → DB + Cache both update at same time
   - Write-Behind → Cache first, DB later (async)

5. Eviction Policies:
   - LRU → Least Recently Used (most common)
   - LFU → Least Frequently Used

6. Interview Question:
   Cache Stampede enti?
   → Cache expire ayyaka sudden ga huge requests DB ki velladam

   Solution:
   - Locking (mutex)
   - Stale cache serving

---

## Sharding vs Replication

1. Problem:
   Single database cannot handle high traffic + large data.


### Replication

2. Definition:
   Same data multiple databases lo copy chestham.

3. Types:
   - Master-Slave → Write master ki, Read slaves nunchi
   - Master-Master → Both can write (conflict possible)

4. Use Case:
   Read-heavy systems (Instagram, Facebook feed)



### Sharding

5. Definition:
   Data ni split chesi multiple databases lo store cheyyadam.

6. Types:
   - Horizontal → Rows split (User 1–1M, 1M–2M)
   - Vertical → Tables split (User DB, Orders DB)

7. Sharding Key:
   - user_id
   - geo location
   - hash-based partition


8. Trade-off:

Replication:
- Easy reads scaling
- Data duplicate

Sharding:
- Scales both read + write
- Complex system


9. Interview Question:
Sharded DB lo JOIN ela chestam?
→ App-level join or denormalization
→ Cross-shard JOIN is expensive

---

## CAP Theorem

### CAP Theorem
CAP = Consistency + Availability + Partition Tolerance

In distributed systems, when network partition happens, we can only choose 2 out of 3.

👉 Pick 2: C, A, P



### Diagram

        C
       / \
      /   \
    CP     AP
      \   /
       \ /
        P

Meaning:
During network failure, system must choose between:
- CP (Consistency + Partition Tolerance)
- AP (Availability + Partition Tolerance)



### 5 Key Points

1. Consistency:
   Every read gets the latest write

2. Availability:
   Every request gets a response (no downtime)

3. Partition Tolerance:
   System works even if network breaks

4. CA is not possible in distributed systems:
   Because network failures are unavoidable

5. Real-world choice:
   - CP → Banking, payments (correct data first)
   - AP → Social media, chat apps (always available)

### Examples

CP Systems:
- MongoDB (strict mode)
- HBase
- Banking systems
- Trading systems

AP Systems:
- Cassandra
- DynamoDB
- Social media feeds
- Shopping cart systems

---

## Message Queue 

### Problem

Order service directly calling Email service → if Email service is down, Order fails → **tight coupling**

### Solution

Message Queue acts as a **middleman**

- Order service sends message
- Email service consumes when available

### Pub/Sub vs Queue

#### Queue (1 → 1)

One message → one consumer

Examples:
- Image processing
- Background jobs

#### Pub/Sub (1 → many)

One message → multiple consumers

Examples:
- Order placed → Email + SMS + Inventory

### Kafka vs RabbitMQ

#### Kafka
- Distributed log system
- High throughput
- Replay messages possible

Use cases:
- Event streaming
- Analytics
- Uber location tracking

#### RabbitMQ
- Message broker
- Flexible routing
- Low latency

Use cases:
- Task queues
- Background jobs

### Why not direct API calls?

- ❌ Tight coupling
- ❌ No buffering
- ❌ Retry failures
- ❌ No backpressure handling

### Real World Example (Flipkart)

Order placed →
→ order_created event goes to queue
→ Email service consumes → sends email
→ Inventory service updates stock
→ Shipping service starts delivery

### Key Idea
Services should communicate via **events**, not direct calls.
