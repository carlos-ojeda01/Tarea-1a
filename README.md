# Tarea 1a - Preparación de Datos

Trabajo correspondiente a la **Tarea 1a** del curso **Minería y Aprendizaje de Datos (MAD)**, perteneciente al programa de **Magíster en Informática**.

## Objetivo

Esta tarea consiste en la **preparación y exploración de datos** provenientes de un contexto de aprendizaje educativo. A partir de dos datasets crudos (`student_data.csv` y `attempts.csv`), se construye un dataset consolidado y depurado (`data_prep.csv`) que reúne variables motivacionales, de engagement y de rendimiento académico, para su uso posterior en tareas de minería de datos y modelado predictivo.

## Dataset

El dataset original proviene de un estudio de **aprendizaje educativo con plataformas interactivas** (QUIZPET y PARSONS). Contiene:

- **`student_data.csv`**: información por estudiante (demográffica, motivacional y de rendimiento).
- **`attempts.csv`**: registro de intentos por estudiante (tiempo, tipo de actividad, etc.).

### Variables del dataset final (`data_prep.csv`)

El dataset final cuenta con **685 registros y 18 columnas**:

| Variable | Descripción |
|---|---|
| `student` | Identificador del estudiante |
| `social` | Grupo social (0 o 1) |
| `gender` | Género del estudiante |
| `take_exam` | Indica si rindió el examen |
| `Fi`, `CBi`, `Vi`, `MApi`, `PApi` | Variables motivacionales (interés, confianza, valor, motivación de acercamiento/evitación) |
| `total_time` | Tiempo total (en segundos) dedicado a las actividades |
| `total_activity` | Cantidad total de intentos registrados |
| `qp_activity` | Cantidad de intentos en QUIZPET o PARSONS |
| `total_activity_be` | Cantidad de intentos con `relativetime` < 5,600,000 |
| `pretest` | Puntaje del pre-test |
| `posttest` | Puntaje del post-test |
| `exam_norm` | Puntaje del examen normalizado (`exam / 100`) |
| `lgain_pp` | Ganancia de aprendizaje post-test vs pre-test: `(posttest - pretest) / (1 - pretest)` |
| `lgain_pe` | Ganancia de aprendizaje examen vs pre-test: `(exam_norm - pretest) / (1 - pretest)` |

## Procesamiento realizado

1. **Limpieza de outliers de tiempo**: los valores de `durationseconds` mayores a 300 se limitaron a 300 (217 casos detectados).
2. **Cálculo de variables de engagement**:
   - `total_time`: suma de `durationseconds` por estudiante.
   - `total_activity`: conteo total de filas por estudiante.
   - `qp_activity`: conteo de filas con `applabel` igual a QUIZPET o PARSONS.
   - `total_activity_be`: conteo de filas con `relativetime` < 5,600,000.
3. **Cálculo de variables de rendimiento derivadas**:
   - `exam_norm`: normalización del examen a escala 0-1.
   - `lgain_pp` y `lgain_pe`: ganancias de aprendizaje normalizadas.
4. **Consolidación**: unión de `student_data` con las features de engagement mediante un left join por `student`, rellenando con 0 a los estudiantes sin actividad registrada.
5. **Selección y ordenamiento de columnas** según la pauta de la tarea.

## Análisis exploratorio

Se generaron gráficos de distribución para todas las variables del dataset final, segmentados por grupo social (`social = 0` y `social = 1`). Las figuras se almacenan en `Data/figuras/` e incluyen:

- Histogramas de densidad para variables motivacionales (`Fi`, `CBi`, `Vi`, `MApi`, `PApi`).
- Histogramas de densidad para variables de rendimiento (`pretest`, `posttest`, `exam_norm`, `lgain_pp`, `lgain_pe`).
- Histogramas de densidad para variables de engagement (con filtrado de outliers por IQR) (`total_time`, `total_activity`, `qp_activity`, `total_activity_be`).
- **Matriz de dispersión (pairplot)** de todas las variables numéricas vs todas, coloreada por grupo social.

## Estructura del repositorio

```
Tarea 1a/
├── README.md
├── Tarea 1a.docx                       # Enunciado de la tarea
├── Tarea_1a_carlos_ojeda.ipynb         # Notebook con el desarrollo completo
├── reporte_carlos_ojeda.docx           # Reporte final
└── Data/
    ├── student_data.csv                # Datos crudos de estudiantes
    ├── attempts.csv                    # Datos crudos de intentos
    ├── data_prep.csv                   # Dataset final preparado
    └── figuras/                        # Gráficos del análisis exploratorio
        ├── dist_CBi.png
        ├── dist_exam_norm.png
        ├── dist_Fi.png
        ├── dist_lgain_pe.png
        ├── dist_lgain_pp.png
        ├── dist_MApi.png
        ├── dist_PApi.png
        ├── dist_posttest.png
        ├── dist_pretest.png
        ├── dist_qp_activity.png
        ├── dist_total_activity.png
        ├── dist_total_activity_be.png
        ├── dist_total_time.png
        ├── dist_Vi.png
        └── pairplot_todos_vs_todos.png
```

## Requisitos

- Python 3.x
- pandas
- numpy
- matplotlib
- seaborn

Ver `requirements.txt` para instalar las dependencias:

```bash
pip install -r requirements.txt
```

## Autor

**Carlos Ojeda**
Magíster en Informática - Universidad Austral de Chile
Curso: Minería y Aprendizaje de Datos (MAD)