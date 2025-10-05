---------------------------------------------------------------
MODERN DATA PLATFORM (END-TO-END): INGESTION → LAKEHOUSE → SERVING
- Keep labels simple (no HTML). Comments start with %%.
- Subgraphs group layers; edges show the main dataflows.
---------------------------------------------------------------
```mermaid

graph TD

%% ------------------------ INGESTION -----------------------------
subgraph I[Ingestion]
    %% Event and change data producers
    P1[Producers: apps, APIs, IoT] --> K[(Kafka topics)]
    P2[CDC from OLTP/ERP/CRM] --> K
    P3[Batch loads: files, exports] --> LZ[Object storage: raw/landing]
    %% Stream → landing zone (optional fan-out)
    K --> LZ
end

%% -------------------- LAKEHOUSE STORAGE -------------------------
subgraph S[Lakehouse storage & tables]
    %% Raw objects become managed ACID tables
    LZ --> LT[(ACID tables: Delta/Iceberg/Hudi)]
    %% Versioned data supports time-travel & schema evolution
end

%% ---------------------- PROCESSING LAYER ------------------------
subgraph C[Compute & processing]
    %% Batch (micro-batch ok) on managed tables
    LT --> SB[Batch: Spark/Databricks/EMR/Glue]
    %% True streaming on Kafka (and table streams)
    K --> ST[Streaming: Flink/Kafka Streams]
    LT --> ST
    %% Curated, conformed outputs (gold datasets/features)
    SB --> CUR[(Curated tables/views)]
    ST --> CUR
end

%% ------------------------ SERVING LAYER -------------------------
subgraph O[Serving & consumption]
    %% Historical analytics & BI
    CUR --> WH[(Warehouse/BI: dashboards, SQL)]
    %% ML training & feature stores
    CUR --> ML[ML training & feature store]
    %% Low-latency products and alerts
    ST --> RT[Real-time APIs, alerting, ops dashboards]
end

%% --------------------- GOVERNANCE (X-CUTTING) -------------------
%% Catalog/lineage/policies touch all layers (shown with dashed links)
G[Governance: catalog, lineage, RBAC, quality] -.-> I
G -.-> S
G -.-> C
G -.-> O

%% ---------------------- EXPLANATORY NOTES -----------------------
%% Ingestion:
%%   - Kafka decouples producers/consumers; durable, replayable streams.
%%   - Batch loads land bulk data into object storage (raw zone).
%%
%% Lakehouse:
%%   - ACID table formats (Delta/Iceberg/Hudi) add transactions, schema, and time travel on top of object storage.
%%
%% Processing:
%%   - Batch (Spark) = high throughput, periodic/backfills/enrichments.
%%   - Streaming (Flink/Kafka Streams) = low latency, event-time windows, stateful ops.
%%   - Both write into curated, conformed datasets for reuse.
%%
%% Serving:
%%   - Warehouse/BI for historical insights.
%%   - ML pipelines for training and online/offline features.
%%   - Real-time outputs for APIs, alerts, and operational decisions.
%%
%% Governance:
%%   - Central catalog + lineage + policies ensure discoverability, trust, and compliance end-to-end.

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
