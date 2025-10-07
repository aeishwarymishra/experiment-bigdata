```mermaid
graph TD
  A[Data Sources] --> B[Ingestion Layer]
  B --> C[Raw Storage: Data Lake (S3, ADLS, OCI Object)]
  C --> D[Processing & Transformation (Spark, DBT, Databricks, Flink)]
  D --> E[Refined Storage: Lakehouse (Delta, Iceberg, Hudi)]
  E --> F[Serving Layer: Data Warehouse (Snowflake, BigQuery, Redshift)]
  F --> G[Consumption Layer: BI, ML, APIs, Agents]
  G --> H[AI & Observability: MLflow, Feature Store, Data Catalog, Governance]
```
