---
title: 'Tutorial: Compilación de un modelo de agrupación en clústeres en R'
titleSuffix: SQL machine learning
description: En la parte tres de esta serie de tutoriales de cuatro partes, creará un modelo de k-means para realizar la agrupación en clústeres en R con aprendizaje automático de SQL.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: e8c4f3d7e3c2fa133fc077a7549a9d6fb2daf9ea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339607"
---
# <a name="tutorial-build-a-clustering-model-in-r-with-sql-machine-learning"></a>Tutorial: Creación de un modelo de agrupación en clústeres en R con el aprendizaje automático de SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
En la parte tres de esta serie de tutoriales de cuatro partes, creará un modelo de k-means en R para realizar la agrupación en clústeres. En la siguiente parte de esta serie, implementará este modelo en una base de datos con SQL Server Machine Learning Services o en clústeres de macrodatos.
::: moniker-end
::: moniker range="=sql-server-2017"
En la parte tres de esta serie de tutoriales de cuatro partes, creará un modelo de k-means en R para realizar la agrupación en clústeres. En la siguiente parte de esta serie, implementará este modelo en una base de datos con SQL Server Machine Learning Services.
::: moniker-end
::: moniker range="=sql-server-2016"
En la parte tres de esta serie de tutoriales de cuatro partes, creará un modelo de k-means en R para realizar la agrupación en clústeres. En la siguiente parte de esta serie, implementará este modelo en una base de datos SQL con SQL Server R Services.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
En la parte tres de esta serie de tutoriales de cuatro partes, creará un modelo de k-means en R para realizar la agrupación en clústeres. En la siguiente parte de esta serie, implementará este modelo en una base de datos con Machine Learning Services en Azure SQL Managed Instance.
::: moniker-end

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Definición del número de clústeres para un algoritmo de k-means
> * Agrupación en clústeres
> * Análisis de los resultados

En la [parte uno](r-clustering-model-introduction.md), ha instalado los requisitos previos y ha restaurado la base de datos de ejemplo.

En la [parte dos](r-clustering-model-prepare-data.md), ha aprendido a preparar los datos de una base de datos para realizar la agrupación en clústeres.

En la [parte cuatro](r-clustering-model-deploy.md), aprenderá a crear un procedimiento almacenado en una base de datos que pueda realizar la agrupación en clústeres en R basándose en datos nuevos.

## <a name="prerequisites"></a>Prerrequisitos

* En la parte tres de esta serie de tutoriales, se da por hecho que ha completado los requisitos previos de la [**parte uno**](r-clustering-model-introduction.md) y que ha realizado los pasos de la [**parte dos**](r-clustering-model-prepare-data.md).

## <a name="define-the-number-of-clusters"></a>Definición del número de clústeres

Para agrupar en clústeres los datos de clientes, usará el algoritmo de agrupación en clústeres **k-means**, una de las formas más sencillas y conocidas de agrupar datos.
Para más información sobre k-means, vea [Guía completa sobre el algoritmo de agrupación en clústeres k-means](https://www.kdnuggets.com/2019/05/guide-k-means-clustering-algorithm.html).

El algoritmo acepta dos entradas: los datos en sí y un número predefinido "*k*", que representa el número de clústeres que se generarán.
El resultado es *k* clústeres con los datos de entrada repartidos entre los clústeres.

Para determinar el número de clústeres que usará el algoritmo, use una representación de la suma de cuadrados dentro de los grupos por el número de clústeres extraídos. El número adecuado de clústeres que se usará se encuentra en la curva o "codo" de la representación.

```r
# Determine number of clusters by using a plot of the within groups sum of squares,
# by number of clusters extracted. 
wss <- (nrow(customer_data) - 1) * sum(apply(customer_data, 2, var))
for (i in 2:20)
    wss[i] <- sum(kmeans(customer_data, centers = i)$withinss)
plot(1:20, wss, type = "b", xlab = "Number of Clusters", ylab = "Within groups sum of squares")
```

![Gráfico de codo](./media/elbow-graph.png)

Según el gráfico, parece que *k = 4* sería un buen valor para probar. El valor *k* agrupará los clientes en cuatro clústeres.

## <a name="perform-clustering"></a>Agrupación en clústeres

En el siguiente script de R, usará la función **kmeans** para realizar la agrupación en clústeres.

```r
# Output table to hold the customer group mappings.
# Generate clusters using Kmeans and output key / cluster to a table
# called return_cluster

## create clustering model
clust <- kmeans(customer_data[,2:5],4)

## create clustering ouput for table
customer_cluster <- data.frame(cluster=clust$cluster,customer=customer_data$customer,orderRatio=customer_data$orderRatio,
        itemsRatio=customer_data$itemsRatio,monetaryRatio=customer_data$monetaryRatio,frequency=customer_data$frequency)

## write cluster output to DB table
sqlSave(ch, customer_cluster, tablename = "return_cluster")

# Read the customer returns cluster table from the database
customer_cluster_check <- sqlFetch(ch, "return_cluster")

head(customer_cluster_check)
```

## <a name="analyze-the-results"></a>Análisis de los resultados

Ahora que ha realizado la agrupación en clústeres mediante K-Means, el siguiente paso es analizar el resultado y ver si puede encontrar información procesable.

```r
#Look at the clustering details to analyze results
clust[-1]
```

```results
$centers
   orderRatio itemsRatio monetaryRatio frequency
1 0.621835791  0.1701519    0.35510836  1.009025
2 0.074074074  0.0000000    0.05886575  2.363248
3 0.004807692  0.0000000    0.04618708  5.050481
4 0.000000000  0.0000000    0.00000000  0.000000

$totss
[1] 40191.83

$withinss
[1] 19867.791   215.714   660.784     0.000

$tot.withinss
[1] 20744.29

$betweenss
[1] 19447.54

$size
[1]  4543   702   416 31675

$iter
[1] 3

$ifault
[1] 0

```

Las cuatro medias de clústeres se proporcionan mediante las variables definidas en la [parte dos](r-clustering-model-prepare-data.md#separate-customers):

* *orderRatio* = índice de devolución de pedidos (número total de pedidos con una devolución total o parcial comparado con el número total de pedidos)
* *itemsRatio* = índice de artículos devueltos (número total de artículos devueltos comparado con el número de artículos comprados)
* *monetaryRatio* = índice de importes de devoluciones (total de importes monetarios de los artículos devueltos comparado con el importe de las compras)
* *frequency* = frecuencia de devolución

Con frecuencia, la minería de datos que usa k-means necesita un análisis más detallado de los resultados y pasos adicionales para comprender mejor cada clúster, pero puede proporcionar pistas adecuadas.
Estas son dos formas en que se podrían interpretar estos resultados:

* El clúster 1 (el clúster más grande) parece estar constituido por un grupo de clientes que no están activos (todos los valores son cero).
* Parece que el clúster 3 es un grupo que destaca en términos de comportamiento de devoluciones.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si no quiere continuar con este tutorial, elimine la base de datos tpcxbb_1gb.

## <a name="next-steps"></a>Pasos siguientes

En la tercera parte de la serie de tutoriales, ha aprendido a:

* Definición del número de clústeres para un algoritmo de k-means
* Agrupación en clústeres
* Análisis de los resultados

Para implementar el modelo de aprendizaje automático que ha creado, siga la parte cuatro de esta serie de tutoriales:

> [!div class="nextstepaction"]
> [Implementación de un modelo de agrupación en clústeres en R con el aprendizaje automático de SQL](r-clustering-model-deploy.md)
