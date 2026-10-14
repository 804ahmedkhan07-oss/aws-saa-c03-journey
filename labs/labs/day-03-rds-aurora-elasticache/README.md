# Lab: RDS Multi-AZ, Read Replica, and ElastiCache

Status: architecture walkthrough. These are the steps to build it in the console.

## Objective
Build a database tier with high availability, read scaling, and caching.

## Steps
1. Create a DB subnet group using private subnets in at least two AZs.
2. Create a security group for the database (allow the DB port only from the application security group).
3. Launch an RDS instance (MySQL or PostgreSQL) with **Multi-AZ** enabled.
4. Create a **Read Replica** from the primary and use its endpoint for SELECT queries.
5. Enable automated backups and note the retention period for point-in-time recovery.
6. Create an ElastiCache **Redis** cluster in the same VPC.
7. In the application, use **Lazy Loading**: check the cache first, query the database on a miss, then store the result in the cache.
8. (Optional) Create an **RDS Proxy** in front of the database to pool connections.

## What to verify
- Reboot the primary with failover and confirm the endpoint stays the same.
- Confirm reads work against the Read Replica endpoint.
- Confirm a repeated query is served from the cache.

## Cleanup
Delete in this order: RDS Proxy, Read Replica, primary DB (skip the final snapshot if not needed), ElastiCache cluster, subnet group, security groups.

## Cost note
Multi-AZ RDS and ElastiCache are billed hourly. Delete everything right after the lab.

Back to the notes: [Day 3](../../docs/day-03-rds-aurora-elasticache.md)
