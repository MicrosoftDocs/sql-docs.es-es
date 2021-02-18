---
title: 'Tutorial: Implementación de un modelo de agrupación en clústeres en R'
titleSuffix: SQL machine learning
description: En la parte cuatro de esta serie de tutoriales de cuatro partes, implementará un modelo de agrupación en clústeres en R con el aprendizaje automático de SQL.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: b1dc019023546a4d1912480bfe452b3067f36bf9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339625"
---
# <a name="tutorial-deploy-a-clustering-model-in-r-with-sql-machine-learning"></a>Tutorial: Implementación de un modelo de agrupación en clústeres en R con el aprendizaje automático de SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
En la parte cuatro de esta serie de tutoriales de cuatro partes, implementará un modelo de agrupación en clústeres, desarrollado en R, en una base de datos con SQL Server Machine Learning Services o en Clústeres de macrodatos.
::: moniker-end
::: moniker range="=sql-server-2017"
En la parte cuatro de esta serie de tutoriales de cuatro partes, implementará un modelo de agrupación en clústeres, desarrollado en R, en una base de datos mediante SQL Server Machine Learning Services.
::: moniker-end
::: moniker range="=sql-server-2016"
En la parte cuatro de esta serie de tutoriales de cuatro partes, implementará un modelo de agrupación en clústeres, desarrollado en R, en una base de datos mediante SQL Server R Services.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
En la parte cuatro de esta serie de tutoriales de cuatro partes, implementará un modelo de agrupación en clústeres, desarrollado en R, en una base de datos mediante Machine Learning Services en Azure SQL Managed Instance.
::: moniker-end

Para realizar la agrupación en clústeres de forma periódica cuando se registren nuevos clientes, necesita llamar al script de R desde cualquier aplicación. Para hacerlo, puede implementar el script de R en una base de datos si lo coloca dentro de un procedimiento almacenado de SQL. Como el modelo se ejecuta en la base de datos, se puede entrenar fácilmente con los datos almacenados en la base de datos.

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Crear un procedimiento almacenado que genere el modelo
> * Agrupación en clústeres
> * Uso de la información de agrupación en clústeres

En la [parte uno](r-clustering-model-introduction.md), ha instalado los requisitos previos y ha restaurado la base de datos de ejemplo.

En la [parte dos](r-clustering-model-prepare-data.md), ha aprendido a preparar los datos de una base de datos para realizar la agrupación en clústeres.

En la [parte tres](r-clustering-model-build.md), ha aprendido a crear y entrenar un modelo de agrupación en clústeres k-means en R.

## <a name="prerequisites"></a>Prerrequisitos

* En la parte cuatro de esta serie de tutoriales, se da por hecho que ha completado los requisitos previos de la [**parte uno**](r-clustering-model-introduction.md) y que ha realizado los pasos de la [**parte dos**](r-clustering-model-build.md) y la [**parte tres**](r-clustering-model-build.md).

## <a name="create-a-stored-procedure-that-generates-the-model"></a>Crear un procedimiento almacenado que genere el modelo

Ejecute el siguiente script de T-SQL para crear el procedimiento almacenado. El procedimiento recrea los pasos que ha desarrollado en las partes dos y tres de esta serie de tutoriales:

* clasificación de los clientes basándose en su historial de compras devoluciones
* generación de cuatro clústeres de clientes mediante un algoritmo k-means

El procedimiento almacena las asignaciones de clústeres de clientes resultantes en la tabla de la base de datos **customer_return_clusters**.

