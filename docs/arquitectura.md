# Arquitectura del pipeline

Diagrama de referencia del pipeline que debes construir. Las piezas exactas (nombres,
configuración) las defines tú con AWS CDK.

```mermaid
flowchart LR
    subgraph S3_RAW["S3 · raw-data"]
        CSV["CSV crudos<br/>calificaciones · asistencia · cursos"]
    end

    subgraph GLUE["AWS Glue"]
        JOB["Glue Job ETL<br/>CSV -> Parquet"]
        CRAWLER["Glue Crawler<br/>descubre el esquema"]
    end

    subgraph S3_PROC["S3 · processed-data"]
        PARQUET["Datos en Parquet<br/>(columnar y comprimido)"]
    end

    CATALOG["Glue Data Catalog<br/>academic_db + tablas"]

    subgraph ATHENA["Amazon Athena"]
        WG["Workgroup<br/>consultas SQL serverless"]
    end

    S3_RESULTS["S3 · resultados de Athena"]
    QS["QuickSight<br/>(opcional)"]

    CSV --> JOB --> PARQUET
    PARQUET --> CRAWLER --> CATALOG
    CATALOG --> WG
    PARQUET --> WG
    WG --> S3_RESULTS
    WG --> QS
```

## Flujo

1. Los **CSV crudos** se guardan en el bucket `raw-data`.
2. El **Glue Job** los transforma a **Parquet** y los escribe en `processed-data`.
3. El **Crawler** recorre `processed-data`, descubre el esquema y registra las tablas
   en el **Data Catalog** (`academic_db`).
4. **Athena** consulta esas tablas con SQL; los resultados se guardan en un bucket de S3.
5. (Opcional) **QuickSight** se conecta a Athena para visualizar los datos.
