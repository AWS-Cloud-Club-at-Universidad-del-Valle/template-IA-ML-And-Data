# Código de la aplicación (ETL)

Aquí va el **código del pipeline**: principalmente el **script del Glue Job** que lee
los CSV crudos y los escribe en formato **Parquet**. **Puedes partir de cero o
estructurarlo como prefieras.**

## Qué debe hacer tu ETL

1. Leer los CSV de la carpeta `raw` en S3 (ver el esquema en [`../data/DATASET_SPEC.md`](../data/DATASET_SPEC.md)).
2. Convertirlos a **Parquet** (columnar y comprimido).
3. Escribirlos en la carpeta `processed` en S3, una "tabla" por archivo
   (`calificaciones`, `asistencia`, `cursos`).

Después, tu **Crawler** registrará esas tablas en el Data Catalog para que Athena las consulte.

## Cómo conectar el script con tu stack CDK

La idea es que tu Glue Job (definido en `../cdk/`) apunte a este script como su
`scriptLocation`. Un enfoque común es subir este archivo a un bucket de S3 con un
`BucketDeployment` y pasar su ruta al `CfnJob`.

**Ejemplo conceptual (TypeScript):**

```ts
// 1) subes este folder a un bucket de scripts con s3deploy.BucketDeployment
// 2) referencias el script en el Glue Job:
command: {
  name: 'glueetl',
  pythonVersion: '3',
  scriptLocation: `s3://${scriptsBucket.bucketName}/scripts/etl_job.py`,
},
// 3) le pasas por defaultArguments las rutas de entrada (raw) y salida (processed)
```

> El resto (buckets, Job, Crawler, Data Catalog, workgroup de Athena) lo diseñas tú.
> Este folder solo resuelve "qué transformación corre"; la infraestructura va en `../cdk/`.

## Boss Fight (pista)

Para 100 GB/mes, considera **particionar** la salida por `year/month` antes de escribir
el Parquet, para que Athena escanee menos datos y pagues menos por query.
