# Referencias

Este documento recopila las principales fuentes de documentación utilizadas durante el desarrollo del proyecto de comparación de herramientas MLOps, incluyendo el seguimiento de experimentos, versionado de datasets, registro de modelos y entrenamiento del modelo de Machine Learning.

---

## 1. MLflow

### Documentación oficial de MLflow

Documentación principal de MLflow utilizada como referencia para la configuración y uso de la herramienta.

**Referencia:**

MLflow. *MLflow Documentation*.
https://mlflow.org/docs/latest/

**Uso en el proyecto:**

- Configuración de MLflow.
- Gestión de experimentos.
- Registro de ejecuciones.
- Registro de métricas y parámetros.
- Manejo de artefactos.
- Revisión de funcionalidades disponibles para MLOps.

---

### MLflow Tracking

Documentación utilizada para implementar el seguimiento de los experimentos.

**Referencia:**

MLflow. *MLflow Tracking*.
https://mlflow.org/docs/latest/ml/tracking/

**Uso en el proyecto:**

- Creación y organización de experimentos.
- Creación de runs.
- Registro de parámetros.
- Registro de métricas.
- Registro de artefactos.
- Consulta y comparación de ejecuciones.

Entre las funciones utilizadas se encuentran:

```python
mlflow.set_experiment()
mlflow.start_run()
mlflow.log_param()
mlflow.log_params()
mlflow.log_metric()
mlflow.log_metrics()
```

---

### MLflow Tracking Quickstart

Guía oficial utilizada como referencia para la implementación inicial del seguimiento de experimentos.

**Referencia:**

MLflow. *MLflow Tracking Quickstart*.
https://mlflow.org/docs/latest/ml/tracking/quickstart/

**Uso en el proyecto:**

- Configuración inicial del tracking.
- Registro manual de parámetros y métricas.
- Registro de modelos.
- Organización de ejecuciones mediante runs.

---

### Integración de MLflow con Scikit-learn

Documentación utilizada para registrar los modelos entrenados mediante Scikit-learn.

**Referencia:**

MLflow. *MLflow Scikit-learn Integration*.
https://mlflow.org/docs/latest/ml/traditional-ml/sklearn/

**Uso en el proyecto:**

- Integración entre MLflow y Scikit-learn.
- Registro de modelos entrenados.
- Seguimiento de parámetros y métricas.
- Almacenamiento del modelo como artefacto.

---

### API `mlflow.sklearn`

Referencia utilizada para trabajar con modelos de Scikit-learn dentro de MLflow.

**Referencia:**

MLflow. *mlflow.sklearn API Documentation*.
https://mlflow.org/docs/latest/api_reference/python_api/mlflow.sklearn.html

**Uso en el proyecto:**

Principalmente para el registro del modelo mediante:

```python
mlflow.sklearn.log_model(
    sk_model=modelo,
    name="random_forest"
)
```

---

### Seguimiento de datasets con MLflow

Documentación utilizada para registrar información relacionada con los datasets empleados durante los experimentos.

**Referencia:**

MLflow. *mlflow.data*.
https://mlflow.org/docs/latest/api_reference/python_api/mlflow.data.html

**Uso en el proyecto:**

- Creación de representaciones de datasets.
- Registro de datasets utilizados durante el entrenamiento.
- Asociación del dataset con una ejecución.
- Manejo de información como nombre, fuente y `digest`.

Entre las funciones utilizadas se encuentra:

```python
mlflow.log_input(
    dataset_mlflow,
    context="training"
)
```

---

## 2. Weights & Biases

### Documentación oficial de Weights & Biases

Documentación utilizada para implementar el seguimiento de experimentos mediante W&B.

**Referencia:**

Weights & Biases. *W&B Documentation*.
https://docs.wandb.ai/

**Uso en el proyecto:**

- Inicialización de experimentos.
- Registro de métricas.
- Seguimiento de ejecuciones.
- Registro de datasets.
- Manejo de artefactos.
- Comparación de resultados.

---

### Experimentos y Runs en W&B

