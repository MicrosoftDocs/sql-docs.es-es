---
title: Trazado de un histograma para la exploración de datos con Python
titleSuffix: SQL machine learning
description: Aprenda a crear un histograma para visualizar datos mediante Python.
author: dphansen
ms.author: davidph
ms.date: 07/14/2020
ms.topic: how-to
ms.prod: sql
ms.technology: machine-learning
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current||=azuresqldb-current'
ms.openlocfilehash: f9c803760a3ee098e841cbc716cde8ac8a2fd648
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349102"
---
# <a name="plot-histograms-in-python"></a>Trazado de histogramas en Python 
[!INCLUDE[SQL Server SQL DB SQL MI](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

En este artículo se describe cómo trazar datos mediante el paquete de Python [pandas'.hist ()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.hist.html). Una base de datos SQL es el origen que se usa para visualizar los intervalos de datos del histograma que tienen valores consecutivos no superpuestos.

## <a name="prerequisites"></a>Requisitos previos:

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
* [SQL Server para Windows](../../database-engine/install-windows/install-sql-server.md) o [para Linux](../../linux/sql-server-linux-overview.md).
::: moniker-end

::: moniker range="=azuresqldb-current"
* [Azure SQL Database](/azure/sql-database/sql-database-get-started-portal)
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
* [Instancia administrada de Azure SQL](/azure/azure-sql/managed-instance/instance-create-quickstart)

* [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) para restaurar la base de datos de ejemplo en Azure SQL Managed Instance.
::: moniker-end

* Azure Data Studio. Para realizar la instalación, vea [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md).

* [Restaure la base de datos DW de ejemplo](../../samples/adventureworks-install-configure.md) para obtener los datos de ejemplo que se usan en este artículo.

## <a name="verify-restored-database"></a>Comprobación de la base de datos restaurada

Para comprobar que la base de datos restaurada existe, consulte la tabla **Person.CountryRegion**:
```sql
USE AdventureWorksDW;
SELECT * FROM Person.CountryRegion;
```
  
## <a name="install-python-packages"></a>Instalación de paquetes de Python

[Descarga e instalación de Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md).

Instale los siguientes paquetes de Python:
  * pyodbc
  * Pandas

  Para instalar estos paquetes:

  1. En el cuaderno de Azure Data Studio, seleccione **Administrar paquetes**.
  2. En el panel **Administrar paquetes**, seleccione la pestaña **Agregar nuevo**.
  3. Para cada uno de los paquetes siguientes, escriba el nombre del paquete, haga clic en **Buscar** y, a continuación, haga clic en **instalar**.

## <a name="plot-histogram"></a>Trazado del histograma

Los datos distribuidos que se muestran en el histograma están basados en una consulta SQL de AdventureWorksDW. El histograma visualiza los datos y la frecuencia de los valores de los mismos. Edite las variables de cadena de conexión: "Server", "Database", "Username" y "Password" para conectarse a SQL Database.

Para crear un nuevo cuaderno:

1. En Azure Data Studio, seleccione **Archivo** y luego **Nuevo cuaderno**.
2. En el bloc de notas, seleccione el kernel **Python3** y luego el comando **+Código**.
3. Pegue el código en el bloc de notas y seleccione **Ejecutar todo**.

```python
import pyodbc 
import pandas as plt
# Some other example server values are
# server = 'localhost\sqlexpress' # for a named instance
# server = 'myserver,port' # to specify an alternate port
server = 'servername' 
database = 'AdventureWorksDW' 
username = 'yourusername' 
password = 'databasename'  
cnxn = pyodbc.connect('DRIVER={SQL Server};SERVER='+server+';DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
sql = "SELECT DATEDIFF(year, c.BirthDate, GETDATE()) AS Age FROM [dbo].[FactInternetSales] s INNER JOIN dbo.DimCustomer c ON s.CustomerKey = c.CustomerKey"
df = pd.read_sql(sql, cnxn)
df.hist(bins=10)
```

La pantalla muestra la distribución de edad de los clientes en la tabla FactInternetSales.

![Histograma Pandas](./media/python-histogram.png)