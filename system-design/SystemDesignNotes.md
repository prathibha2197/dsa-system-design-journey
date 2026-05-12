# System Design Journey

## Day 1: May 09, 2026 - Load Balancer

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


## Day 2: Database Indexing

1. **Index**: Book lo index laaga, data fast ga find cheyadaniki
2. **B-Tree Index**: Default. Range queries ki best. O(log n)
3. **Hash Index**: = operator ki matrame. O(1) but range queries radu
4. **Trade-off**: Read fast, Write slow. Extra storage padutundi
5. **When**: WHERE, JOIN, ORDER BY lo use chese columns ki pettu


System Design Journey
Day 3: May 11, 2026 - Caching
Problem: Database ki every request hit chesthe slow avthundi. High traffic lo DB crash avvachu.
Solution: Cache = Frequently used data ni RAM lo store cheyyadam for fast access.
Types of Caching:
Client-side cache → Browser cache (images, CSS, JS)
CDN cache → Cloudflare, Akamai (static content near users)
Server-side cache → Redis, Memcached (API/DB results)
Database cache → Query cache, buffer cache
Cache Patterns:
Cache-Aside → App first cache check chesthundi, miss aithe DB nunchi fetch
Write-Through → DB + Cache both update at same time
Write-Behind → Cache first, DB later (async)
Eviction Policies:
LRU → Least Recently Used (most common)
LFU → Least Frequently Used

Interview Question:
Cache Stampede enti?
→ Cache expire ayyaka sudden ga huge requests DB ki velladam

Solution:

Locking (mutex)
Stale cache serving
Day 4: May 12, 2026 - Sharding vs Replication
Problem: Single database cannot handle high traffic + large data.
Replication
Definition: Same data multiple databases lo copy chestham.
Types:
Master-Slave → Write master ki, Read slaves nunchi
Master-Master → Both can write (conflict possible)
Use Case:
Read-heavy systems (Instagram, Facebook feed)
Sharding
Definition: Data ni split chesi multiple databases lo store cheyyadam.
Types:
Horizontal → Rows split (User 1–1M, 1M–2M)
Vertical → Tables split (User DB, Orders DB)
Sharding Key:
user_id
geo location
hash-based partition
Trade-off:

Replication:

Easy reads scaling
Data duplicate

Sharding:

Scales both read + write
Complex system
Interview Question:
Sharded DB lo JOIN ela chestam?
→ App-level join or denormalization
→ Cross-shard JOIN is expensive