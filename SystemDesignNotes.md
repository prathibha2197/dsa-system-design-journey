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