---
title: 'Tutorial de R: Ejecución de predicciones en procedimientos almacenados de SQL'
titleSuffix: SQL machine learning
description: En la parte cinco de esta serie de tutoriales de cinco partes, podrá usar scripts de R insertados en procedimientos almacenados de SQL con funciones de T-SQL con el aprendizaje automático de SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 10/15/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: a3c46e719f4889ee53c98e671bd9569193b86e2a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349059"
---
# <a name="r-tutorial-run-predictions-in-sql-stored-procedures"></a>Tutorial de R: Ejecución de predicciones en procedimientos almacenados de SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

En la parte cinco de esta serie de tutoriales de cinco partes, aprenderá a *usar* el modelo que ha entrenado y guardado en la parte anterior usando el modelo para predecir posibles resultados. Se ajusta el modelo en un procedimiento almacenado, al que otras aplicaciones pueden llamar directamente.

En este artículo se muestran dos maneras de realizar la puntuación:

+ **Modo de puntuación por lotes**: Use una consulta SELECT como entrada para el procedimiento almacenado. El procedimiento almacenado devuelve una tabla de observaciones correspondientes a los casos de entrada.

+ **Modo de puntuación individual**: pasar un conjunto de valores de parámetros individuales como entrada.  El procedimiento almacenado devuelve una sola fila o valor.

En este artículo, hará lo siguiente:

> [!div class="checklist"]
> + Crear y usar procedimientos almacenados para la puntuación por lotes
> + Crear y usar procedimientos almacenados para puntuar una sola fila

En la [parte uno](r-taxi-classification-introduction.md), ha instalado los requisitos previos y ha restaurado la base de datos de ejemplo.

En la [parte dos](r-taxi-classification-explore-data.md), revisó los datos de ejemplo y generó algunos trazados.

En la [tres](r-taxi-classification-create-features.md), aprendió a crear características a partir de datos sin procesar mediante una función de Transact-SQL. Después, llamó a esa función desde un procedimiento almacenado para crear una tabla que contiene los valores de las características.

En la [parte cuatro](r-taxi-classification-train-model.md), cargó los módulos y llamó a las funciones necesarias para crear y entrenar el modelo mediante un procedimiento almacenado de SQL Server.

## <a name="basic-scoring"></a>Puntuaciones básicas

El procedimiento almacenado **RPredict** muestra la sintaxis básica para ajustar una llamada de `PREDICT` en un procedimiento almacenado.

```sql
CREATE PROCEDURE [dbo].[RPredict] (@model varchar(250), @inquery nvarchar(max))
AS 
BEGIN 

DECLARE @lmodel2 varbinary(max) = (SELECT model FROM nyc_taxi_models WHERE name = @model);  
EXEC sp_execute_external_script @language = N'R',
  @script = N' 
    mod <- unserialize(as.raw(model));
    print(summary(mod))
    OutputDataSet <- data.frame(predict(mod, InputDataSet, type = "response"));
    str(OutputDataSet)
    print(OutputDataSet)
    ',
  @input_data_1 = @inquery,
  @params = N'@model varbinary(max)',
  @model = @lmodel2 
  WITH RESULT SETS (("Score" float));
END
GO
```

+ La instrucción SELECT obtiene el modelo serializado de la base de datos y lo almacena en la variable de R `mod` para su posterior procesamiento con R.

+ Los nuevos casos que se van a puntuar se obtienen de la consulta de [!INCLUDE[tsql](../../includes/tsql-md.md)] especificada en `@inquery`, el primer parámetro del procedimiento almacenado. Cuando se leen los datos de la consulta, las filas se guardan en la trama de datos predeterminada, `InputDataSet`. Esta trama de datos se pasa a la función [PREDICT](../../t-sql/queries/predict-transact-sql.md) que genera las puntuaciones.
  
  `OutputDataSet <- data.frame(predict(mod, InputDataSet, type = "response"));`
  
  Como data.frame puede contener una sola fila, puede usar el mismo código para la puntuación individual o por lotes.
  
+ El valor devuelto por la función `PREDICT` es un parámetro **float** que representa la probabilidad de que el taxista reciba una propina de cualquier importe.

## <a name="batch-scoring-a-list-of-predictions"></a>Puntuación por lotes (lista de predicciones)

Un escenario más común es generar predicciones para varias observaciones en el modo por lotes. En este paso veremos cómo funciona la puntuación por lotes.

