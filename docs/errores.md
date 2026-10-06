### Error al registrar Random Forest en MLflow

Al intentar registrar el modelo mediante `mlflow.sklearn.log_model()`
se produjo el error:

`UntrustedTypesFoundException: ['sklearn.tree._tree.Tree']`

#### Causa

MLflow utiliza el formato de serialización `skops`, que valida los
tipos contenidos en el modelo antes de permitir su posterior carga.
RandomForestRegressor utiliza internamente objetos del tipo
`sklearn.tree._tree.Tree`, que no fueron considerados confiables
automáticamente.

#### Solución

Como el modelo fue entrenado localmente dentro del propio pipeline,
se añadió explícitamente dicho tipo mediante:

`skops_trusted_types=["sklearn.tree._tree.Tree"]`

Esto permitió mantener el formato seguro `skops` y registrar
correctamente el modelo.
## Error al registrar el dataset en MLflow: digest demasiado largo

### Error

Al intentar registrar el dataset procesado en MLflow mediante `mlflow.log_input()`, se produjo la siguiente excepción:

```text
MlflowException: 'digest' exceeds the maximum length of 36 characters

#### Causa
Se utilizó directamente el hash SHA-256 completo del archivo como valor del parámetro digest al crear el dataset con mlflow.data.from_pandas().
Un hash SHA-256 contiene 64 caracteres hexadecimales, mientras que MLflow limita el campo digest a un máximo de 36 caracteres.
Código que produjo el error:
dataset_mlflow = mlflow.data.from_pandas(
    df=df,
    source="https://archive.ics.uci.edu/dataset/186/wine+quality",
    targets="quality",
    name="wine-quality-clean-v1",
    digest=sha256_datos
)

#### Solución
Se decidió dejar que MLflow genere automáticamente su propio digest y conservar el SHA-256 completo como un parámetro adicional de trazabilidad.
Código corregido:
dataset_mlflow = mlflow.data.from_pandas(
    df=df,
    source="https://archive.ics.uci.edu/dataset/186/wine+quality",
    targets="quality",
    name="wine-quality-clean-v1"
)

with mlflow.start_run(
    run_name="dataset-v1"
) as run:

    mlflow.log_input(
        dataset_mlflow,
        context="training"
    )

    mlflow.log_param(
        "dataset_sha256",
        sha256_datos
    )
