# Lab: S3 Versioning, Replication, Lifecycle, Events, and Storage Lens

Status: architecture walkthrough. These are the steps to build it in the console.

## Objective
Create a secure, versioned S3 bucket with replication, lifecycle rules, event notifications, and organization-level visibility.

## Steps
1. Create a bucket with a globally unique name. Keep **Block Public Access** on.
2. Enable **Versioning**. Upload a file, upload it again, and view both versions.
3. Delete the file and observe the **Delete Marker**. Remove the marker to restore the file.
4. Create a second bucket in another Region and enable Versioning on it.
5. Create a **Replication rule** (CRR) from the first bucket to the second. Upload a new object and confirm it appears in the destination.
6. Create a **Lifecycle rule**: move to Standard-IA after 30 days, Glacier Flexible Retrieval after 90 days, expire after 365 days.
7. Create an **Event Notification** for object create events that triggers a Lambda function (or sends to an SQS queue).
8. Enable **Static Website Hosting** on a test bucket and open the HTTP endpoint. Note that HTTPS needs CloudFront.
9. Enable **Transfer Acceleration** and compare upload speed using the accelerated endpoint.
10. Upload a large file using **Multipart Upload** (AWS CLI or console).
11. Open **Storage Lens** and review the default dashboard. Note which metrics need the Advanced tier.
12. (Optional) Turn on **Requester Pays** and read the bucket from another account.

## What to verify
- Deleting a versioned object does not remove older versions.
- A new object appears in the destination bucket after replication.
- The Lambda function or queue receives an event after each upload.
- The site is not public unless a bucket policy and Block Public Access settings allow it.

## Cleanup
Empty buckets first (delete all versions and delete markers), then delete in this order: event notifications, replication and lifecycle rules, Lambda function, both buckets, and any extra Storage Lens dashboards.

## Cost note
Replication, Transfer Acceleration, Advanced Storage Lens, and cross-Region data transfer are billed. Delete everything right after the lab.

Back to the notes: [Day 6](../../docs/day-06-s3-fundamentals-and-management.md)
