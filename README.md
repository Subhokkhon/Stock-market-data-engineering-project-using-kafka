# Stock-market-data-engineering-project-using-kafka
Developed a real-time stock market data engineering pipeline using Apache Kafka to stream, process, and store high-velocity market feeds with millisecond latency for scalable analytics.
This project implements a real-time stock market data streaming pipeline using Apache Kafka, AWS S3, AWS Glue, and Amazon Athena.
A Python-based Kafka Producer simulates live stock prices from a dataset and streams them into Kafka topics.
A Kafka Consumer ingests the streaming data and stores it in Amazon S3, where it is automatically cataloged by AWS Glue, enabling real-time querying through Amazon Athena.

🏗️ System Architecture

Below is the architecture you provided, integrated with the README:

🔄 Overall Workflow
Dataset → Python Producer → Kafka (on EC2) → Python Consumer → S3 → Glue Crawler → Glue Data Catalog → Athena

🚀 Key Features
🎯 Real-Time Data Streaming

Streams high-velocity stock market events using Apache Kafka with millisecond latency.

Python producer simulates real-world price movement using a CSV dataset.

🗃️ Cloud-Scale Data Storage

Kafka consumer writes streaming data to Amazon S3 in JSON/CSV format.

S3 acts as a data lake for historical and real-time analytics.

🧹 Automated Schema Discovery

AWS Glue crawler scans S3 and automatically updates the Data Catalog.

Enables schema-on-read for downstream analytics.

🔎 Interactive SQL Analytics

Amazon Athena queries S3 data serverlessly using standard SQL.

Supports aggregations, time-series analysis, and trend detection.

🧰 Tech Stack
Data Streaming

Apache Kafka (on Amazon EC2)

Kafka Producer & Consumer (Python)

Cloud Services

AWS S3 (Data Lake)

AWS Glue Crawler + Data Catalog

Amazon Athena (Query Engine)

Programming

Python 3.13

Libraries: kafka-python, boto3, pandas, json

1️⃣ Prerequisites

AWS account

EC2 instance with Kafka installed

S3 bucket created

Glue Crawler IAM permissions

Athena query results bucket

2️⃣ Start Kafka on EC2
zookeeper-server-start.sh config/zookeeper.properties
kafka-server-start.sh config/server.properties

3️⃣ Create Kafka Topic
kafka-topics.sh --create \
--topic stock_stream \
--bootstrap-server localhost:9092 \
--replication-factor 1 \
--partitions 1

4️⃣ Run the Producer

Streams data from dataset.csv:

python producer/producer.py

5️⃣ Run the Consumer

Consumes Kafka messages and uploads to S3:

python consumer/consumer.py

6️⃣ Configure AWS Glue Crawler

Source: S3 bucket folder

Destination: Glue Data Catalog

Run crawler → Table appears under database

7️⃣ Query Data with Amazon Athena

Example query:

SELECT symbol, AVG(close_price) AS avg_price
FROM stock_market_data
GROUP BY symbol
ORDER BY avg_price DESC;

🧪 Sample Producer Workflow

The Producer:

Loads dataset.csv

Reads line-by-line

Sends stock event to Kafka topic as JSON:

{
  "symbol": "AAPL",
  "open": 187.25,
  "close": 188.91,
  "volume": 24300000,
  "timestamp": "2024-05-01T09:30:00Z"
}

🪣 Sample Consumer Workflow

The Consumer:

Reads messages from Kafka

Stores them in S3:

s3://your-bucket/stock-data/date=2025-02-10/part-00001.json

Partitioning improves Athena performance.

📊 Analytics & Insights Enabled

With Athena + partitioned S3 data, you can analyze:

📈 Real-time price fluctuations

🕒 Symbol-wise time-series trends

🔥 Market volatility per sector

🏅 Top performing stocks by average return

⚡ High-velocity trades & anomalies



Serverless, scalable querying for stock market insights
