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