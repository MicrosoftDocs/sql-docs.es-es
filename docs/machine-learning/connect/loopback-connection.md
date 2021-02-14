---
title: Conexión de bucle invertido de SQL en Python y R
description: Obtenga información sobre cómo usar una conexión de bucle invertido para volver a conectar con SQL Server a través de ODBC a fin de leer o escribir datos de un script de Python o R ejecutado desde sp_execute_external_script.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 08/20/2020
ms.topic: how-to
author: Aniruddh25
ms.author: anmunde
ms.reviewer: dphansen
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 062c62eedaeed16215a538fca8303acb7e88343b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082526"
---
# <a name="loopback-connection-to-sql-server-from-a-python-or-r-script"></a>Conexión de bucle invertido con SQL Server desde un script de Python o R
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Obtenga información sobre cómo usar una conexión de bucle invertido con [Machine Learning Services](../sql-server-machine-learning-services.md) para volver a conectar con SQL  Server a través de [ODBC](../../connect/odbc/microsoft-odbc-driver-for-sql-server.md) a fin de leer o escribir datos de un script de Python o R ejecutado desde [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Se puede utilizar cuando no sea posible usar los argumentos **InputDataSet** y **OutputDataSet** de `sp_execute_external_script`.

## <a name="connection-string"></a>Cadena de conexión

Para crear una conexión de bucle invertido, debe usar una cadena de conexión correcta. Los argumentos obligatorios comunes son el nombre del [controlador ODBC](../../connect/odbc/microsoft-odbc-driver-for-sql-server.md), la dirección del servidor y el nombre de la base de datos.

### <a name="connection-string-on-windows"></a>Cadena de conexión en Windows

Para la autenticación en SQL Server en Windows, el script de Python o R puede usar el atributo de cadena de conexión **Trusted_Connection** para autenticarse como el mismo usuario que ejecutó sp_execute_external_script.

Este es un ejemplo de la cadena de conexión de bucle invertido en Windows:

``` 
"Driver=SQL Server;Server=.;Database=nameOfDatabase;Trusted_Connection=Yes;"
```

### <a name="connection-string-on-linux"></a>Cadena de conexión en Linux

Para la autenticación en SQL Server en Linux, el script de Python o R debe utilizar los atributos **ClientCertificate** y **ClientKey** del controlador ODBC para autenticarse como el mismo usuario que ejecutó `sp_execute_external_script`. Esto requiere el uso del [controlador ODBC más reciente](../../connect/odbc/download-odbc-driver-for-sql-server.md) versión 17.4.1.1.

Este es un ejemplo de la cadena de conexión de bucle invertido en Linux:

```
"Driver=ODBC Driver 17 for SQL Server;Server=fe80::8012:3df5:0:5db1%eth0;Database=nameOfDatabase;ClientCertificate=file:/var/opt/mssql-extensibility/data/baeaac72-60b3-4fae-acfd-c50eff5d34a2/sqlsatellitecert.pem;ClientKey=file:/var/opt/mssql-extensibility/data/baeaac72-60b3-4fae-acfd-c50eff5d34a2/sqlsatellitekey.pem;TrustServerCertificate=Yes;Trusted_Connection=no;Encrypt=Yes"
```

La dirección del servidor, la ubicación del archivo de certificado de cliente y la ubicación del archivo de clave de cliente son únicas para cada `sp_execute_external_script` y se pueden obtener mediante el uso de la API **rx_get_sql_loopback_connection_string()** para Python o **rxGetSqlLoopbackConnectionString()** para R.

Para obtener más información sobre los atributos de la cadena de conexión, consulte [Palabras clave y atributos de DSN y de la cadena de conexión](../../connect/odbc/dsn-connection-string-attribute.md#new-connection-string-keywords-and-connection-attributes) para Microsoft ODBC Driver for SQL Server.

## <a name="generate-connection-string-with-revoscalepy-for-python"></a>Generación de una cadena de conexión con revoscalepy para Python

Puede usar la API **rx_get_sql_loopback_connection_string()** en [revoscalepy](../python/ref-py-revoscalepy.md) con el fin de generar una cadena de conexión correcta para una conexión de bucle invertido en un script de Python.

Acepta los argumentos siguientes:

| Argumento | Descripción |
|-|-|
| name_of_database | Nombre de la base de datos con la que se va a realizar la conexión |
| odbc_driver | Nombre del controlador ODBC |

### <a name="examples"></a>Ejemplos

Ejemplo para SQL Server en Windows:

```sql
EXECUTE sp_execute_external_script
@language = N'Python',
@script = N'
from revoscalepy import rx_get_sql_loopback_connection_string, RxSqlServerData, rx_data_step
loopback_connection_string = rx_get_sql_loopback_connection_string(odbc_driver="SQL Server", name_of_database="DBName")
print("Connection String:{0}".format(loopback_connection_string))
data_set = RxSqlServerData(sql_query = "select col1, col2 from tableName",
                           connection_string = loopback_connection_string)
OutputDataSet = rx_data_step(data_set)
'
WITH RESULT SETS ((col1 int, col2 int))
GO
```

Ejemplo para SQL Server en Linux:

```sql
EXECUTE sp_execute_external_script
@language = N'Python',
@script = N'
from revoscalepy import rx_get_sql_loopback_connection_string, RxSqlServerData, rx_data_step
loopback_connection_string = rx_get_sql_loopback_connection_string(odbc_driver="ODBC Driver 17 for SQL Server",
                                                                   name_of_database="DBName")
print("Loopback Connection String:{0}".format(loopback_connection_string))
data_set = RxSqlServerData(sql_query = "select col1, col2 from tableName",
                           connection_string = loopback_connection_string)
OutputDataSet = rx_data_step(data_set)
'
WITH RESULT SETS ((col1 int, col2 int))
GO
```

## <a name="generate-connection-string-with-revoscaler-for-r"></a>Generación de una cadena de conexión con RevoScaleR para R

Puede usar la API **rxGetSqlLoopbackConnectionString()** en [RevoScaleR](../r/ref-r-revoscaler.md) para generar una cadena de conexión correcta para una conexión de bucle invertido en un script de R.

Acepta los argumentos siguientes:

| Argumento | Descripción |
|-|-|
| nameOfDatabase | Nombre de la base de datos con la que se va a realizar la conexión |
| odbcDriver | Nombre del controlador ODBC |

### <a name="examples"></a>Ejemplos

Ejemplo para SQL Server en Windows:

```sql
EXECUTE sp_execute_external_script
@language = N'R',
@script = N'
    loopbackConnectionString <- rxGetSqlLoopbackConnectionString(nameOfDatabase="DBName", odbcDriver ="SQL Server")
    print(paste("Connection String:", loopbackConnectionString))
    dataSet <- RxSqlServerData(sqlQuery = "select col1, col2 from tableName",
                               connectionString = loopbackConnectionString)
    OutputDataSet <- rxDataStep(dataSet)
'
WITH RESULT SETS ((col1 int, col2 int))
GO
```

Ejemplo para SQL Server en Linux:

```sql
EXECUTE sp_execute_external_script
@language = N'R',
@script = N'
    loopbackConnectionString <-  rxGetSqlLoopbackConnectionString(nameOfDatabase="DBName", 
                                                                  odbcDriver ="ODBC Driver 17 for SQL Server")
    print(paste("Connection String:", loopbackConnectionString))
    dataSet <- RxSqlServerData(sqlQuery = "select col1, col2 from tableName", 
                               connectionString = loopbackConnectionString)
    OutputDataSet <- rxDataStep(dataSet)
'
WITH RESULT SETS ((col1 int, col2 int))
GO
```

## <a name="next-steps"></a>Pasos siguientes

+ [Microsoft ODBC driver for SQL Server](../../connect/odbc/microsoft-odbc-driver-for-sql-server.md)
+ [revoscalepy](../python/ref-py-revoscalepy.md)
+ [RevoScaleR](../r/ref-r-revoscaler.md)
