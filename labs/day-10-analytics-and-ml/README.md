# Lab: Serverless Analytics on S3 and ML Services

Status: architecture walkthrough. These are the steps to build it in the console.

## Objective
Query data in S3 with Athena, convert it to Parquet with Glue, stream data through Kinesis and Firehose, and try the ML services.

## Steps
1. Create an S3 bucket and upload a small CSV file under a prefix such as `raw/`.
2. Create a **Glue Crawler** pointing to `raw/`. Run it and check the table in the Glue Data Catalog.
3. Open **Athena**, set a query result location in S3, and run a `SELECT` on the table.
4. Create a **Glue ETL job** that writes the data as **Parquet** under `parquet/`. Run the same query on both and compare data scanned.
5. Create a **Kinesis Data Stream** and send test records.
6. Create a **Data Firehose** delivery stream to S3 and note the buffer size and interval.
7. (Optional) Create a **Managed Service for Apache Flink** application that reads the stream.
8. (Optional) Create an **OpenSearch** domain, index a few log lines, and search them.
9. Run **Textract** on a sample invoice image and view the extracted text and tables.
10. Run **Polly** on a short text and download the MP3.
11. Run **Rekognition** on a sample image, **Transcribe** on a short audio file, and **Comprehend** on a sample sentence.
12. (Review only) Compare DocumentDB, Neptune, Keyspaces, and Timestream using the tables in the Day 10 notes.

## What to verify
- The Glue Crawler creates a table with the correct columns.
- The Parquet query scans less data than the CSV query.
- Firehose delivers files to S3 after the buffer interval.
- Textract returns text and table cells from the invoice.

## Cleanup
Delete in this order: Flink application, Firehose delivery stream, Kinesis Data Stream, OpenSearch domain, Glue ETL job, Glue Crawler and Catalog tables, Transcribe jobs, S3 buckets (including Athena result location), IAM roles.

## Cost note
OpenSearch domains, Kinesis Data Streams, and Flink applications are billed hourly. Athena is billed by data scanned. Delete everything right after the lab.

Back to the notes: [Day 10](../../docs/day-10-databases-analytics-ml.md)
