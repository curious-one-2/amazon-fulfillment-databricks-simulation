# amazon-fulfillment-databricks-simulation

# Architecting a Real-Time Medallion Pipeline for Warehouse Operations

# Project Inspiration

Inspired by a tour of an Amazon Fulfillment Center, this project simulates the high-velocity data lifecycle of automated inventory movement, robotic picking, and order shipment. The goal was to build a robust Medallion Architecture that handles state changes and provides real-time visibility into warehouse bottlenecks.


# Technical Stack
- Language: Python (PySpark), SQL
- Platform: Databricks, Unity Catalog
- Storage: Delta Lake (SCD Type 1 & 2)
- Orchestration: Databricks Workflows (Cron-scheduled)
- Schema Design: Star Schema with Cumulative Snapshot Fact tables

# Data Engineering Pipeline
1. Bronze (Ingestion)
- Implemented Auto Loader to incrementally ingest raw CSVs from cloud storage.
- Schema evolution was enabled to handle potential upstream data changes.

2. Silver (Transformation)
- State Management: Utilized SCD Type 2 for robots and employees to track historical status changes (e.g., Idle -> Picking -> Charging).
- Deduplication: Ensured "Exactly-once" semantics by deduplicating event streams using watermarking.

3. Gold (Analytics & Business Logic)
- Cumulative Snapshot Fact: Built a fact_order_lifecycle table. This allows the business to measure "Time-to-Ship" by tracking timestamps for Initial, Picking, and Shipped milestones in a single row.
- Periodic Snapshot Fact: Created a daily volume table aggregated by Geography and Time to monitor regional throughput.
- Optimization: Implemented Liquid Clustering on order_id and date_key to ensure high-performance joins and fast "Change Data Feed" (CDF) reads.


# The Data Challenge (Simulation)
To mirror real-world variability, I developed a custom data generator that:
- Produces dynamic order volumes based on "Time of Day" (e.g., peak afternoon surges vs. midnight lulls).
- Simulates robot telemetry (battery life, weight capacity) and employee availability constraints.
- Generates raw event streams in CSV format stored in cloud volumes.

# Orchestration & Monitoring
The image of the Job:

![job](Images/Job_run.jpeg)

The image of multiple successful runs:

![successful_job](Images/Successful_Job_runs.jpeg)

# Showcasing some results

- Chart showing the average order placement per hour

![Average_order_hour_chart](Images/Average_orders_per_hour_chart.jpeg)

- Chart showing the orders fullfillment breakdown

![Fullfillment_breakdown_chart](Images/Fullfilement_time_breakdown_chart.jpeg)

- Map of Canada showing the location where the orders have been placed.

![orders_canada_map](Images/Orders_in_Geography.jpeg)

