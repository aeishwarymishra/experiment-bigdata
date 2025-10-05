---------------------------------------------------------------
MODERN DATA PLATFORM (END-TO-END): INGESTION → LAKEHOUSE → SERVING

---------------------------------------------------------------
```mermaid

%% LEFT TO RIGHT VARIANT
graph LR

subgraph I[Ingestion]
    P1[Producers - apps, APIs, IoT] --> K[(Kafka topics)]
    P2[CDC from OLTP or ERP or CRM] --> K
    P3[Batch loads - files or exports] --> LZ[Object storage - raw or landing]
    K --> LZ
end

subgraph S[Lakehouse storage and tables]
    LZ --> LT[(ACID tables - Delta or Iceberg or Hudi)]
end

subgraph C[Compute and processing]
    LT --> SB[Batch - Spark or Databricks or EMR or Glue]
    K --> ST[Streaming - Flink or Kafka Streams]
    LT --> ST
    SB --> CUR[(Curated datasets or views)]
    ST --> CUR
end

subgraph O[Serving and consumption]
    CUR --> WH[(Warehouse and BI - dashboards and SQL)]
    CUR --> ML[ML training and feature store]
    ST --> RT[Real time APIs and alerting and ops dashboards]
end

G[Governance - catalog and lineage and RBAC and quality] -.-> I
G -.-> S
G -.-> C
G -.-> O

```



---------------------------------------------------------------
DATA STORAGE + PROCESSING LAYERS
---------------------------------------------------------------

```mermaid

graph TD
    %% Raw storage layer
    A[🏗️ Storage Layer<br/>HDFS / S3 / ADLS / OCI Object Storage]
    %% Unified transactional layer
    A --> B[🧩 Data Management Layer<br/>Delta Lake / Apache Iceberg / Apache Hudi]
    %% Processing engines
    B --> C[⚙️ Processing & Compute Layer<br/>Spark / Flink / Databricks / Glue / EMR / OCI Data Flow]
    %% Serving and analytics
    C --> D[📊 Serving Layer<br/>BI Tools / ML Models / Dashboards]

    %% Comments
    %% A = where raw data lands (cheap, scalable storage)
    %% B = adds ACID, schema, and versioning to make lakes reliable
    %% C = transforms, aggregates, and enriches data
    %% D = delivers insights to business or ML systems

```
-------------------------------------------------------------------
STREAMING DATA FLOW
-------------------------------------------------------------------

```mermaid

graph LR
    %% Producers (event sources)
    A[📥 Producers<br/>Apps / APIs / IoT / DB Streams]
    %% Kafka as central event hub
    A -->|Publish Events| B[(🌀 Kafka Topics)]
    %% Stream processors
    B -->|Stream Processing| C[⚙️ Stream Engines<br/>Spark Structured Streaming / Flink / kSQL / Kafka Streams]
    %% Persistent sink
    C -->|Processed Data| D[(💾 S3 / Delta Lake / Data Warehouse)]
    %% Real-time consumers
    C -->|Enriched Stream| E[📡 Serving Layer<br/>APIs / Dashboards / Alerts]

    %% Comments
    %% A = producers push data continuously
    %% B = Kafka buffers, partitions, and stores messages durably
    %% C = stream processors consume and enrich in-flight data
    %% D = long-term sinks for analytics or history
    %% E = low-latency consumers for real-time insights


```
-------------------------------------------------------------------
 BATCH VS STREAMING TRADE-OFFS
-------------------------------------------------------------------
```mermaid

graph TD
    %% Batch path
    A[🧮 Batch Processing<br/>Spark / Hadoop] -->|High Latency<br/>Minutes to Hours| B[🏢 Data Warehouse<br/>Historical Analytics]
    %% Streaming path
    C[⚡ Streaming Processing<br/>Flink / Kafka Streams] -->|Low Latency<br/>Milliseconds to Seconds| D[📈 Real-Time Dashboards / APIs]
    %% Shared storage foundation
    E[🗄️ Shared Storage<br/>S3 / Delta Lake] --> B
    E --> D

    %% Comments
    %% Batch = periodic, throughput-focused, used for deep analytics
    %% Streaming = continuous, latency-focused, used for instant insights
    %% Both converge on same storage foundation (Lakehouse)

```
```mermaid
graph TD
    A[Airflow / Prefect - Orchestrator] -->|Triggers| B[Spark / Flink - Processing Engine]
    B -->|Reads/Writes| C[(Data Lake / Delta / Warehouse)]
    B -->|Logs / Metrics| A
```
