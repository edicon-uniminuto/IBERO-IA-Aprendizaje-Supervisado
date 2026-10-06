# Predicción de retrasos ferroviarios con Árbol de Decisión

**Asignatura:** Inteligencia Artificial\
**Institución:** Corporación Universitaria Iberoamericana (IBERO)\
**Actividad 3:** Métodos de aprendizaje supervisado\
**Lenguaje:** Python

## Descripción

Proyecto académico de **aprendizaje supervisado** orientado a predecir si una observación ferroviaria presentará un retraso superior a **15 minutos**. El sistema utiliza información disponible antes de conocer el retraso final, como la fecha, la posición de la parada, la distancia desde el origen, el tipo de tren y la zona ferroviaria.

Se implementa un único método de clasificación: **Árbol de Decisión**. Este algoritmo fue seleccionado por su capacidad para construir reglas interpretables, visualizar las decisiones aprendidas y determinar qué variables tienen mayor influencia sobre la predicción.

El proyecto utiliza el dataset público **Indian Railways - Delay Dataset** de Kaggle y realiza la descarga directamente con `kagglehub`.

## Problema

Los retrasos ferroviarios afectan la puntualidad y confiabilidad del servicio. A partir de los registros históricos disponibles, se plantea el problema de identificar si una observación puede clasificarse como **a tiempo** o **retrasada**, sin utilizar directamente el valor real del retraso como entrada del modelo.

La variable objetivo se define de la siguiente forma:

- `0`: observación a tiempo, con retraso menor o igual a 15 minutos.
- `1`: observación retrasada, con retraso superior a 15 minutos.

La columna `delay` se utiliza únicamente para construir esta etiqueta y posteriormente se excluye de las variables predictoras para evitar **fuga de información**.

## Datos utilizados

La fuente corresponde al dataset público de Kaggle:

**Indian Railways - Delay Dataset**  
`rxydenxd/indian-railways-delay-dataset`

Para construir el conjunto de análisis se utilizan cuatro tablas del dataset:

| **Tabla** | **Uso** |
| --- | --- |
| `combined_delay` | Fecha, tren, estación y retraso observado. |
| `combined_schedule` | Posición de la parada y distancia desde el origen. |
| `train_details` | Tipo de tren. |
| `station_full_names` | Zona ferroviaria de la estación. |

Las tablas se relacionan principalmente mediante `train_no` y `station_name`.

En la ejecución documentada se utilizaron **92.261 registros válidos** después de la limpieza. La variable objetivo quedó distribuida en **33.729 observaciones a tiempo** y **58.532 observaciones retrasadas**.

Las variables utilizadas por el modelo son:

| **Tipo** | **Variables** |
| --- | --- |
| Numéricas | `month`, `day_of_week`, `is_weekend`, `distance_from_origin`, `stop_sequence` |
| Categóricas | `train_type`, `zone` |
| Objetivo | `is_delayed` |

## Objetivo

Construir y validar un modelo de aprendizaje supervisado capaz de clasificar observaciones ferroviarias según su probabilidad de presentar un retraso superior a 15 minutos, manteniendo un proceso interpretable mediante reglas, visualización del árbol e importancia de variables.

## Características

- Descarga automática del dataset público mediante `kagglehub`.
- Integración de cuatro tablas de información ferroviaria.
- Limpieza y normalización de fechas, retrasos y variables operacionales.
- Corrección de la anomalía de rollover identificada en los valores de retraso.
- Construcción de la variable objetivo `is_delayed` con umbral de 15 minutos.
- Análisis exploratorio con distribución de clases, retrasos por mes, zonas ferroviarias y distribución de minutos de retraso.
- División estratificada de los datos en entrenamiento y prueba.
- Imputación de valores faltantes y codificación de variables categóricas.
- Clasificación mediante **Árbol de Decisión**.
- Matriz de confusión y curva ROC.
- Visualización gráfica de las principales reglas del árbol.
- Gráfica de importancia de variables.
- 10 pruebas automatizadas de validación.

## Cómo funciona

