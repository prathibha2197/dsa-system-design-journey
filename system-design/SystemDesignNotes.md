# System Design Journey

## Day 1: May 10, 2026 - Load Balancer

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