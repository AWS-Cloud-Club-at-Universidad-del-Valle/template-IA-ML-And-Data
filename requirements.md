# Requisitos — Plataforma de Datos Educativos

## 📖 Contexto (historia del problema)

La **Universidad del Valle** quiere analizar patrones académicos de sus estudiantes:
**tasas de aprobación** por asignatura, **correlación entre asistencia y notas**, y
**materias con mayor reprobación**. Los datos llegan en **archivos CSV** y se actualizan
**mensualmente**.

Tu misión como builder es diseñar y desplegar un **pipeline de datos serverless** usando
**AWS CDK**, eligiendo el lenguaje de programación que prefieras. El pipeline debe:

- **Ingerir** los CSV crudos en un data lake.
- **Transformarlos** a un formato eficiente para analítica (**Parquet**).
- **Catalogar** el esquema automáticamente.
- **Consultar** los datos con SQL serverless.

---

## 📋 Requisitos funcionales

| # | Requisito |
|---|---|
| R1 | **S3 (Data Lake)**: un bucket para datos **crudos** (CSV) y otro para datos **procesados** (Parquet). |
| R2 | **Glue Job (ETL)** que transforma los CSV a formato **Parquet**. |
| R3 | **Glue Crawler** que actualiza el **Data Catalog** con el esquema de los datos. |
| R4 | **Glue Data Catalog**: base de datos y tablas generadas por el Crawler. |
| R5 | **Athena**: un **workgroup** configurado que guarde los resultados de las queries en S3. |
| R6 | Al menos **3 consultas SQL** que respondan preguntas de negocio (ver `docs/preguntas-de-negocio.md`). |
| R7 | **(Opcional) QuickSight**: un dashboard visual de los datos procesados. |

---

## 🧩 Servicios AWS

| Servicio | Rol en el reto |
|---|---|
| **S3** | Almacén del Data Lake (raw + processed) y resultados de Athena |
| **AWS Glue** | ETL (CSV → Parquet) + Crawler + Data Catalog |
| **Amazon Athena** | Consultas SQL serverless sobre Parquet |
| **Amazon QuickSight** | Visualización (opcional) |

**Conceptos clave:** Data Lake · ETL · Data Analytics · Serverless query

---

## 📦 Entregables

- [ ] Dos buckets S3 creados (**raw** y **processed**) con CDK.
- [ ] Job de Glue que transforma al menos un CSV de muestra a **Parquet**.
- [ ] Crawler de Glue que actualiza el catálogo con el esquema de los datos.
- [ ] Workgroup de Athena configurado con resultados en S3.
- [ ] Al menos **3 queries SQL** que respondan preguntas de negocio.
- [ ] `cdk deploy` **sin errores manuales**.
- [ ] Diagrama de arquitectura documentado (ver `docs/architecture.md`).

---

## 🏁 Criterios de éxito

1. El **Glue Job** procesa el CSV y genera archivos **Parquet** en el bucket `processed`.
2. El **Crawler** descubre el esquema y lo registra en el **Glue Data Catalog**.
3. Una **query en Athena** retorna resultados correctos sobre los datos procesados.
4. Los buckets tienen las **políticas de acceso correctas** (Athena puede leer `processed`).

---

## 🐉 Boss Fight (cambio de requisitos en vivo)

> ⚠️ **¡El volumen de datos pasa de 1 GB a 100 GB mensuales!** El pipeline actual tarda demasiado.

**¿Qué cambia?**

- El Glue Job necesita configuración de **DPU (Data Processing Units)** apropiada para el nuevo volumen.
- Los datos deben **particionarse** en S3 por `year/month/` para que Athena solo escanee lo necesario.
- *Opcional:* habilita **Glue Job Bookmarks** para procesar solo los archivos nuevos en cada ejecución.
- Calcula el **costo estimado de Athena por query** antes y después de la partición.
  (Athena cobra por **datos escaneados**, ~$5 por TB.)

---

## 🧹 Cleanup obligatorio

Al terminar:

```bash
cd cdk
cdk destroy
```

> ⚠️ Si `cdk destroy` falla porque un bucket tiene objetos, vacíalo primero.
> Revisa la consola de AWS por recursos huérfanos (buckets, resultados de Athena).
