# Detección de tráfico anómalo en dispositivos IoT mediante aprendizaje automático

Este proyecto evalúa modelos de aprendizaje automático para la detección de tráfico anómalo y ataques de botnets en dispositivos IoT, utilizando el dataset **N-BaIoT**.

El análisis se enfoca en el dispositivo IoT **Provision PT-737E**, una cámara de vigilancia incluida en el dataset. Se trabajó con un subconjunto de **80,000 registros** distribuidos entre tráfico benigno y cuatro clases de ataque.

## Objetivo

Analizar la capacidad de diferentes modelos de aprendizaje automático para distinguir entre tráfico benigno y tráfico asociado a botnets en dispositivos IoT.

Los modelos evaluados fueron:

- K-Means multiclase
- K-Means binario
- Gaussian Naive Bayes
- Isolation Forest multiclase adaptado

## Dataset

Se utilizó el dataset **N-BaIoT**, orientado a la detección de ataques de botnets en dispositivos IoT.

Fuentes originales:

- Kaggle: https://www.kaggle.com/datasets/mkashifn/nbaiot-dataset
- UCI Machine Learning Repository: https://doi.org/10.24432/C5RC8J
- Paper base: https://doi.org/10.48550/arXiv.1805.03409

El dataset completo contiene tráfico de distintos dispositivos IoT y múltiples archivos CSV asociados a tráfico benigno y ataques de botnets como Mirai y BASHLITE.

## Subconjunto utilizado

Para este proyecto no se trabajó directamente con el dataset completo, sino con un subconjunto preparado por el equipo.

El subconjunto corresponde al dispositivo IoT **Provision PT-737E**, una cámara de vigilancia incluida en el dataset N-BaIoT.

A partir de este dispositivo se utilizaron los siguientes archivos originales:

- `5.benign.csv`
- `5.gafgyt.scan.csv`
- `5.gafgyt.udp.csv`
- `5.mirai.scan.csv`
- `5.mirai.udp.csv`

La distribución final utilizada fue:

| Archivo original | Clase asignada | Registros usados |
|---|---|---:|
| `5.benign.csv` | `benign` | 40,000 |
| `5.gafgyt.scan.csv` | `gafgyt.scan` | 10,000 |
| `5.gafgyt.udp.csv` | `gafgyt.udp` | 10,000 |
| `5.mirai.scan.csv` | `mirai.scan` | 10,000 |
| `5.mirai.udp.csv` | `mirai.udp` | 10,000 |
| Total | — | 80,000 |

Del conjunto original de variables se seleccionaron cinco características estadísticas del tráfico:

| Variable | Descripción |
|---|---|
| `H_L0.1_weight` | Volumen o peso del tráfico del host en una ventana de 0.1 s |
| `H_L0.1_mean` | Promedio del comportamiento del tráfico del host |
| `H_L0.1_variance` | Variación o dispersión del tráfico del host |
| `HH_jit_L0.01_mean` | Promedio del jitter entre host origen y host destino |
| `HpHp_L0.1_radius` | Medida de dispersión en la comunicación entre sockets |

El archivo CSV que descarga el notebook desde Google Drive contiene este subconjunto ya integrado, con las cinco variables seleccionadas y una columna adicional llamada `Etiqueta`.

Por esta razón, el archivo utilizado en el notebook no representa el dataset N-BaIoT completo, sino una versión filtrada y preparada para el experimento de este proyecto.

El dataset se descarga automáticamente mediante `gdown`, por lo que el CSV no se incluye directamente en el repositorio.

## Modelos evaluados

### K-Means multiclase

Se configuró K-Means con `n_clusters=5` para analizar si los datos podían agruparse naturalmente en las cinco clases originales del dataset.

### K-Means binario

Se transformó el problema a dos clases:

- `benign`
- `attack`

Esto permitió evaluar si K-Means podía separar tráfico normal frente a tráfico malicioso.

### Gaussian Naive Bayes

Se utilizó como modelo supervisado multiclase. El dataset se dividió en:

- 70% entrenamiento
- 30% prueba

La división se realizó con estratificación para conservar la proporción de clases.

### Isolation Forest multiclase adaptado

Se entrenó un detector Isolation Forest por cada etiqueta. Para clasificar una muestra, se asignó la clase cuyo modelo produjo el mayor puntaje de normalidad.

## Resultados principales

| Modelo | Exactitud | Precisión | Sensibilidad / Recall | F1-score | AUC binario |
|---|---:|---:|---:|---:|---:|
| K-Means multiclase | 0.5827 | 0.6201 | 0.5827 | 0.5744 | N/A |
| K-Means binario | 0.8824 | 0.9041 | 0.8824 | 0.8808 | 0.9975 |
| Isolation Forest multiclase adaptado | 0.9418 | 0.9602 | 0.9418 | 0.9459 | 0.9995 |
| Gaussian Naive Bayes | 0.9982 | 0.9983 | 0.9982 | 0.9983 | 0.9989 |

El modelo con mejor rendimiento general fue **Gaussian Naive Bayes**, clasificando correctamente **23,958 de 24,000 registros** del conjunto de prueba.

## Estructura del repositorio

```text
MachineLearning-IoT-Botnet-Detection/
├── README.md
├── requirements.txt
├── notebooks/
│   └── deteccion_trafico_iot_ml.ipynb
├── src/
│   └── deteccion_trafico_iot_ml.py
├── img/
│   ├── distribucion_variable_objetivo.png
│   ├── matriz_confusion_kmeans_5_variables_legible.png
│   ├── matriz_confusion_kmeans_binario_legible.png
│   ├── matriz_confusion_naive_bayes_legible.png
│   ├── matriz_confusion_isolation_forest_multiclase.png
│   └── curvas_roc_modelos.png
└── docs/
    └── reporte.pdf
