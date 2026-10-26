# Lab: Serverless API, Decoupled Processing, and Fan-Out

Status: architecture walkthrough. These are the steps to build it in the console.

## Objective
Build a serverless API with authentication, decouple processing with SQS, and fan out events with SNS.

## Steps
1. Create a **DynamoDB** table with a partition key.
2. Create a **Lambda** function that reads and writes the table. Give it an execution role with only the needed permissions.
3. Create an **API Gateway** REST API with a route that invokes the Lambda function.
4. Create a **Cognito User Pool**, add it as an authorizer on the API, sign in, and call the API with the JWT.
5. Add throttling (rate and burst limits) on the API stage.
6. Create a **Cognito Identity Pool** linked to the User Pool, with a role allowing upload to one S3 prefix.
7. Create an **SQS queue** and change the API route to send comments to the queue. Add a Lambda that consumes the queue and writes to DynamoDB.
8. Test the **Visibility Timeout**: set it lower than the processing time and observe duplicate processing, then raise it.
9. Create an **SNS topic** with two SQS queues subscribed (fan-out). Publish one message and confirm both queues receive it.
10. Create a **Step Functions** state machine with two Lambda steps and a Choice state.
11. (Optional) Enable **DynamoDB Streams** and trigger a Lambda on item changes.
12. (Optional) Add **Provisioned Concurrency** to a function and compare the first-request latency.
13. (Review only) Compare CloudFront Functions and Lambda@Edge, and DAX and Global Tables, using the Day 9 notes.

## What to verify
- Calls without a valid JWT are rejected.
- Excess requests are throttled by API Gateway.
- Both queues receive the same message from the SNS topic.
- The state machine follows the correct branch.

## Cleanup
Delete in this order: Step Functions state machine, API Gateway API, Lambda functions, event source mappings, SNS subscriptions and topic, SQS queues, Cognito Identity Pool and User Pool, DynamoDB table, IAM roles.

## Cost note
Most of this stays in the free tier at low volume. Provisioned Concurrency and DAX are billed hourly, so remove them right after the lab.

Back to the notes: [Day 9](../../docs/day-09-serverless-and-decoupling.md)