Documentación utilizada como referencia para la creación y gestión de ejecuciones.

**Referencia:**

Weights & Biases. *Run*.
https://docs.wandb.ai/ref/python/experiments/run/

**Uso en el proyecto:**

La creación de ejecuciones se realizó mediante:

```python
with wandb.init(
    project="...",
    name="..."
) as run:
    ...
```

Cada ejecución permite asociar métricas, configuración y artefactos con un experimento determinado.

---

### Registro de métricas en W&B

Documentación utilizada para almacenar los resultados obtenidos durante los experimentos.

**Referencia:**

Weights & Biases. *Log objects and media*.
https://docs.wandb.ai/guides/track/log/

**Uso en el proyecto:**

- Registro de métricas.
- Registro de resultados de entrenamiento.
- Seguimiento de valores asociados a cada ejecución.
- Visualización de resultados desde W&B.

---

### W&B Artifacts

Documentación utilizada para implementar el versionado y seguimiento del dataset.

**Referencia:**

Weights & Biases. *Create an artifact version*.
https://docs.wandb.ai/models/artifacts/create-a-new-artifact-version

**Uso en el proyecto:**

- Creación de artefactos.
- Registro del dataset.
- Versionado de archivos.
- Asociación de metadata al dataset.

Por ejemplo:

```python
artefacto_datos = wandb.Artifact(
    name="wine-quality-clean",
    type="dataset",
    metadata=metadata_datos
)
```

Posteriormente, el artefacto se registra dentro de una ejecución de W&B.

El sistema de Artifacts permite generar nuevas versiones cuando el contenido del artefacto cambia.

---

### Descarga y reutilización de W&B Artifacts

Documentación utilizada como referencia para recuperar versiones previamente registradas de los artefactos.

**Referencia:**

Weights & Biases. *Download and use artifacts*.
https://docs.wandb.ai/models/artifacts/download-and-use-an-artifact

**Uso en el proyecto:**

- Recuperación de artefactos.
- Identificación de versiones.
- Reutilización de datasets registrados.
- Reproducibilidad de experimentos.

---

### Reproducción de experimentos en W&B

Documentación consultada para comprender los mecanismos proporcionados por W&B para reproducir ejecuciones.

**Referencia:**

Weights & Biases. *Reproduce experiments*.
https://docs.wandb.ai/models/track/reproduce-experiments

**Uso en el proyecto:**

Esta documentación sirvió como referencia para analizar las capacidades de W&B relacionadas con:

- Reproducción de experimentos.
- Conservación de información asociada a las ejecuciones.
- Configuración de las ejecuciones.
- Relación entre una ejecución y los artefactos utilizados.

---

## 3. Scikit-learn

### Random Forest Regressor

Documentación oficial utilizada para implementar el algoritmo de Machine Learning empleado en los experimentos.

**Referencia:**

Scikit-learn. *RandomForestRegressor*.
https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestRegressor.html

**Uso en el proyecto:**

El modelo utilizado para los experimentos fue un Random Forest para regresión.

La documentación fue utilizada como referencia para comprender y configurar parámetros como:

```text
n_estimators
max_depth
min_samples_split
min_samples_leaf
random_state
```

El uso de `random_state` fue especialmente importante para mantener condiciones reproducibles entre ejecuciones.

---

### División de datos de entrenamiento y prueba

Documentación utilizada para realizar la separación del dataset en conjuntos de entrenamiento y prueba.

**Referencia:**

Scikit-learn. *train_test_split*.
https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html

**Uso en el proyecto:**

Se utilizó `train_test_split` para mantener una división controlada de los datos durante los experimentos.

El parámetro `random_state` permitió mantener la misma división entre diferentes ejecuciones y herramientas.

---

### Métricas de regresión

Documentación utilizada para evaluar el rendimiento del modelo entrenado.

**Referencia:**

Scikit-learn. *Regression metrics*.
https://scikit-learn.org/stable/api/sklearn.metrics.html

**Uso en el proyecto:**

Se utilizaron métricas de regresión para evaluar y comparar los experimentos, incluyendo:

