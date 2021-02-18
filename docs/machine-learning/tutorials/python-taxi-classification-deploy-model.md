---
title: 'Tutorial de Python: Ejecución de predicciones en procedimientos almacenados de SQL'
titleSuffix: SQL machine learning
description: En la parte cinco de esta serie de tutoriales de cinco partes, pondrá en marcha scripts de Python insertados en procedimientos almacenados de SQL con funciones T-SQL con el aprendizaje automático de SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 07/29/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 39717778ca2456d77b6ceff4ba72e8de06dac75f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339703"
---
# <a name="python-tutorial-run-predictions-using-python-embedded-in-a-stored-procedure"></a>Tutorial de Python: Ejecución de predicciones con Python insertado en un procedimiento almacenado
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

En la parte cinco de esta serie de tutoriales de cinco partes, aprenderá a *poner en marcha* los modelos entrenados y guardados en la parte anterior.

En este escenario, por operacionalización se entiende implementar el modelo en producción para su puntuación. La integración con SQL Server hace que esto sea bastante sencillo, ya que se puede insertar código de Python en un procedimiento almacenado. Para obtener predicciones del modelo en función de nuevas entradas, basta con llamar al procedimiento almacenado desde una aplicación y pasar los nuevos datos.

En esta parte del tutorial se muestran dos métodos para crear predicciones basadas en un modelo de Python: puntuación por lotes y puntuación fila por fila.

+ **Puntuación por lotes:** para proporcionar varias filas de datos de entrada, pase una consulta SELECT como argumento al procedimiento almacenado. El resultado es una tabla de observaciones correspondientes a los casos de entrada.
+ **Puntuación individual:** Pase un conjunto de valores de parámetros individuales como entrada.  El procedimiento almacenado devuelve una sola fila o valor.

Todo el código de Python necesario para la puntuación se facilita como parte de los procedimientos almacenados.

En este artículo, hará lo siguiente:

> [!div class="checklist"]
> + Crear y usar procedimientos almacenados para la puntuación por lotes
> + Crear y usar procedimientos almacenados para puntuar una sola fila

En la [parte uno](python-taxi-classification-introduction.md), ha instalado los requisitos previos y ha restaurado la base de datos de ejemplo.

En la [parte dos](python-taxi-classification-explore-data.md), ha explorado los datos de ejemplo y ha generado algunos trazados.

En la [tres](python-taxi-classification-create-features.md), aprendió a crear características a partir de datos sin procesar mediante una función de Transact-SQL. Después, llamó a esa función desde un procedimiento almacenado para crear una tabla que contiene los valores de las características.

En la [parte cuatro](python-taxi-classification-train-model.md), cargó los módulos y llamó a las funciones necesarias para crear y entrenar el modelo mediante un procedimiento almacenado de SQL Server.

## <a name="batch-scoring"></a>Puntuación por lotes

Los dos primeros procedimientos almacenados ilustran la sintaxis básica para ajustar una llamada de predicción de Python en un procedimiento almacenado. Ambos procedimientos almacenados requieren una tabla de datos como entradas.

+ El nombre del modelo exacto que se debe usar se proporciona como un parámetro de entrada en el procedimiento almacenado. El procedimiento almacenado carga el modelo serializado desde la tabla de base de datos `nyc_taxi_models` mediante la instrucción SELECT del procedimiento almacenado.
+ El modelo serializado se almacena en la variable de Python `mod` para su posterior procesamiento mediante Python.
+ Los nuevos casos que se deben puntuar se obtienen de la consulta [!INCLUDE[tsql](../../includes/tsql-md.md)] especificada en `@input_data_1`. Cuando se leen los datos de la consulta, las filas se guardan en la trama de datos predeterminada, `InputDataSet`.
+ Ambos procedimientos almacenados usan funciones de `sklearn` para calcular una métrica de precisión, AUC (correspondiente a área en curva). Las métricas de precisión, como AUC, solo se pueden generar si se proporciona también la etiqueta de destino (la columna _tipped_). Las predicciones no necesitan la etiqueta de destino (variable `y`), pero sí el cálculo de la métrica de precisión.

  En consecuencia, si no hay etiquetas de destino correspondientes a los datos que se van a puntuar, puede modificar el procedimiento almacenado para quitar los cálculos de AUC y devolver solo las probabilidades de propina de las características (variable `X` del procedimiento almacenado).

### <a name="predicttipscikitpy"></a>PredictTipSciKitPy

Ejecute las siguientes instrucciones T-SQL para crear los procedimientos almacenados. Este procedimiento almacenado requiere un modelo basado en el paquete scikit-learn, ya que usa funciones específicas de ese paquete.