1. Para empezar se obtiene un conjunto más pequeño de los datos de entrada con el que trabajar. Esta consulta crea una lista de los "Diez mejores" viajes con número de pasajeros y otras características necesarias para realizar una predicción.
  
   ```sql
   SELECT TOP 10 a.passenger_count AS passenger_count, a.trip_time_in_secs AS trip_time_in_secs, a.trip_distance AS trip_distance, a.dropoff_datetime AS dropoff_datetime, dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude,dropoff_longitude) AS direct_distance
   
   FROM (SELECT medallion, hack_license, pickup_datetime, passenger_count,trip_time_in_secs,trip_distance, dropoff_datetime, pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude FROM nyctaxi_sample)a

   LEFT OUTER JOIN

   (SELECT medallion, hack_license, pickup_datetime FROM nyctaxi_sample TABLESAMPLE (70 percent) REPEATABLE (98052)    )b

   ON a.medallion=b.medallion AND a.hack_license=b.hack_license 
   AND a.pickup_datetime=b.pickup_datetime
   WHERE b.medallion IS NULL
   ```

   **Ejemplo de resultados**

   ```text
   passenger_count   trip_time_in_secs    trip_distance  dropoff_datetime          direct_distance
   1                 283                  0.7            2013-03-27 14:54:50.000   0.5427964547
   1                 289                  0.7            2013-02-24 12:55:29.000   0.3797099614
   1                 214                  0.7            2013-06-26 13:28:10.000   0.6970098661
   ```

2. Cree un procedimiento almacenado llamado **RPredictBatchOutput** en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].

   ```sql
   CREATE PROCEDURE [dbo].[RPredictBatchOutput] (@model varchar(250), @inquery nvarchar(max))
   AS
   BEGIN
   DECLARE @lmodel2 varbinary(max) = (SELECT model FROM nyc_taxi_models WHERE name = @model);
   EXEC sp_execute_external_script 
     @language = N'R',
     @script = N'
       mod <- unserialize(as.raw(model));
       print(summary(mod))
       OutputDataSet <- data.frame(predict(mod, InputDataSet, type = "response"));
       str(OutputDataSet)
       print(OutputDataSet)
     ',
     @input_data_1 = @inquery,
     @params = N'@model varbinary(max)',
     @model = @lmodel2
     WITH RESULT SETS ((Score float));
   END
   ```

3. Proporcione el texto de la consulta en una variable y páselo como parámetro al procedimiento almacenado:

   ```sql
   -- Define the input data
   DECLARE @query_string nvarchar(max)
   SET @query_string='SELECT TOP 10 a.passenger_count as passenger_count, a.trip_time_in_secs AS trip_time_in_secs, a.trip_distance AS trip_distance, a.dropoff_datetime AS dropoff_datetime, dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude,dropoff_longitude) AS direct_distance FROM  (SELECT medallion, hack_license, pickup_datetime, passenger_count,trip_time_in_secs,trip_distance, dropoff_datetime, pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude FROM nyctaxi_sample  )a   LEFT OUTER JOIN (SELECT medallion, hack_license, pickup_datetime FROM nyctaxi_sample TABLESAMPLE (70 percent) REPEATABLE (98052))b ON a.medallion=b.medallion AND a.hack_license=b.hack_license AND a.pickup_datetime=b.pickup_datetime WHERE b.medallion is null'
   
   -- Call the stored procedure for scoring and pass the input data
   EXEC [dbo].[RPredictBatchOutput] @model = 'RTrainLogit_model', @inquery = @query_string;
   ```
  
El procedimiento almacenado devuelve una serie de valores que representan la predicción para cada uno de las "Diez mejores carreras". Pero las mejores carreras de taxi también son viajes de un solo pasajero con un recorrido relativamente corto, por lo que es poco probable que el taxista reciba una propina.

> [!TIP]
> En lugar de devolver resultados de tipo "propina sí" y "propina no", también podría devolver la puntuación de probabilidad de la predicción y, después, aplicar una cláusula WHERE a los valores de la columna _Score_ para clasificar el resultado como "propina probable" o "propina improbable", con un valor de umbral como 0,5 o 0,7. Este paso no se incluye en el procedimiento almacenado, pero es fácil de implementar.

## <a name="single-row-scoring-of-multiple-inputs"></a>Puntuación de una sola fila de varias entradas

A veces puede interesarle pasar varios valores de entrada y obtener una sola predicción basada en esos valores. Por ejemplo, podría configurar una hoja de cálculo de Excel, una aplicación web o un informe de Reporting Services para llamar al procedimiento almacenado y proporcionar entradas escritas o seleccionadas por los usuarios de esas aplicaciones.

