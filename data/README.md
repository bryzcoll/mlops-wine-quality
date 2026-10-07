# Datos del proyecto

## Fuente

El proyecto utiliza el conjunto de datos **Wine Quality** del UCI Machine
Learning Repository:

- Identificador UCI: `186`
- DOI: <https://doi.org/10.24432/C56S3T>
- Licencia: Creative Commons Attribution 4.0 International (CC BY 4.0)
- Fuente: <https://archive.ics.uci.edu/dataset/186/wine+quality>

El notebook obtiene los datos originales mediante `fetch_ucirepo(id=186)`.
Por ello, no se almacena una copia adicional de los datos crudos.

## Procesamiento

El conjunto original contiene 6497 registros. El pipeline:

1. combina las variables predictoras y la variable objetivo `quality`;
2. verifica tipos de datos y valores faltantes;
3. elimina 1179 registros completamente duplicados;
4. conserva 5318 observaciones y 12 columnas;
5. guarda exactamente el conjunto limpio utilizado por los experimentos.

No se imputan valores porque el conjunto obtenido no contiene datos faltantes.

## Archivos versionados

- `processed/wine_quality_clean.csv`: conjunto limpio utilizado por el pipeline.
- `processed/wine_quality_clean_metadata.json`: procedencia, dimensiones,
  versión lógica y huella SHA-256.

La integridad del CSV se comprueba durante la ejecución comparando su hash con
el valor registrado en la metadata. La versión actual es `v1`.

## División experimental

Con `random_state=42`, los datos limpios se separan de forma estratificada en:

- 3190 registros (aprox. 60 %) para entrenamiento;
- 1064 registros (aprox. 20 %) para validación y selección de hiperparámetros;
- 1064 registros (aprox. 20 %) para la evaluación final del modelo seleccionado.

El conjunto de prueba no participa en la selección de hiperparámetros.
