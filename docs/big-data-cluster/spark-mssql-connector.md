---
title: Uso del conector de Apache Spark para SQL Server y Azure SQL
titleSuffix: SQL Server big data clusters
description: Obtenga información sobre cómo usar el conector de Apache Spark para SQL Server y Azure SQL a fin de leer y escribir en SQL Server.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: machine-learning-bdc
ms.openlocfilehash: 29a660452841ee0057485c1a44c8ff30abd6f230
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045595"
---
# <a name="use-the-apache-spark-connector-for-sql-server-and-azure-sql"></a>Uso del conector de Apache Spark para SQL Server y Azure SQL

El [conector de Apache Spark para SQL Server y Azure SQL](https://github.com/microsoft/sql-spark-connector) es un conector de alto rendimiento que le permite usar datos transaccionales en análisis de macrodatos y conservar los resultados para consultas ad hoc o la generación de informes. El conector permite usar cualquier base de datos SQL, local o en la nube, como origen de datos de entrada o receptor de datos de salida para trabajos de Spark. El conector utiliza API de escritura masiva de SQL Server. El usuario puede pasar los parámetros de escritura masiva como parámetros opcionales, y el conector los pasa tal cual a la API subyacente. Para más información sobre las operaciones de escritura masiva, consulte [Uso de la copia masiva con el controlador JDBC]( ../connect/jdbc/using-bulk-copy-with-the-jdbc-driver.md#sqlserverbulkcopyoptions).

El conector se incluye de forma predeterminada en los clústeres de macrodatos de SQL Server.

Más información sobre el conector en el [repositorio de código abierto](https://github.com/microsoft/sql-spark-connector). Puede encontrar ejemplos [aquí](https://github.com/microsoft/sql-spark-connector/tree/master/samples).

## <a name="write-to-a-new-sql-table"></a>Escritura en una nueva tabla de SQL

>[!CAUTION]
> En el modo `overwrite`, el conector descarta primero la tabla si ya existe en la base de datos de forma predeterminada. Use esta opción con cuidado para evitar pérdidas de datos inesperadas.
> 
> Cuando se usa el modo `overwrite` sin la opción `truncate`, al volver a crear la tabla se perderán los índices. Por ejemplo, una tabla de almacén de columnas se convierte en un montón. Si quiere mantener la indexación existente, especifique también la opción `truncate` con el valor `true`. Por ejemplo, `.option("truncate",true)`.

```python
server_name = "jdbc:sqlserver://{SERVER_ADDR}"
database_name = "database_name"
url = server_name + ";" + "databaseName=" + database_name + ";"

table_name = "table_name"
username = "username"
password = "password123!#" # Please specify password here

try:
  df.write \
    .format("com.microsoft.sqlserver.jdbc.spark") \
    .mode("overwrite") \
    .option("url", url) \
    .option("dbtable", table_name) \
    .option("user", username) \
    .option("password", password) \
    .save()
except ValueError as error :
    print("Connector write failed", error)
```

## <a name="append-to-sql-table"></a>Anexión a una tabla SQL
```python
try:
  df.write \
    .format("com.microsoft.sqlserver.jdbc.spark") \
    .mode("append") \
    .option("url", url) \
    .option("dbtable", table_name) \
    .option("user", username) \
    .option("password", password) \
    .save()
except ValueError as error :
    print("Connector write failed", error)
```

## <a name="specify-the-isolation-level"></a>Especificación del nivel de aislamiento

De forma predeterminada, este conector usa el nivel de aislamiento READ_COMMITTED al realizar la inserción masiva en la base de datos. Si quiere reemplazarlo por otro nivel de aislamiento, use la opción `mssqlIsolationLevel` como se muestra a continuación.
```python
    .option("mssqlIsolationLevel", "READ_UNCOMMITTED") \
```

## <a name="read-from-sql-table"></a>Lectura de una tabla SQL

```python
jdbcDF = spark.read \
        .format("com.microsoft.sqlserver.jdbc.spark") \
        .option("url", url) \
        .option("dbtable", table_name) \
        .option("user", username) \
        .option("password", password).load()
```

## <a name="non-active-directory-mode"></a>Modo sin Active Directory

En la seguridad del modo sin Active Directory, cada usuario tiene un nombre de usuario y una contraseña que se deben proporcionar como parámetros durante la creación de instancias del conector para realizar operaciones de lectura o escritura.

A continuación, se muestra un ejemplo de creación de instancias del conector para el modo sin Active Directory: Antes de ejecutar el script, reemplace `?` por el valor de su cuenta.

```python
# Note: '?' is a placeholder for a necessary user-specified value
connector_type = "com.microsoft.sqlserver.jdbc.spark" 

url = "jdbc:sqlserver://master-p-svc;databaseName=?;"
writer = df.write \ 
   .format(connector_type)\ 
   .mode("overwrite") 
   .option("url", url) \ 
   .option("user", ?) \ 
   .option("password",?) 
writer.save() 
```

## <a name="active-directory-mode"></a>Modo de Active Directory

En la seguridad del modo de Active Directory, una vez que un usuario ha generado un archivo keytab, tiene que proporcionar los valores `principal` y `keytab` como parámetros durante la creación de instancias del conector.

En este modo, el controlador carga el archivo keytab en los contenedores del ejecutor correspondiente. Después, los ejecutores usan el nombre de entidad de seguridad y el archivo keytab para generar un token que se utiliza para crear un conector JDBC para lectura y escritura.

A continuación, se muestra un ejemplo de creación de instancias del conector para el modo de Active Directory: Antes de ejecutar el script, reemplace `?` por el valor de su cuenta.

```python
# Note: '?' is a placeholder for a necessary user-specified value
connector_type = "com.microsoft.sqlserver.jdbc.spark"

url = "jdbc:sqlserver://master-p-svc;databaseName=?;integratedSecurity=true;authenticationScheme=JavaKerberos;" 
writer = df.write \ 
   .format(connector_type)\ 
   .mode("overwrite") 
   .option("url", url) \ 
   .option("principal", ?) \ 
   .option("keytab", ?)   

writer.save() 
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre los clústeres de macrodatos, vea [Cómo implementar [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en Kubernetes](deployment-guidance.md).

¿Tiene comentarios o recomendaciones sobre características para los clústeres de macrodatos de SQL Server? [Deje una nota en Comentarios sobre clústeres de macrodatos de SQL Server](https://aka.ms/sql-server-bdc-feedback).
