---
title: 'Tutorial de Python: Implementación de un modelo'
titleSuffix: SQL machine learning
description: En la parte cuatro de esta serie de tutoriales de cuatro partes, implementará un modelo de Python para predecir los alquileres de esquíes en una base de datos con aprendizaje automático de SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 08/06/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 4df492591109bef94802e1bf09a162eda09563c7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351501"
---
# <a name="python-tutorial-deploy-a-linear-regression-model-with-sql-machine-learning"></a>Tutorial de Python: Implementación de un modelo de regresión lineal con aprendizaje automático de SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
En la parte cuatro de esta serie de tutoriales de cuatro partes, implementará un modelo de regresión lineal desarrollado en Python en una base de datos de SQL Server mediante Machine Learning Services o clústeres de macrodatos.
::: moniker-end
::: moniker range="=sql-server-2017"
En la parte cuatro de esta serie de tutoriales de cuatro partes, implementará un modelo de regresión lineal desarrollado en Python en una base de datos de SQL Server mediante Machine Learning Services.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
En la parte cuatro de esta serie de tutoriales de cuatro partes, implementará un modelo de regresión lineal desarrollado en Python en una base de datos de Azure SQL Managed Instance mediante Machine Learning Services.
::: moniker-end

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Crear un procedimiento almacenado que genere el modelo de aprendizaje automático
> * Almacenar el modelo en una tabla de base de datos
> * Crear un procedimiento almacenado que realice predicciones mediante el modelo
> * Ejecutar el modelo con datos nuevos

En la [parte uno](python-ski-rental-linear-regression.md), ha aprendido a restaurar la base de datos de ejemplo.

En la [parte dos](python-ski-rental-linear-regression-prepare-data.md), ha aprendido a cargar los datos desde una base de datos en una trama de datos de Python y a preparar los datos en Python.

En la [parte tres](python-ski-rental-linear-regression-train-model.md), ha aprendido a entrenar un modelo de aprendizaje automático de regresión lineal en Python.

## <a name="prerequisites"></a>Prerrequisitos

* En la parte cuatro de este tutorial, se da por hecho que ha completado la [parte uno](python-ski-rental-linear-regression.md) y los requisitos previos.

## <a name="create-a-stored-procedure-that-generates-the-model"></a>Crear un procedimiento almacenado que genere el modelo

Ahora, use los scripts de Python que ha desarrollado para crear un procedimiento almacenado llamado **generate_rental_py_model** que entrene y genere el modelo de regresión lineal mediante LinearRegression de scikit-learn.

Ejecute la siguiente instrucción T-SQL en Azure Data Studio para crear el procedimiento almacenado con el fin de entrenar el modelo.

```sql
-- Stored procedure that trains and generates a Python model using the rental_data and a linear regression algorithm
DROP PROCEDURE IF EXISTS generate_rental_py_model;
go
CREATE PROCEDURE generate_rental_py_model (@trained_model varbinary(max) OUTPUT)
AS
BEGIN
    EXECUTE sp_execute_external_script
      @language = N'Python'
    , @script = N'
from sklearn.linear_model import LinearRegression
import pickle

df = rental_train_data

# Get all the columns from the dataframe.
columns = df.columns.tolist()

# Store the variable well be predicting on.
target = "RentalCount"

# Initialize the model class.
lin_model = LinearRegression()

# Fit the model to the training data.
lin_model.fit(df[columns], df[target])

# Before saving the model to the DB table, convert it to a binary object
trained_model = pickle.dumps(lin_model)'

, @input_data_1 = N'select "RentalCount", "Year", "Month", "Day", "WeekDay", "Snow", "Holiday" from dbo.rental_data where Year < 2015'
, @input_data_1_name = N'rental_train_data'
, @params = N'@trained_model varbinary(max) OUTPUT'
, @trained_model = @trained_model OUTPUT;
END;
GO
```

## <a name="store-the-model-in-a-database-table"></a>Almacenar el modelo en una tabla de base de datos

Cree una tabla en la base de datos TutorialDB y, después, guarde el modelo en la tabla.

1. Ejecute la siguiente instrucción T-SQL en Azure Data Studio para crear una tabla denominada **dbo.rental_py_models**, que se usará para almacenar el modelo.

   ```sql
   USE TutorialDB;
   DROP TABLE IF EXISTS dbo.rental_py_models;
   GO
   CREATE TABLE dbo.rental_py_models (
       model_name VARCHAR(30) NOT NULL DEFAULT('default model') PRIMARY KEY,
       model VARBINARY(MAX) NOT NULL
   );
   GO
   ```

