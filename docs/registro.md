# Registro de implementación

## MLflow - ejemplo mínimo

**Fecha:** 4 de octubre de 2026

### Objetivo

Comprobar el funcionamiento básico de MLflow antes de integrarlo
al pipeline de Wine Quality.

### Entorno

- Python: 3.13
- MLflow: 3.16.1
- Scikit-learn: 1.9.1

### Procedimiento

1. Se creó un entorno virtual específico para el proyecto.
2. Se configuró el kernel `Python (mlops-wine-quality)`.
3. Se creó el experimento `mlflow-prueba-minima`.
4. Se registró una primera ejecución.
5. Se registraron parámetros y métricas.
6. Se verificó el almacenamiento local mediante SQLite.
7. Se comprobó la ejecución desde MLflow UI.

### Resultado

MLflow registró correctamente los parámetros y métricas de las
ejecuciones y permitió consultar los experimentos almacenados.

### Errores encontrados

Durante la configuración inicial, VS Code utilizaba un kernel
perteneciente a otro proyecto, por lo que no encontraba el módulo
`mlflow`.

**Solución:** se creó y configuró un entorno virtual propio y se
registró como kernel de Jupyter.

### Tiempo de configuración

Inicio: 14:17
Fin: 14:50
Tiempo total: 33 minutos