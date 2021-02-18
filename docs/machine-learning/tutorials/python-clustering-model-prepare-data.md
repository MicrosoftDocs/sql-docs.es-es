---
title: 'Tutorial de Python: Preparación de los datos del clúster'
titleSuffix: SQL machine learning
description: En la parte dos de esta serie de tutoriales de cuatro partes, preparará los datos de SQL para realizar la agrupación en clústeres en Python con aprendizaje automático de SQL.
ms.prod: sql
ms.technology: machine-learning
ms.devlang: python
ms.date: 05/21/2020
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 7d73f57f771a52de065e0fdc8236e280d5d85ab2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339718"
---
# <a name="python-tutorial-prepare-data-to-categorize-customers-with-sql-machine-learning"></a>Tutorial de Python: Preparación de datos para clasificar clientes por categorías con aprendizaje automático de SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
En la parte dos de esta serie de tutoriales de cuatro partes, restaurará y preparará los datos de una base de datos mediante Python. Más adelante en esta serie, usará estos datos para entrenar e implementar un modelo de agrupación en clústeres en Python con SQL Server Machine Learning Services o en clústeres de macrodatos.
::: moniker-end
::: moniker range="=sql-server-2017"
En la parte dos de esta serie de tutoriales de cuatro partes, restaurará y preparará los datos de una base de datos mediante Python. Más adelante en esta serie, usará estos datos para entrenar e implementar un modelo de agrupación en clústeres en Python con SQL Server Machine Learning Services.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
En la parte dos de esta serie de tutoriales de cuatro partes, restaurará y preparará los datos de una base de datos mediante Python. Más adelante en esta serie, usará estos datos para entrenar e implementar un modelo de agrupación en clústeres en Python con Machine Learning Services en Azure SQL Managed Instance.
::: moniker-end

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Separación de los clientes en distintas dimensiones mediante Python
> * Carga de datos de la base de datos en una trama de datos de Python

En la [parte uno](python-clustering-model.md), ha instalado los requisitos previos y ha restaurado la base de datos de ejemplo.

En la [parte tres](python-clustering-model-build.md), aprenderá a crear y entrenar un modelo de agrupación en clústeres k-means en Python.

En la [parte cuatro](python-clustering-model-deploy.md), aprenderá a crear un procedimiento almacenado en una base de datos que pueda realizar la agrupación en clústeres en Python basándose en datos nuevos.

## <a name="prerequisites"></a>Prerrequisitos

* En la parte dos de este tutorial, se da por hecho que ha completado los requisitos previos de la [**parte uno**](python-clustering-model.md).

## <a name="separate-customers"></a>Separación de clientes

Para preparar la agrupación en clústeres de los clientes, primero separe los clientes en las dimensiones siguientes:

* **orderRatio** = índice de devolución de pedidos (número total de pedidos con una devolución total o parcial comparado con el número total de pedidos)
* **itemsRatio** = índice de artículos devueltos (número total de artículos devueltos comparado con el número de artículos comprados)
* **monetaryRatio** = índice de importes de devoluciones (total de importes monetarios de los artículos devueltos comparado con el importe de las compras)
* **frequency** = frecuencia de devolución

Abra un nuevo cuaderno en Azure Data Studio y escriba el script siguiente.

En la cadena de conexión, reemplace los detalles de conexión según corresponda.

```python
# Load packages.
import pyodbc
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
from scipy.spatial import distance as sci_distance
from sklearn import cluster as sk_cluster

################################################################################################

## Connect to DB and select data

################################################################################################

# Connection string to connect to SQL Server named instance.
conn_str = pyodbc.connect('DRIVER={ODBC Driver 17 for SQL Server}; SERVER=<server>; DATABASE=tpcxbb_1gb; UID=<username>; PWD=<password>')

input_query = '''SELECT
ss_customer_sk AS customer,
ROUND(COALESCE(returns_count / NULLIF(1.0*orders_count, 0), 0), 7) AS orderRatio,
ROUND(COALESCE(returns_items / NULLIF(1.0*orders_items, 0), 0), 7) AS itemsRatio,
ROUND(COALESCE(returns_money / NULLIF(1.0*orders_money, 0), 0), 7) AS monetaryRatio,
COALESCE(returns_count, 0) AS frequency
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
GROUP BY sr_customer_sk ) returned ON ss_customer_sk=sr_customer_sk'''


# Define the columns we wish to import.
column_info = {
    "customer": {"type": "integer"},
    "orderRatio": {"type": "integer"},
    "itemsRatio": {"type": "integer"},
    "frequency": {"type": "integer"}
}
```

## <a name="load-the-data-into-a-data-frame"></a>Carga de los datos en una trama de datos

Los resultados de la consulta se devuelven a Python mediante la función **read_sql** de Pandas. Como parte del proceso, usará la información sobre columnas que ha definido en el script anterior.

```python
customer_data = pandas.read_sql(input_query, conn_str)
```

Ahora, muestre el principio de la trama de datos para asegurarse de que sea correcta.

```python
print("Data frame:", customer_data.head(n=5))
```

```results
Rows Read: 37336, Total Rows Processed: 37336, Total Chunk Time: 0.172 seconds
Data frame:     customer  orderRatio  itemsRatio  monetaryRatio  frequency
0    29727.0    0.000000    0.000000       0.000000          0
1    97643.0    0.068182    0.078176       0.037034          3
2    57247.0    0.000000    0.000000       0.000000          0
3    32549.0    0.086957    0.068657       0.031281          4
4     2040.0    0.000000    0.000000       0.000000          0
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Si no quiere continuar con este tutorial, elimine la base de datos tpcxbb_1gb.

## <a name="next-steps"></a>Pasos siguientes

En la parte dos de esta serie de tutoriales, ha completado estos pasos:

* Separación de los clientes en distintas dimensiones mediante Python
* Carga de datos de la base de datos en una trama de datos de Python

Para crear un modelo de aprendizaje automático que use estos datos de clientes, siga la parte tres de esta serie de tutoriales:

> [!div class="nextstepaction"]
> [Tutorial de Python: Creación de un modelo predictivo](python-clustering-model-build.md)