```sql
USE [tpcxbb_1gb]
DROP PROC IF EXISTS generate_customer_return_clusters;
GO
CREATE procedure [dbo].[generate_customer_return_clusters]
AS
/*
  This procedure uses R to classify customers into different groups
  based on their purchase & return history.
*/
BEGIN
    DECLARE @duration FLOAT
    , @instance_name NVARCHAR(100) = @@SERVERNAME
    , @database_name NVARCHAR(128) = db_name()
-- Input query to generate the purchase history & return metrics
    , @input_query NVARCHAR(MAX) = N'
SELECT ss_customer_sk AS customer,
    round(CASE 
            WHEN (
                    (orders_count = 0)
                    OR (returns_count IS NULL)
                    OR (orders_count IS NULL)
                    OR ((returns_count / orders_count) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_count AS NCHAR(10)) / orders_count)
            END, 7) AS orderRatio,
    round(CASE 
            WHEN (
                    (orders_items = 0)
                    OR (returns_items IS NULL)
                    OR (orders_items IS NULL)
                    OR ((returns_items / orders_items) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_items AS NCHAR(10)) / orders_items)
            END, 7) AS itemsRatio,
    round(CASE 
            WHEN (
                    (orders_money = 0)
                    OR (returns_money IS NULL)
                    OR (orders_money IS NULL)
                    OR ((returns_money / orders_money) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_money AS NCHAR(10)) / orders_money)
            END, 7) AS monetaryRatio,
    round(CASE 
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
    ) returned ON ss_customer_sk = sr_customer_sk
 '
EXECUTE sp_execute_external_script
      @language = N'R'
    , @script = N'
# Define the connection string

connStr <- paste("Driver=SQL Server; Server=", instance_name,
                 "; Database=", database_name,
                 "; uid=Username;pwd=Password; ",
                 sep="" )

# Input customer data that needs to be classified.
# This is the result we get from the query.
library(RODBC)

ch <- odbcDriverConnect(connStr);

customer_data <- sqlQuery(ch, input_query)

sqlDrop(ch, "customer_return_clusters")

## create clustering model
clust <- kmeans(customer_data[,2:5],4)

## create clustering output for table
customer_cluster <- data.frame(cluster=clust$cluster,customer=customer_data$customer,orderRatio=customer_data$orderRatio,
            itemsRatio=customer_data$itemsRatio,monetaryRatio=customer_data$monetaryRatio,frequency=customer_data$frequency)

## write cluster output to DB table
sqlSave(ch, customer_cluster, tablename = "customer_return_clusters")

## clean up
odbcClose(ch)
'
    , @input_data_1 = N''
    , @params = N'@instance_name nvarchar(100), @database_name nvarchar(128), @input_query nvarchar(max), @duration float OUTPUT'
    , @instance_name = @instance_name
    , @database_name = @database_name
    , @input_query = @input_query
    , @duration = @duration OUTPUT;
END;

GO
```

## <a name="perform-clustering"></a>Agrupación en clústeres

Ahora que ha creado el procedimiento almacenado, ejecute el siguiente script para realizar la agrupación en clústeres.

```sql
--Empty table of the results before running the stored procedure
TRUNCATE TABLE customer_return_clusters;

--Execute the clustering
--This will load the table customer_return_clusters with cluster mappings
EXECUTE [dbo].[generate_customer_return_clusters];
```

Compruebe que funciona y que realmente tenemos la lista de los clientes y sus asignaciones de clúster.

```sql
--Select data from table customer_return_clusters
--to verify that the clustering data was loaded
SELECT TOP (5) *
FROM customer_return_clusters;
```

```result
cluster  customer  orderRatio  itemsRatio  monetaryRatio  frequency
1        29727     0           0           0              0
4        26429     0           0           0.041979       1
2        60053     0           0           0.065762       3
2        97643     0           0           0.037034       3
2        32549     0           0           0.031281       4
```

## <a name="use-the-clustering-information"></a>Uso de la información de agrupación en clústeres

Como ha almacenado el procedimiento de agrupación en clústeres en la base de datos, puede realizar la agrupación en clústeres de forma eficiente con los datos de cliente almacenados en la misma base de datos. Puede ejecutar el procedimiento cada vez que se actualicen los datos de clientes y usar la información de agrupación en clústeres actualizada.

Imagine que quiere enviar un correo electrónico promocional a los clientes del clúster 0, el grupo que estaba inactivo (vea una descripción de los cuatro clústeres en la [parte tres](r-clustering-model-build.md#analyze-the-results) de este tutorial). El código siguiente selecciona las direcciones de correo electrónico de los clientes en el clúster 0.

```sql
USE [tpcxbb_1gb]
--Get email addresses of customers in cluster 0 for a promotion campaign
SELECT customer.[c_email_address], customer.c_customer_sk
  FROM dbo.customer
  JOIN
  [dbo].[customer_clusters] as c
  ON c.Customer = customer.c_customer_sk
  WHERE c.cluster = 0
```

Puede cambiar el valor de **c.cluster** a las direcciones de correo electrónico de retorno de los clientes en otros clústeres.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando termine este tutorial, puede eliminar la base de datos tpcxbb_1gb.

## <a name="next-steps"></a>Pasos siguientes

En la parte cuatro de la serie de tutoriales, ha aprendido a:

* Crear un procedimiento almacenado que genere el modelo
* Realización de la agrupación en clústeres con aprendizaje automático de SQL
* Uso de la información de agrupación en clústeres

Para más información sobre cómo usar R en Machine Learning Services, vea:

* [Ejecución de scripts de R simples](quickstart-r-create-script.md)
* [Estructuras de datos, tipos y objetos de R](quickstart-r-data-types-and-objects.md)
* [Funciones de R](quickstart-r-functions.md)
