---
title: 'Tutorial: Implementación de un modelo predictivo en R'
titleSuffix: SQL machine learning
description: En la cuarta parte de este tutorial de cuatro partes, implementará un modelo predictivo en R con el aprendizaje automático de SQL.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: e0f264f0996a3597085c8e5103eed9ebc0ee2c27
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348051"
---
# <a name="tutorial-deploy-a-predictive-model-in-r-with-sql-machine-learning"></a>Tutorial: Implementación de un modelo predictivo en R con el aprendizaje automático de SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
En la parte cuatro de esta serie de tutoriales de cuatro partes, implementará un modelo de aprendizaje automático desarrollado en R en SQL Server Machine Learning Services o en Clústeres de macrodatos.
::: moniker-end
::: moniker range="=sql-server-2017"
En la parte cuatro de esta serie de tutoriales de cuatro partes, implementará un modelo de aprendizaje automático desarrollado en R en SQL Server con Machine Learning Services.
::: moniker-end
::: moniker range="=sql-server-2016"
En la parte cuatro de esta serie de tutoriales de cuatro partes, implementará un modelo de Machine Learning desarrollado en R en SQL Server con SQL Server R Services.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
En la parte cuatro de esta serie de tutoriales de cuatro partes, implementará un modelo de Machine Learning desarrollado en R en Azure SQL Managed Instance con Machine Learning Services.
::: moniker-end

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Crear un procedimiento almacenado que genere el modelo de aprendizaje automático
> * Almacenar el modelo en una tabla de base de datos
> * Crear un procedimiento almacenado que realice predicciones mediante el modelo
> * Ejecutar el modelo con datos nuevos

En la [parte uno](r-predictive-model-introduction.md), ha aprendido a restaurar la base de datos de ejemplo.

En la [parte dos](r-predictive-model-prepare-data.md), aprendió a importar una base de datos de ejemplo y, luego, a preparar los datos para su uso en el entrenamiento de un modelo predictivo en R.

En la [parte tres](r-predictive-model-train.md), aprendió a crear y entrenar varios modelos de Machine Learning en R y, luego, elegir el más preciso.

## <a name="prerequisites"></a>Prerrequisitos

En la parte cuatro de este tutorial se da por hecho que ha completado los requisitos previos de la [**parte uno**](r-predictive-model-introduction.md) y que ha realizado los pasos de la [**parte dos**](r-predictive-model-prepare-data.md) y la [**parte tres**](r-predictive-model-train.md).

## <a name="create-a-stored-procedure-that-generates-the-model"></a>Crear un procedimiento almacenado que genere el modelo

En la parte tres de esta serie de tutoriales, decidió que un modelo de árbol de decisión (dtree) era el más preciso. Ahora, con los scripts de R desarrollados, cree un procedimiento almacenado (`generate_rental_model`) que entrene y genere el modelo dtree mediante rpart desde el paquete de R.

Ejecute los comandos siguientes en Azure Data Studio.

```sql
USE [TutorialDB]
DROP PROCEDURE IF EXISTS generate_rental_model;
GO
CREATE PROCEDURE generate_rental_model (@trained_model VARBINARY(max) OUTPUT)
AS
BEGIN
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
rental_train_data$Month   <- factor(rental_train_data$Month);
rental_train_data$Day     <- factor(rental_train_data$Day);
rental_train_data$Holiday <- factor(rental_train_data$Holiday);
rental_train_data$Snow    <- factor(rental_train_data$Snow);
rental_train_data$WeekDay <- factor(rental_train_data$WeekDay);

#Create a dtree model and train it using the training data set
library(rpart);
model_dtree <- rpart(RentalCount ~ Month + Day + WeekDay + Snow + Holiday, data = rental_train_data);
#Serialize the model before saving it to the database table
trained_model <- as.raw(serialize(model_dtree, connection=NULL));
'
        , @input_data_1 = N'
            SELECT RentalCount
                 , Year
                 , Month
                 , Day
                 , WeekDay
                 , Snow
                 , Holiday
            FROM dbo.rental_data
            WHERE Year < 2015
            '
        , @input_data_1_name = N'rental_train_data'
        , @params = N'@trained_model varbinary(max) OUTPUT'
        , @trained_model = @trained_model OUTPUT;
END;
GO
```

## <a name="store-the-model-in-a-database-table"></a>Almacenar el modelo en una tabla de base de datos

