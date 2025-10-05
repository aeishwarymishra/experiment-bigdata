```mermaid
graph TD
    A[HDFS / S3 / ADLS / OCI Object Storage] --> B[Delta Lake / Apache Iceberg / Apache Hudi]
    B --> C[Processing: Spark, Flink, Databricks, Glue, EMR, OCI Data Flow]
    C --> D[Serving Layer: BI Tools, ML Models, Dashboards]
```

```mermaid
graph LR
  A[Producers<br>(Apps, APIs, IoT, DB Streams)] -->|Publish Events| B[(Kafka Topics)]
  B -->|Stream Processing| C[Spark Structured Streaming / Flink / kSQL / Kafka Streams]
  C -->|Processed Data| D[(Sink: S3 / Delta Lake / Data Warehouse)]
  C -->|Enriched Stream| E[Serving APIs / Dashboards / Alerts]
```
