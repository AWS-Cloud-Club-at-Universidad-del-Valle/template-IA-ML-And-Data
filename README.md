# Reto 5 · Plataforma de Datos Educativos 🎓📊

**Nivel:** Avanzado
**Servicios:** S3 · AWS Glue · Athena · (QuickSight opcional)
**IaC:** AWS CDK

> Este repositorio es un **punto de partida mínimo**. Te damos el enunciado y unos
> datos de muestra. **Todo el proyecto lo construyes tú**: el código de CDK, el Glue
> Job, el Crawler, el workgroup de Athena y las consultas. No hay solución incluida.

---

## 🎯 Objetivo

Construir un **pipeline de datos serverless** que ingiera, transforme y consulte datos
académicos (calificaciones, asistencia, cursos), aprovisionando **toda** la
infraestructura con **AWS CDK**.

El camino de los datos:

```
CSV crudos          Glue Job (ETL)        Parquet            Crawler            Athena
(S3 raw)      ──▶   CSV → Parquet   ──▶   (S3 processed)  ──▶  Data Catalog  ──▶  SQL serverless
```

---

## 📖 Contexto

La **Universidad del Valle** quiere analizar patrones académicos de sus estudiantes:
tasas de aprobación por asignatura, correlación entre asistencia y notas, y materias
con mayor reprobación. Los datos llegan en archivos CSV y se actualizan mensualmente.

---

## 📦 Qué te damos (lo mínimo)

```
.
├── README.md                     ← este enunciado
├── requirements.md               ← requisitos, entregables y criterios de éxito
├── application/                  ← (vacío) aquí va tu código de app / script ETL
├── cdk/                          ← (vacío) aquí va tu proyecto AWS CDK
├── data/
│   ├── calificaciones.csv        ← datos de muestra
│   ├── asistencia.csv
│   ├── cursos.csv
│   └── DATASET_SPEC.md           ← esquema de cada CSV y clave de unión
├── docs/
│   ├── arquitectura.md           ← diagrama del pipeline (Mermaid)
│   └── preguntas-de-negocio.md   ← preguntas que deben responder tus queries
└── .gitignore
```

Las carpetas `cdk/` y `application/` están vacías a propósito: **ese código lo creas tú**.

---

## 📋 Requisitos y entregables

Están en [`requirements.md`](requirements.md): la lista completa de requisitos,
entregables, criterios de éxito y el Boss Fight. El diagrama del pipeline está en
[`docs/arquitectura.md`](docs/arquitectura.md).

---

## 🧰 Pistas para empezar (no es la solución)

```bash
# dentro de este repo
cdk init app --language typescript   # o el lenguaje que prefieras
# ... escribe tu stack ...
cdk synth
cdk bootstrap   # la primera vez en la cuenta/región
cdk deploy
```

Decisiones que tendrás que tomar: qué construct usar para Glue (los L1 `CfnJob` /
`CfnCrawler` o el paquete alpha de Glue), cómo subir los CSV al bucket raw, y cómo
pasarle al Job las rutas de entrada y salida.

---

## 📤 Entrega

Sube tu solución a tu propio repositorio (o rama) con:
- El código CDK completo.
- El script del ETL.
- El archivo SQL con tus consultas.
- Un breve apunte del Boss Fight (qué cambiarías y el cálculo de costos).
