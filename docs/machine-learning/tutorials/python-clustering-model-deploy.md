---
title: 'Tutorial de Python: Implementación del modelo del clúster'
titleSuffix: SQL machine learning
description: En la parte cuatro de esta serie de tutoriales de cuatro partes, implementará un modelo de agrupación en clústeres en Python con el aprendizaje automático de SQL.
ms.prod: sql
ms.technology: machine-learning
ms.devlang: python
ms.date: 05/21/2020
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 32b009f8155000dfc44f714cfa6f67fc8333813e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349049"
---
# <a name="python-tutorial-deploy-a-model-to-categorize-customers-with-sql-machine-learning"></a>Tutorial de Python: Implementación de un modelo para clasificar clientes por categorías con aprendizaje automático de SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
En la parte cuatro de esta serie de tutoriales de cuatro partes, implementará un modelo de agrupación en clústeres, desarrollado en Python, en una base de datos con SQL Server Machine Learning Services o en Clústeres de macrodatos.
::: moniker-end
::: moniker range="=sql-server-2017"
En la parte cuatro de esta serie de tutoriales de cuatro partes, implementará un modelo de agrupación en clústeres, desarrollado en Python, en una base de datos mediante SQL Server Machine Learning Services.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
En la parte cuatro de esta serie de tutoriales de cuatro partes, implementará un modelo de agrupación en clústeres, desarrollado en Python, en una base de datos mediante Machine Learning Services en Azure SQL Managed Instance.
::: moniker-end

Para realizar la agrupación en clústeres de forma periódica cuando se registren nuevos clientes, necesita llamar al script de Python desde cualquier aplicación. Para hacerlo, puede implementar el script de Python en una base de datos si lo coloca dentro de un procedimiento almacenado de SQL. Como el modelo se ejecuta en la base de datos, se puede entrenar fácilmente con los datos almacenados en la base de datos.

En esta sección, moverá el código de Python que acaba de escribir en el servidor e implementará la agrupación en clústeres.

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Crear un procedimiento almacenado que genere el modelo
> * Agrupación en clústeres en el servidor
> * Uso de la información de agrupación en clústeres

En la [parte uno](python-clustering-model.md), ha instalado los requisitos previos y ha restaurado la base de datos de ejemplo.

En la [parte dos](python-clustering-model-prepare-data.md), ha aprendido a preparar los datos de una base de datos para realizar la agrupación en clústeres.

En la [parte tres](python-clustering-model-build.md), ha aprendido a crear y entrenar un modelo de agrupación en clústeres k-means en Python.

## <a name="prerequisites"></a>Prerrequisitos

* En la parte cuatro de esta serie de tutoriales, se da por hecho que ha completado los requisitos previos de la [**parte uno**](python-clustering-model.md) y que ha realizado los pasos de la [**parte dos**](python-clustering-model-prepare-data.md) y la [**parte tres**](python-clustering-model-build.md).

## <a name="create-a-stored-procedure-that-generates-the-model"></a>Crear un procedimiento almacenado que genere el modelo

Ejecute el siguiente script de T-SQL para crear el procedimiento almacenado. El procedimiento recrea los pasos que ha desarrollado en las partes uno y dos de esta serie de tutoriales:

* clasificación de los clientes basándose en su historial de compras devoluciones
* generación de cuatro clústeres de clientes mediante un algoritmo k-means

