---
title: 'Tutorial: Preparación de los datos para entrenar un modelo predictivo en R'
titleSuffix: SQL machine learning
description: En la segunda parte de esta serie de tutoriales de cuatro partes, preparará los datos para entrenar un modelo predictivo en R con el aprendizaje automático de SQL.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: e1929f0db9fb1d70fc8c370297d2d6f1c4e74513
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348078"
---
# <a name="tutorial-prepare-data-to-train-a-predictive-model-in-r-with-sql-machine-learning"></a>Tutorial: Preparación de los datos para entrenar un modelo predictivo en R con el aprendizaje automático de SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
En la segunda parte de esta serie de tutoriales de cuatro partes, preparará los datos de una base de datos mediante R. Más adelante en la serie, usará estos datos para entrenar e implementar un modelo predictivo en R con SQL Server Machine Learning Services o con clústeres de macrodatos.
::: moniker-end
::: moniker range="=sql-server-2017"
En la parte uno de esta serie de tutoriales de tres partes, importará y preparará los datos de una base de datos de Azure SQL mediante R. Más adelante en la serie, usará estos datos para entrenar e implementar un modelo de Machine Learning predictivo en R con SQL Server Machine Learning Services.
::: moniker-end
::: moniker range="=sql-server-2016"
En la parte dos de esta serie de tutoriales de cuatro partes, preparará los datos de una base de datos mediante R. Más adelante en la serie, usará estos datos para entrenar e implementar un modelo predictivo en R con SQL Server R Services.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
En la parte dos de esta serie de tutoriales de cuatro partes, preparará los datos de una base de datos mediante R. Más adelante en la serie, usará estos datos para entrenar e implementar un modelo predictivo en R con Machine Learning Services en Azure SQL Managed Instance.
::: moniker-end

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Restaurar una base de datos de ejemplo en una base de datos
> * Cargar los datos de la base de datos en una trama de datos de R
> * Preparar los datos en R mediante la identificación de algunas columnas como de categoría

En la [parte uno](r-predictive-model-introduction.md), ha aprendido a restaurar la base de datos de ejemplo.

En la [parte tres](r-predictive-model-train.md), aprenderá a entrenar un modelo de Machine Learning en R.

En la [parte cuatro](r-predictive-model-deploy.md), aprenderá a almacenar el modelo en una base de datos y, luego, a crear procedimientos almacenados a partir de los scripts de R desarrollados en las partes dos y tres. Los procedimientos almacenados se ejecutarán en el servidor para realizar predicciones basándose en datos nuevos.

## <a name="prerequisites"></a>Prerrequisitos

En la segunda parte de este tutorial se da por hecho que ha realizado la [**primera parte**](r-predictive-model-introduction.md) y que satisface sus requisitos previos.

## <a name="load-the-data-into-a-data-frame"></a>Carga de los datos en una trama de datos

Para usar los datos en R, deberá cargarlos desde la base de datos hasta una trama de datos (`rentaldata`).

Cree un archivo de RScript en RStudio y ejecute el siguiente script. Reemplace **ServerName** por su propia información de conexión.

```r
#Define the connection string to connect to the TutorialDB database
connStr <- "Driver=SQL Server;Server=ServerName;Database=TutorialDB;uid=Username;pwd=Password"


#Get the data from the table
library(RODBC)

ch <- odbcDriverConnect(connStr)

#Import the data from the table
rentaldata <- sqlFetch(ch, "dbo.rental_data")

#Take a look at the structure of the data and the top rows
head(rentaldata)
str(rentaldata)
```

Se mostrarán resultados similares a los siguientes.

```results
   Year  Month  Day  RentalCount  WeekDay  Holiday  Snow
1  2014    1     20      445         2        1      0
2  2014    2     13       40         5        0      0
3  2013    3     10      456         1        0      0
4  2014    3     31       38         2        0      0
5  2014    4     24       23         5        0      0
6  2015    2     11       42         4        0      0
'data.frame':       453 obs. of  7 variables:
$ Year       : int  2014 2014 2013 2014 2014 2015 2013 2014 2013 2015 ...
$ Month      : num  1 2 3 3 4 2 4 3 4 3 ...
$ Day        : num  20 13 10 31 24 11 28 8 5 29 ...
$ RentalCount: num  445 40 456 38 23 42 310 240 22 360 ...
$ WeekDay    : num  2 5 1 2 5 4 1 7 6 1 ...
$ Holiday    : int  1 0 0 0 0 0 0 0 0 0 ...
$ Snow       : num  0 0 0 0 0 0 0 0 0 0 ...
```

## <a name="prepare-the-data"></a>Preparación de los datos

En esta base de datos de ejemplo, ya se ha realizado la mayor parte de la preparación, pero aún tiene que hacer una cosa más.
Use el siguiente script de R para identificar tres columnas como *categorías*, para lo cual debe cambiar los tipos de datos a *factor*.



```r
#Changing the three factor columns to factor types
rentaldata$Holiday <- factor(rentaldata$Holiday);
rentaldata$Snow    <- factor(rentaldata$Snow);
rentaldata$WeekDay <- factor(rentaldata$WeekDay);



#Visualize the dataset after the change
str(rentaldata);
```

Se mostrarán resultados similares a los siguientes.

```results
data.frame':      453 obs. of  7 variables:
$ Year       : int  2014 2014 2013 2014 2014 2015 2013 2014 2013 2015 ...
$ Month      : num  1 2 3 3 4 2 4 3 4 3 ...
$ Day        : num  20 13 10 31 24 11 28 8 5 29 ...
$ RentalCount: num  445 40 456 38 23 42 310 240 22 360 ...
$ WeekDay    : Factor w/ 7 levels "1","2","3","4",..: 2 5 1 2 5 4 1 7 6 1 ...
$ Holiday    : Factor w/ 2 levels "0","1": 2 1 1 1 1 1 1 1 1 1 ...
$ Snow       : Factor w/ 2 levels "0","1": 1 1 1 1 1 1 1 1 1 1 ...
```

Los datos están ya preparados para el entrenamiento.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si no quiere continuar con este tutorial, elimine la base de datos TutorialDB.

## <a name="next-steps"></a>Pasos siguientes

En la parte dos de la serie de tutoriales, ha aprendido a:

* Carga de los datos de ejemplo en una trama de datos de R
* Preparar los datos en R mediante la identificación de algunas columnas como de categoría

Para crear un modelo de aprendizaje automático que use datos de la base de datos TutorialDB, siga la parte tres de esta serie de tutoriales:

> [!div class="nextstepaction"]
> [Creación de un modelo predictivo en R con el aprendizaje automático de SQL](r-predictive-model-train.md)
