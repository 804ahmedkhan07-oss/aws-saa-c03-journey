# Lab: Hybrid Storage, Migration Services, and ECS on Fargate

Status: architecture walkthrough. These are the steps to build it in the console.

## Objective
Review the hybrid storage and migration options, then run a containerized app on ECS Fargate with the correct IAM roles, shared EFS storage, and auto scaling.

## Steps
1. Create an **ECR** private repository.
2. Build a small Docker image locally and push it to ECR (use the "View push commands" shown in the console).
3. Create an **EFS** file system with mount targets in each AZ.
4. Create two IAM roles: a **Task Execution Role** (ECR pull, CloudWatch Logs) and a **Task Role** (S3 or DynamoDB access your app needs).
5. Create an **ECS cluster** and a **Task Definition** for **Fargate**. Select both roles, point to the ECR image, and add the EFS volume mount.
6. Create an **ECS Service** with at least 2 tasks across 2 AZs, attached to an **ALB** target group.
7. Add **Service Auto Scaling** with target tracking on `RequestCountPerTarget`.
8. Open the ALB DNS name and confirm the app responds.
9. (Optional) Create an **AWS DataSync** task between a source location and an S3 bucket and run it once.
10. (Optional) Create a **Transfer Family** SFTP server pointing to an S3 bucket and connect with an SFTP client.
11. (Review only) Compare Snowball Edge, Storage Gateway types, and FSx options using the tables in the Day 8 notes.

## What to verify
- Tasks start successfully and pull the image from ECR.
- Logs appear in CloudWatch Logs (Task Execution Role works).
- The app can reach S3 or DynamoDB (Task Role works).
- A file written by one task is visible from another task (EFS works).
- Stopping a task makes the service start a replacement.

## Cleanup
Delete in this order: ECS service (set desired count to 0 first), ECS cluster, ALB and target group, Transfer Family server, DataSync task, EFS file system, ECR images and repository, IAM roles, security groups.

## Cost note
Fargate tasks, ALB, EFS, Transfer Family endpoints (billed hourly), and DataSync are billed. Delete everything right after the lab.

Back to the notes: [Day 8](../../docs/day-08-storage-hybrid-and-containers.md)