La trama de datos que contiene entradas se pasa a la función `predict_proba` del modelo de regresión logística, `mod`. La función `predict_proba` (`probArray = mod.predict_proba(X)`) devuelve un valor **float** que representa la probabilidad de que conseguir propina (del importe que sea).

```sql
DROP PROCEDURE IF EXISTS PredictTipSciKitPy;
GO

CREATE PROCEDURE [dbo].[PredictTipSciKitPy] (@model varchar(50), @inquery nvarchar(max))
AS
BEGIN
DECLARE @lmodel2 varbinary(max) = (select model from nyc_taxi_models where name = @model);
EXEC sp_execute_external_script
  @language = N'Python',
  @script = N'
import pickle;
import numpy;
from sklearn import metrics

mod = pickle.loads(lmodel2)
X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]
y = numpy.ravel(InputDataSet[["tipped"]])

probArray = mod.predict_proba(X)
probList = []
for i in range(len(probArray)):
  probList.append((probArray[i])[1])

probArray = numpy.asarray(probList)
fpr, tpr, thresholds = metrics.roc_curve(y, probArray)
aucResult = metrics.auc(fpr, tpr)
print ("AUC on testing data is: " + str(aucResult))

OutputDataSet = pandas.DataFrame(data = probList, columns = ["predictions"])
',  
  @input_data_1 = @inquery,
  @input_data_1_name = N'InputDataSet',
  @params = N'@lmodel2 varbinary(max)',
  @lmodel2 = @lmodel2
WITH RESULT SETS ((Score float));
END
GO
```

### <a name="predicttiprxpy"></a>PredictTipRxPy

Este procedimiento almacenado usa las mismas entradas y crea el mismo tipo de puntuaciones que el procedimiento almacenado anterior, pero usa las funciones del paquete **revoscalepy** proporcionado con SQL Server Machine Learning.

```sql
DROP PROCEDURE IF EXISTS PredictTipRxPy;
GO

CREATE PROCEDURE [dbo].[PredictTipRxPy] (@model varchar(50), @inquery nvarchar(max))
AS
BEGIN
DECLARE @lmodel2 varbinary(max) = (select model from nyc_taxi_models where name = @model);
EXEC sp_execute_external_script 
  @language = N'Python',
  @script = N'
import pickle;
import numpy;
from sklearn import metrics
from revoscalepy.functions.RxPredict import rx_predict;

mod = pickle.loads(lmodel2)
X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]
y = numpy.ravel(InputDataSet[["tipped"]])

probArray = rx_predict(mod, X)
probList = probArray["tipped_Pred"].values 

probArray = numpy.asarray(probList)
fpr, tpr, thresholds = metrics.roc_curve(y, probArray)
aucResult = metrics.auc(fpr, tpr)
print ("AUC on testing data is: " + str(aucResult))

OutputDataSet = pandas.DataFrame(data = probList, columns = ["predictions"])
',
  @input_data_1 = @inquery,
  @input_data_1_name = N'InputDataSet',
  @params = N'@lmodel2 varbinary(max)',
  @lmodel2 = @lmodel2
WITH RESULT SETS ((Score float));
END
GO
```

## <a name="run-batch-scoring-using-a-select-query"></a>Ejecución de una puntuación por lotes mediante una consulta SELECT

Los procedimientos almacenados **PredictTipSciKitPy** y **PredictTipRxPy** requieren dos parámetros de entrada:

+ La consulta que recupera los datos para la puntuación
+ El nombre de un modelo entrenado

Al pasar esos argumentos al procedimiento almacenado, puede seleccionar un modelo determinado o cambiar los datos que se usan para la puntuación.

1. Para usar el modelo **scikit-learn** para la puntuación, llame al procedimiento almacenado **PredictTipSciKitPy**, pasando como entradas el nombre del modelo y la cadena de consulta.

   ```sql
   DECLARE @query_string nvarchar(max) -- Specify input query
     SET @query_string='
     select tipped, fare_amount, passenger_count, trip_time_in_secs, trip_distance,
     dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as direct_distance
     from nyctaxi_sample_testing'
   EXEC [dbo].[PredictTipSciKitPy] 'SciKit_model', @query_string;
   ```

   El procedimiento almacenado devuelve las probabilidades previstas de cada trayecto que se pasaron como parte de la consulta de entrada. 

   Si usa SSMS (SQL Server Management Studio) para ejecutar las consultas, las probabilidades aparecerán como una tabla en el panel **Resultados**. El panel **Mensajes** muestra la métrica de precisión (AUC, o área en curva) con un valor aproximado de 0,56.

