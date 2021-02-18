---
title: 'Tutorial de R: Entrenamiento y guardado del modelo'
titleSuffix: SQL machine learning
description: En la parte cuatro de esta serie de tutoriales de cinco partes, podrá entrenar y guardar un modelo en R con Transact-SQL en SQL Server con el aprendizaje automático de SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 10/15/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 3b5606b9f14006632bfa2782d1d547b4565e36ab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350930"
---
# <a name="r-tutorial-train-and-save-model"></a>Tutorial de R: Entrenamiento y guardado del modelo
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

En la parte cuatro de esta serie de tutoriales de cinco partes, aprenderá a entrenar un modelo de Machine Learning mediante R. Deberá entrenar el modelo con las características de datos que creó en la parte anterior y después guardar el modelo entrenado en una tabla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En este caso, los paquetes de R ya se han instalado con [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)], por lo que todo se puede hacer desde SQL.

En este artículo, hará lo siguiente:

> [!div class="checklist"]
> + Crear y entrenar un modelo mediante un procedimiento almacenado de SQL
> + Guardar el modelo entrenado en una tabla SQL

En la [parte uno](r-taxi-classification-introduction.md), ha instalado los requisitos previos y ha restaurado la base de datos de ejemplo.

En la [parte dos](r-taxi-classification-explore-data.md), revisó los datos de ejemplo y generó algunos trazados.

En la [tres](r-taxi-classification-create-features.md), aprendió a crear características a partir de datos sin procesar mediante una función de Transact-SQL. Después, llamó a esa función desde un procedimiento almacenado para crear una tabla que contiene los valores de las características.

En la [parte cinco](r-taxi-classification-deploy-model.md), aprenderá a poner en marcha los modelos entrenados y guardados en la parte cuatro.

## <a name="create-the-stored-procedure"></a>Creación del procedimiento almacenado

Al llamar a R desde T-SQL, utilice el procedimiento almacenado del sistema [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Sin embargo, para procesos que repite a menudo, como volver a entrenar un modelo, es más fácil encapsular la llamada a `sp_execute_external_script` en otro procedimiento almacenado.

1. En [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], abra una nueva ventana de **consulta**.

2. Ejecute la siguiente instrucción para crear el procedimiento almacenado **RTrainLogitModel**. Este procedimiento almacenado define los datos de entrada y usa **glm** para crear un modelo de regresión logística.

   ```sql
   CREATE PROCEDURE [dbo].[RTrainLogitModel] (@trained_model varbinary(max) OUTPUT)
   
   AS
   BEGIN
     DECLARE @inquery nvarchar(max) = N'
       select tipped, fare_amount, passenger_count,trip_time_in_secs,trip_distance,
       pickup_datetime, dropoff_datetime,
       dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as direct_distance
       from nyctaxi_sample
       tablesample (70 percent) repeatable (98052)
   '
   
     EXEC sp_execute_external_script @language = N'R',
                                     @script = N'
   ## Create model
   logitObj <- glm(tipped ~ passenger_count + trip_distance + trip_time_in_secs + direct_distance, data = InputDataSet, family = binomial)
   summary(logitObj)
   
   ## Serialize model 
   trained_model <- as.raw(serialize(logitObj, NULL));
   ',
     @input_data_1 = @inquery,
     @params = N'@trained_model varbinary(max) OUTPUT',
     @trained_model = @trained_model OUTPUT; 
   END
   GO
   ```

   + Para asegurarse de que se dejan algunos datos para probar el modelo, el 70 % de los datos se seleccionan aleatoriamente de la tabla de datos de taxi con fines de entrenamiento.

   + La consulta SELECT usa la función escalar personalizada *fnCalculateDistance* para calcular la distancia directa entre las ubicaciones de origen y destino. Los resultados de la consulta se almacenan en la variable de entrada predeterminada de R `InputDataset`.
  
   + El script de R llama a la función **glm** de R para crear el modelo de regresión logística.
  
     La variable binaria _tipped_ se usa como la *etiqueta* o columna del resultado y el modelo se ajusta mediante estas columnas específicas:  _passenger_count_, _trip_distance_, _trip_time_in_secs_ y _direct_distance_.
  
   + El modelo entrenado, guardado en la variable de R `logitObj`, se serializa y se devuelve como un parámetro de salida.

## <a name="train-and-deploy-the-r-model-using-the-stored-procedure"></a>Entreno e implementación del modelo de R usando el procedimiento almacenado

Como el procedimiento almacenado ya incluye una definición de los datos de entrada, no es necesario proporcionar una consulta de entrada.

1. Para entrenar e implementar el modelo de R, llame al procedimiento almacenado e insértelo en la tabla de base de datos _nyc_taxi_models_ para poder usarlo en predicciones futuras:

   ```sql
   DECLARE @model VARBINARY(MAX);
   EXEC RTrainLogitModel @model OUTPUT;
   INSERT INTO nyc_taxi_models (name, model) VALUES('RTrainLogit_model', @model);
   ```

2. Vea en la ventana **Mensajes** de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] los mensajes que se canalizan al flujo **stdout** de R: 

   Mensaje(s) STDOUT del script externo: Filas leídas: 1193025, Total de filas procesadas: 1193025, Tiempo total de fragmentos: 0,093 segundos

3. Una vez completada la instrucción, abra la tabla *nyc_taxi_models*. Es posible que el procesamiento de los datos y el ajuste del modelo tarden un rato.

   Puede ver que se ha agregado una fila nueva, que contiene el modelo serializado en la columna _model_ y el nombre del modelo **RxTrainLogit_model** en la columna _name_.

   ```text
   model                        name
   ---------------------------- ------------------
   0x580A00000002000302020....  RTrainLogit_model
   ```

En la siguiente parte de este tutorial, usará el modelo entrenado para generar predicciones.

## <a name="next-steps"></a>Pasos siguientes

En este artículo:

> [!div class="checklist"]
> + Creó y entrenó un modelo mediante un procedimiento almacenado de SQL
> + Guardó el modelo entrenado en una tabla SQL

> [!div class="nextstepaction"]
> [Tutorial de R: Ejecución de predicciones en procedimientos almacenados de SQL](r-taxi-classification-deploy-model.md)