Cree una tabla en la base de datos TutorialDB y, después, guarde el modelo en la tabla.

1. Cree una tabla (`rental_models`) para almacenar el modelo.

    ```sql
    USE TutorialDB;
    DROP TABLE IF EXISTS rental_models;
    GO
    CREATE TABLE rental_models (
          model_name VARCHAR(30) NOT NULL DEFAULT('default model') PRIMARY KEY
        , model VARBINARY(MAX) NOT NULL
        );
    GO
    ```

1. Guarde el modelo en la tabla como un objeto binario, con el nombre "DTree".

    ```sql
    -- Save model to table
    TRUNCATE TABLE rental_models;
    
    DECLARE @model VARBINARY(MAX);
    
    EXECUTE generate_rental_model @model OUTPUT;
    
    INSERT INTO rental_models (
          model_name
        , model
        )
    VALUES (
         'DTree'
        , @model
        );
    
    SELECT *
    FROM rental_models;
    ```

## <a name="create-a-stored-procedure-that-makes-predictions"></a>Creación de un procedimiento almacenado que realiza predicciones

Cree un procedimiento almacenado (`predict_rentalcount_new`) que hace predicciones mediante el modelo entrenado y un conjunto de nuevos datos.

```sql
-- Stored procedure that takes model name and new data as input parameters and predicts the rental count for the new data
USE [TutorialDB]
DROP PROCEDURE IF EXISTS predict_rentalcount_new;
GO
CREATE PROCEDURE predict_rentalcount_new (
      @model_name VARCHAR(100)
    , @input_query NVARCHAR(MAX)
    )
AS
BEGIN
    DECLARE @model VARBINARY(MAX) = (
            SELECT model
            FROM rental_models
            WHERE model_name = @model_name
            );

    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
#Convert types to factors
rentals$Month   <- factor(rentals$Month);
rentals$Day     <- factor(rentals$Day);
rentals$Holiday <- factor(rentals$Holiday);
rentals$Snow    <- factor(rentals$Snow);
rentals$WeekDay <- factor(rentals$WeekDay);

#Before using the model to predict, we need to unserialize it
rental_model <- unserialize(model);

#Call prediction function
rental_predictions <- predict(rental_model, rentals);
rental_predictions <- data.frame(rental_predictions);
'
        , @input_data_1 = @input_query
        , @input_data_1_name = N'rentals'
        , @output_data_1_name = N'rental_predictions'
        , @params = N'@model varbinary(max)'
        , @model = @model
    WITH RESULT SETS(("RentalCount_Predicted" FLOAT));
END;
GO
```

## <a name="execute-the-model-with-new-data"></a>Ejecutar el modelo con datos nuevos

Ahora puede usar el procedimiento almacenado `predict_rentalcount_new` para predecir el número de alquileres a partir de los nuevos datos.

```sql
-- Use the predict_rentalcount_new stored procedure with the model name and a set of features to predict the rental count
EXECUTE dbo.predict_rentalcount_new @model_name = 'DTree'
    , @input_query = '
        SELECT CONVERT(INT,  3) AS Month
             , CONVERT(INT, 24) AS Day
             , CONVERT(INT,  4) AS WeekDay
             , CONVERT(INT,  1) AS Snow
             , CONVERT(INT,  1) AS Holiday
        ';
GO
```

Verá un resultado similar al siguiente:

```results
RentalCount_Predicted
332.571428571429
```

Ha creado, entrenado e implementado correctamente un modelo en una base de datos. Después, ha usado el modelo en un procedimiento almacenado para predecir valores basándose en datos nuevos.


## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando termine de usar la base de datos TutorialDB, elimínela del servidor.

## <a name="next-steps"></a>Pasos siguientes

En la cuarta parte de la serie de tutoriales, ha aprendido a:

* Crear un procedimiento almacenado que genere el modelo de aprendizaje automático
* Almacenar el modelo en una tabla de base de datos
* Crear un procedimiento almacenado que realice predicciones mediante el modelo
* Ejecutar el modelo con datos nuevos

Para más información sobre cómo usar R en Machine Learning Services, vea:

* [Ejecución de scripts de R simples](quickstart-r-create-script.md)
* [Estructuras de datos, tipos y objetos de R](quickstart-r-data-types-and-objects.md)
* [Funciones de R](quickstart-r-functions.md)
