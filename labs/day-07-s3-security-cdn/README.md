# Lab: S3 Security, CloudFront with OAC, and Global Accelerator

Status: architecture walkthrough. These are the steps to build it in the console.

## Objective
Encrypt and protect S3 data, share it with a pre-signed URL, serve it privately through CloudFront, and compare with Global Accelerator.

## Steps
1. Create a bucket. Keep **Block Public Access** on and confirm default encryption is **SSE-S3**.
2. Change default encryption to **SSE-KMS** with a KMS key. Upload a file and check the CloudTrail event for the key use.
3. Add a **CORS** configuration that allows one specific origin and the GET method.
4. Enable **Versioning**, then enable **Object Lock** when creating a second test bucket. Set a short retention in **Governance mode** and try to delete a version.
5. Generate a **Pre-Signed URL** for an object (console or `aws s3 presign`) with a short expiry. Open it in a private browser window, then try again after it expires.
6. Create an **Access Point** for one team with its own policy.
7. (Optional) Create an **S3 Object Lambda Access Point** with a Lambda function that masks a field in a text file.
8. Create a **CloudFront distribution** with the bucket as origin. Choose **Origin Access Control (OAC)** and apply the generated bucket policy.
9. Confirm the S3 URL is denied and the CloudFront URL works.
10. Update the file and run an **Invalidation** for `/*` (or a specific path). Confirm the new version is served.
11. Add **Geo Restriction** (blocklist one country) in the distribution settings.
12. Create a **Global Accelerator** with a listener on TCP 80 and an ALB endpoint. Note the 2 static IPs and test access through them.

## What to verify
- Direct S3 access is denied; access through CloudFront works.
- The pre-signed URL stops working after expiry.
- A Governance mode object cannot be deleted without the bypass permission.
- The Global Accelerator static IPs reach the application.

## Cleanup
Delete in this order: Global Accelerator, CloudFront distribution (disable first, then delete), Object Lambda Access Point and Lambda function, Access Points, empty buckets (all versions; Governance mode objects need bypass, Compliance mode objects cannot be deleted until retention ends), then the buckets and the KMS key (scheduled deletion).

## Cost note
CloudFront, Global Accelerator, KMS keys, and KMS requests are billed. Use only Governance mode with a short retention in tests. Never use Compliance mode in a test, because the data cannot be deleted until retention ends. Delete everything right after the lab.

Back to the notes: [Day 7](../../docs/day-07-s3-security-cloudfront-global-accelerator.md)