```sql
USE [tpcxbb_1gb]
GO

CREATE procedure [dbo].[py_generate_customer_return_clusters]
AS

BEGIN
    DECLARE

-- Input query to generate the purchase history & return metrics
     @input_query NVARCHAR(MAX) = N'
SELECT
  ss_customer_sk AS customer,
  CAST( (ROUND(COALESCE(returns_count / NULLIF(1.0*orders_count, 0), 0), 7) ) AS FLOAT) AS orderRatio,
  CAST( (ROUND(COALESCE(returns_items / NULLIF(1.0*orders_items, 0), 0), 7) ) AS FLOAT) AS itemsRatio,
  CAST( (ROUND(COALESCE(returns_money / NULLIF(1.0*orders_money, 0), 0), 7) ) AS FLOAT) AS monetaryRatio,
  CAST( (COALESCE(returns_count, 0)) AS FLOAT) AS frequency
FROM
  (
    SELECT
      ss_customer_sk,
      -- return order ratio
      COUNT(distinct(ss_ticket_number)) AS orders_count,
      -- return ss_item_sk ratio
      COUNT(ss_item_sk) AS orders_items,
      -- return monetary amount ratio
      SUM( ss_net_paid ) AS orders_money
    FROM store_sales s
    GROUP BY ss_customer_sk
  ) orders
  LEFT OUTER JOIN
  (
    SELECT
      sr_customer_sk,
      -- return order ratio
      count(distinct(sr_ticket_number)) as returns_count,
      -- return ss_item_sk ratio
      COUNT(sr_item_sk) as returns_items,
      -- return monetary amount ratio
      SUM( sr_return_amt ) AS returns_money
    FROM store_returns
    GROUP BY sr_customer_sk
  ) returned ON ss_customer_sk=sr_customer_sk
 '

EXEC sp_execute_external_script
      @language = N'Python'
    , @script = N'

import pandas as pd
from sklearn.cluster import KMeans

#get data from input query
customer_data = my_input_data

#We concluded in step 2 in the tutorial that 4 would be a good number of clusters
n_clusters = 4

#Perform clustering
est = KMeans(n_clusters=n_clusters, random_state=111).fit(customer_data[["orderRatio","itemsRatio","monetaryRatio","frequency"]])
clusters = est.labels_
customer_data["cluster"] = clusters

OutputDataSet = customer_data
'
    , @input_data_1 = @input_query
    , @input_data_1_name = N'my_input_data'
             with result sets (("Customer" int, "orderRatio" float,"itemsRatio" float,"monetaryRatio" float,"frequency" float,"cluster" float));
END;
GO
```

## <a name="perform-clustering"></a>Agrupación en clústeres

Después de crear el procedimiento almacenado, ejecute el script siguiente para realizar la agrupación en clústeres mediante el procedimiento.

```sql
--Create a table to store the predictions in

DROP TABLE IF EXISTS [dbo].[py_customer_clusters];
GO

CREATE TABLE [dbo].[py_customer_clusters] (
    [Customer] [bigint] NULL
  , [OrderRatio] [float] NULL
  , [itemsRatio] [float] NULL
  , [monetaryRatio] [float] NULL
  , [frequency] [float] NULL
  , [cluster] [int] NULL
  ,
    ) ON [PRIMARY]
GO

--Execute the clustering and insert results into table
INSERT INTO py_customer_clusters
EXEC [dbo].[py_generate_customer_return_clusters];

-- Select contents of the table to verify it works
SELECT * FROM py_customer_clusters;
```

## <a name="use-the-clustering-information"></a>Uso de la información de agrupación en clústeres

Como ha almacenado el procedimiento de agrupación en clústeres en la base de datos, puede realizar la agrupación en clústeres de forma eficiente con los datos de cliente almacenados en la misma base de datos. Puede ejecutar el procedimiento cada vez que se actualicen los datos de clientes y usar la información de agrupación en clústeres actualizada.

Imagine que quiere enviar un correo electrónico promocional a los clientes del clúster 0, el grupo que estaba inactivo (vea una descripción de los cuatro clústeres en la [parte tres](python-clustering-model-build.md#analyze-the-results) de este tutorial). El código siguiente selecciona las direcciones de correo electrónico de los clientes en el clúster 0.

```sql
USE [tpcxbb_1gb]
--Get email addresses of customers in cluster 0 for a promotion campaign
SELECT customer.[c_email_address], customer.c_customer_sk
  FROM dbo.customer
  JOIN
  [dbo].[py_customer_clusters] as c
  ON c.Customer = customer.c_customer_sk
  WHERE c.cluster = 0
```

Puede cambiar el valor de **c.cluster** a las direcciones de correo electrónico de retorno de los clientes en otros clústeres.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando termine este tutorial, puede eliminar la base de datos tpcxbb_1gb.

## <a name="next-steps"></a>Pasos siguientes

En la parte cuatro de esta serie de tutoriales, ha aprendido a:

* Crear un procedimiento almacenado que genere el modelo
* Agrupación en clústeres en el servidor
* Uso de la información de agrupación en clústeres

Para más información sobre cómo usar Python en el aprendizaje automático de SQL, consulte:

* [Inicio rápido: Creación y ejecución de scripts de Python](quickstart-python-create-script.md)
* [Otros tutoriales de Python para aprendizaje automático de SQL](python-tutorials.md)
* [Instalación de paquetes de Python con sqlmlutils](../package-management/install-additional-python-packages-on-sql-server.md)