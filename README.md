# Pipeline MLOps para Wine Quality

Proyecto desarrollado para la asignatura **Ciencia de Datos** de la **Universidad de Cuenca**.

El objetivo del proyecto es construir un pipeline reproducible de Machine Learning para predecir la calidad del vino a partir de sus características fisicoquímicas y evaluar herramientas de seguimiento de experimentos dentro de un flujo MLOps.

La herramienta principal utilizada es **MLflow**, comparada con **Weights & Biases (W&B)** como herramienta alternativa.

---

## Equipo y roles

| Integrante | Rol principal | Aportes |
|---|---|---|
| Bryan Macas | Coordinación, reproducibilidad y documentación | Estructura inicial del repositorio, configuración del entorno, prueba mínima de MLflow, documentación de errores y soluciones, revisión del README e integración de entregables. |
| Joseph Sangurima | Desarrollo del pipeline y experimentación MLOps | Implementación y ampliación del pipeline, experimentos con MLflow y W&B, registro de métricas, versionado de datos y modelos, y generación de artefactos y resultados comparativos. |

### Responsabilidades compartidas

- revisión del código y de los resultados;
- interpretación de las métricas;
- verificación de la reproducibilidad;
- elaboración del material educativo;
- preparación del video y de la socialización.

---

## Objetivo

Construir un pipeline reproducible de Machine Learning que permita:

- cargar y analizar un conjunto de datos real;
- realizar limpieza y preparación de los datos;
- entrenar un modelo de Machine Learning;
- ejecutar al menos 10 configuraciones de hiperparámetros;
- registrar parámetros y métricas de los experimentos;
- almacenar artefactos y modelos;
- versionar datos y modelos;
- identificar el mejor experimento;
- reproducir el mejor resultado;
- comparar MLflow con Weights & Biases;
- evaluar ambas herramientas mediante criterios empíricos.

---

## Dataset

Se utiliza el dataset **Wine Quality** del UCI Machine Learning Repository.

Características principales:

- UCI Dataset ID: `186`
- Tipo de problema: regresión
- Variables predictoras: 11 características fisicoquímicas
- Variable objetivo: `quality`
- Registros originales: 6497
- Registros duplicados eliminados: 1179
- Registros utilizados después de la limpieza: 5318
- Licencia: Creative Commons Attribution 4.0 International (CC BY 4.0)
- DOI: `10.24432/C56S3T`

El dataset se obtiene mediante el paquete `ucimlrepo`.

Fuente oficial:

https://archive.ics.uci.edu/dataset/186/wine+quality

---

## Modelo utilizado

Se utiliza el modelo:

`RandomForestRegressor`

de la biblioteca Scikit-learn.

Random Forest fue seleccionado porque es adecuado para trabajar con datos tabulares, permite modelar relaciones no lineales entre las variables y ofrece diferentes hiperparámetros que permiten construir múltiples configuraciones experimentales.

Los principales hiperparámetros evaluados fueron:

- `n_estimators`
- `max_depth`
- `min_samples_split`
- `min_samples_leaf`

Para favorecer la reproducibilidad se utiliza una semilla fija:

```python
random_state = 42
```

---

## Herramientas MLOps

### MLflow

MLflow se utiliza como herramienta principal para:

- crear y organizar experimentos;
- registrar hiperparámetros;
- registrar métricas;
- almacenar modelos;
- registrar datasets;
- almacenar artefactos;
- mantener trazabilidad entre ejecuciones;
- comparar diferentes configuraciones experimentales.

### Weights & Biases

Weights & Biases se utiliza como herramienta alternativa para repetir las mismas configuraciones realizadas con MLflow y comparar ambas herramientas bajo condiciones equivalentes.

Además, W&B Artifacts se utiliza para:

- registrar versiones del dataset;
- registrar el mejor modelo;
- mantener trazabilidad entre datos, ejecuciones y modelos.

Evidencias en W&B:

