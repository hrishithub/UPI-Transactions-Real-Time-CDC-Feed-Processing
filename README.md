# UPI Transactions Real Time CDC Feed Processing

## Introduction
This project demonstrates a real-time Change Data Capture (CDC) pipeline built entirely within the Databricks ecosystem using Spark Structured Streaming and Delta Lake. It simulates UPI merchant transactions with changing status (e.g., initiated → completed → refunded) and applies streaming CDC consumption to maintain aggregated financial metrics per merchant.

## Problem Statement
In digital payment ecosystems, it's crucial to process transaction changes (status updates, corrections, refunds) in near real-time for downstream systems like dashboards and financial reporting. Traditional batch-based pipelines are inefficient and lack the responsiveness required for such use cases. This project addresses that by using Delta Lake’s Change Data Feed (CDF) feature to process inserts and updates efficiently in micro-batches, while maintaining accurate aggregates per merchant.

## Architecture
The solution runs entirely within Databricks, with two scheduled notebooks:
- Mock Data Generator Notebook: Simulates UPI transaction events (inserts and updates) via Delta Lake merge operations to a source table.
- Real-Time Aggregator Notebook: Reads changes using Delta’s read Change Data Feed (CDF) and applies micro-batch aggregations using Structured Streaming.

## Technology Used
- Databricks
- Delta Lake
- Spark Structured Streaming
- Databricks Workflows
- PySpark

## Dataset Used
No external dataset was used in this project. Instead, mock data was generated to simulate real-world UPI merchant payment transactions. The data captures various state transitions (e.g., initiated → completed → refunded) to effectively model real-time transaction updates and enable Change Data Capture (CDC) processing in the pipeline.

- `raw_upi_transactions`: transaction_id, upi_id, merchant_id, transaction_amount, transaction_timestamp, transaction_status

Transactions evolve through different statuses: initiated, completed, failed, refunded.

## Scripts for Project
#### upi_merchant_pay_trx_mock_data.ipynb:
- Simulates three batches of transactions with inserts and updates.
- Applies Delta Lake merge logic to upsert into the source Delta table.
- Enables CDC by triggering updates (e.g., status changes) over time. (i.e. When you set up table property enableChangeDataFeed = True, it enable tracking row level changes like (e.g., inserts, update pre-images, update post-images, deletes) and it automatically includes metadata columns like _change_type, _commit_version, and _commit_timestamp for each row in table) 

#### realtime_merchant_aggregation.ipynb
- Consumes CDC feed from the source table using readChangeFeed.
 Filters for inserts and update_postimage change types.
- Aggregates total sales, total refunds, and net sales per merchant.
- Upserts the aggregated results into a target Delta table aggregated_upi_transactions.

## Output and Impact
- Captures real-time updates and status changes on UPI transactions.
- Maintains merchant-level KPIs such as: Total Sales, Total Refunds, Net Sales
- Ensures consistency and idempotence via Delta Lake’s transactional capabilities.
- Pipeline scheduled and orchestrated using Databricks Workflows to run notebooks sequentially.

## My Learnings
- Simulated CDC workflows using merge operations and Delta Lake.
- Implemented real-time streaming aggregations using Spark Structured Streaming.
- Gained hands-on experience with Delta Lake CDF and readChangeFeed API.
- Designed and scheduled end-to-end pipelines within the Databricks ecosystem.