En esta sección, aprenderá a crear predicciones únicas mediante un procedimiento almacenado que toma varias entradas, como el número de pasajeros, la distancia de la carrera, etc. El procedimiento almacenado crea una puntuación basada en el modelo de R almacenado anteriormente.
  
Si llama al procedimiento almacenado desde una aplicación externa, asegúrese de que los datos coinciden con los requisitos del modelo de R. Esto puede incluir asegurarse de que los datos de entrada se pueden convertir a un tipo de datos R o validar el tipo y la longitud de los datos.

1. Cree un procedimiento almacenado **RPredictSingleRow**.
  
   ```sql
   CREATE PROCEDURE [dbo].[RPredictSingleRow] @model varchar(50), @passenger_count int = 0, @trip_distance float = 0, @trip_time_in_secs int = 0, @pickup_latitude float = 0, @pickup_longitude float = 0, @dropoff_latitude float = 0, @dropoff_longitude float = 0
   AS
   BEGIN
   DECLARE @inquery nvarchar(max) = N'SELECT * FROM [dbo].[fnEngineerFeatures](@passenger_count, @trip_distance, @trip_time_in_secs,  @pickup_latitude, @pickup_longitude, @dropoff_latitude, @dropoff_longitude)';
   DECLARE @lmodel2 varbinary(max) = (SELECT model FROM nyc_taxi_models WHERE name = @model);
   EXEC sp_execute_external_script  
     @language = N'R',
     @script = N'  
       mod <- unserialize(as.raw(model));  
       print(summary(mod));  
       OutputDataSet <- data.frame(predict(mod, InputDataSet, type = "response"));
       str(OutputDataSet);
       print(OutputDataSet); 
       ',  
     @input_data_1 = @inquery,  
     @params = N'@model varbinary(max),@passenger_count int,@trip_distance float,@trip_time_in_secs int ,  @pickup_latitude float ,@pickup_longitude float ,@dropoff_latitude float ,@dropoff_longitude float', @model = @lmodel2, @passenger_count =@passenger_count, @trip_distance=@trip_distance, @trip_time_in_secs=@trip_time_in_secs, @pickup_latitude=@pickup_latitude, @pickup_longitude=@pickup_longitude, @dropoff_latitude=@dropoff_latitude, @dropoff_longitude=@dropoff_longitude  
     WITH RESULT SETS ((Score float));  
   END
   ```

2. Pruébelo, proporcionando los valores manualmente.
  
   Abra una nueva ventana **Consulta** para llamar al procedimiento almacenado, indicando valores en cada uno de los parámetros. Los parámetros representan las columnas de características usadas por el modelo y son obligatorios.

   ```sql
   EXEC [dbo].[RPredictSingleRow] @model = 'RTrainLogit_model',
   @passenger_count = 1,
   @trip_distance = 2.5,
   @trip_time_in_secs = 631,
   @pickup_latitude = 40.763958,
   @pickup_longitude = -73.973373,
   @dropoff_latitude =  40.782139,
   @dropoff_longitude = -73.977303
   ```

   Si lo prefiere, puede usar este método más breve compatible con [parámetros para un procedimiento almacenado](../../relational-databases/stored-procedures/specify-parameters.md):
  
   ```sql
   EXEC [dbo].[RPredictSingleRow] 'RTrainLogit_model', 1, 2.5, 631, 40.763958,-73.973373, 40.782139,-73.977303
   ```

3. Los resultados indican que la probabilidad de obtener una propina es baja (cero) en estas diez mejores carreras, ya todas ellas son carreras donde viaja un único pasajero en una distancia relativamente corta.

## <a name="conclusions"></a>Conclusiones

Ahora que ya sabe insertar código de R en procedimientos almacenados, puede aplicarlo para crear sus propios modelos. La integración con [!INCLUDE[tsql](../../includes/tsql-md.md)] hace mucho más fácil la implementación de modelos de R para la predicción y la incorporación del reciclaje de modelos como parte de un flujo de trabajo de datos empresarial.

## <a name="next-steps"></a>Pasos siguientes

En este artículo:

> [!div class="checklist"]
> + Creó y usó procedimientos almacenados para la puntuación por lotes
> + Creó y usó procedimientos almacenados para puntuar una sola fila

Para obtener más información sobre R, consulte [Extensión de R en SQL Server](../concepts/extension-r.md).