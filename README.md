# Reto 5 — Plataforma de Datos Educativos

> **Nivel:** Avanzado · **Programa:** AWS Cloud Practitioner Challenge · **AWS SBG Univalle**

Construye un **pipeline de datos serverless** que ingiere, transforma y consulta datos
académicos (calificaciones, asistencia, cursos) usando **S3 + AWS Glue + Athena**, todo
definido con **AWS CDK**.

---

## 🎯 Objetivo

Diseñar un pipeline que convierta archivos **CSV crudos** en datos **Parquet**
consultables con **SQL serverless**, aprovisionando toda la infraestructura analítica
con **AWS CDK**.

La historia completa del problema y los requisitos funcionales están en **[`requirements.md`](./requirements.md)**.

```
CSV crudos        Glue Job (ETL)        Parquet           Crawler            Athena
(S3 raw)    ──▶   CSV → Parquet   ──▶   (S3 processed) ──▶ Data Catalog ──▶  SQL serverless
```

---

## 🧰 Prerrequisitos

Antes de empezar, asegúrate de tener:

- [ ] **Node.js** 18+ (necesario para el CLI de CDK, sin importar el lenguaje que elijas)
- [ ] **AWS CLI** configurado con credenciales válidas (`aws configure`)
- [ ] **AWS CDK CLI** instalado globalmente:
  ```bash
  npm install -g aws-cdk
  cdk --version
  ```
- [ ] Runtime del lenguaje que vayas a usar (uno de):
  - **TypeScript / JavaScript** → Node.js 18+
  - **Python** → Python 3.9+ y `pip`
  - **Java** → JDK 11+ y Maven
  - **Go** → Go 1.18+
  - **C#** → .NET 6+

---

## 🚀 Inicio Rápido

Este Starter Kit **no incluye el proyecto CDK ya inicializado**: tú eliges el lenguaje.

### 1. Inicializa tu proyecto CDK en la carpeta `cdk/`

```bash
cd cdk
```

Elige **uno** de los siguientes lenguajes y ejecútalo dentro de `cdk/`:

```bash
# TypeScript
cdk init app --language typescript

# Python
cdk init app --language python

# Java
cdk init app --language java

# Go
cdk init app --language go

# C#
cdk init app --language csharp
```

> 💡 `cdk init` requiere que la carpeta esté vacía. La carpeta `cdk/` viene vacía a propósito para que puedas inicializar en tu lenguaje preferido.

### 2. Bootstrap del entorno (solo una vez por cuenta/región)

```bash
cdk bootstrap
```

### 3. Sintetiza, despliega y destruye

```bash
cdk synth      # Genera la plantilla de CloudFormation (valida tu código)
cdk deploy     # Despliega la infraestructura en AWS
cdk destroy    # Elimina TODOS los recursos (¡obligatorio al terminar!)
```

### 4. Ejecuta el pipeline (tras el deploy)

1. Corre tu **Glue Job** → genera el Parquet en el bucket `processed`.
2. Corre tu **Crawler** → registra las tablas en el Data Catalog.
3. Consulta en **Athena** usando tu workgroup y la base de datos del catálogo.

---

## 📁 Estructura del repositorio

```
.
├── README.md              # Este archivo (inicio rápido)
├── requirements.md        # Historia del problema y requisitos funcionales
├── cdk/                   # 👉 Inicializa aquí tu proyecto CDK (viene vacía)
├── application/           # Código del ETL (guía + dónde va tu script de Glue)
│   └── README.md
├── data/                  # Datos de muestra (CSV) + esquema
│   ├── calificaciones.csv
│   ├── asistencia.csv
│   ├── cursos.csv
│   └── DATASET_SPEC.md
└── docs/
    ├── architecture.md         # Plantilla para que documentes tu arquitectura
    └── preguntas-de-negocio.md # Preguntas que deben responder tus queries
```

---

## 🗂️ ¿Qué hay en cada carpeta?

| Carpeta / Archivo | Para qué sirve |
|---|---|
| `cdk/` | Aquí construyes tu infraestructura como código. Empieza vacía. |
| `application/` | Guía para el **script del Glue Job (ETL)** que transforma los CSV a Parquet. **Lo escribes tú.** |
| `data/` | **CSV de muestra** y su esquema (`DATASET_SPEC.md`). Son los datos que tu pipeline debe procesar. |
| `docs/architecture.md` | Plantilla para que documentes tu diseño de arquitectura. |
| `docs/preguntas-de-negocio.md` | Las preguntas que tus consultas SQL deben responder. |
| `requirements.md` | Lo que tu solución debe cumplir. |

---

## ✅ Lo que debes construir (resumen)

1. **S3**: buckets para datos crudos (`raw`) y procesados (`processed`) — más uno para los resultados de Athena.
2. **Glue Job (ETL)** que transforma los CSV a Parquet.
3. **Glue Crawler** que registra el esquema en el Data Catalog.
4. **Glue Data Catalog**: base de datos y tablas.
5. **Athena**: un workgroup que guarda resultados en S3.
6. Al menos **3 consultas SQL** que respondan preguntas de negocio.
7. **(Opcional) QuickSight** para visualizar.

Consulta **[`requirements.md`](./requirements.md)** para el detalle y los criterios de éxito.

---

## 🧹 Cleanup (¡importante!)

Al terminar el reto, elimina todos los recursos para evitar costos:

```bash
cd cdk
cdk destroy
```

> ⚠️ Revisa la consola de AWS por **recursos huérfanos** (buckets con objetos, resultados
> de Athena). Vacía los buckets si `cdk destroy` no los elimina por tener contenido.

---

## 📚 Recursos útiles

- [AWS CDK — Documentación oficial](https://docs.aws.amazon.com/cdk/v2/guide/home.html)
- [AWS CDK API Reference](https://docs.aws.amazon.com/cdk/api/v2/)
- [Amazon S3](https://docs.aws.amazon.com/s3/) · [AWS Glue](https://docs.aws.amazon.com/glue/) · [Amazon Athena](https://docs.aws.amazon.com/athena/) · [Amazon QuickSight](https://docs.aws.amazon.com/quicksight/)
- [AWS Free Tier](https://aws.amazon.com/free/)

---

_AWS Student Builder Group — Universidad del Valle_
