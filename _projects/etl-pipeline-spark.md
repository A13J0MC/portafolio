---
title: "Batch ETL Pipeline with Apache Spark"
description: "End-to-end batch pipeline that ingests raw e-commerce events from S3, transforms them with PySpark, and loads enriched data into Redshift for BI reporting."
icon: "⚡"
category: "Batch Processing"
status: completed
featured: true
tech_stack:
  - Python
  - PySpark
  - Apache Airflow
  - AWS S3
  - AWS Redshift
  - AWS Glue
  - Terraform
github: "https://github.com/AlejandroMendoza/etl-pipeline-spark"
---

## Overview

This project implements a production-grade batch ETL pipeline for an e-commerce platform generating ~10 million events per day. The pipeline ingests raw JSON click-stream data from S3, runs a series of PySpark transformations, and loads clean, aggregated tables into Amazon Redshift for consumption by BI and analytics teams.

## Architecture

```
S3 Raw Zone → Glue Crawler → PySpark Jobs (EMR) → S3 Curated Zone → Redshift
                                                        ↑
                                               Airflow DAG (orchestration)
```

The pipeline follows the **Medallion Architecture**:

- **Bronze layer** — raw JSON as-is, partitioned by date.
- **Silver layer** — cleaned and deduplicated Parquet files, schema-enforced.
- **Gold layer** — business-level aggregates loaded into Redshift dimension/fact tables.

## Key Features

- **Schema evolution** — handles upstream schema changes without breaking downstream jobs using Glue Schema Registry.
- **Idempotency** — each DAG run writes to a staging table and swaps partitions atomically, enabling safe reruns.
- **Data quality checks** — Great Expectations suite validates row counts, null rates, and referential integrity before each load.
- **Alerting** — Slack notifications on pipeline failure or SLA breach via Airflow callbacks.
- **Infrastructure as Code** — all AWS resources provisioned with Terraform.

## Results

- Processes **50M+ records/day** within a 2-hour SLA.
- Reduced data freshness latency from 12 hours (legacy CRON jobs) to **under 3 hours**.
- Achieved **99.8% pipeline uptime** over 6 months in production.

## Data Quality

```python
# Example Great Expectations check
suite.add_expectation(
    ExpectationConfiguration(
        expectation_type="expect_column_values_to_not_be_null",
        kwargs={"column": "user_id", "mostly": 0.99}
    )
)
```

## How to Run

```bash
# Clone and set up environment
git clone https://github.com/AlejandroMendoza/etl-pipeline-spark
cd etl-pipeline-spark
pip install -r requirements.txt

# Run locally with Docker Compose (Airflow + Spark)
docker-compose up -d

# Trigger a backfill
airflow dags backfill ecommerce_etl --start-date 2024-01-01 --end-date 2024-01-31
```
