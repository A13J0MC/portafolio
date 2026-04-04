---
title: "Real-Time Streaming Pipeline with Kafka & Flink"
description: "Real-time event streaming pipeline that processes user activity events at 100K events/sec, detects anomalies, and delivers live dashboards with sub-second latency."
icon: "🌊"
category: "Streaming"
status: completed
featured: true
tech_stack:
  - Apache Kafka
  - Apache Flink
  - Python
  - PostgreSQL
  - Grafana
  - Docker
  - Kubernetes
github: "https://github.com/AlejandroMendoza/streaming-pipeline"
---

## Overview

A real-time data streaming system built to detect fraudulent transactions and surface live operational metrics for a fintech platform. The pipeline consumes events from Kafka, applies windowed aggregations and ML-based anomaly scoring in Flink, and serves results to downstream dashboards and alerting systems.

## Architecture

```
Producer Services
      ↓
Kafka (partitioned topics)
      ↓
Flink Streaming Job
  ├── Windowed aggregations (1min / 5min / 15min)
  ├── Anomaly detection (scoring via external ML model)
  └── Enrichment (join with dimension data from Redis)
      ↓
  ┌───────────────────┐
  │  PostgreSQL (OLTP)│  ← alerts & flagged transactions
  │  Kafka (DLQ)      │  ← malformed events
  │  S3 (archive)     │  ← full event log
  └───────────────────┘
```

## Key Features

- **Low-latency processing** — end-to-end latency under 500ms at P99.
- **Exactly-once semantics** — Flink checkpoints with Kafka offsets for guaranteed delivery.
- **Backpressure handling** — auto-scaling consumer groups with consumer lag monitoring.
- **Dead letter queue** — malformed or unprocessable events routed to a DLQ for manual inspection.
- **Observability** — custom Flink metrics exported to Prometheus and visualized in Grafana.

## Flink Job Snippet

```python
# Windowed aggregation with PyFlink
from pyflink.datastream import StreamExecutionEnvironment
from pyflink.datastream.window import TumblingEventTimeWindows
from pyflink.common.time import Time

env = StreamExecutionEnvironment.get_execution_environment()

stream = (
    env.add_source(kafka_consumer)
       .key_by(lambda e: e["user_id"])
       .window(TumblingEventTimeWindows.of(Time.minutes(5)))
       .aggregate(TransactionAggregator())
)
```

## Results

- Handles **100K+ events/second** with horizontal scaling on Kubernetes.
- Detected **15% more fraud cases** compared to the previous batch-based approach.
- Reduced time-to-alert from 30 minutes to **under 30 seconds**.
