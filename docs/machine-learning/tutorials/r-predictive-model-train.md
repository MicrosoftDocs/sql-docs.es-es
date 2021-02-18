---
title: 'Tutorial: Entrenamiento y comparación de modelos predictivos en R'
titleSuffix: SQL machine learning
description: En la parte tres de esta serie de tutoriales de cuatro partes, desarrollará dos modelos predictivos en R con aprendizaje automático de SQL y luego seleccionará el modelo más preciso.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 0ae6a293ab1e7e888a83ceb3216e8ef74aa9f95a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348077"
---
# <a name="tutorial-create-a-predictive-model-in-r-with-sql-machine-learning"></a>Tutorial: Creación de un modelo predictivo en R con el aprendizaje automático de SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
En la parte tres de esta serie de tutoriales de cuatro partes, entrenará un modelo predictivo en R. En la siguiente parte de esta serie, implementará este modelo en una base de datos de SQL Server con Machine Learning Services o en clústeres de macrodatos.
::: moniker-end
::: moniker range="=sql-server-2017"
En la parte tres de esta serie de tutoriales de cuatro partes, entrenará un modelo predictivo en R. En la siguiente parte de esta serie, implementará este modelo en una base de datos de SQL Server con Machine Learning Services.
::: moniker-end
::: moniker range="=sql-server-2016"
En la parte tres de esta serie de tutoriales de cuatro partes, entrenará un modelo predictivo en R. En la siguiente parte de esta serie, implementará este modelo en una base de datos con SQL Server R Services.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
En la parte tres de esta serie de tutoriales de cuatro partes, entrenará un modelo predictivo en R. En la siguiente parte de esta serie, implementará este modelo en una base de datos de Azure SQL Managed Instance con Machine Learning Services.
::: moniker-end

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Entrenar dos modelos de aprendizaje automático
> * Hacer predicciones a partir de ambos modelos
> * Comparar los resultados para elegir el modelo más preciso

En la [parte uno](r-predictive-model-introduction.md), ha aprendido a restaurar la base de datos de ejemplo.

En la [parte dos](r-predictive-model-prepare-data.md), ha obtenido información sobre cómo cargar los datos desde una base de datos en una trama de datos de Python y a preparar los datos en R.

En la [parte cuatro](r-predictive-model-deploy.md), aprenderá a almacenar el modelo en una base de datos y, luego, a crear procedimientos almacenados a partir de los scripts de Python desarrollados en las partes dos y tres. Los procedimientos almacenados se ejecutarán en el servidor para realizar predicciones basándose en datos nuevos.

## <a name="prerequisites"></a>Prerrequisitos

En la parte tres de esta serie de tutoriales, se da por hecho que ha completado los requisitos previos de la [**parte uno**](r-predictive-model-introduction.md) y que ha realizado los pasos de la [**parte dos**](r-predictive-model-prepare-data.md).

## <a name="train-two-models"></a>Entrenamiento de dos modelos

Para encontrar el mejor modelo para los datos de alquiler de esquís, cree dos modelos distintos (regresión lineal y árbol de decisión) y vea cuál es más preciso en sus predicciones. Deberá usar la trama de datos `rentaldata` que creó en la primera parte de esta serie.

```r
#First, split the dataset into two different sets:
# one for training the model and the other for validating it
train_data = rentaldata[rentaldata$Year < 2015,];
test_data  = rentaldata[rentaldata$Year == 2015,];


#Use the RentalCount column to check the quality of the prediction against actual values
actual_counts <- test_data$RentalCount;

#Model 1: Use lm to create a linear regression model, trained with the training data set
model_lm <- lm(RentalCount ~  Month + Day + WeekDay + Snow + Holiday, data = train_data);

#Model 2: Use rpart to create a decision tree model, trained with the training data set
library(rpart);
model_rpart  <- rpart(RentalCount ~ Month + Day + WeekDay + Snow + Holiday, data = train_data);
```

## <a name="make-predictions-from-both-models"></a>Hacer predicciones a partir de ambos modelos

Use una función de predicción para predecir el número de alquileres con cada modelo entrenado.

```r
#Use both models to make predictions using the test data set.
predict_lm <- predict(model_lm, test_data)
predict_lm <- data.frame(RentalCount_Pred = predict_lm, RentalCount = test_data$RentalCount, 
                         Year = test_data$Year, Month = test_data$Month,
                         Day = test_data$Day, Weekday = test_data$WeekDay,
                         Snow = test_data$Snow, Holiday = test_data$Holiday)

predict_rpart  <- predict(model_rpart,  test_data)
predict_rpart <- data.frame(RentalCount_Pred = predict_rpart, RentalCount = test_data$RentalCount, 
                         Year = test_data$Year, Month = test_data$Month,
                         Day = test_data$Day, Weekday = test_data$WeekDay,
                         Snow = test_data$Snow, Holiday = test_data$Holiday)

#To verify it worked, look at the top rows of the two prediction data sets.
head(predict_lm);
head(predict_rpart);
```

```results
    RentalCount_Pred  RentalCount  Month  Day  WeekDay  Snow  Holiday
1         27.45858          42       2     11     4      0       0
2        387.29344         360       3     29     1      0       0
3         16.37349          20       4     22     4      0       0
4         31.07058          42       3      6     6      0       0
5        463.97263         405       2     28     7      1       0
6        102.21695          38       1     12     2      1       0
    RentalCount_Pred  RentalCount  Month  Day  WeekDay  Snow  Holiday
1          40.0000          42       2     11     4      0       0
2         332.5714         360       3     29     1      0       0
3          27.7500          20       4     22     4      0       0
4          34.2500          42       3      6     6      0       0
5         645.7059         405       2     28     7      1       0
6          40.0000          38       1     12     2      1       0
```

## <a name="compare-the-results"></a>Comparación de los resultados

Ahora quiere ver cuál de los modelos ofrece las mejores predicciones. Una manera rápida y sencilla de hacerlo es usar una función de trazado básica para ver la diferencia entre los valores reales de los datos de entrenamiento y los valores previstos.

```r
#Use the plotting functionality in R to visualize the results from the predictions
par(mfrow = c(1, 1));
plot(predict_lm$RentalCount_Pred - predict_lm$RentalCount, main = "Difference between actual and predicted. lm")
plot(predict_rpart$RentalCount_Pred  - predict_rpart$RentalCount,  main = "Difference between actual and predicted. rpart")
```

![Comparación de los dos modelos](./media/compare-models.png)

Parece que el modelo de árbol de decisión es el más preciso de los dos modelos.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si no quiere continuar con este tutorial, elimine la base de datos TutorialDB.

## <a name="next-steps"></a>Pasos siguientes

En la tercera parte de la serie de tutoriales, ha aprendido a:

* Entrenar dos modelos de aprendizaje automático
* Hacer predicciones a partir de ambos modelos
* Comparar los resultados para elegir el modelo más preciso

Para implementar el modelo de aprendizaje automático que ha creado, siga la parte cuatro de esta serie de tutoriales:

> [!div class="nextstepaction"]
> [Implementación de un modelo predictivo en R con el aprendizaje automático de SQL](r-predictive-model-deploy.md)