1. **Descarga:** el notebook obtiene el dataset público desde Kaggle.
2. **Integración:** se cargan y relacionan las tablas de retrasos, horarios, trenes y estaciones.
3. **Limpieza:** se normalizan tipos de datos y se corrigen registros anómalos.
4. **Objetivo:** se crea `is_delayed`, considerando retrasada una observación con más de 15 minutos.
5. **EDA:** se explora la distribución del objetivo y el comportamiento de los retrasos.
6. **Preparación:** las variables numéricas se imputan y las categóricas se transforman mediante One-Hot Encoding.
7. **Entrenamiento:** se ajusta un `DecisionTreeClassifier` sobre el conjunto de entrenamiento.
8. **Evaluación:** se calculan métricas, matriz de confusión y curva ROC.
9. **Interpretación:** se visualiza el árbol y se identifican las variables con mayor aporte.
10. **Validación:** se ejecutan automáticamente los casos de prueba definidos en el notebook.

## Resultados principales

La ejecución documentada obtuvo los siguientes resultados sobre el conjunto de prueba:

| **Métrica** | **Resultado** |
| --- | ---: |
| Accuracy | 0,7711 |
| Precision | 0,8382 |
| Recall | 0,7920 |
| F1-score | 0,8145 |
| ROC-AUC | 0,8238 |

Las variables con mayor aporte al árbol fueron `distance_from_origin` y algunas categorías de `zone`, seguidas por el tipo de tren y variables temporales. La visualización del árbol permite revisar las reglas principales utilizadas para separar observaciones a tiempo y retrasadas.

## Requisitos

- Python 3.10 o superior.
- Acceso a Internet para descargar el dataset desde Kaggle.
- Paquetes: `kagglehub`, `numpy`, `pandas`, `matplotlib` y `scikit-learn`.

## Instalación y ejecución

Descarga o clona el repositorio y abre una terminal en su carpeta. Se recomienda crear un entorno virtual:

```bash
python -m venv .venv
```

Actívalo en Windows (PowerShell):

```powershell
.\.venv\Scripts\Activate.ps1
```

O en macOS/Linux:

```bash
source .venv/bin/activate
```

Instala las dependencias:

```bash
python -m pip install kagglehub numpy pandas matplotlib scikit-learn
```

Para trabajar en Jupyter o Google Colab, abre:

```text
Actividad3_Supervisado_Retraso_Trenes.ipynb
```

y ejecuta las celdas en orden.

Si se utiliza la versión independiente en Python:

```bash
python actividad3_supervisado.py
```

## Archivos de salida

Durante la ejecución, el notebook crea la carpeta `salidas_actividad3/` con las visualizaciones del análisis:

| **Archivo** | **Contenido** |
| --- | --- |
| `distribucion_objetivo.png` | Distribución entre observaciones a tiempo y retrasadas. |
| `retraso_por_mes.png` | Tasa de retraso observada por mes. |
| `retraso_por_zona.png` | Zonas ferroviarias con mayor tasa de retraso. |
| `distribucion_retraso.png` | Distribución de los minutos de retraso. |
| `matriz_confusion.png` | Comparación entre clases reales y predichas. |
| `curva_roc.png` | Curva ROC del modelo. |
| `arbol_decision.png` | Principales reglas aprendidas por el Árbol de Decisión. |
| `importancia_variables.png` | Variables con mayor aporte al modelo. |

Además, el notebook muestra las métricas, el reporte de clasificación, la importancia de variables y el resultado de las pruebas.

## Pruebas implementadas

| **ID** | **Caso de prueba** |
| --- | --- |
| P1 | Verificar que el dataset tenga registros disponibles. |
| P2 | Comprobar que la variable objetivo sea binaria. |
| P3 | Confirmar que `delay` no participe como variable predictora. |
| P4 | Validar que los conjuntos de entrenamiento y prueba hayan sido creados. |
| P5 | Verificar que el Árbol de Decisión haya sido entrenado. |
| P6 | Comprobar que exista una predicción por cada registro de prueba. |
| P7 | Validar que las probabilidades estén entre 0 y 1. |
| P8 | Comprobar que el valor ROC-AUC se encuentre dentro del rango válido. |
| P9 | Verificar la generación de la matriz de confusión. |
| P10 | Verificar la generación de la gráfica del Árbol de Decisión. |

Las pruebas se ejecutan automáticamente al finalizar el notebook. Si alguna condición no se cumple, se genera una excepción para hacer visible el fallo.

---

**Proyecto académico – Inteligencia Artificial, IBERO.**
