---
title: Conector de Apache Spark para SQL Server
description: Obtenga información sobre cómo usar el conector de Apache Spark para SQL Server.
ms.custom: ''
ms.date: 08/31/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: rajmera3
ms.author: raajmera
ms.reviewer: mikeray
ms.openlocfilehash: 15a4bdb66ea87f85337540adfec4b5e175b1c55b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058130"
---
# <a name="apache-spark-connector-sql-server--azure-sql"></a>Conector de Apache Spark: SQL Server y Azure SQL

El conector de Apache Spark para SQL Server y Azure SQL es un conector de alto rendimiento que le permite usar datos transaccionales en análisis de macrodatos y conservar los resultados para consultas ad hoc o la generación de informes. El conector permite usar cualquier base de datos SQL, local o en la nube, como origen de datos de entrada o receptor de datos de salida para trabajos de Spark.

Esta biblioteca contiene el código fuente del conector de Apache Spark para SQL Server y Azure SQL.

[Apache Spark](https://spark.apache.org/) es un motor de análisis unificado para el procesamiento de datos a gran escala.

Puede obtener el conector en [Maven](https://search.maven.org/search?q=spark-mssql-connector). Para importar el conector, use la coordenada `com.microsoft.azure:spark-mssql-connector:<latest version>`. También puede compilar el conector desde el origen o descargar el archivo jar de la sección Versión de GitHub. Para obtener la información más reciente sobre el conector, vea el [repositorio de GitHub del conector de Spark para SQL](https://github.com/microsoft/sql-spark-connector).

## <a name="supported-features"></a>Características admitidas

* Compatibilidad con todos los enlaces de Spark (Scala, Python y R)
* Compatibilidad con keytab de Active Directory (AD) y la autenticación básica
* Compatibilidad con la escritura de `dataframe` reordenada
* Compatibilidad con el grupo de datos y la instancia única de SQL Server en clústeres de macrodatos de SQL Server
* Compatibilidad de conectores confiables con una instancia única de SQL Server

| Componente                            | Versiones admitidas              |
|--------------------------------------|---------------------------------|
| Spark de Apache                         | 2.4.5 (Spark 3.0 no compatible) |
| Scala                                | 2.11                            |
| Microsoft JDBC Driver para SQL Server | 8,2                             |
| Microsoft SQL Server                 | SQL Server 2008 o posterior        |
| Azure SQL Database                  | Compatible                       |

> [!NOTE]
> El uso de Azure Synapse Analytics no se ha probado con este conector. Aunque puede funcionar, puede haber consecuencias imprevistas.

### <a name="supported-options"></a>Opciones admitidas
El conector de Apache Spark para SQL Server y Azure SQL admite las opciones definidas aquí: [JDBC para un origen de datos de SQL](https://spark.apache.org/docs/latest/sql-data-sources-jdbc.html)

Además, se admiten estas opciones:

| Opción | Default | Descripción |
| --------- | ------------------ | ------------------------------------------ |
| `reliabilityLevel` | `BEST_EFFORT` | `BEST_EFFORT` o `NO_DUPLICATES`. `NO_DUPLICATES` implementa una inserción confiable en escenarios de reinicio del ejecutor. |
| `dataPoolDataSource` | `none` | `none` implica que el valor no está establecido y el conector debe escribir en una única instancia de SQL Server. Establezca este valor en el nombre del origen de datos para escribir en una tabla de grupo de datos en un clúster de macrodatos.|
| `isolationLevel` | `READ_COMMITTED` | Especifica el nivel de aislamiento. |
| `tableLock` | `false` | Implementa una instrucción insert con la opción `TABLOCK` para mejorar el rendimiento de escritura. |
| `schemaCheckEnabled` | `true` | Deshabilita la trama de datos estricta y la comprobación de esquema de tabla SQL cuando se establece en false. |

Otras [opciones de copia masiva](../jdbc/using-bulk-copy-with-the-jdbc-driver.md#sqlserverbulkcopyoptions) pueden establecerse como opciones en `dataframe` y se pasarán a API `bulkcopy` en la escritura.

## <a name="performance-comparison"></a>Comparación del rendimiento

El conector de Apache Spark para SQL Server y Azure SQL es hasta 15 veces más rápido que el conector JDBC genérico para escribir en SQL Server. Las características de rendimiento varían en cuanto al tipo, el volumen de datos y las opciones utilizadas, y pueden variar entre ejecuciones. Los siguientes resultados de rendimiento son el tiempo empleado para sobrescribir una tabla SQL con 143,9 millones de filas en un elemento `dataframe` de Spark. El elemento `dataframe` de Spark se construye con la lectura de la tabla HDFS `store_sales` generada con el [banco de pruebas TPCDS de Spark](https://github.com/databricks/spark-sql-perf). Se excluye el tiempo para leer `store_sales` en `dataframe`. Los resultados se calculan con el promedio de tres ejecuciones.

| Tipo de conector | Opciones | Descripción |  Tiempo de escritura |
| --------- | ------------------ | -------------------------------------| ---------- |
| `JDBCConnector` | Default | Conector JDBC genérico con opciones predeterminadas |  1385 segundos |
| `sql-spark-connector` | `BEST_EFFORT` | Mejor esfuerzo de `sql-spark-connector` con opciones predeterminadas |580 segundos |
| `sql-spark-connector` | `NO_DUPLICATES ` | `sql-spark-connector` confiable | 709 segundos |
| `sql-spark-connector` | `BEST_EFFORT` + tabLock=true | Mejor esfuerzo de `sql-spark-connector` con el bloqueo de tabla habilitado | 72 segundos |
| `sql-spark-connector` | `NO_DUPLICATES ` + tabLock=true| `sql-spark-connector` confiable con el bloqueo de tabla habilitado| 198 segundos |

Configuración

- Configuración de Spark: num_executors = 20, executor_memory = '1664 m', executor_cores = 2
- Configuración de la generación de datos: scale_factor=50, partitioned_tables=true
- Archivo de datos `store_sales` con un número de filas de 143 997 590

Entorno

- [Clúster de macrodatos de SQL Server](../../big-data-cluster/release-notes-big-data-cluster.md) CU5
- `master` + 6 nodos
- Cada servidor de nodos de generación 5, 512 GB de RAM, NVM de 4 TB por nodo y NIC de 10 GB

## <a name="commonly-faced-issues"></a>Problemas más frecuentes

### `java.lang.NoClassDefFoundError: com/microsoft/aad/adal4j/AuthenticationException`

Este problema se produce al usar una versión anterior del controlador MSSQL (que ahora se incluye en este conector) en el entorno de Hadoop. Si ha usado el conector de Azure SQL anterior y ha instalado manualmente los controladores en dicho clúster para la compatibilidad con Azure Active Directory, necesita quitar esos controladores.

Pasos para corregir el problema:

1. Si usa un entorno de Hadoop genérico, compruebe y quite el archivo jar de MSSQL: `rm $HADOOP_HOME/share/hadoop/yarn/lib/mssql-jdbc-6.2.1.jre7.jar`. Si va a usar Databricks, agregue un script init global o de clúster para quitar las versiones anteriores del controlador mssql de la carpeta `/databricks/jars` o agregue esta línea a un script existente: `rm /databricks/jars/*mssql*`.
2. Agregue los paquetes `adal4j` y `mssql`. Por ejemplo, puede usar Maven, pero debería funcionar cualquier modo. 

   > [!CAUTION]
   > No instale el conector de Spark para SQL de esta forma.

1. Agregue la clase de controlador a la configuración de la conexión. Por ejemplo:

   ```csharp
   connectionProperties = {
     `Driver`: `com.microsoft.sqlserver.jdbc.SQLServerDriver`
   }`
   ```

Para obtener más información y una explicación, consulte la resolución de [https://github.com/microsoft/sql-spark-connector/issues/26](https://github.com/microsoft/sql-spark-connector/issues/26#issuecomment-672006339).

## <a name="get-started"></a>Introducción

El conector de Apache Spark para SQL Server y Azure SQL se basa en DataSourceV1 API de Spark y Bulk API de SQL Server, y usa la misma interfaz que el conector de Spark-SQL integrado de JDBC. Esta integración le permite integrar fácilmente el conector y migrar los trabajos de Spark existentes simplemente actualizando el parámetro de formato con `com.microsoft.sqlserver.jdbc.spark`.

Para incluir el conector en los proyectos, descargue este repositorio y compile el archivo jar con SBT.

## <a name="write-to-a-new-sql-table"></a>Escritura en una nueva tabla SQL

> [!WARNING]
> El modo `overwrite` descarta primero la tabla si ya existe en la base de datos de forma predeterminada. Use esta opción con cuidado para evitar pérdidas de datos inesperadas.

Cuando se usa el modo `overwrite` sin la opción `truncate`, al volver a crear la tabla se perderán los índices. Una tabla de almacén de columnas ahora sería un montón. Si quiere mantener la indexación existente, especifique también la opción `truncate` con el valor true. Por ejemplo, `.option("truncate","true")`.

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

### <a name="append-to-sql-table"></a>Anexión a una tabla SQL

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

## <a name="specify-the-isolation-level"></a>Especifica el nivel de aislamiento.

De forma predeterminada, este conector usa el nivel de aislamiento `READ_COMMITTED` al realizar la inserción masiva en la base de datos. Si quiere reemplazar el nivel de aislamiento, use la opción `mssqlIsolationLevel` como se muestra a continuación.

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

## <a name="azure-active-directory-authentication"></a>Autenticación con Azure Active Directory

### <a name="python-example-with-service-principal"></a>Ejemplo de Python con una entidad de servicio

```python
context = adal.AuthenticationContext(authority)
token = context.acquire_token_with_client_credentials(resource_app_id_url, service_principal_id, service_principal_secret)
access_token = token["accessToken"]

jdbc_db = spark.read \
        .format("com.microsoft.sqlserver.jdbc.spark") \
        .option("url", url) \
        .option("dbtable", table_name) \
        .option("accessToken", access_token) \
        .option("encrypt", "true") \
        .option("hostNameInCertificate", "*.database.windows.net") \
        .load()
```

### <a name="python-example-with-active-directory-password"></a>Ejemplo de Python con la contraseña de Active Directory

```python
jdbc_df = spark.read \
        .format("com.microsoft.sqlserver.jdbc.spark") \
        .option("url", url) \
        .option("dbtable", table_name) \
        .option("authentication", "ActiveDirectoryPassword") \
        .option("user", user_name) \
        .option("password", password) \
        .option("encrypt", "true") \
        .option("hostNameInCertificate", "*.database.windows.net") \
        .load()
```

Se debe instalar una dependencia necesaria para poder autenticarse mediante Active Directory.

Para **Scala**, se debe instalar el artefacto `_com.microsoft.aad.adal4j_`.

Para **Python**, se debe instalar la biblioteca `_adal_`.  Está disponible mediante PIP.

Consulte los [cuadernos de ejemplo](https://github.com/microsoft/sql-spark-connector/tree/master/samples) para obtener ejemplos.

## <a name="support"></a>Soporte técnico

El conector de Apache Spark para Azure SQL y SQL Server es un proyecto de código abierto. Este conector no incluye soporte técnico de Microsoft. Si surgen problemas con el conector o preguntas relacionadas, cree una incidencia en este repositorio del proyecto. La comunidad de conectores está activa y supervisa las consultas.

## <a name="next-steps"></a>Pasos siguientes

Visite el [repositorio de GitHub del conector de Spark para SQL](https://github.com/microsoft/sql-spark-connector).

Para conocer los niveles de aislamiento, vea [SET TRANSACTION ISOLATION LEVEL (Transact-SQL)](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md).
