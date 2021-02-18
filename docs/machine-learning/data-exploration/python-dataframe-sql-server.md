---
title: Insertar un dataframe de Python en una tabla SQL
titleSuffix: SQL machine learning
description: Inserción de datos de un dataframe en una tabla SQL.
author: dphansen
ms.author: davidph
ms.date: 07/23/2020
ms.topic: how-to
ms.prod: sql
ms.technology: machine-learning
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current||=azuresqldb-current'
ms.openlocfilehash: 8d27ae33ab283209725c119ed6d96c4ed511b4ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349116"
---
# <a name="insert-python-dataframe-into-sql-table"></a>Insertar un dataframe de Python en una tabla SQL
[!INCLUDE[SQL Server SQL DB SQL MI](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

En este artículo se describe cómo insertar un dataframe de [Pandas](https://pandas.pydata.org/) en una base de datos SQL mediante el paquete [pyodbc](../../connect/python/pyodbc/python-sql-driver-pyodbc.md) en Python.

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

Para comprobar que la base de datos restaurada existe, consulte la tabla **HumanResources.Department**:

```sql
USE AdventureWorks;
SELECT * FROM HumanResources.Department;
```

## <a name="install-python-packages"></a>Instalación de paquetes de Python

* [Descarga e instalación de Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md)

Instale los siguientes paquetes de Python:
  * pyodbc
  * Pandas

  Para instalar estos paquetes:

  1. En el cuaderno de Azure Data Studio, seleccione **Administrar paquetes**.
  2. En el panel **Administrar paquetes**, seleccione la pestaña **Agregar nuevo**.
  3. Para cada uno de los paquetes siguientes, escriba el nombre del paquete, haga clic en **Buscar** y, a continuación, haga clic en **instalar**.

## <a name="connect-to-sql-server-using-azure-data-studio"></a>Conectarse a SQL Server con Azure Data Studio

[Conexión mediante Azure Data Studio](../../azure-data-studio/quickstart-sql-server.md).

1. Conéctese a la base de datos AdventureWorks para crear la nueva tabla, HumanResources.DepartmentTest. La tabla SQL se utilizará para la inserción de un dataframe.

```sql
CREATE TABLE [HumanResources].[DepartmentTest](
[DepartmentID] [smallint] NOT NULL,
[Name] [dbo].[Name] NOT NULL,
[GroupName] [dbo].[Name] NOT NULL
)
GO
```

## <a name="create-csv-file"></a>Creación de un archivo CSV

Copie el texto y guarde el archivo como department.csv para el dataframe.

```text
DepartmentID,Name,GroupName,
1,Engineering,Research and Development,
2,Tool Design,Research and Development,
3,Sales,Sales and Marketing,
4,Marketing,Sales and Marketing,
5,Purchasing,Inventory Management,
6,Research and Development,Research and Development,
7,Production,Manufacturing,
8,Production Control,Manufacturing,
9,Human Resources,Executive General and Administration,
10,Finance,Executive General and Administration,
11,Information Services,Executive General and Administration,
12,Document Control,Quality Assurance,
13,Quality Assurance,Quality Assurance,
14,Facilities and Maintenance,Executive General and Administration,
15,Shipping and Receiving,Inventory Management,
16,Executive,Executive General and Administration
```

## <a name="connect-to-sql-using-python"></a>Conexión a SQL mediante Python

1. Edite las variables de cadena de conexión "Server", "Database", "Username" y "Password" para conectarse a la base de datos SQL.

2. Edite la ruta de acceso del archivo CSV.

## <a name="load-dataframe-from-csv-file"></a>Carga del dataframe de un archivo CSV

Use el paquete `pandas` de Python para crear un dataframe y cargue el archivo CSV. Conéctese a SQL para cargar un dataframe en la nueva tabla SQL, HumanResources.DepartmentTest.

Para crear un nuevo cuaderno:

1. En Azure Data Studio, seleccione **Archivo** y luego **Nuevo cuaderno**.
2. En el bloc de notas, seleccione el kernel **Python3** y luego el comando **+Código**.
3. Pegue el código en el bloc de notas y seleccione **Ejecutar todo**.

 ```Python
import pyodbc
import pandas as pd
# insert data from csv file into dataframe.
# working directory for csv file: type "pwd" in Azure Data Studio or Linux
# working directory in Windows c:\users\username
df = pd.read_csv("c:\\user\\username\department.csv")
# Some other example server values are
# server = 'localhost\sqlexpress' # for a named instance
# server = 'myserver,port' # to specify an alternate port
server = 'yourservername' 
database = 'AdventureWorks' 
username = 'username' 
password = 'yourpassword' 
cnxn = pyodbc.connect('DRIVER={SQL Server};SERVER='+server+';DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
# Insert Dataframe into SQL Server:
for index, row in df.iterrows():
     cursor.execute("INSERT INTO HumanResources.DepartmentTest (DepartmentID,Name,GroupName) values(?,?,?)", row.DepartmentID, row.Name, row.GroupName)
cnxn.commit()
cursor.close()
```

## <a name="confirm-row-count-in-sql"></a>Confirmación del recuento de filas en SQL

Ejecute la instrucción SQL para confirmar que la tabla se cargó correctamente con los datos del dataframe.

```sql
SELECT count(*) from HumanResources.DepartmentTest;
```

Results

```bash
(No column name)
16
```

## <a name="next-steps"></a>Pasos siguientes

+ [Trazado de un histograma para la exploración de datos con Python](../data-exploration/python-plot-histogram.md)