---
title: 'Tutorial de Python: Entrenamiento de un modelo'
titleSuffix: SQL machine learning
description: En la tercera parte de esta serie de tutoriales de cuatro partes, entrenará un modelo de regresión lineal en Python para predecir los alquileres de esquíes con aprendizaje automático de SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/21/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: cb31c5486668daf64b86df50ecb493a778a9b61a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351467"
---
# <a name="python-tutorial-train-a-linear-regression-model-with-sql-machine-learning"></a>Tutorial de Python: Entrenamiento de un modelo de regresión lineal con aprendizaje automático de SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
En la parte tres de esta serie de tutoriales de cuatro partes, entrenará un modelo de regresión lineal en Python. En la siguiente parte de esta serie, implementará este modelo en una base de datos de SQL Server con Machine Learning Services o en clústeres de macrodatos.
::: moniker-end
::: moniker range="=sql-server-2017"
En la parte tres de esta serie de tutoriales de cuatro partes, entrenará un modelo de regresión lineal en Python. En la siguiente parte de esta serie, implementará el modelo en una base de datos de SQL Server con Machine Learning Services.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
En la parte tres de esta serie de tutoriales de cuatro partes, entrenará un modelo de regresión lineal en Python. En la siguiente parte de esta serie, implementará este modelo en una base de datos de Azure SQL Managed Instance con Machine Learning Services.
::: moniker-end

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Entrenamiento de un modelo de regresión lineal
> * Predicciones mediante el modelo de regresión lineal

En la [parte uno](python-ski-rental-linear-regression.md), ha aprendido a restaurar la base de datos de ejemplo.

En la [parte dos](python-ski-rental-linear-regression-prepare-data.md), ha aprendido a cargar los datos desde una base de datos en una trama de datos de Python y a preparar los datos en Python.

En la [parte cuatro](python-ski-rental-linear-regression-deploy-model.md), aprenderá a almacenar el modelo en una base de datos y, luego, a crear procedimientos almacenados a partir de los scripts de Python desarrollados en las partes dos y tres. Los procedimientos almacenados se ejecutarán en el servidor para realizar predicciones basándose en datos nuevos.

## <a name="prerequisites"></a>Prerrequisitos

* En la parte tres de este tutorial, se da por hecho que ha completado la [parte uno](python-ski-rental-linear-regression.md) y los requisitos previos.

## <a name="train-the-model"></a>Entrenamiento del modelo

Para realizar predicciones, necesita identificar una función (modelo) que describa mejor la dependencia entre las variables del conjunto de datos. Este proceso se denomina entrenar el modelo. El conjunto de datos de entrenamiento será un subconjunto de todo el conjunto de datos de la trama de datos de pandas **df** que ha creado en la parte dos de esta serie.

Entrenará el modelo **lin_model** mediante un algoritmo de regresión lineal.

```python
# Store the variable we'll be predicting on.
target = "Rentalcount"

# Generate the training set.  Set random_state to be able to replicate results.
train = df.sample(frac=0.8, random_state=1)

# Select anything not in the training set and put it in the testing set.
test = df.loc[~df.index.isin(train.index)]

# Print the shapes of both sets.
print("Training set shape:", train.shape)
print("Testing set shape:", test.shape)

# Initialize the model class.
lin_model = LinearRegression()

# Fit the model to the training data.
lin_model.fit(train[columns], train[target])
```

Se mostrarán resultados similares a los siguientes.

```results
Training set shape: (362, 7)
Testing set shape: (91, 7)
```

## <a name="make-predictions"></a>Realización de predicciones

Use una función de predicción para predecir el número de alquileres mediante el modelo **lin_model**.

```python
# Generate our predictions for the test set.
lin_predictions = lin_model.predict(test[columns])
print("Predictions:", lin_predictions)
# Compute error between our test predictions and the actual values.
lin_mse = mean_squared_error(lin_predictions, test[target])
print("Computed error:", lin_mse)
```

Se mostrarán resultados similares a los siguientes.

```results
Predictions: [ 40.  38. 240.  39. 514.  48. 297.  25. 507.  24.  30.  54.  40.  26.
  30.  34.  42. 390. 336.  37.  22.  35.  55. 350. 252. 370. 499.  48.
  37. 494.  46.  25. 312. 390.  35.  35. 421.  39. 176.  21.  33. 452.
  34.  28.  37. 260.  49. 577. 312.  24.  24. 390.  34.  64.  26.  32.
  33. 358. 348.  25.  35.  48.  39.  44.  58.  24. 350. 651.  38. 468.
  26.  42. 310. 709. 155.  26. 648. 617.  26. 846. 729.  44. 432.  25.
  39.  28. 325.  46.  36.  50.  63.]
Computed error: 2.9960763804270902e-27
```

## <a name="next-steps"></a>Pasos siguientes

En la parte tres de esta serie de tutoriales, ha completado estos pasos:

* Entrenamiento de un modelo de regresión lineal
* Predicciones mediante el modelo de regresión lineal

Para implementar el modelo de aprendizaje automático que ha creado, siga la parte cuatro de esta serie de tutoriales:

> [!div class="nextstepaction"]
> [Tutorial de Python: Implementación de un modelo de aprendizaje automático](python-ski-rental-linear-regression-deploy-model.md)
