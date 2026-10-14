# Lab: Route 53 Hosted Zone, Routing Policies, and Health Checks

Status: architecture walkthrough. These are the steps to build it in the console.

## Objective
Route traffic to web endpoints using Alias, Weighted, Failover, and Latency records, and connect on-premises DNS using Route 53 Resolver.

## Steps
1. Create a **Public Hosted Zone** for your domain.
2. If the domain is registered elsewhere, copy the 4 NS values from the hosted zone into the registrar's name server settings.
3. Create an **A record with Alias** at the root domain pointing to an Application Load Balancer.
4. Create two **Weighted** records for the same name (for example 90 and 10) pointing to two endpoints.
5. Create a **Health Check** for the primary endpoint (HTTP on port 80).
6. Create **Failover** records: primary with the health check, secondary as the backup.
7. Create **Latency** records for endpoints in two Regions.
8. Create a **Private Hosted Zone** and associate it with a VPC. Resolve a private name from an EC2 instance using `nslookup`.
9. For a private resource, create a CloudWatch alarm and a health check based on that alarm.
10. (Optional) Create Route 53 Resolver **Inbound** and **Outbound** endpoints with a forwarding rule.

## What to verify
- `nslookup` or `dig` on the domain returns the expected target.
- Stopping the primary endpoint makes the Failover record return the secondary.
- Weighted records return both endpoints over repeated queries.

## Cleanup
Delete in this order: Resolver endpoints and rules, health checks, extra DNS records, private hosted zone, public hosted zone (if no longer needed).

## Cost note
Hosted zones, health checks, and Resolver endpoints are billed. Resolver endpoints are charged hourly, so delete them right after the lab.

Back to the notes: [Day 4](../../docs/day-04-route53-dns.md)
