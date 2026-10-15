# Lab: Stateless Web App, Session Store, Shared Files, and Elastic Beanstalk

Status: architecture walkthrough. These are the steps to build it in the console.

## Objective
Build a scalable web tier, externalize session state, share files across instances, and deploy an app with Elastic Beanstalk.

## Steps
1. Create a VPC with public and private subnets across at least two AZs.
2. Create three security groups: ALB (80/443 from internet), EC2 (from ALB SG), and data tier (from EC2 SG).
3. Create an ALB in the public subnets and a Target Group.
4. Create a Launch Template and an Auto Scaling Group in the private subnets, attached to the Target Group.
5. Create an ElastiCache Redis cluster and store sessions in it using a `session_id` cookie.
6. Create an RDS Multi-AZ database and one Read Replica.
7. Create an EFS file system with mount targets in each AZ and mount it on the instances at `/wp-content/uploads`.
8. Create a Route 53 Alias record pointing the domain to the ALB.
9. Deploy a sample app with **Elastic Beanstalk** and compare what it created (EC2, ALB, ASG) with the manual setup.

## What to verify
- Stopping one instance does not break the site.
- A session created on one instance is still valid when the request lands on another.
- A file uploaded on one instance is visible from the other.
- The EC2 instances are not reachable directly from the internet.

## Cleanup
Delete in this order: Elastic Beanstalk environment, Route 53 record, Auto Scaling Group, ALB and Target Group, Launch Template, RDS Read Replica and database, ElastiCache cluster, EFS file system, security groups.

## Cost note
RDS Multi-AZ, ElastiCache, ALB, and EFS are billed. Delete everything right after the lab.

Back to the notes: [Day 5](../../docs/day-05-classic-architectures.md)
