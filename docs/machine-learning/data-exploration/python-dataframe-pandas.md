---
title: Inserción de datos de una tabla SQL en un dataframe de Pandas de Python
titleSuffix: SQL machine learning
description: Obtenga información sobre cómo leer datos de una tabla SQL e insertarlos en un dataframe de Pandas con Python.
author: dphansen
ms.author: davidph
ms.date: 07/23/2020
ms.topic: how-to
ms.prod: sql
ms.technology: machine-learning
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current||=azuresqldb-current'
ms.openlocfilehash: 7b26838dceee9b933b4f33621af3b32bdb0a0c67
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349097"
---
# <a name="insert-data-from-a-sql-table-into-a-python-pandas-dataframe"></a>Inserción de datos de una tabla SQL en un dataframe de Pandas de Python
[!INCLUDE[SQL Server SQL DB SQL MI](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

En este artículo se describe cómo insertar datos de SQL en un dataframe de [Pandas](https://pandas.pydata.org/) mediante el paquete [pyodbc](../../connect/python/pyodbc/python-sql-driver-pyodbc.md) en Python. Las filas y las columnas de datos contenidas en el dataframe se pueden usar para seguir explorando los datos.

## <a name="prerequisites"></a>Requisitos previos

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

* [Restaure la base de datos de ejemplo](../../samples/adventureworks-install-configure.md) para obtener los datos de ejemplo que se usan en este artículo.

## <a name="verify-restored-database"></a>Comprobación de la base de datos restaurada

Para comprobar que la base de datos restaurada existe, consulte la tabla **Person.CountryRegion**:

```sql
USE AdventureWorks;
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

## <a name="insert-data"></a>Insertar datos

Use el siguiente script para seleccionar datos de la tabla Person.CountryRegion e insertarlos en un dataframe. Edite las variables de cadena de conexión: "Server", "Database", "Username" y "Password" para conectarse a SQL.

Para crear un nuevo cuaderno:

1. En Azure Data Studio, seleccione **Archivo** y luego **Nuevo cuaderno**.
2. En el bloc de notas, seleccione el kernel **Python3** y luego el comando **+Código**.
3. Pegue el código en el bloc de notas y seleccione **Ejecutar todo**.

```python
import pyodbc
import pandas as pd
# Some other example server values are
# server = 'localhost\sqlexpress' # for a named instance
# server = 'myserver,port' # to specify an alternate port
server = 'servername' 
database = 'AdventureWorks' 
username = 'yourusername' 
password = 'databasename'  
cnxn = pyodbc.connect('DRIVER={SQL Server};SERVER='+server+';DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
# select 26 rows from SQL table to insert in dataframe.
query = "SELECT [CountryRegionCode], [Name] FROM Person.CountryRegion;"
df = pd.read_sql(query, cnxn)
print(df.head(26))
```

**Salida**

El comando `print` del script anterior muestra las filas de datos del dataframe `df` de `pandas`.

```text
CountryRegionCode                 Name
0                 AF          Afghanistan
1                 AL              Albania
2                 DZ              Algeria
3                 AS       American Samoa
4                 AD              Andorra
5                 AO               Angola
6                 AI             Anguilla
7                 AQ           Antarctica
8                 AG  Antigua and Barbuda
9                 AR            Argentina
10                AM              Armenia
11                AW                Aruba
12                AU            Australia
13                AT              Austria
14                AZ           Azerbaijan
15                BS         Bahamas, The
16                BH              Bahrain
17                BD           Bangladesh
18                BB             Barbados
19                BY              Belarus
20                BE              Belgium
21                BZ               Belize
22                BJ                Benin
23                BM              Bermuda
24                BT               Bhutan
25                BO              Bolivia
```

## <a name="next-steps"></a>Pasos siguientes

+ [Inserción de un dataframe de Python en SQL](../data-exploration/python-dataframe-sql-server.md)