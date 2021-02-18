---
title: 'Tutorial: Preparación de datos para realizar la agrupación en clústeres en R'
titleSuffix: SQL machine learning
description: En la parte dos de esta serie de tutoriales de cuatro partes, preparará los datos de una base de datos para realizar la agrupación en clústeres en R con el aprendizaje automático de SQL.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 358f34e281c5c5b1a750135117886247c5e504c0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348064"
---
# <a name="tutorial-prepare-data-to-perform-clustering-in-r-with-sql-machine-learning"></a>Tutorial: Preparación de datos para realizar la agrupación en clústeres en R con el aprendizaje automático de SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
En la parte dos de esta serie de tutoriales de cuatro partes, preparará los datos de una base de datos para realizar la agrupación en clústeres en R con SQL Server Machine Learning Services o en Clústeres de macrodatos.
::: moniker-end
::: moniker range="=sql-server-2017"
En la parte dos de esta serie de tutoriales de cuatro partes, preparará los datos de una base de datos para realizar la agrupación en clústeres en R con SQL Server Machine Learning Services.
::: moniker-end
::: moniker range="=sql-server-2016"
En la parte dos de esta serie de tutoriales de cuatro partes, preparará los datos de una base de datos para realizar la agrupación en clústeres en R con SQL Server 2016 R Services.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
En la parte dos de esta serie de tutoriales de cuatro partes, preparará los datos de una base de datos para realizar la agrupación en clústeres en R con Machine Learning Services en Azure SQL Managed Instance.
::: moniker-end

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Separar clientes en diferentes dimensiones mediante R
> * Cargar los datos de la base de datos en una trama de datos de R

En la [parte uno](r-clustering-model-introduction.md), ha instalado los requisitos previos y ha restaurado la base de datos de ejemplo.

En la [parte tres](r-clustering-model-build.md), aprenderá a crear y entrenar un modelo de agrupación en clústeres k-means en R.

En la [parte cuatro](r-clustering-model-deploy.md), descubrirá cómo crear un procedimiento almacenado en una base de datos que pueda realizar la agrupación en clústeres en R basándose en datos nuevos.

## <a name="prerequisites"></a>Prerrequisitos

* En la parte dos de este tutorial, se da por hecho que ha completado la [**parte uno**](r-clustering-model-introduction.md).

## <a name="separate-customers"></a>Separación de clientes

Cree un archivo de RScript en RStudio y ejecute el siguiente script.
En la consulta SQL, está dividiendo a los clientes entre las dimensiones siguientes:

* **orderRatio** = índice de devolución de pedidos (número total de pedidos con una devolución total o parcial comparado con el número total de pedidos)
* **itemsRatio** = índice de artículos devueltos (número total de artículos devueltos comparado con el número de artículos comprados)
* **monetaryRatio** = índice de importes de devoluciones (total de importes monetarios de los artículos devueltos comparado con el importe de las compras)
* **frequency** = frecuencia de devolución

En la función **connStr**, reemplace **ServerName** por su propia información de conexión.

```r
# Define the connection string to connect to the tpcxbb_1gb database

connStr <- "Driver=SQL Server;Server=ServerName;Database=tpcxbb_1gb;uid=Username;pwd=Password"

#Define the query to select data
input_query <- "
SELECT ss_customer_sk AS customer
    ,round(CASE 
            WHEN (
                       (orders_count = 0)
                    OR (returns_count IS NULL)
                    OR (orders_count IS NULL)
                    OR ((returns_count / orders_count) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_count AS NCHAR(10)) / orders_count)
            END, 7) AS orderRatio
    ,round(CASE 
            WHEN (
                     (orders_items = 0)
                  OR (returns_items IS NULL)
                  OR (orders_items IS NULL)
                  OR ((returns_items / orders_items) IS NULL)
                 )
            THEN 0.0
            ELSE (cast(returns_items AS NCHAR(10)) / orders_items)
            END, 7) AS itemsRatio
    ,round(CASE 
            WHEN (
                     (orders_money = 0)
                  OR (returns_money IS NULL)
                  OR (orders_money IS NULL)
                  OR ((returns_money / orders_money) IS NULL)
                 )
            THEN 0.0
            ELSE (cast(returns_money AS NCHAR(10)) / orders_money)
            END, 7) AS monetaryRatio
    ,round(CASE 
            WHEN (returns_count IS NULL)
            THEN 0.0
            ELSE returns_count
            END, 0) AS frequency
FROM (
    SELECT ss_customer_sk,
        -- return order ratio
        COUNT(DISTINCT (ss_ticket_number)) AS orders_count,
        -- return ss_item_sk ratio
        COUNT(ss_item_sk) AS orders_items,
        -- return monetary amount ratio
        SUM(ss_net_paid) AS orders_money
    FROM store_sales s
    GROUP BY ss_customer_sk
    ) orders
LEFT OUTER JOIN (
    SELECT sr_customer_sk,
        -- return order ratio
        count(DISTINCT (sr_ticket_number)) AS returns_count,
        -- return ss_item_sk ratio
        COUNT(sr_item_sk) AS returns_items,
        -- return monetary amount ratio
        SUM(sr_return_amt) AS returns_money
    FROM store_returns
    GROUP BY sr_customer_sk
    ) returned ON ss_customer_sk = sr_customer_sk";
```

## <a name="load-the-data-into-a-data-frame"></a>Carga de los datos en una trama de datos

Ahora, use el siguiente script para que se devuelvan los resultados de la consulta a una trama de datos de R.

```r
# Query using input_query and get the results back
# to data frame customer_data

library(RODBC)

ch <- odbcDriverConnect(connStr)

customer_data <- sqlQuery(ch, input_query)

# Take a look at the data just loaded
head(customer_data, n = 5);
```

Se mostrarán resultados similares a los siguientes.

```results
  customer orderRatio itemsRatio monetaryRatio frequency
1    29727          0          0      0.000000         0
2    26429          0          0      0.041979         1
3    60053          0          0      0.065762         3
4    97643          0          0      0.037034         3
5    32549          0          0      0.031281         4
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Si no quiere continuar con este tutorial, elimine la base de datos tpcxbb_1gb.

## <a name="next-steps"></a>Pasos siguientes

En la parte dos de la serie de tutoriales, ha aprendido a:

* Separar clientes en diferentes dimensiones mediante R
* Cargar los datos de la base de datos en una trama de datos de R

Para crear un modelo de aprendizaje automático que use estos datos de clientes, siga la parte tres de esta serie de tutoriales:

> [!div class="nextstepaction"]
> [Creación de un modelo predictivo en R con el aprendizaje automático de SQL](r-clustering-model-build.md)
