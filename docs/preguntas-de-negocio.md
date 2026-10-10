# Preguntas de negocio (escribe tú las queries)

La Universidad del Valle quiere respuestas a estas preguntas. Tu tarea es escribir
las consultas **SQL en Athena** que las respondan, sobre las tablas que genere tu
pipeline (`calificaciones`, `asistencia`, `cursos` en la base `academic_db`).

Entrega **al menos 3** consultas. Sugerencias:

1. **Tasa de aprobación por asignatura.**
   ¿Qué materias tienen mayor reprobación? (porcentaje de aprobados por curso)

2. **Relación asistencia ↔ notas.**
   ¿Los estudiantes que más asisten sacan mejores notas? (nota promedio por rango de asistencia)

3. **Top estudiantes del periodo.**
   ¿Quiénes tienen el mejor promedio general?

4. **Estudiantes en riesgo (extra).**
   ¿Quiénes reprobaron y además tienen baja asistencia?

> Guarda tus consultas en este mismo folder, por ejemplo en un archivo
> `consultas-athena.sql`, con un comentario encima de cada una explicando qué responde.