- MAE.
- RMSE.
- R².

Estas métricas permitieron determinar el rendimiento de cada configuración y seleccionar el mejor experimento.

---

## 4. UCI Machine Learning Repository

### Wine Quality Dataset

El dataset utilizado en los experimentos corresponde a **Wine Quality**, disponible en UCI Machine Learning Repository.

**Referencia:**

Cortez, P., Cerdeira, A., Almeida, F., Matos, T., & Reis, J. (2009b).
*Wine Quality*. UCI Machine Learning Repository.

**DOI:**

```text
10.24432/C56S3T
```

**Dataset:**

https://archive.ics.uci.edu/dataset/186/wine+quality

**Uso en el proyecto:**

El dataset se obtuvo programáticamente mediante el paquete `ucimlrepo`:

```python
from ucimlrepo import fetch_ucirepo
```

La utilización de una fuente pública y documentada permite identificar claramente el origen de los datos empleados durante los experimentos.

---

## 5. ucimlrepo

### Cliente Python de UCI Machine Learning Repository

La librería `ucimlrepo` fue utilizada para obtener programáticamente el dataset desde UCI Machine Learning Repository.

**Referencia:**

UCI Machine Learning Repository. *ucimlrepo*.
https://github.com/uci-ml-repo/ucimlrepo

**Uso en el proyecto:**

La librería permitió cargar el dataset directamente desde Python mediante:

```python
from ucimlrepo import fetch_ucirepo
```

Esto permite mantener documentado el origen de los datos y facilita la reproducción del proceso de obtención del dataset.

---

## 6. Pandas

### Documentación oficial de Pandas

Pandas fue utilizado para la manipulación de datos y la construcción de tablas con los resultados de los experimentos.

**Referencia:**

Pandas. *Pandas Documentation*.
https://pandas.pydata.org/docs/

**Uso en el proyecto:**

- Manipulación de datasets.
- Creación de DataFrames.
- Organización de métricas.
- Construcción del ranking de experimentos.
- Exportación de resultados a archivos CSV.

Por ejemplo:

```python
ranking.to_csv(
    DIR_METRICAS / "mlflow_experimentos.csv",
    index=False
)
```

---

## 7. Reproducibilidad del entorno

Las dependencias necesarias para ejecutar el proyecto se enumeran en:

```text
requirements.txt
```

Este archivo fija explícitamente las versiones de todas las dependencias directas utilizadas por el pipeline, incluidas MLflow, Weights & Biases, Scikit-learn, Pandas, NumPy, Jupyter y `ucimlrepo`. Las versiones observadas durante la ejecución también se documentan en el README y en las salidas del notebook. En conjunto, esta información permite reconstruir el entorno experimental con mayor precisión.

Las principales librerías utilizadas fueron:

| Librería | Uso principal |
|---|---|
| MLflow | Seguimiento y gestión de experimentos |
| Weights & Biases | Seguimiento, visualización y versionado |
| Scikit-learn | Entrenamiento y evaluación del modelo |
| Pandas | Manipulación y almacenamiento de datos |
| NumPy | Operaciones numéricas |
| ucimlrepo | Obtención del dataset desde UCI |

---

## 8. Fuentes técnicas y académicas

### Artículo técnico original de MLflow

Zaharia et al. (2018) presentan los problemas de trazabilidad, reproducibilidad y despliegue que motivaron la creación de MLflow, así como los componentes iniciales de la plataforma.

**Uso en el proyecto:**

- fundamentar la selección de MLflow como herramienta principal;
- relacionar el seguimiento de experimentos con la reproducibilidad;
- contextualizar el registro de parámetros, métricas, modelos y artefactos.

### Arquitectura y definición de MLOps

Kreuzberger et al. (2023) proponen una definición de MLOps y una arquitectura de referencia para integrar desarrollo, operación, automatización y monitoreo de sistemas de Machine Learning.

**Uso en el proyecto:**

- ubicar el seguimiento experimental dentro del ciclo de vida de Machine Learning;
- fundamentar la importancia del versionado y la reproducibilidad;
- distinguir un pipeline experimental de un sistema MLOps completo en producción.