2. Para usar el modelo **revoscalepy** para la puntuación, llame al procedimiento almacenado **PredictTipRxPy**, pasando como entradas el nombre del modelo y la cadena de consulta.

   ```sql
   DECLARE @query_string nvarchar(max) -- Specify input query
     SET @query_string='
     select tipped, fare_amount, passenger_count, trip_time_in_secs, trip_distance,
     dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as direct_distance
     from nyctaxi_sample_testing'
   EXEC [dbo].[PredictTipRxPy] 'revoscalepy_model', @query_string;
   ```

## <a name="single-row-scoring"></a>Puntuación de fila única

A veces, en lugar de la puntuación por lotes, puede que prefiera pasar un solo caso, para lo cual se obtienen los valores de una aplicación y se devuelve un único resultado en función de esos valores. Por ejemplo, podría configurar una hoja de cálculo de Excel, una aplicación web o un informe para llamar al procedimiento almacenado y proporcionarle entradas escritas o seleccionadas por los usuarios.

En esta sección, aprenderá a crear predicciones únicas llamando a dos procedimientos almacenados:

+ [PredictTipSingleModeSciKitPy](#predicttipsinglemodescikitpy) está diseñado para la puntuación de fila única con el modelo scikit-learn.
+ [PredictTipSingleModeRxPy](#predicttipsinglemoderxpy) está diseñado para la puntuación de fila única con el modelo revoscalepy.
+ Si aún no ha entrenado un modelo, vuelva a la [parte cinco](python-taxi-classification-train-model.md).

Ambos modelos toman como entrada una serie de valores únicos, como el número de pasajeros, la distancia del trayecto, etc. Una función con valores de tabla, `fnEngineerFeatures`, se usa para convertir los valores de latitud y longitud de las entradas en una nueva característica: distancia directa. La [parte cuatro](python-taxi-classification-create-features.md) contiene una descripción de esta función con valores de tabla.

Ambos procedimientos almacenados crean una puntuación basada en el modelo de Python.

> [!NOTE]
>
> Si se llama al procedimiento almacenado desde una aplicación externa, es importante facilitar todas las características de entrada que el modelo de Python requiere. Para no cometer errores, puede que tenga que convertir los datos de entrada en un tipo de datos de Python, además de validar el tipo y la longitud de los datos.

### <a name="predicttipsinglemodescikitpy"></a>PredictTipSingleModeSciKitPy

Dedique un minuto a revisar el código del procedimiento almacenado que realiza la puntuación con el modelo **scikit-learn**.

```sql
DROP PROCEDURE IF EXISTS PredictTipSingleModeSciKitPy;
GO

CREATE PROCEDURE [dbo].[PredictTipSingleModeSciKitPy] (@model varchar(50), @passenger_count int = 0,
  @trip_distance float = 0,
  @trip_time_in_secs int = 0,
  @pickup_latitude float = 0,
  @pickup_longitude float = 0,
  @dropoff_latitude float = 0,
  @dropoff_longitude float = 0)
AS
BEGIN
  DECLARE @inquery nvarchar(max) = N'
  SELECT * FROM [dbo].[fnEngineerFeatures]( 
    @passenger_count,
    @trip_distance,
    @trip_time_in_secs,
    @pickup_latitude,
    @pickup_longitude,
    @dropoff_latitude,
    @dropoff_longitude)
    '
DECLARE @lmodel2 varbinary(max) = (select model from nyc_taxi_models where name = @model);
EXEC sp_execute_external_script 
  @language = N'Python',
  @script = N'
import pickle;
import numpy;

# Load model and unserialize
mod = pickle.loads(model)

# Get features for scoring from input data
X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]

# Score data to get tip prediction probability as a list (of float)
probList = []
probList.append((mod.predict_proba(X)[0])[1])

# Create output data frame
OutputDataSet = pandas.DataFrame(data = probList, columns = ["predictions"])
',
  @input_data_1 = @inquery,
  @params = N'@model varbinary(max),@passenger_count int,@trip_distance float,
    @trip_time_in_secs int ,
    @pickup_latitude float ,
    @pickup_longitude float ,
    @dropoff_latitude float ,
    @dropoff_longitude float',
    @model = @lmodel2,
    @passenger_count =@passenger_count ,
    @trip_distance=@trip_distance,
    @trip_time_in_secs=@trip_time_in_secs,
    @pickup_latitude=@pickup_latitude,
    @pickup_longitude=@pickup_longitude,
    @dropoff_latitude=@dropoff_latitude,
    @dropoff_longitude=@dropoff_longitude
WITH RESULT SETS ((Score float));
END
GO
```

### <a name="predicttipsinglemoderxpy"></a>PredictTipSingleModeRxPy

El siguiente procedimiento almacenado realiza la puntuación mediante el modelo **revoscalepy**.

```sql
DROP PROCEDURE IF EXISTS PredictTipSingleModeRxPy;
GO

CREATE PROCEDURE [dbo].[PredictTipSingleModeRxPy] (@model varchar(50), @passenger_count int = 0,
  @trip_distance float = 0,
  @trip_time_in_secs int = 0,
  @pickup_latitude float = 0,
  @pickup_longitude float = 0,
  @dropoff_latitude float = 0,
  @dropoff_longitude float = 0)
AS
BEGIN
DECLARE @inquery nvarchar(max) = N'
  SELECT * FROM [dbo].[fnEngineerFeatures]( 
    @passenger_count,
    @trip_distance,
    @trip_time_in_secs,
    @pickup_latitude,
    @pickup_longitude,
    @dropoff_latitude,
    @dropoff_longitude)
  '
DECLARE @lmodel2 varbinary(max) = (select model from nyc_taxi_models where name = @model);
EXEC sp_execute_external_script 
  @language = N'Python',
  @script = N'
import pickle;
import numpy;
from revoscalepy.functions.RxPredict import rx_predict;

# Load model and unserialize
mod = pickle.loads(model)

# Get features for scoring from input data
X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]

# Score data to get tip prediction probability as a list (of float)

probArray = rx_predict(mod, X)

probList = []
probList = probArray["tipped_Pred"].values

# Create output data frame
OutputDataSet = pandas.DataFrame(data = probList, columns = ["predictions"])
',
  @input_data_1 = @inquery,
  @params = N'@model varbinary(max),@passenger_count int,@trip_distance float,
    @trip_time_in_secs int ,
    @pickup_latitude float ,
    @pickup_longitude float ,
    @dropoff_latitude float ,
    @dropoff_longitude float',
    @model = @lmodel2,
    @passenger_count =@passenger_count ,
    @trip_distance=@trip_distance,
    @trip_time_in_secs=@trip_time_in_secs,
    @pickup_latitude=@pickup_latitude,
    @pickup_longitude=@pickup_longitude,
    @dropoff_latitude=@dropoff_latitude,
    @dropoff_longitude=@dropoff_longitude
WITH RESULT SETS ((Score float));
END
GO
```

### <a name="generate-scores-from-models"></a>Generación de puntuaciones a partir de modelos

Una vez creados los procedimientos almacenados, es fácil generar una puntuación basada en cualquiera de los modelos. Solo tiene que abrir una nueva ventana **Consulta** y escribir o pegar los parámetros de cada una de las columnas de característica. Los siete valores necesarios en estas columnas de característica son, en orden:

+ *passenger_count*
+ *trip_distance*
+ *trip_time_in_secs*
+ *pickup_latitude*
+ *pickup_longitude*
+ *dropoff_latitude*
+ *dropoff_longitude*

1. Ejecute esta instrucción para generar una predicción mediante el modelo **revoscalepy**:
  
   ```sql
   EXEC [dbo].[PredictTipSingleModeRxPy] 'revoscalepy_model', 1, 2.5, 631, 40.763958,-73.973373, 40.782139,-73.977303
   ```

2. Ejecute esta instrucción para generar una puntuación mediante el modelo **scikit-learn**:

   ```sql
   EXEC [dbo].[PredictTipSingleModeSciKitPy] 'SciKit_model', 1, 2.5, 631, 40.763958,-73.973373, 40.782139,-73.977303
   ```

La salida de ambos procedimientos es la probabilidad de que se abone una propina por el trayecto en taxi, dados los parámetros o características especificados.

## <a name="conclusions"></a>Conclusiones

En esta serie de tutoriales, ha aprendido a trabajar con código de Python insertado en procedimientos almacenados. La integración con [!INCLUDE[tsql](../../includes/tsql-md.md)] hace mucho más fácil la implementación de modelos de Python para la predicción y la incorporación del reciclaje de modelos como parte de un flujo de trabajo de datos empresarial.

## <a name="next-steps"></a>Pasos siguientes

En este artículo:

> [!div class="checklist"]
> + Creó y usó procedimientos almacenados para la puntuación por lotes
> + Creó y usó procedimientos almacenados para puntuar una sola fila

Para más información sobre Python, consulte [Extensión de Python en SQL Server](../concepts/extension-python.md).
