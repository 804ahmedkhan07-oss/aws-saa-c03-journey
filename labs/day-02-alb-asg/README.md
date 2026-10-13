# Lab: ALB with Auto Scaling Group

## Objective
Run a web app behind an Application Load Balancer across two AZs with automatic scaling.

## Steps
1. Create a security group for the ALB (allow port 80 from anywhere).
2. Create a security group for the instances (allow port 80 only from the ALB security group).
3. Create a Target Group (instance type, HTTP:80).
4. Create an internet-facing ALB in at least two AZ subnets and forward to the Target Group.
5. Create a Launch Template (AMI, instance type, instance security group, user data).
6. Create an Auto Scaling Group from the template, attach the Target Group, set min/desired/max.
7. Add a target tracking policy on `RequestCountPerTarget`.
8. Open the ALB DNS name in a browser and confirm the response.

## Cleanup
Delete in this order: Auto Scaling Group, ALB, Target Group, Launch Template, security groups.

## Result
Traffic is served from instances across multiple AZs, and the instances are not directly reachable from the internet.

Back to the notes: [Day 2](../../docs/day-02-ha-elb-asg.md)
