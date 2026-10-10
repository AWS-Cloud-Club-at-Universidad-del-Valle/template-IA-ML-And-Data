# Requisitos del Reto 5 · Plataforma de Datos Educativos

## Requisitos funcionales

1. **S3 (Data Lake)** — dos buckets:
   - `raw-data`: datos crudos en CSV.
   - `processed-data`: datos transformados en Parquet.
   - (Necesitarás además un bucket para los resultados de Athena.)

2. **AWS Glue**
   - **Job ETL** que transforma los CSV a formato Parquet.
   - **Crawler** que actualiza el Data Catalog con el esquema de los datos.

3. **Glue Data Catalog** — base de datos y tablas generadas automáticamente por el Crawler.

4. **Athena** — un **workgroup** configurado que guarde los resultados de las queries en S3.

5. **(Opcional) QuickSight** — dashboard visual de los datos procesados.

6. **Todo el pipeline definido con AWS CDK.**

---

## Entregables

- [ ] Dos buckets S3 creados (`raw-data` y `processed-data`).
- [ ] Job de Glue que transforma al menos un CSV de muestra a Parquet.
- [ ] Crawler de Glue que actualiza el catálogo con el esquema de los datos.
- [ ] Workgroup de Athena configurado con resultados en S3.
- [ ] Al menos **3 queries SQL** de ejemplo que respondan preguntas de negocio
      (ver `docs/preguntas-de-negocio.md`).
- [ ] `cdk deploy` sin errores.

---

## Criterios de éxito

- El Glue Job procesa el CSV y genera archivos Parquet en `processed-data`.
- El Crawler descubre el esquema y lo registra en el Glue Data Catalog.
- Una query en Athena retorna resultados correctos sobre los datos procesados.
- Los buckets tienen las políticas de acceso correctas (Athena puede leer `processed-data`).

---

## Boss Fight: 1 GB → 100 GB

El volumen de datos pasa de 1 GB a 100 GB mensuales y el pipeline tarda demasiado.
¿Qué cambia?

1. El Glue Job necesita configuración de **DPU (Data Processing Units)** apropiada
   para el nuevo volumen.
2. Los datos deben **particionarse** en S3 por `year/month/` para que Athena solo
   escanee los datos necesarios.
3. Considera habilitar **Glue Job Bookmarks** para procesar solo los archivos nuevos
   en cada ejecución.
4. Calcula el **costo estimado de Athena por query** antes y después de la partición.

---

## Cleanup

```bash
cdk destroy
```
