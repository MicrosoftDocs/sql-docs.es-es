---
title: 'Tutorial de R: Implementación del modelo'
description: Aprenda a implementar modelos de R en un entorno de producción mediante una llamada a un modelo entrenado desde un procedimiento almacenado.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/26/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 1830c441e51465f9c7cf7eab7ca503eadd677caf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272976"
---
# <a name="deploy-the-r-model-and-use-it-in-sql-server-walkthrough"></a>Implementación del modelo de R y su uso en SQL Server (tutorial)
[!INCLUDE [SQL Server 2016](../../includes/applies-to-version/sqlserver2016.md)]

En esta lección, aprenderá a implementar modelos de R en un entorno de producción mediante una llamada a un modelo entrenado desde un procedimiento almacenado. Puede invocar el procedimiento almacenado de R o de cualquier lenguaje de programación de aplicaciones compatible con [!INCLUDE[tsql](../../includes/tsql-md.md)] (como C#, Java, Python, etc.) y usar el modelo para realizar predicciones a partir de observaciones nuevas.

En este artículo se muestran las dos formas más habituales de usar un modelo en la puntuación:

> [!div class="checklist"]
> * El **modo de puntuación por lotes** genera varias predicciones.
> * El **modo de puntuación individual** genera predicciones de una en una.

## <a name="batch-scoring"></a>Puntuación por lotes

Cree un procedimiento almacenado, *PredictTipBatchMode*, que genere varias predicciones, pasando una consulta o tabla SQL como entrada. Se devuelve una tabla de resultados, que puede insertar directamente en una tabla o escribir en un archivo.

- Obtiene un conjunto de datos de entrada como una consulta SQL
- Llama al modelo de regresión logística entrenado que ha guardado en la lección anterior
- Predice la probabilidad de que el conductor reciba una propina

1. En Management Studio, abra una nueva ventana de consulta y ejecute el siguiente script de T-SQL para crear el procedimiento almacenado PredictTipBatchMode.
  
    ```sql
    USE [NYCTaxi_Sample]
    GO

    SET ANSI_NULLS ON
    GO
    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type = 'P' AND name = 'PredictTipBatchMode')
    DROP PROCEDURE v
    GO

    CREATE PROCEDURE [dbo].[PredictTipBatchMode] @input nvarchar(max)
    AS
    BEGIN
      DECLARE @lmodel2 varbinary(max) = (SELECT TOP 1 model  FROM nyc_taxi_models);
      EXEC sp_execute_external_script @language = N'R',
         @script = N'
           mod <- unserialize(as.raw(model));
           print(summary(mod))
           OutputDataSet<-rxPredict(modelObject = mod,
             data = InputDataSet,
             outData = NULL,
             predVarNames = "Score", type = "response",
             writeModelVars = FALSE, overwrite = TRUE);
           str(OutputDataSet)
           print(OutputDataSet)',
      @input_data_1 = @input,
      @params = N'@model varbinary(max)',
      @model = @lmodel2
      WITH RESULT SETS ((Score float));
    END
    ```

    + Use una instrucción SELECT para llamar al modelo almacenado desde una tabla SQL. El modelo se recupera de la tabla como datos **varbinary(max)** , se almacena en la variable SQL _\@lmodel2_ y se pasa como parámetro *mod* al procedimiento almacenado del sistema [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

    + Los datos que se usan como entradas de la puntuación se definen como una consulta SQL y se almacenan como una cadena en la variable SQL _\@input_. A medida que se recuperan datos de la base de datos, se van almacenando en una trama de datos llamada *InputDataSet*, que es sencillamente el nombre predeterminado de los datos de entrada en el procedimiento [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Si lo necesita, puede establecer otro nombre de variable con el parámetro _\@input_data_1_name_.

    + Para generar las puntuaciones, el procedimiento almacenado llama a la función rxPredict de la biblioteca **RevoScaleR**.

    + El valor devuelto, *Score*, es la probabilidad de que el conductor reciba una propina, según el modelo. De manera opcional, podría aplicar fácilmente algún tipo de filtro a los valores devueltos para clasificarlos en grupos tipo "propina" o "sin propina".  Por ejemplo, una probabilidad menor que 0,5 significaría que no es probable que reciba una propina.
  
2.  Para llamar al procedimiento almacenado en el modo por lotes, se define la consulta requerida como entrada del procedimiento almacenado. La siguiente es la consulta SQL que se puede ejecutar en SSMS para comprobar que esto funciona.

    ```sql
    SELECT TOP 10
      a.passenger_count AS passenger_count,
      a.trip_time_in_secs AS trip_time_in_secs,
      a.trip_distance AS trip_distance,
      a.dropoff_datetime AS dropoff_datetime,
      dbo.fnCalculateDistance( pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS direct_distance
      FROM 
        (SELECT medallion, hack_license, pickup_datetime, passenger_count,trip_time_in_secs,trip_distance, dropoff_datetime, pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude 
        FROM nyctaxi_sample)a 
      LEFT OUTER JOIN
      ( SELECT medallion, hack_license, pickup_datetime
      FROM nyctaxi_sample  tablesample (1 percent) repeatable (98052)  )b
      ON a.medallion=b.medallion
      AND a.hack_license=b.hack_license
      AND a.pickup_datetime=b.pickup_datetime
      WHERE b.medallion is null
    ```

3. Use este código de R para crear la cadena de entrada a partir de la consulta SQL:

    ```R
    input <- "N'SELECT TOP 10 a.passenger_count AS passenger_count, a.trip_time_in_secs AS trip_time_in_secs, a.trip_distance AS trip_distance, a.dropoff_datetime AS dropoff_datetime, dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS direct_distance FROM (SELECT medallion, hack_license, pickup_datetime, passenger_count,trip_time_in_secs,trip_distance, dropoff_datetime, pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude FROM nyctaxi_sample)a LEFT OUTER JOIN ( SELECT medallion, hack_license, pickup_datetime FROM nyctaxi_sample  tablesample (1 percent) repeatable (98052)  )b ON a.medallion=b.medallion AND a.hack_license=b.hack_license AND  a.pickup_datetime=b.pickup_datetime WHERE b.medallion is null'";
    q <- paste("EXEC PredictTipBatchMode @input = ", input, sep="");
    ```

4. Para ejecutar el procedimiento almacenado desde R, llame al método **sqlQuery** del paquete **RODBC** y use la conexión SQL `conn` de que definió anteriormente:

    ```R
    sqlQuery (conn, q);
    ```

    Si obtiene un error de ODBC, compruebe si hay errores de sintaxis y si el número de comillas es el adecuado. 
    
    Si obtiene un error de permisos, asegúrese de que el inicio de sesión tiene capacidad para ejecutar el procedimiento almacenado.

## <a name="single-row-scoring"></a>Puntuación de fila única

El modo de puntuación individual genera predicciones de una en una, pasando un conjunto de valores individuales al procedimiento almacenado como entrada. Los valores se corresponden con las características del modelo, que el modelo usa para crear una predicción, o para generar otro resultado, como un valor de probabilidad. Tras ello, puede devolver ese valor a la aplicación o al usuario.

Cuando se llama al modelo para la predicción por filas, se pasa un conjunto de valores que representan las características de cada caso individual. Después, el procedimiento almacenado devuelve una sola predicción o probabilidad. 

El procedimiento almacenado *PredictTipSingleMode* muestra este método. Toma como entrada varios parámetros que representan valores de características (por ejemplo, el número de pasajeros y la distancia del recorrido), puntúa estas características con el modelo de R almacenado y genera como salida la probabilidad de recibir propina.

1. Ejecute la siguiente instrucción de Transact-SQL para crear el procedimiento almacenado.

    ```sql
    USE [NYCTaxi_Sample]
    GO

    SET ANSI_NULLS ON
    GO
    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type = 'P' AND name = 'PredictTipSingleMode')
    DROP PROCEDURE v
    GO

    CREATE PROCEDURE [dbo].[PredictTipSingleMode] @passenger_count int = 0,
    @trip_distance float = 0,
    @trip_time_in_secs int = 0,
    @pickup_latitude float = 0,
    @pickup_longitude float = 0,
    @dropoff_latitude float = 0,
    @dropoff_longitude float = 0
    AS
    BEGIN
      DECLARE @inquery nvarchar(max) = N'
        SELECT * FROM [dbo].[fnEngineerFeatures](@passenger_count, @trip_distance, @trip_time_in_secs, @pickup_latitude, @pickup_longitude, @dropoff_latitude, @dropoff_longitude)'
      DECLARE @lmodel2 varbinary(max) = (SELECT TOP 1 model FROM nyc_taxi_models);

      EXEC sp_execute_external_script @language = N'R',  @script = N'
            mod <- unserialize(as.raw(model));
            print(summary(mod))
            OutputDataSet<-rxPredict(
              modelObject = mod,
              data = InputDataSet,
              outData = NULL,
              predVarNames = "Score",
              type = "response",
              writeModelVars = FALSE,
              overwrite = TRUE);
            str(OutputDataSet)
            print(OutputDataSet)
            ',
      @input_data_1 = @inquery,
      @params = N'
      -- passthrough columns
      @model varbinary(max) ,
      @passenger_count int ,
      @trip_distance float ,
      @trip_time_in_secs int ,
      @pickup_latitude float ,
      @pickup_longitude float ,
      @dropoff_latitude float ,
      @dropoff_longitude float',
      -- mapped variables
      @model = @lmodel2 ,
      @passenger_count =@passenger_count ,
      @trip_distance=@trip_distance ,
      @trip_time_in_secs=@trip_time_in_secs ,
      @pickup_latitude=@pickup_latitude ,
      @pickup_longitude=@pickup_longitude ,
      @dropoff_latitude=@dropoff_latitude ,
      @dropoff_longitude=@dropoff_longitude
      WITH RESULT SETS ((Score float));
    END
    ```

2. En SQL Server Management Studio, pude usar el procedimiento de [!INCLUDE[tsql](../../includes/tsql-md.md)] **EXEC** (o **EXECUTE**) para llamar al procedimiento almacenado y pasar las entradas necesarias. Pruebe a ejecutar, por ejemplo, esta instrucción en Management Studio:

    ```sql
    EXEC [dbo].[PredictTipSingleMode] 1, 2.5, 631, 40.763958,-73.973373, 40.782139,-73.977303
    ```

    Los valores pasados aquí se corresponden, respectivamente, con las variables _passenger\_count_, _trip_distance_, _trip\_time\_in\_secs_, _pickup\_latitude_, _pickup\_longitude_, _dropoff\_latitude_ y _dropoff\_longitude_.

3. Para ejecutar esta misma llamada desde código R, basta con definir una variable de R que contenga toda la llamada al procedimiento almacenado, como el siguiente:

    ```R
    q2 = "EXEC PredictTipSingleMode 1, 2.5, 631, 40.763958,-73.973373, 40.782139,-73.977303 ";
    ```

    Los valores pasados aquí se corresponden, respectivamente, con las variables _passenger\_count_, _trip\_distance_, _trip\_time\_in\_secs_, _pickup\_latitude_, _pickup\_longitude_, _dropoff\_latitude_ y _dropoff\_longitude_.

4. Llame a `sqlQuery` (desde el paquete **RODBC**) y pase la cadena de conexión, así como la variable de cadena que contiene la llamada al procedimiento almacenado.

    ```R
    # predict with stored procedure in single mode
    sqlQuery (conn, q2);
    ```

    >[!TIP]
    > Herramientas de R para Visual Studio (RTVS) se integra tremendamente bien con SQL Server y con R. Vea este artículo para obtener más ejemplos del uso de RODBC con una conexión de SQL Server: [Trabajar con SQL Server y R](/visualstudio/rtvs/sql-server).

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a trabajar con datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y a conservar modelos entrenados de R en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], debería serle relativamente fácil crear modelos basados en este conjunto de datos. Por ejemplo, podría intentar crear estos otros modelos:

+ Un modelo de regresión que predice la cantidad de propina
+ Un modelo de clasificación de varias clases que predice si la propina es pequeña, mediana o grande

Puede que también le interese explorar estos otros ejemplos y recursos:

+ [Escenarios de ciencia de datos y plantillas de soluciones](data-science-scenarios-and-solution-templates.md)
+ [Análisis avanzado en base de datos](r-taxi-classification-introduction.md)
+ [Guías de procedimientos de Machine Learning Server](/machine-learning-server/r/how-to-introduction)
+ [Recursos adicionales de Machine Learning Server](/machine-learning-server/resources-more)