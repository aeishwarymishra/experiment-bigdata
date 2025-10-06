```mermaid
graph TD
  %% ===== Sources =====
  subgraph L0[Sources]
    S1[Operational Apps]
    S2[Event Streams / Kafka]
    S3[IoT / Sensors]
    S4[Files / Docs / Media]
    S5[3rd-Party APIs]
  end

  %% ===== Ingestion =====
  subgraph L1[Ingestion]
    I1[Batch Ingest (ETL/ELT)]
    I2[Stream Ingest (Kafka, Kinesis, Pulsar)]
    I3[Change Data Capture]
    I4[Data Contracts & Schemas]
  end

  %% ===== Storage =====
  subgraph L2[Storage]
    ST1[Data Lake (S3, ADLS, OCI)]
    ST2[Lakehouse (Delta, Iceberg, Hudi)]
    ST3[Warehouse (Snowflake, BQ, Redshift)]
    ST4[Vector Store (Milvus, Pinecone, pgvector)]
    ST5[Document Store (S3/Blob + Catalog)]
  end

  %% ===== Processing =====
  subgraph L3[Processing & Compute]
    P1[Spark / Flink / Beam]
    P2[SQL Engines (Athena, Trino, BigQuery)]
    P3[Feature Engineering Jobs]
    P4[Batch + Streaming DAGs (Airflow, Prefect, Dagster)]
  end

  %% ===== Analytics/ML =====
  subgraph L4[Analytics, ML & Feature Layer]
    A1[BI / Semantic Models]
    A2[Feature Store]
    A3[Model Training / Fine-tuning]
    A4[Embeddings Generation]
  end

  %% ===== Generative AI Layer =====
  subgraph L5[Generative AI Layer]
    G1[LLMs (Hosted/Custom)]
    G2[RAG Orchestrator]
    G3[Prompt/Tool Router (Agents)]
    G4[Synthetic Data Generation]
    G5[NL2SQL / Code Gen for ETL]
  end

  %% ===== Serving / Experience =====
  subgraph L6[Serving & Experience]
    SV1[APIs / Microservices]
    SV2[Conversational BI / Copilots]
    SV3[Apps / Dashboards]
    SV4[Notifications / Actions]
  end

  %% ===== Governance/Observability =====
  subgraph G0[Governance, Security & Observability]
    C1[Metadata Catalog (Glue, Atlas, Purview)]
    C2[Lineage & Impact Analysis]
    C3[Data Quality / PII Detection]
    C4[Access Controls / Policy / Audit]
    C5[Cost / Perf / Drift Monitoring]
    C6[Documentation Auto-Gen]
  end

  %% Flows (left-to-right, top-down)
  S1 --> I1
  S2 --> I2
  S3 --> I2
  S4 --> I1
  S5 --> I1

  I1 --> ST1
  I1 --> ST2
  I1 --> ST3
  I2 --> ST2
  I3 --> ST2

  ST1 --> P1
  ST2 --> P1
  ST3 --> P2
  P1 --> A2
  P1 --> A3
  P1 --> A4
  P2 --> A1
  P2 --> A4

  %% RAG data paths
  ST5 --> ST4
  ST2 --> A4
  A4 --> ST4

  %% GenAI integration
  ST3 --> G5
  ST2 --> G2
  ST4 --> G2
  G2 --> G1
  G1 --> SV2
  G5 --> P1

  %% Synthetic data feedback
  G4 --> ST2
  G4 --> ST3

  %% Serving
  A1 --> SV3
  A2 --> SV1
  A3 --> SV1
  SV1 --> SV3
  SV2 --> SV3
  SV3 --> SV4

  %% Governance/Observability taps (sidecar)
  C1 --- ST1
  C1 --- ST2
  C1 --- ST3
  C2 --- P1
  C2 --- P2
  C3 --- I1
  C3 --- I2
  C4 --- SV1
  C4 --- L5
  C5 --- L3
  C5 --- L5
  C6 --- L2
  C6 --- L5

```