1. Guarde el modelo en la tabla como un objeto binario, con el nombre de modelo **linear_model**.

   ```sql
   DECLARE @model VARBINARY(MAX);
   EXECUTE generate_rental_py_model @model OUTPUT;
   
   INSERT INTO rental_py_models (model_name, model) VALUES('linear_model', @model);
   ```

## <a name="create-a-stored-procedure-that-makes-predictions"></a>Creación de un procedimiento almacenado que realiza predicciones

1. Cree un procedimiento almacenado llamado **py_predict_rentalcount** que realice predicciones mediante el modelo entrenado y un conjunto de datos nuevos. Ejecute la consulta T-SQL siguiente en Azure Data Studio.

   ```sql
   DROP PROCEDURE IF EXISTS py_predict_rentalcount;
   GO
   CREATE PROCEDURE py_predict_rentalcount (@model varchar(100))
   AS
   BEGIN
       DECLARE @py_model varbinary(max) = (select model from rental_py_models where model_name = @model);
   
       EXECUTE sp_execute_external_script
                   @language = N'Python',
                   @script = N'
   
   # Import the scikit-learn function to compute error.
   from sklearn.metrics import mean_squared_error
   import pickle
   import pandas
   
   rental_model = pickle.loads(py_model)
   
   df = rental_score_data
   
   # Get all the columns from the dataframe.
   columns = df.columns.tolist()
   
   # Variable you will be predicting on.
   target = "RentalCount"
   
   # Generate the predictions for the test set.
   lin_predictions = rental_model.predict(df[columns])
   print(lin_predictions)
   
   # Compute error between the test predictions and the actual values.
   lin_mse = mean_squared_error(lin_predictions, df[target])
   #print(lin_mse)
   
   predictions_df = pandas.DataFrame(lin_predictions)
   
   OutputDataSet = pandas.concat([predictions_df, df["RentalCount"], df["Month"], df["Day"], df["WeekDay"], df["Snow"], df["Holiday"], df["Year"]], axis=1)
   '
   , @input_data_1 = N'Select "RentalCount", "Year" ,"Month", "Day", "WeekDay", "Snow", "Holiday"  from rental_data where Year = 2015'
   , @input_data_1_name = N'rental_score_data'
   , @params = N'@py_model varbinary(max)'
   , @py_model = @py_model
   with result sets (("RentalCount_Predicted" float, "RentalCount" float, "Month" float,"Day" float,"WeekDay" float,"Snow" float,"Holiday" float, "Year" float));
   
   END;
   GO
    ```

1. Cree una tabla para almacenar las predicciones.

   ```sql
   DROP TABLE IF EXISTS [dbo].[py_rental_predictions];
   GO
   
   CREATE TABLE [dbo].[py_rental_predictions](
    [RentalCount_Predicted] [int] NULL,
    [RentalCount_Actual] [int] NULL,
    [Month] [int] NULL,
    [Day] [int] NULL,
    [WeekDay] [int] NULL,
    [Snow] [int] NULL,
    [Holiday] [int] NULL,
    [Year] [int] NULL
   ) ON [PRIMARY]
   GO
   ```

1. Ejecución del procedimiento almacenado para predecir el número de alquileres

   ```sql
   --Insert the results of the predictions for test set into a table
   INSERT INTO py_rental_predictions
   EXEC py_predict_rentalcount 'linear_model';
   
   -- Select contents of the table
   SELECT * FROM py_rental_predictions;
   ```

   Se mostrarán resultados similares a los siguientes.

   :::image type="content" source="media/python-tutorial-prediction-results.png" alt-text="Resultados de predicción del procedimiento almacenado":::

Ha creado, entrenado e implementado correctamente un modelo. Después, ha usado el modelo en un procedimiento almacenado para predecir valores basándose en datos nuevos.

## <a name="next-steps"></a>Pasos siguientes

En la parte cuatro de esta serie de tutoriales, ha aprendido a:

* Crear un procedimiento almacenado que genere el modelo de aprendizaje automático
* Almacenar el modelo en una tabla de base de datos
* Crear un procedimiento almacenado que realice predicciones mediante el modelo
* Ejecutar el modelo con datos nuevos

Para más información sobre cómo usar Python en el aprendizaje automático de SQL, consulte:

+ [Tutoriales de Python](python-tutorials.md)