- [Experimentos de Random Forest](https://wandb.ai/bryzcoll-universidad-de-cuenca/wine-quality-random-forest)
- [Benchmark de sobrecarga](https://wandb.ai/bryzcoll-universidad-de-cuenca/wine-quality-overhead-benchmark)
- [Versionado de datasets y modelos](https://wandb.ai/bryzcoll-universidad-de-cuenca/wine-quality-random-forest-artifacts)

---

## Métricas

Debido a que `quality` se trata como una variable numérica, el problema se aborda como una tarea de regresión.

Las métricas utilizadas son:

- MAE: Mean Absolute Error
- RMSE: Root Mean Squared Error
- R²: coeficiente de determinación

El criterio principal utilizado para seleccionar el mejor experimento fue el menor valor de RMSE.

---

## Experimentación

Se ejecutaron 10 configuraciones diferentes de Random Forest.

Todos los experimentos utilizaron:

- el mismo dataset;
- la misma etapa de limpieza;
- la misma partición estratificada de entrenamiento/validación/prueba (60/20/20);
- la misma semilla aleatoria;
- las mismas métricas de evaluación.

Las mismas configuraciones fueron ejecutadas tanto con MLflow como con Weights & Biases.

Esto permite realizar una comparación directa entre ambas herramientas.

---

## Mejor experimento

El mejor resultado de validación correspondió al **experimento 10**.

Configuración:

```text
n_estimators = 300
max_depth = None
min_samples_split = 5
min_samples_leaf = 2
random_state = 42
```

Resultados obtenidos:

| Métrica | Validación para selección | Prueba final aislada |
|---|---:|---:|
| MAE | 0.535146 | 0.534196 |
| RMSE | 0.688779 | 0.689675 |
| R² | 0.387516 | 0.385921 |

El R² de prueba indica que el modelo explica aproximadamente el 38.59 % de la variabilidad observada en la calidad del vino. Por tanto, su capacidad predictiva es moderada. El objetivo principal del proyecto no fue optimizar exhaustivamente el modelo, sino demostrar un flujo reproducible de seguimiento, comparación y versionado de experimentos.

---

## Reproducibilidad

Después de identificar el mejor experimento, se volvió a entrenar el modelo utilizando:

- el mismo dataset;
- la misma división entrenamiento/validación;
- los mismos hiperparámetros;
- la misma semilla aleatoria.

Las métricas reproducidas fueron equivalentes a las obtenidas originalmente dentro de la tolerancia numérica definida.

El notebook verifica este comportamiento mediante:

```text
Experimento reproducible: True
```

Por tanto, el mejor experimento pudo reproducirse correctamente dentro del entorno utilizado. Después de esta comprobación se reentrenó su configuración con todo el conjunto de desarrollo (80 %) y se evaluó una sola vez sobre la prueba aislada (20 %).

---

## Comparación MLflow vs Weights & Biases

La comparación entre MLflow y Weights & Biases se realizó utilizando criterios cualitativos y criterios medidos empíricamente dentro del pipeline.

Los principales criterios considerados fueron:

- integración con el pipeline;
- trazabilidad;
- reproducibilidad;
- esfuerzo de instrumentación;
- sobrecarga de ejecución;
- análisis de experimentos.

---

## Criterio empírico 1: sobrecarga de ejecución

Se realizó un benchmark para medir el costo adicional introducido por las herramientas de seguimiento.

Se realizaron cinco repeticiones bajo tres condiciones:

1. entrenamiento sin herramienta de tracking;
2. entrenamiento utilizando MLflow;
3. entrenamiento utilizando Weights & Biases.

Resultados de la última ejecución:

| Herramienta | Mediana | Sobrecarga |
|---|---:|---:|
| Sin tracking | 0.7557 s | 0 % |
| MLflow | 0.7514 s | -0.58 % |
| Weights & Biases | 4.1075 s | 443.51 % |

Los resultados representan únicamente el entorno y la configuración utilizados durante este proyecto.

La diferencia de -0.58 % de MLflow es pequeña y puede atribuirse a la variabilidad temporal de las cinco repeticiones; no demuestra que el tracking acelere el entrenamiento. En este benchmark no se detectó una sobrecarga relevante de MLflow.

MLflow trabajó con almacenamiento local.

Weights & Biases utilizó sincronización remota, por lo que el tiempo registrado incluye:

- inicialización del run;
- entrenamiento;
- registro de métricas;
- finalización del run;
- sincronización con los servidores de W&B.

Por este motivo, los resultados no permiten concluir que W&B sea universalmente más lento que MLflow.

---

## Criterio empírico 2: esfuerzo de instrumentación

También se midió el esfuerzo necesario para integrar el seguimiento básico de experimentos.

Para evitar que el resultado dependa del formato del código o del número de saltos de línea, se utilizó como unidad de medida el número de métodos distintos de las APIs necesarios para instrumentar el seguimiento básico: inicialización, registro de configuración y registro de métricas.

### MLflow

Se utilizaron tres métodos principales:

```python
mlflow.start_run()
mlflow.log_params()
mlflow.log_metrics()
```

Total:

```text
3 métodos API
```

### Weights & Biases

Se utilizaron dos métodos principales:

```python
wandb.init(config=...)
run.log()
```

Total:

```text
2 métodos API
```

Resumen:

| Herramienta | Métodos API distintos |
|---|---:|
| MLflow | 3 |
| Weights & Biases | 2 |

Dentro del pipeline desarrollado, W&B necesitó un método específico menos para instrumentar el seguimiento básico.

Este resultado no significa que W&B sea universalmente más sencillo que MLflow.

El esfuerzo de integración puede variar según las funcionalidades utilizadas, por ejemplo:

- registro de modelos;
- versionado de datasets;
- gestión de artefactos;
- configuración del servidor;
- autenticación;
- almacenamiento remoto.

---

## Versionado de datos

Después de la limpieza se genera una versión persistente del conjunto de datos:

```text
data/processed/wine_quality_clean.csv
```

La versión del dataset incluye:

- nombre del dataset;
- UCI Dataset ID;
- versión lógica;
- número de filas;
- número de columnas;
- variable objetivo;
- número de duplicados eliminados;
- proporciones de entrenamiento, validación y prueba;
- semilla utilizada para la partición;
- hash SHA-256.

La versión utilizada en el proyecto corresponde a:

```text
v1
```

El dataset es registrado en:

- MLflow como Dataset y artefacto;
- Weights & Biases como Artifact de tipo `dataset`.

Esto permite conocer exactamente qué versión de los datos fue utilizada durante los experimentos.

---

## Versionado del modelo

Después de reproducir la configuración seleccionada, el modelo se reentrena con todo el conjunto de desarrollo y se almacena localmente como:

```text
results/modelos/random_forest_mejor.joblib
```

El modelo final también se registra en MLflow y en Weights & Biases como Artifact:

```text
random-forest-wine-quality
```

Esto permite mantener trazabilidad entre:

```text
Dataset
   ↓
Configuración
   ↓
Experimento
   ↓
Métricas
   ↓
Modelo
```

La carpeta de modelos generados localmente no se versiona mediante Git.

---

## Estructura del repositorio

```text
mlops-wine-quality/
├── README.md
├── requirements.txt
├── .gitignore
│
├── data/
│   ├── README.md
│   └── processed/
│       ├── wine_quality_clean.csv
│       └── wine_quality_clean_metadata.json
│
├── notebooks/
│   ├── 00_mlflow_minimo.ipynb
│   └── wine_quality_mlops.ipynb
│
├── results/
│   ├── metrics/
│   │   ├── mlflow_experimentos.csv
│   │   ├── wandb_experimentos.csv
│   │   ├── comparacion_sobrecarga.csv
│   │   └── evaluacion_final.csv
│   │
│   └── screenshots/                # evidencias de errores
│
└── docs/
    ├── errores.md
    ├── referencias.md
    └── registro.md
```

Los directorios generados automáticamente por MLflow, W&B y los modelos locales se encuentran excluidos mediante `.gitignore`.

---

## Entorno utilizado

El proyecto fue desarrollado y probado con:

```text
Python 3.13.2
NumPy 2.5.3
Pandas 3.0.6
Scikit-learn 1.9.1
MLflow 3.16.1
Weights & Biases 0.30.0
ucimlrepo 0.0.7
```

Sistema utilizado durante el desarrollo:

```text
Linux
```

---

## Instalación

Clonar el repositorio:

```bash
git clone https://github.com/bryzcoll/mlops-wine-quality.git
```

Entrar al proyecto:

```bash
cd mlops-wine-quality
```

Crear un entorno virtual:

```bash
python -m venv .venv
```

---

## Activar el entorno virtual

### Linux

```bash
source .venv/bin/activate
```

### Windows PowerShell

```powershell
.venv\Scripts\Activate.ps1
```

---

## Instalar dependencias

```bash
pip install -r requirements.txt
```

---

## Ejecución del notebook

Abrir:

```text
notebooks/wine_quality_mlops.ipynb
```

Puede ejecutarse utilizando:

- Visual Studio Code;
- Jupyter Notebook;
- JupyterLab.

Las rutas se resuelven desde la raíz del repositorio, por lo que se recomienda iniciar Jupyter desde esa carpeta:

```bash
jupyter lab notebooks/wine_quality_mlops.ipynb
```

Se recomienda reiniciar el kernel y ejecutar todas las celdas en orden desde el inicio.

El flujo general del notebook es:

```text
Carga del dataset
        ↓
Exploración inicial
        ↓
Limpieza
        ↓
Eliminación de duplicados
        ↓
Partición train/validation/test (60/20/20)
        ↓
Modelo baseline
        ↓
10 experimentos con MLflow
        ↓
Selección del mejor experimento
        ↓
Reproducción
        ↓
Reentrenamiento con el conjunto de desarrollo
        ↓
Evaluación única sobre prueba aislada
        ↓
10 experimentos con W&B
        ↓
Benchmark de sobrecarga
        ↓
Registro del mejor modelo
        ↓
Versionado del dataset
        ↓
Segundo criterio empírico
        ↓
Comparación final
        ↓
Conclusiones
```

---

## Autenticación de Weights & Biases

Para ejecutar las secciones relacionadas con W&B es necesario disponer de una cuenta de Weights & Biases.

La autenticación puede realizarse desde la terminal:

```bash
wandb login
```

En modo `online`, el notebook también ejecuta:

```python
wandb.login()
```

La clave API no debe almacenarse directamente dentro del notebook ni versionarse en Git.

Para comprobar el pipeline sin sincronizar nuevas corridas se puede iniciar Jupyter con `WANDB_MODE=offline`; el modo utilizado queda impreso en la ejecución.

---

## Archivos ignorados por Git

Los siguientes directorios y archivos se generan automáticamente durante la ejecución y no deben almacenarse en Git:

```text
mlruns/
mlflow.db
wandb/
results/modelos/
```

Estas rutas están incluidas dentro de `.gitignore`.

MLflow y W&B se encargan de mantener el registro correspondiente de los experimentos y artefactos.

---

## Resultados generados

Los resultados resumidos de los experimentos son almacenados en:

```text
results/metrics/mlflow_experimentos.csv
results/metrics/wandb_experimentos.csv
results/metrics/comparacion_sobrecarga.csv
results/metrics/evaluacion_final.csv
```

Estos archivos permiten consultar los resultados sin necesidad de volver a ejecutar todos los experimentos.

---

## Reproducibilidad del proyecto

Para favorecer la reproducibilidad se aplicaron las siguientes prácticas:

- semilla fija `random_state=42`;
- partición estratificada fija de entrenamiento, validación y prueba;
- aislamiento del conjunto de prueba hasta la evaluación final;
- mismas configuraciones para MLflow y W&B;
- registro de hiperparámetros;
- registro de métricas;
- versionado del dataset;
- cálculo de hash SHA-256;
- versionado del modelo;
- registro de versiones de las principales dependencias;
- reproducción del mejor experimento.

Los tiempos de ejecución no necesariamente serán idénticos al ejecutar el proyecto en otro entorno.

Estos valores pueden variar debido a:

- hardware;
- carga del sistema;
- versión de las dependencias;
- conexión a Internet;
- latencia de servicios externos.

---

## Documentación de errores

Durante el desarrollo se registraron diferentes errores y las soluciones aplicadas.

La documentación se encuentra en [docs/errores.md](docs/errores.md).

Entre los problemas documentados se encuentran:

- kernel de Jupyter incorrecto;
- dependencias faltantes;
- problemas de serialización de modelos en MLflow;
- restricciones del campo `digest` de MLflow;
- errores de ejecución de celdas fuera de orden;
- problemas de registro de Artifacts en W&B;
- manejo de directorios generados automáticamente.

La documentación de estos errores forma parte del proceso de reproducibilidad y trazabilidad del proyecto.

---

## Referencias

Las fuentes oficiales, técnicas y académicas utilizadas para el desarrollo del proyecto se encuentran en [docs/referencias.md](docs/referencias.md).

Se incluyen:

- documentación oficial de MLflow;
- documentación oficial de Weights & Biases;
- documentación del UCI Machine Learning Repository;
- artículos técnicos y académicos relacionados con MLflow y MLOps.

---

## Uso de inteligencia artificial generativa

Durante el desarrollo del proyecto se utilizó **ChatGPT** como herramienta de apoyo.

La inteligencia artificial generativa fue utilizada principalmente para:

- explicar mensajes de error;
- analizar posibles causas de errores;
- revisar fragmentos de código;
- proponer alternativas de solución;
- apoyar la estructuración del notebook;
- mejorar la documentación técnica;
- analizar criterios para comparar MLflow y Weights & Biases;
- apoyar la redacción del README y otros documentos del proyecto.

Las respuestas proporcionadas por la inteligencia artificial no fueron incorporadas automáticamente al proyecto.

Cada sugerencia relevante fue validada mediante:

- ejecución directa del código;
- revisión de los resultados producidos;
- comparación con la documentación oficial;
- consulta de fuentes técnicas y académicas;
- revisión manual de los integrantes del grupo.

Los errores encontrados durante este proceso y sus respectivas correcciones se encuentran documentados en:

```text
docs/errores.md
```

No se utilizaron datos personales ni información sensible durante las consultas realizadas a herramientas de inteligencia artificial generativa.

---

## Limitaciones

Los resultados obtenidos deben interpretarse dentro del contexto experimental del proyecto.

En particular:

- el benchmark fue realizado en un único entorno;
- MLflow utilizó almacenamiento local;
- W&B utilizó sincronización remota;
- el rendimiento puede cambiar en otros equipos o redes;
- el número de métodos API distintos no representa por sí solo la complejidad completa de una herramienta;
- Random Forest fue utilizado como modelo experimental y no se realizó una comparación exhaustiva entre diferentes algoritmos de Machine Learning.
- la selección utiliza una única partición de validación; como trabajo futuro se recomienda validación cruzada anidada para reducir la dependencia de una sola partición.

Por tanto, los resultados permiten comparar ambas herramientas dentro del entorno evaluado, pero no establecer que una herramienta sea universalmente superior a la otra.

---

## Conclusiones principales

El proyecto permitió implementar un pipeline reproducible de experimentación con Machine Learning utilizando MLflow y Weights & Biases.

Se realizaron diez configuraciones experimentales de Random Forest y se identificó el experimento 10 como la mejor configuración según el RMSE de validación.

El mejor experimento pudo reproducirse utilizando la misma configuración, datos, división y semilla. La configuración seleccionada obtuvo RMSE 0.688779 en validación y 0.689675 en la prueba final aislada.

También se implementó versionado explícito de datos y modelos.

En la comparación experimental:

- MLflow presentó menor sobrecarga en el entorno local evaluado;
- W&B necesitó un método específico menos para realizar el tracking básico;
- W&B proporcionó una interfaz web centralizada;
- MLflow ofreció un flujo ligero para seguimiento local;
- ambas herramientas permitieron mantener trazabilidad de los experimentos.

El proyecto demuestra que un flujo MLOps no se limita únicamente al entrenamiento de un modelo, sino que también requiere conservar información sobre:

- datos;
- configuraciones;
- métricas;
- artefactos;
- modelos;
- entorno de ejecución.

Esto permite que los experimentos puedan ser identificados, comparados y reproducidos posteriormente.

---

## Créditos

Proyecto desarrollado por:

**Bryan Macas**

**Joseph Sangurima**

Asignatura:

**Ciencia de Datos**

Institución:

**Universidad de Cuenca**