---

## Referencias bibliográficas en formato APA 7

Cortez, P., Cerdeira, A., Almeida, F., Matos, T., & Reis, J. (2009a). Modeling wine preferences by data mining from physicochemical properties. *Decision Support Systems, 47*(4), 547–553. https://doi.org/10.1016/j.dss.2009.05.016

Cortez, P., Cerdeira, A., Almeida, F., Matos, T., & Reis, J. (2009b). *Wine quality* [Conjunto de datos]. UCI Machine Learning Repository. https://doi.org/10.24432/C56S3T

Kreuzberger, D., Kühl, N., & Hirschl, S. (2023). Machine learning operations (MLOps): Overview, definition, and architecture. *IEEE Access, 11*, 31866–31879. https://doi.org/10.1109/ACCESS.2023.3262138

MLflow. (s. f.-a). *MLflow data API documentation*. Recuperado el 6 de octubre de 2026, de https://mlflow.org/docs/latest/api_reference/python_api/mlflow.data.html

MLflow. (s. f.-b). *MLflow documentation*. Recuperado el 6 de octubre de 2026, de https://mlflow.org/docs/latest/

MLflow. (s. f.-c). *MLflow scikit-learn integration*. Recuperado el 6 de octubre de 2026, de https://mlflow.org/docs/latest/ml/traditional-ml/sklearn/

MLflow. (s. f.-d). *MLflow Tracking*. Recuperado el 6 de octubre de 2026, de https://mlflow.org/docs/latest/ml/tracking/

MLflow. (s. f.-e). *MLflow Tracking quickstart*. Recuperado el 6 de octubre de 2026, de https://mlflow.org/docs/latest/ml/tracking/quickstart/

Pandas development team. (s. f.). *Pandas documentation*. Recuperado el 6 de octubre de 2026, de https://pandas.pydata.org/docs/

Scikit-learn developers. (s. f.-a). *RandomForestRegressor*. Recuperado el 6 de octubre de 2026, de https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestRegressor.html

Scikit-learn developers. (s. f.-b). *Regression metrics*. Recuperado el 6 de octubre de 2026, de https://scikit-learn.org/stable/api/sklearn.metrics.html

Scikit-learn developers. (s. f.-c). *train_test_split*. Recuperado el 6 de octubre de 2026, de https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html

UCI Machine Learning Repository. (s. f.). *ucimlrepo* [Software]. GitHub. Recuperado el 6 de octubre de 2026, de https://github.com/uci-ml-repo/ucimlrepo

Weights & Biases. (s. f.-a). *Create an artifact version*. Recuperado el 6 de octubre de 2026, de https://docs.wandb.ai/models/artifacts/create-a-new-artifact-version

Weights & Biases. (s. f.-b). *Download and use artifacts*. Recuperado el 6 de octubre de 2026, de https://docs.wandb.ai/models/artifacts/download-and-use-an-artifact

Weights & Biases. (s. f.-c). *Log objects and media*. Recuperado el 6 de octubre de 2026, de https://docs.wandb.ai/guides/track/log/

Weights & Biases. (s. f.-d). *Reproduce experiments*. Recuperado el 6 de octubre de 2026, de https://docs.wandb.ai/models/track/reproduce-experiments

Weights & Biases. (s. f.-e). *Run*. Recuperado el 6 de octubre de 2026, de https://docs.wandb.ai/ref/python/experiments/run/

Weights & Biases. (s. f.-f). *W&B documentation*. Recuperado el 6 de octubre de 2026, de https://docs.wandb.ai/

Zaharia, M., Chen, A., Davidson, A., Ghodsi, A., Hong, S. A., Konwinski, A., Murching, S., Nykodym, T., Ogilvie, P., Parkhe, M., Xie, F., & Zumar, C. (2018). Accelerating the machine learning lifecycle with MLflow. *IEEE Data Engineering Bulletin, 41*(4), 39–45. http://sites.computer.org/debull/A18dec/p39.pdf
