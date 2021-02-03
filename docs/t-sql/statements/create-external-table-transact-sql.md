---
description: CREATE EXTERNAL TABLE (Transact-SQL)
title: CREATE EXTERNAL TABLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/27/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_EXTERNAL_TABLE
- CREATE EXTERNAL TABLE
- PolyBase, T-SQL
dev_langs:
- TSQL
helpviewer_keywords:
- External
- External, table create
- PolyBase, external table
ms.assetid: 6a6fd8fe-73f5-4639-9908-2279031abdec
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5348c8af5912cbabcc0d094a0f727c5f2d1a7c39
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190994"
---
# <a name="create-external-table-transact-sql"></a>CREATE EXTERNAL TABLE (Transact-SQL)

Crea una tabla externa

En este artículo se proporciona la sintaxis, argumentos, comentarios, permisos y ejemplos para cualquier producto SQL que elija.

Para obtener más información sobre las convenciones de sintaxis, vea [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL Database](create-external-table-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-external-table-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-external-table-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-sql-server"></a>Introducción: SQL Server

Este comando crea una tabla externa para PolyBase con el fin de acceder a los datos almacenados en un clúster de Hadoop o una tabla externa de PolyBase en Azure Blob Storage en la que se hace referencia a datos almacenados en un clúster de Hadoop o en Azure Blob Storage.

**SE APLICA A**: SQL Server 2016 (o posterior)

Usa una tabla externa con un origen de datos externo para consultas de PolyBase. Los orígenes de datos externos se usan para establecer la conectividad y admiten estos casos de uso principales:

- Virtualización y carga de datos mediante [PolyBase](../../relational-databases/polybase/polybase-guide.md)
- Operaciones de carga masiva mediante SQL Server o SQL Database utilizando `BULK INSERT` o `OPENROWSET`

Vea también [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) y [DROP EXTERNAL TABLE](../../t-sql/statements/drop-external-table-transact-sql.md).

## <a name="syntax"></a>Sintaxis

```syntaxsql
-- Create a new external table
CREATE EXTERNAL TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( <column_definition> [ ,...n ] )
    WITH (
        LOCATION = 'folder_or_filepath',
        DATA_SOURCE = external_data_source_name,
        FILE_FORMAT = external_file_format_name
        [ , <reject_options> [ ,...n ] ]
    )
[;]

<reject_options> ::=
{
    | REJECT_TYPE = value | percentage
    | REJECT_VALUE = reject_value
    | REJECT_SAMPLE_VALUE = reject_sample_value
}

<column_definition> ::=
column_name <data_type>
    [ COLLATE collation_name ]
    [ NULL | NOT NULL ]
```

## <a name="arguments"></a>Argumentos

*{ nombre_base_de_datos.nombre_de_esquema.nombre_de_tabla | nombre_de_esquema.nombre_de_tabla | nombre_de_tabla }* El nombre de uno a tres elementos de la tabla que se va a crear. En una tabla externa, SQL solo almacena los metadatos de tabla junto con estadísticas básicas sobre el archivo o carpeta a los que se hace referencia en Hadoop o Azure Blob Storage. Ningún dato real se mueve o se almacena en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

> [!IMPORTANT]
> Para obtener el mejor rendimiento, si el controlador de origen de datos externo admite un nombre de tres partes, se recomienda encarecidamente que lo proporcione.  

\<column_definition> [ ,...*n* ] CREATE EXTERNAL TABLE admite la capacidad de configurar el nombre de columna, el tipo de datos, la nulabilidad y la intercalación. No se puede usar DEFAULT CONSTRAINT en tablas externas.

Las definiciones de columna, incluidos los tipos de datos y el número de columnas, deben coincidir con los datos de los archivos externos. Si hay algún error de coincidencia, se rechazarán las filas de archivo al consultar los datos reales.

LOCATION = "*carpeta_o_ruta_de_archivo*" Especifica la carpeta o la ruta de archivo y el nombre de archivo de los datos reales en Hadoop o Azure Blob Storage. La ubicación empieza desde la carpeta raíz. La carpeta raíz es la ubicación de datos especificada en el origen de datos externo.

En SQL Server, la instrucción CREATE EXTERNAL TABLE crea la ruta de acceso y la carpeta si todavía no existen. Luego puede usar INSERT INTO para exportar datos de una tabla de SQL Server local al origen de datos externo. Para obtener más información, vea [Escenarios de consulta de PolyBase](../../relational-databases/polybase/polybase-queries.md).

Si se especifica LOCATION para que sea una carpeta, una consulta de PolyBase que seleccione en la tabla externa recuperará los archivos de la carpeta y todas sus subcarpetas. Al igual que Hadoop, PolyBase no devuelve carpetas ocultas. Tampoco devuelve los archivos cuyo nombre comienza con un carácter de subrayado (_) o un punto (.).

En este ejemplo, si LOCATION='/webdata/', una consulta de PolyBase devolverá filas de mydata.txt y mydata2.txt. No devolverá mydata3.txt porque es una archivo en una carpeta oculta. Y no devolverá _hidden.txt porque es un archivo oculto.

![Datos recursivos para tablas externas](../../t-sql/statements/media/aps-polybase-folder-traversal.png "Datos recursivos para tablas externas")

Para cambiar el valor predeterminado y leer solo de la carpeta raíz, establezca el atributo \<polybase.recursive.traversal> en "false" en el archivo de configuración core-site.xml. Este archivo se encuentra en `<SqlBinRoot>\PolyBase\Hadoop\Conf with SqlBinRoot the bin root of SQl Server`. Por ejemplo, `C:\\Program Files\\Microsoft SQL Server\\MSSQL13.XD14\\MSSQL\\Binn`.

DATA_SOURCE = *nombre_del_origen_de_datos_externo* Especifica el nombre del origen de datos externo que contiene la ubicación de los datos externos. Esta ubicación es un sistema de archivos de Hadoop (HDFS), un contenedor de blobs de Azure Storage o Azure Data Lake Store. Para crear un origen de datos externo, use[CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

FILE_FORMAT = *nombre_formato_de_archivo_externo* Especifica el nombre del objeto de formato de archivo externo que almacena el tipo de archivo y el método de compresión de los datos externos. Para crear un formato de archivo externo, use [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

Opciones de Reject Puede especificar parámetros de Reject que determinen cómo va a administrar PolyBase los registros *desfasados* que recupera del origen de datos externo. Un registro de datos se considera "desfasado" si los tipos de datos reales o el número de columnas no coinciden con las definiciones de columna de la tabla externa.

Si no se especifican ni se cambian los valores de Reject, PolyBase usa los valores predeterminados. Esta información sobre los parámetros de Reject se almacena como metadatos adicionales al crear una tabla externa con la instrucción CREATE EXTERNAL TABLE. Cuando una futura instrucción SELECT o SELECT INTO SELECT selecciona datos de la tabla externa, PolyBase usa las opciones de rechazo para determinar el número o porcentaje de filas que se pueden rechazar antes de que se produzca un error en la consulta real. La consulta devolverá resultados (parciales) hasta que se supere el umbral de rechazo. Después, se produce un error con el mensaje de error correspondiente.

REJECT_TYPE = **valor** | porcentaje Aclara si la opción REJECT_VALUE se especifica como un valor literal o como un porcentaje.

valor REJECT_VALUE es un valor literal, no un porcentaje. Si el número de filas rechazadas supera el valor *reject_value*, se produce un error en la consulta de PolyBase.

Por ejemplo, si REJECT_VALUE = 5 y REJECT_TYPE = value, se producirá un error en la consulta SELECT de PolyBase después de que se hayan rechazado cinco filas.

porcentaje REJECT_VALUE es un porcentaje, no un valor literal. Si el *porcentaje* de filas con errores supera el valor *reject_value*, se produce un error en la consulta de PolyBase. El porcentaje de filas con errores se calcula a intervalos.

REJECT_VALUE = *valor_de_rechazo* Especifica el valor o el porcentaje de filas que se pueden rechazar antes de que se produzca un error en la consulta.

Para REJECT_TYPE = value, *reject_value* debe ser un entero comprendido entre 0 y 2.147.483.647.

Para REJECT_TYPE = percentage, *reject_value* debe ser un valor float comprendido entre 0 y 100.

REJECT_SAMPLE_VALUE = *valor_de_muestra_de_rechazo* Este atributo es necesario cuando se especifica REJECT_TYPE = percentage. Determina el número de filas que se intentan recuperar antes de que PolyBase vuelva a calcular el porcentaje de filas rechazadas.

El parámetro *reject_sample_value* debe ser un entero comprendido entre 0 y 2.147.483.647.

Por ejemplo, si REJECT_SAMPLE_VALUE = 1000, PolyBase calcula el porcentaje de filas con errores después de haber intentado importar 1000 filas desde el archivo de datos externos. Si el porcentaje de filas con errores es inferior al valor de *reject_value*, PolyBase intenta recuperar otras 1000 filas. Sigue recalculando el porcentaje de filas con errores después de intentar importar cada 1000 filas más.

> [!NOTE]
> Puesto que PolyBase calcula el porcentaje de filas con errores a intervalos, el porcentaje real de filas con errores puede superar el valor de *reject_value*.

Ejemplo:

En este ejemplo se muestra cómo interactúan entre sí las tres opciones REJECT. Por ejemplo, si REJECT_TYPE = percentage, REJECT_VALUE = 30 y REJECT_SAMPLE_VALUE = 100, sucederá lo siguiente:

- PolyBase intenta recuperar las 100 primeras filas; 25 no se importan y 75 sí.
- El porcentaje de las filas con errores se calcula en un 25 %, que es menor que el valor de rechazo de 30 %. Por tanto, PolyBase seguirá recuperando datos del origen de datos externo.
- PolyBase intenta cargar las siguientes 100 filas; esta vez, 25 lo hacen y 75 no.
- El porcentaje de filas con errores se recalcula en un 50 %. El porcentaje de filas con errores supera pues el valor de rechazo de 30 %.
- Se produce un error en la consulta de PolyBase con un 50 % de filas rechazadas después de intentar devolver las 200 primeras filas. Tenga en cuenta que se han devuelto filas coincidentes antes de que la consulta de PolyBase detecte que se ha superado el umbral de rechazo.

SCHEMA_NAME La cláusula SCHEMA_NAME ofrece la posibilidad de asignar la definición de tabla externa a una tabla de otro esquema en la base de datos remota. Use esta cláusula para eliminar la ambigüedad entre los esquemas existentes en las bases de datos local y remota.

OBJECT_NAME La cláusula OBJECT_NAME ofrece la posibilidad de asignar la definición de tabla externa a una tabla con otro nombre en la base de datos remota. Use esta cláusula para eliminar la ambigüedad entre los nombres de objeto que hay en las bases de datos local y remota.

DISTRIBUTION es opcional. Este argumento solo es obligatorio para bases de datos de tipo SHARD_MAP_MANAGER. Este argumento controla si una tabla se trata como una tabla con particiones o una tabla replicada. Con las tablas **SHARDED** (*nombre de columna*), los datos de las distintas tablas no se superponen. **REPLICATED** especifica que las tablas tienen los mismos datos en cada partición. **ROUND_ROBIN** indica que se usa un método específico de la aplicación para distribuir los datos.

## <a name="permissions"></a>Permisos

Requiere estos permisos de usuario:

- **CREATE TABLE**
- **ALTER ANY SCHEMA**
- **ALTER ANY EXTERNAL DATA SOURCE**
- **ALTER ANY EXTERNAL FILE FORMAT**
- **CONTROL DATABASE**

Tenga en cuenta que el inicio de sesión que crea el origen de datos externo debe tener permiso para leer y escribir en el origen de datos externo, ubicado en Hadoop o Azure Blob Storage.

> [!IMPORTANT]
> El permiso ALTER ANY EXTERNAL DATA SOURCE concede a cualquier entidad de seguridad la capacidad de crear y modificar cualquier objeto de origen de datos externo y, por tanto, también permite acceder a todas las credenciales con ámbito de base de datos de la base de datos. Debe considerarse como un permiso con muchos privilegios, por lo que solo debe concederse a las entidades de seguridad de confianza del sistema.

## <a name="error-handling"></a>Tratamiento de errores

Al ejecutar la instrucción CREATE EXTERNAL TABLE, PolyBase intenta conectarse al origen de datos externo. Si se produce un error en el intento de conexión, se producirá un error en la instrucción y no se creará la tabla externa. El error en el comando puede tardar un minuto o más en producirse, ya que PolyBase vuelve a intentar conectar hasta que al final da como errónea la consulta.

## <a name="general-remarks"></a>Notas generales

En escenarios de consulta "ad-hoc", como SELECT FROM EXTERNAL TABLE, PolyBase almacena en una tabla temporal las filas que se recuperan del origen de datos externo. Una vez finalizada la consulta, PolyBase quita y elimina la tabla temporal. En las tablas SQL no se almacenan datos permanentes.

En cambio, en el escenario de importación, como SELECT INTO FROM EXTERNAL TABLE, PolyBase almacena las filas que se recuperan del origen de datos externo como datos permanentes en la tabla SQL. La tabla se crea durante la ejecución de la consulta cuando PolyBase recupera los datos externos.

PolyBase puede insertar algunos de los cálculos de la consulta en Hadoop para mejorar el rendimiento. Esta acción se denomina "aplicación de predicado". Para habilitarla, especifique la opción de ubicación del administrador de recursos de Hadoop en [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

Puede crear varias tablas externas que hagan referencia a los mismos orígenes de datos externos o a otros.

## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones

Puesto que los datos de una tabla externa no están bajo el control de administración directo de SQL Server, un proceso externo puede modificarlos o quitarlos en cualquier momento. Como resultado, no se garantiza que los resultados de la consulta sobre una tabla externa sean deterministas. La misma consulta puede devolver resultados diferentes cada vez que se ejecute en una tabla externa. Del mismo modo, es posible que se produzca un error en una consulta si los datos externos se mueven o se quitan.

Puede crear varias tablas externas que hagan referencia a diferentes orígenes de datos externos. Si ejecuta consultas de forma simultánea sobre otros orígenes de datos de Hadoop, cada uno tendrá que usar el mismo valor de configuración de servidor "hadoop connectivity". Por ejemplo, no se puede ejecutar simultáneamente una consulta en un clúster de Cloudera Hadoop y un clúster de Hortonworks Hadoop, ya que usan valores de configuración diferentes. Para obtener los valores de configuración y las combinaciones admitidas, vea [Configuración de conectividad de PolyBase](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md).

En tablas externas solo se admiten estas instrucciones de lenguaje de definición de datos (DDL):

- CREATE TABLE y DROP TABLE
- CREATE STATISTICS y DROP STATISTICS
- CREATE VIEW y DROP VIEW

Construcciones y operaciones no admitidas:

- Restricción DEFAULT en columnas de tabla externa
- Operaciones de lenguaje de manipulación de datos (DML) delete, insert y update

### <a name="query-limitations"></a>Limitaciones de las consultas

PolyBase puede consumir un máximo de 33 000 archivos por carpeta cuando se ejecutan 32 consultas simultáneas de PolyBase. Esta cifra máxima engloba los archivos y las subcarpetas de cada carpeta de HDFS. Si el grado de simultaneidad es inferior a 32, un usuario puede ejecutar consultas de PolyBase en carpetas de HDFS que contengan más de 33 000 archivos. Se recomienda tener unas rutas de acceso de archivos externos cortas y no usar más de 30.000 archivos por carpeta de HDFS. Si hay referencias a demasiados archivos, podría producirse una excepción de memoria insuficiente de Máquina virtual Java (JVM).

### <a name="table-width-limitations"></a>Limitaciones de ancho de tabla

PolyBase en SQL Server 2016 tiene un límite de ancho de fila de 32 KB, en función del tamaño máximo de una sola fila válida por definición de tabla. Si la suma del esquema de columnas es superior a 32 KB, PolyBase no puede consultar los datos.

### <a name="data-type-limitations"></a>Limitaciones de tipos de datos

Los tipos de datos siguientes no se pueden usar en las tablas externas de PolyBase:

- `geography`
- `geometry`
- `hierarchyid`
- `image`
- `text`
- `nText`
- `xml`
- Cualquier tipo definido por el usuario

## <a name="locking"></a>Bloqueo

Bloqueo compartido en el objeto SCHEMARESOLUTION.

## <a name="security"></a>Seguridad

Los archivos de datos de una tabla externa se almacenan en Hadoop o Azure Blob Storage. Son sus propios procesos los que crean y administran estos archivos de datos. Es responsabilidad suya administrar la seguridad de los datos externos.

## <a name="examples"></a>Ejemplos

### <a name="a-create-an-external-table-with-data-in-text-delimited-format"></a>A. Crear una tabla externa con datos en formato delimitado por texto

En este ejemplo se muestran todos los pasos necesarios para crear una tabla externa que tenga datos con formato de archivos delimitados por texto. Se define un origen de datos externo *mydatasource* y un formato de archivo externo *myfileformat*. Luego se hace referencia a estos objetos de nivel de base de datos en la instrucción CREATE EXTERNAL TABLE. Para más información, vea [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) y [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

```sql
CREATE EXTERNAL DATA SOURCE mydatasource
WITH (
    TYPE = HADOOP,
    LOCATION = 'hdfs://xxx.xxx.xxx.xxx:8020'
)

CREATE EXTERNAL FILE FORMAT myfileformat
WITH (
    FORMAT_TYPE = DELIMITEDTEXT,
    FORMAT_OPTIONS (FIELD_TERMINATOR ='|')
);

CREATE EXTERNAL TABLE ClickStream (
    url varchar(50),
    event_date date,
    user_IP varchar(50)
)
WITH (
        LOCATION='/webdata/employee.tbl',
        DATA_SOURCE = mydatasource,
        FILE_FORMAT = myfileformat
    )
;
```

### <a name="b-create-an-external-table-with-data-in-rcfile-format"></a>B. Crear una tabla externa con datos en formato RCFile

En este ejemplo se muestran todos los pasos necesarios para crear una tabla externa que tenga datos con formato de RCFiles. Se define un origen de datos externo *mydatasource_rc* y un formato de archivo externo *myfileformat_rc*. Luego se hace referencia a estos objetos de nivel de base de datos en la instrucción CREATE EXTERNAL TABLE. Para más información, vea [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) y [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

```sql
CREATE EXTERNAL DATA SOURCE mydatasource_rc
WITH (
    TYPE = HADOOP,
    LOCATION = 'hdfs://xxx.xxx.xxx.xxx:8020'
)

CREATE EXTERNAL FILE FORMAT myfileformat_rc
WITH (
    FORMAT_TYPE = RCFILE,
    SERDE_METHOD = 'org.apache.hadoop.hive.serde2.columnar.LazyBinaryColumnarSerDe'
)
;

CREATE EXTERNAL TABLE ClickStream_rc (
    url varchar(50),
    event_date date,
    user_ip varchar(50)
)
WITH (
        LOCATION='/webdata/employee_rc.tbl',
        DATA_SOURCE = mydatasource_rc,
        FILE_FORMAT = myfileformat_rc
    )
;
```

### <a name="c-create-an-external-table-with-data-in-orc-format"></a>C. Crear una tabla externa con datos en formato ORC

En este ejemplo se muestran todos los pasos necesarios para crear una tabla externa que tenga datos con formato de archivos ORC. Se define un origen de datos externo mydatasource_orc y un formato de archivo externo myfileformat_orc. Luego se hace referencia a estos objetos de nivel de base de datos en la instrucción CREATE EXTERNAL TABLE. Para más información, vea [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) y [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

```sql
CREATE EXTERNAL DATA SOURCE mydatasource_orc
WITH (
    TYPE = HADOOP,
    LOCATION = 'hdfs://xxx.xxx.xxx.xxx:8020'
)

CREATE EXTERNAL FILE FORMAT myfileformat_orc
WITH (
    FORMAT = ORC,
    COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'
)
;

CREATE EXTERNAL TABLE ClickStream_orc (
    url varchar(50),
    event_date date,
    user_ip varchar(50)
)
WITH (
        LOCATION='/webdata/',
        DATA_SOURCE = mydatasource_orc,
        FILE_FORMAT = myfileformat_orc
    )
;
```

### <a name="d-querying-hadoop-data"></a>D. Consultar datos de Hadoop

Clickstream es una tabla externa que se conecta con el archivo de texto delimitado employee.tbl en un clúster de Hadoop. La consulta siguiente parece una consulta en una tabla estándar, pero recupera datos de Hadoop y luego calcula los resultados.

```sql
SELECT TOP 10 (url) FROM ClickStream WHERE user_ip = 'xxx.xxx.xxx.xxx'
;
```

### <a name="e-join-hadoop-data-with-sql-data"></a>E. Combinar datos de Hadoop con datos SQL

Esta consulta parece una operación JOIN estándar en dos tablas SQL. La diferencia es que PolyBase recupera los datos de Clickstream de Hadoop y luego los combina con la tabla UrlDescription. Una tabla es una tabla externa y la otra es una tabla SQL estándar.

```sql
SELECT url.description
FROM ClickStream cs
JOIN UrlDescription url ON cs.url = url.name
WHERE cs.url = 'msdn.microsoft.com'
;
```

### <a name="f-import-data-from-hadoop-into-a-sql-table"></a>F. Importar datos de Hadoop en una tabla SQL

En este ejemplo se crea una nueva tabla SQL ms_user que almacena de forma permanente el resultado de una combinación entre la tabla SQL estándar *user* y la tabla externa *ClickStream*.

```sql
SELECT DISTINCT user.FirstName, user.LastName
INTO ms_user
FROM user INNER JOIN (
    SELECT * FROM ClickStream WHERE cs.url = 'www.microsoft.com'
    ) AS ms
ON user.user_ip = ms.user_ip
;
```

### <a name="g-create-an-external-table-for-a-sharded-data-source"></a>G. Crear una tabla externa para un origen de datos con particiones

En este ejemplo se vuelve a asignar una DMV remota a una tabla externa mediante las cláusulas SCHEMA_NAME y OBJECT_NAME.

```sql
CREATE EXTERNAL TABLE [dbo].[all_dm_exec_requests]([session_id] smallint NOT NULL,
  [request_id] int NOT NULL,
  [start_time] datetime NOT NULL,
  [status] nvarchar(30) NOT NULL,
  [command] nvarchar(32) NOT NULL,
  [sql_handle] varbinary(64),
  [statement_start_offset] int,
  [statement_end_offset] int,
  [cpu_time] int NOT NULL)
WITH
(
  DATA_SOURCE = MyExtSrc,
  SCHEMA_NAME = 'sys',
  OBJECT_NAME = 'dm_exec_requests',
  DISTRIBUTION=ROUND_ROBIN
);
```

### <a name="h-create-an-external-table-for-sql-server"></a>H. Crear una tabla externa para SQL Server

```sql
     -- Create a Master Key
      CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo';
    GO
     /*  specify credentials to external data source
     *  IDENTITY: user name for external source.
     *  SECRET: password for external source.
     */
     CREATE DATABASE SCOPED CREDENTIAL SqlServerCredentials
     WITH IDENTITY = 'username', Secret = 'password';
    GO

    /* LOCATION: Location string should be of format '<vendor>://<server>[:<port>]'.
    * PUSHDOWN: specify whether computation should be pushed down to the source. ON by default.
    * CREDENTIAL: the database scoped credential, created above.
    */
    CREATE EXTERNAL DATA SOURCE SQLServerInstance
    WITH (
    LOCATION = 'sqlserver://SqlServer',
    -- PUSHDOWN = ON | OFF,
      CREDENTIAL = SQLServerCredentials
    );
    GO

    CREATE SCHEMA sqlserver;
    GO

     /* LOCATION: sql server table/view in 'database_name.schema_name.object_name' format
     * DATA_SOURCE: the external data source, created above.
     */
     CREATE EXTERNAL TABLE sqlserver.customer(
     C_CUSTKEY INT NOT NULL,
     C_NAME VARCHAR(25) NOT NULL,
     C_ADDRESS VARCHAR(40) NOT NULL,
     C_NATIONKEY INT NOT NULL,
     C_PHONE CHAR(15) NOT NULL,
     C_ACCTBAL DECIMAL(15,2) NOT NULL,
     C_MKTSEGMENT CHAR(10) NOT NULL,
     C_COMMENT VARCHAR(117) NOT NULL
      )
      WITH (
      LOCATION='tpch_10.dbo.customer',
      DATA_SOURCE=SqlServerInstance
     );
```

### <a name="i-create-an-external-table-for-oracle"></a>I. Crear una tabla externa para Oracle

```sql
  -- Create a Master Key
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
   /*
   * Specify credentials to external data source
   * IDENTITY: user name for external source.
   * SECRET: password for external source.
   */
   CREATE DATABASE SCOPED CREDENTIAL credential_name
   WITH IDENTITY = 'username', Secret = 'password';

   /*
   * LOCATION: Location string should be of format '<vendor>://<server>[:<port>]'.
   * PUSHDOWN: specify whether computation should be pushed down to the source. ON by default.
   * CONNECTION_OPTIONS: Specify driver location
   * CREDENTIAL: the database scoped credential, created above.
   */
   CREATE EXTERNAL DATA SOURCE external_data_source_name
   WITH (
     LOCATION = 'oracle://<server address>[:<port>]',
     -- PUSHDOWN = ON | OFF,
     CREDENTIAL = credential_name)

   /*
   * LOCATION: Oracle table/view in '.<schema_name>.<object_name>' format
   * DATA_SOURCE: the external data source, created above.
   */
   CREATE EXTERNAL TABLE customers(
   [O_ORDERKEY] DECIMAL(38) NOT NULL,
   [O_CUSTKEY] DECIMAL(38) NOT NULL,
   [O_ORDERSTATUS] CHAR COLLATE Latin1_General_BIN NOT NULL,
   [O_TOTALPRICE] DECIMAL(15,2) NOT NULL,
   [O_ORDERDATE] DATETIME2(0) NOT NULL,
   [O_ORDERPRIORITY] CHAR(15) COLLATE Latin1_General_BIN NOT NULL,
   [O_CLERK] CHAR(15) COLLATE Latin1_General_BIN NOT NULL,
   [O_SHIPPRIORITY] DECIMAL(38) NOT NULL,
   [O_COMMENT] VARCHAR(79) COLLATE Latin1_General_BIN NOT NULL
   )
   WITH (
    LOCATION='.mySchema.customer',
    DATA_SOURCE= external_data_source_name
   );
```

### <a name="j-create-an-external-table-for-teradata"></a>J. Crear una tabla externa para Teradata

```sql
  -- Create a Master Key
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';

   /*
   * Specify credentials to external data source
   * IDENTITY: user name for external source.
   * SECRET: password for external source.
   */
   CREATE DATABASE SCOPED CREDENTIAL credential_name
   WITH IDENTITY = 'username', Secret = 'password';

    /* LOCATION: Location string should be of format '<vendor>://<server>[:<port>]'.
    * PUSHDOWN: specify whether computation should be pushed down to the source. ON by default.
    * CONNECTION_OPTIONS: Specify driver location
    * CREDENTIAL: the database scoped credential, created above.
    */
    CREATE EXTERNAL DATA SOURCE external_data_source_name
    WITH (
    LOCATION = teradata://<server address>[:<port>],
   -- PUSHDOWN = ON | OFF,
    CREDENTIAL =credential_name
    );


     /* LOCATION: Teradata table/view in '<database_name>.<object_name>' format
      * DATA_SOURCE: the external data source, created above.
      */
     CREATE EXTERNAL TABLE customer(
      L_ORDERKEY INT NOT NULL,
      L_PARTKEY INT NOT NULL,
     L_SUPPKEY INT NOT NULL,
     L_LINENUMBER INT NOT NULL,
     L_QUANTITY DECIMAL(15,2) NOT NULL,
     L_EXTENDEDPRICE DECIMAL(15,2) NOT NULL,
     L_DISCOUNT DECIMAL(15,2) NOT NULL,
     L_TAX DECIMAL(15,2) NOT NULL,
     L_RETURNFLAG CHAR NOT NULL,
     L_LINESTATUS CHAR NOT NULL,
     L_SHIPDATE DATE NOT NULL,
     L_COMMITDATE DATE NOT NULL,
     L_RECEIPTDATE DATE NOT NULL,
     L_SHIPINSTRUCT CHAR(25) NOT NULL,
     L_SHIPMODE CHAR(10) NOT NULL,
     L_COMMENT VARCHAR(44) NOT NULL
     )
     WITH (
     LOCATION='customer',
     DATA_SOURCE= external_data_source_name
     );
```

### <a name="k-create-an-external-table-for-mongodb"></a>K. Crear una tabla externa para MongoDB

```sql
  -- Create a Master Key
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';

   /*
   * Specify credentials to external data source
   * IDENTITY: user name for external source.
   * SECRET: password for external source.
   */
   CREATE DATABASE SCOPED CREDENTIAL credential_name
   WITH IDENTITY = 'username', Secret = 'password';

     /* LOCATION: Location string should be of format '<type>://<server>[:<port>]'.
    * PUSHDOWN: specify whether computation should be pushed down to the source. ON by default.
    * CONNECTION_OPTIONS: Specify driver location
    * CREDENTIAL: the database scoped credential, created above.
    */
    CREATE EXTERNAL DATA SOURCE external_data_source_name
    WITH (
    LOCATION = mongodb://<server>[:<port>],
    -- PUSHDOWN = ON | OFF,
      CREDENTIAL = credential_name
    );

     /* LOCATION: MongoDB table/view in '<database_name>.<schema_name>.<object_name>' format
     * DATA_SOURCE: the external data source, created above.
     */
     CREATE EXTERNAL TABLE customers(
     [O_ORDERKEY] DECIMAL(38) NOT NULL,
     [O_CUSTKEY] DECIMAL(38) NOT NULL,
     [O_ORDERSTATUS] CHAR COLLATE Latin1_General_BIN NOT NULL,
     [O_TOTALPRICE] DECIMAL(15,2) NOT NULL,
     [O_ORDERDATE] DATETIME2(0) NOT NULL,
     [O_COMMENT] VARCHAR(79) COLLATE Latin1_General_BIN NOT NULL
     )
     WITH (
     LOCATION='customer',
     DATA_SOURCE= external_data_source_name
     );
```

## <a name="see-also"></a>Consulte también

- [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md)
- [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](create-external-table-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Database \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-external-table-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-external-table-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-azure-sql-database"></a>Introducción: Azure SQL Database

En Azure SQL Database, crea una tabla externa para [consultas elásticas (en versión preliminar)](/azure/sql-database/sql-database-elastic-query-overview/).


Vea también [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

## <a name="syntax"></a>Sintaxis

```syntaxsql
-- Create a table for use with elastic query  
CREATE EXTERNAL TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( <column_definition> [ ,...n ] )  
    WITH ( <sharded_external_table_options> )  
[;]  

<column_definition> ::=
column_name <data_type>
    [ COLLATE collation_name ]
    [ NULL | NOT NULL ]
  
<sharded_external_table_options> ::=  
        DATA_SOURCE = external_data_source_name,
        SCHEMA_NAME = N'nonescaped_schema_name',  
        OBJECT_NAME = N'nonescaped_object_name',  
        [DISTRIBUTION  = SHARDED(sharding_column_name) | REPLICATED | ROUND_ROBIN]]  
    )  
[;]  
```

## <a name="arguments"></a>Argumentos

*{ nombre_base_de_datos.nombre_de_esquema.nombre_de_tabla | nombre_de_esquema.nombre_de_tabla | nombre_de_tabla }* El nombre de uno a tres elementos de la tabla que se va a crear. En una tabla externa, SQL solo almacena los metadatos de tabla junto con estadísticas básicas sobre el archivo o la carpeta a los que se hace referencia en Azure SQL Database. Ningún dato real se mueve o se almacena en Azure SQL Database.

> [!IMPORTANT]
> Para obtener el mejor rendimiento, si el controlador de origen de datos externo admite un nombre de tres partes, se recomienda encarecidamente que lo proporcione.

\<column_definition> [ ,...*n* ] CREATE EXTERNAL TABLE admite la capacidad de configurar el nombre de columna, el tipo de datos, la nulabilidad y la intercalación. No se puede usar DEFAULT CONSTRAINT en tablas externas.

> [!NOTE]
> `Text`, `nText` y `XML` no son tipos de datos admitidos para las columnas de tablas externas para Azure SQL Database.

Las definiciones de columna, incluidos los tipos de datos y el número de columnas, deben coincidir con los datos de los archivos externos. Si hay algún error de coincidencia, se rechazarán las filas de archivo al consultar los datos reales.

Sharded external table options

Especifica el origen de datos externo (un origen de datos no SQL Server) y un método de distribución para la [consulta elástica](/azure/azure-sql/database/elastic-query-overview).

DATA_SOURCE La cláusula DATA_SOURCE define el origen de datos externo (un mapa de particiones) que se usa para la tabla externa. Para obtener un ejemplo, vea [Creación de tablas externas](/azure/sql-database/sql-database-elastic-query-horizontal-partitioning#13-create-external-tables).

SCHEMA_NAME y OBJECT_NAME Las cláusulas SCHEMA_NAME y OBJECT_NAME asignan la definición de tabla externa a una tabla en otro esquema. Si se omite, se considera que el esquema del objeto remoto es "dbo" y que su nombre es idéntico al de la tabla externa que se define. Esto es útil si el nombre de la tabla remota ya existe en la base de datos donde desea crear la tabla externa. Por ejemplo, quiere definir una tabla externa para obtener una vista agregada de las vistas de catálogo o DMV en la capa de datos con escala horizontal. Puesto que las vistas de catálogo y DMV ya existen localmente, no se pueden usar sus nombres para la definición de la tabla externa. En su lugar, use otro nombre y el nombre de la vista de catálogo o la DMV en las cláusulas SCHEMA_NAME u OBJECT_NAME. Para obtener un ejemplo, vea [Creación de tablas externas](/azure/sql-database/sql-database-elastic-query-horizontal-partitioning#13-create-external-tables).

DISTRIBUTION La cláusula DISTRIBUTION especifica la distribución de datos que se usa en esta tabla. El procesador de consultas usa la información proporcionada en la cláusula DISTRIBUTION para crear los planes de consulta más eficaces.

- SHARDED significa que los datos se han particionado horizontalmente en la base de datos. La clave de creación de particiones para la distribución de datos es el parámetro <sharding_column_name>.
- REPLICATED significa que copias idénticas de la tabla están presentes en cada base de datos. Es responsabilidad suya asegurarse de que las réplicas son idénticas en las bases de datos.
- ROUND_ROBIN significa que la tabla tiene particiones horizontales mediante un método de distribución que depende de la aplicación.

## <a name="permissions"></a>Permisos

Los usuarios con acceso a la tabla externa obtienen automáticamente acceso a las tablas remotas subyacentes con la credencial proporcionada en la definición del origen de datos externo. Evite la elevación no deseada de privilegios a través de la credencial del origen de datos externo. Use GRANT o REVOKE para una tabla externa como si fuera una tabla normal. Una vez que defina el origen de datos externo y las tablas externas, puede usar el T-SQL completo en las tablas externas.

## <a name="error-handling"></a>Tratamiento de errores

Mientras se ejecuta la instrucción CREATE EXTERNAL TABLE, si se produce un error en el intento de conexión, se producirá un error en la instrucción y no se creará la tabla externa. El error en el comando puede tardar un minuto o más en producirse, ya que SQL Database reintenta la conexión antes de que se produzca un error en la consulta.

## <a name="general-remarks"></a>Notas generales

En escenarios de consulta "ad-hoc", como SELECT FROM EXTERNAL TABLE, SQL Database almacena en una tabla temporal las filas que se recuperan del origen de datos externo. Una vez que ha finalizado la consulta, SQL Database quita y elimina la tabla temporal. En las tablas SQL no se almacenan datos permanentes.

En cambio, en el escenario de importación, como SELECT INTO FROM EXTERNAL TABLE, SQL Database almacena las filas que se recuperan del origen de datos externo como datos permanentes en la tabla SQL. La tabla se crea durante la ejecución de la consulta cuando SQL Database recupera los datos externos.

Puede crear varias tablas externas que hagan referencia a los mismos orígenes de datos externos o a otros.

## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones

El acceso a los datos a través de una tabla externa no se adhiere a la semántica de aislamiento en SQL Server. Esto significa que la consulta de un origen externo no impone ningún aislamiento de instantánea o bloqueo, por lo que los datos devueltos pueden cambiar si los datos del origen de datos externo cambian.  La misma consulta puede devolver resultados diferentes cada vez que se ejecute en una tabla externa. Del mismo modo, es posible que se produzca un error en una consulta si los datos externos se mueven o se quitan.

Puede crear varias tablas externas que hagan referencia a diferentes orígenes de datos externos.

En tablas externas solo se admiten estas instrucciones de lenguaje de definición de datos (DDL):

- CREATE TABLE y DROP TABLE
- CREATE VIEW y DROP VIEW

Construcciones y operaciones no admitidas:

- Restricción DEFAULT en columnas de tabla externa
- Operaciones de lenguaje de manipulación de datos (DML) delete, insert y update

Solo los predicados literales definidos en una consulta se pueden insertar en el origen de datos externo. Esto es diferente de los servidores vinculados y el acceso donde se pueden usar los predicados que se determinan durante la ejecución de la consulta, es decir, cuando se usan en conjunto con un bucle anidado en un plan de consulta. Con frecuencia, la tabla externa completa se copia localmente y, después, se produce la unión.

```sql
  \\ Assuming External.Orders is an external table and Customer is a local table.
  \\ This query  will copy the whole of the external locally as the predicate needed
  \\ to filter isn't known at compile time. Its only known during execution of the query
  
  SELECT Orders.OrderId, Orders.OrderTotal
    FROM External.Orders
   WHERE CustomerId in (SELECT TOP 1 CustomerId
                          FROM Customer
                          WHERE CustomerName = 'MyCompany')
```

El uso de tablas externas evita el uso de paralelismo en el plan de consulta.

Las tablas externas se implementan como una consulta remota y, como tal, el número estimado de filas devueltas es generalmente 1000; hay otras reglas basadas en el tipo de predicado que se usan para filtrar la tabla externa. Son estimaciones basadas en reglas en lugar de estimaciones basadas en los datos reales de la tabla externa. El optimizador no accede al origen de datos remoto para obtener una estimación más precisa.

### <a name="data-type-limitations"></a>Limitaciones de tipos de datos

Los tipos de datos siguientes no se pueden usar en las tablas externas de PolyBase:

- `geography`
- `geometry`
- `hierarchyid`
- `image`
- `text`
- `nText`
- `xml`
- Cualquier tipo definido por el usuario

## <a name="locking"></a>Bloqueo

Bloqueo compartido en el objeto SCHEMARESOLUTION.

## <a name="examples"></a>Ejemplos

### <a name="a-create-external-table-for-azure-sql-database"></a>A. Crear tabla externa para Azure SQL Database

```sql
CREATE EXTERNAL TABLE [dbo].[CustomerInformation]
( [CustomerID] [int] NOT NULL,
  [CustomerName] [varchar](50) NOT NULL,
  [Company] [varchar](50) NOT NULL)
WITH
( DATA_SOURCE = MyElasticDBQueryDataSrc)
```

## <a name="see-also"></a>Consulte también

- [Información general sobre las consultas elásticas de Azure SQL Database](/azure/sql-database/sql-database-elastic-query-overview)
- [Informes de bases de datos escaladas horizontalmente en la nube](/azure/sql-database/sql-database-elastic-query-horizontal-partitioning)
- [Introducción a las consultas entre bases de datos (particiones verticales)](/azure/sql-database/sql-database-elastic-query-getting-started-vertical)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](create-external-table-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Database](create-external-table-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-external-table-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-azure-synapse-analytics"></a>Introducción: Azure Synapse Analytics

Use una tabla externa para:

- Consultar datos de Hadoop o Azure Blob Storage con instrucciones de [!INCLUDE[tsql](../../includes/tsql-md.md)].
- Importar y almacenar datos de Hadoop o Azure Blob Storage.
- Importar y almacenar datos de Azure Data Lake Store.

Vea también [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) y [DROP EXTERNAL TABLE](../../t-sql/statements/drop-external-table-transact-sql.md).  

## <a name="syntax"></a>Sintaxis

### [[!INCLUDE[sss-dedicated-pool-md.md](../../includes/sss-dedicated-pool-md.md)]](#tab/dedicated)
```syntaxsql
CREATE EXTERNAL TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( <column_definition> [ ,...n ] )  
    WITH (
        LOCATION = 'hdfs_folder_or_filepath',  
        DATA_SOURCE = external_data_source_name,  
        FILE_FORMAT = external_file_format_name  
        [ , <reject_options> [ ,...n ] ]  
    )  
[;]  

<column_definition> ::=
column_name <data_type>
    [ COLLATE collation_name ]
    [ NULL | NOT NULL ]
  
<reject_options> ::=  
{  
    | REJECT_TYPE = value | percentage,  
    | REJECT_VALUE = reject_value,  
    | REJECT_SAMPLE_VALUE = reject_sample_value,
    | REJECTED_ROW_LOCATION = '/REJECT_Directory'
}  
```
### [[!INCLUDE[sssod-md.md](../../includes/sssod-md.md)]](#tab/serverless)
```syntaxsql
CREATE EXTERNAL TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name } 
    ( <column_definition> [ ,...n ] )   
    WITH ( 
        LOCATION = 'folder_or_filepath',   
        DATA_SOURCE = external_data_source_name,   
        FILE_FORMAT = external_file_format_name   
    )   
[;]   
<column_definition> ::= 
column_name <data_type> 
    [ COLLATE collation_name ] 
```
---

## <a name="arguments"></a>Argumentos

*{ nombre_base_de_datos.nombre_de_esquema.nombre_de_tabla | nombre_de_esquema.nombre_de_tabla | nombre_de_tabla }* El nombre de uno a tres elementos de la tabla que se va a crear. En una tabla externa, solo los metadatos de tabla junto con estadísticas básicas sobre el archivo o la carpeta a los que se hace referencia en Azure Data Lake, Hadoop o Azure Blob Storage. No se mueven ni almacenan datos reales cuando se crean tablas externas.

> [!IMPORTANT]
> Para obtener el mejor rendimiento, si el controlador de origen de datos externo admite un nombre de tres partes, se recomienda encarecidamente que lo proporcione.

\<column_definition> [ ,...*n* ] CREATE EXTERNAL TABLE admite la capacidad de configurar el nombre de columna, el tipo de datos, la nulabilidad y la intercalación. No se puede usar DEFAULT CONSTRAINT en tablas externas.

> [!NOTE]
> `Text`, `nText` y `XML` no son tipos de datos admitidos para las columnas de tablas externas para Azure SQL Warehouse.

Las definiciones de columna, incluidos los tipos de datos y el número de columnas, deben coincidir con los datos de los archivos externos. Si hay algún error de coincidencia, se rechazarán las filas de archivo al consultar los datos reales.

LOCATION = "*carpeta_o_ruta_de_archivo*" Especifica la carpeta o la ruta de archivo y el nombre de archivo de los datos reales en Azure Data Lake, Hadoop o Azure Blob Storage. La ubicación empieza desde la carpeta raíz. La carpeta raíz es la ubicación de datos especificada en el origen de datos externo. La instrucción [CREATE EXTERNAL TABLE AS SELECT](create-external-table-as-select-transact-sql.md) crea la ruta de acceso y la carpeta si no existen. `CREATE EXTERNAL TABLE` no crea la ruta de acceso y la carpeta.

Si se especifica LOCATION para que sea una carpeta, una consulta de PolyBase que seleccione en la tabla externa recuperará los archivos de la carpeta y todas sus subcarpetas. Al igual que Hadoop, PolyBase no devuelve carpetas ocultas. Tampoco devuelve los archivos cuyo nombre comienza con un carácter de subrayado (_) o un punto (.).

En este ejemplo, si LOCATION='/webdata/', una consulta de PolyBase devolverá filas de mydata.txt y mydata2.txt. No devolverá mydata3.txt porque es una subcarpeta de una carpeta oculta. Y no devolverá _hidden.txt porque es un archivo oculto.

![Datos recursivos para tablas externas](../../t-sql/statements/media/aps-polybase-folder-traversal.png "Datos recursivos para tablas externas")

DATA_SOURCE = *nombre_del_origen_de_datos_externo* Especifica el nombre del origen de datos externo que contiene la ubicación de los datos externos. Esta ubicación se encuentra en Azure Data Lake. Para crear un origen de datos externo, use[CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

FILE_FORMAT = *nombre_formato_de_archivo_externo* Especifica el nombre del objeto de formato de archivo externo que almacena el tipo de archivo y el método de compresión de los datos externos. Para crear un formato de archivo externo, use [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

Opciones de Reject Puede especificar parámetros de Reject que determinen cómo va a administrar PolyBase los registros *desfasados* que recupera del origen de datos externo. Un registro de datos se considera "desfasado" si los tipos de datos reales o el número de columnas no coinciden con las definiciones de columna de la tabla externa.

Si no se especifican ni se cambian los valores de Reject, PolyBase usa los valores predeterminados. Esta información sobre los parámetros de Reject se almacena como metadatos adicionales al crear una tabla externa con la instrucción CREATE EXTERNAL TABLE. Cuando una futura instrucción SELECT o SELECT INTO SELECT selecciona datos de la tabla externa, PolyBase usa las opciones de rechazo para determinar el número o porcentaje de filas que se pueden rechazar antes de que se produzca un error en la consulta real. La consulta devolverá resultados (parciales) hasta que se supere el umbral de rechazo. Después, se produce un error con el mensaje de error correspondiente.

REJECT_TYPE = **valor** | porcentaje Aclara si la opción REJECT_VALUE se especifica como un valor literal o como un porcentaje.

valor REJECT_VALUE es un valor literal, no un porcentaje. Si el número de filas rechazadas supera el valor *reject_value*, se produce un error en la consulta de PolyBase.

Por ejemplo, si REJECT_VALUE = 5 y REJECT_TYPE = value, se producirá un error en la consulta SELECT de PolyBase después de que se hayan rechazado cinco filas.

porcentaje REJECT_VALUE es un porcentaje, no un valor literal. Si el *porcentaje* de filas con errores supera el valor *reject_value*, se produce un error en la consulta de PolyBase. El porcentaje de filas con errores se calcula a intervalos.

REJECT_VALUE = *valor_de_rechazo* Especifica el valor o el porcentaje de filas que se pueden rechazar antes de que se produzca un error en la consulta.

Para REJECT_TYPE = value, *reject_value* debe ser un entero comprendido entre 0 y 2.147.483.647.

Para REJECT_TYPE = percentage, *reject_value* debe ser un valor float comprendido entre 0 y 100.

REJECT_SAMPLE_VALUE = *valor_de_muestra_de_rechazo* Este atributo es necesario cuando se especifica REJECT_TYPE = percentage. Determina el número de filas que se intentan recuperar antes de que PolyBase vuelva a calcular el porcentaje de filas rechazadas.

El parámetro *reject_sample_value* debe ser un entero comprendido entre 0 y 2.147.483.647.

Por ejemplo, si REJECT_SAMPLE_VALUE = 1000, PolyBase calcula el porcentaje de filas con errores después de haber intentado importar 1000 filas desde el archivo de datos externos. Si el porcentaje de filas con errores es inferior al valor de *reject_value*, PolyBase intenta recuperar otras 1000 filas. Sigue recalculando el porcentaje de filas con errores después de intentar importar cada 1000 filas más.

> [!NOTE]
> Puesto que PolyBase calcula el porcentaje de filas con errores a intervalos, el porcentaje real de filas con errores puede superar el valor de *reject_value*.

Ejemplo:

En este ejemplo se muestra cómo interactúan entre sí las tres opciones REJECT. Por ejemplo, si REJECT_TYPE = percentage, REJECT_VALUE = 30 y REJECT_SAMPLE_VALUE = 100, sucederá lo siguiente:

- PolyBase intenta recuperar las 100 primeras filas; 25 no se importan y 75 sí.
- El porcentaje de las filas con errores se calcula en un 25 %, que es menor que el valor de rechazo de 30 %. Por tanto, PolyBase seguirá recuperando datos del origen de datos externo.
- PolyBase intenta cargar las siguientes 100 filas; esta vez, 25 lo hacen y 75 no.
- El porcentaje de filas con errores se recalcula en un 50 %. El porcentaje de filas con errores supera pues el valor de rechazo de 30 %.
- Se produce un error en la consulta de PolyBase con un 50 % de filas rechazadas después de intentar devolver las 200 primeras filas. Tenga en cuenta que se han devuelto filas coincidentes antes de que la consulta de PolyBase detecte que se ha superado el umbral de rechazo.

REJECTED_ROW_LOCATION = *Ubicación del directorio*

Especifica el directorio del origen de datos externo en el que se deben escribir las filas rechazadas y el archivo de errores correspondiente.
Si la ruta de acceso especificada no existe, PolyBase creará una en su nombre. Se crea un directorio secundario con el nombre “\_rejectedrows”. El carácter “\_” garantiza que se escape el directorio para otro procesamiento de datos, a menos que se mencione explícitamente en el parámetro de ubicación. En este directorio hay una carpeta que se crea según la hora de envío de la carga con el formato AñoMesDía-HoraMinutoSegundo (por ejemplo, 20180330-173205). En esta carpeta se escriben dos tipos de archivos: el archivo _reason y el archivo de datos.

Los archivos reason y los archivos de datos tienen el identificador de consulta asociado a la instrucción CTAS. Como los datos y los archivos reason están en archivos independientes, los archivos correspondientes tienen un sufijo coincidente.

## <a name="permissions"></a>Permisos

Requiere estos permisos de usuario:

- **CREATE TABLE**
- **ALTER ANY SCHEMA**
- **ALTER ANY EXTERNAL DATA SOURCE**
- **ALTER ANY EXTERNAL FILE FORMAT**

> [!NOTE]
> Se necesitan permisos CONTROL DATABASE para crear solo los valores MASTER KEY, DATABASE SCOPED CREDENTIAL y EXTERNAL DATA SOURCE

Tenga en cuenta que el inicio de sesión que crea el origen de datos externo debe tener permiso para leer y escribir en el origen de datos externo, ubicado en Hadoop o Azure Blob Storage.

> [!IMPORTANT]
> El permiso ALTER ANY EXTERNAL DATA SOURCE concede a cualquier entidad de seguridad la capacidad de crear y modificar cualquier objeto de origen de datos externo y, por tanto, también permite acceder a todas las credenciales con ámbito de base de datos de la base de datos. Debe considerarse como un permiso con muchos privilegios, por lo que solo debe concederse a las entidades de seguridad de confianza del sistema.

## <a name="error-handling"></a>Tratamiento de errores

Al ejecutar la instrucción CREATE EXTERNAL TABLE, PolyBase intenta conectarse al origen de datos externo. Si se produce un error en el intento de conexión, se producirá un error en la instrucción y no se creará la tabla externa. El error en el comando puede tardar un minuto o más en producirse, ya que PolyBase vuelve a intentar conectar hasta que al final da como errónea la consulta.

## <a name="general-remarks"></a>Notas generales

En escenarios de consulta "ad-hoc", como SELECT FROM EXTERNAL TABLE, PolyBase almacena en una tabla temporal las filas que se recuperan del origen de datos externo. Una vez finalizada la consulta, PolyBase quita y elimina la tabla temporal. En las tablas SQL no se almacenan datos permanentes.

En cambio, en el escenario de importación, como SELECT INTO FROM EXTERNAL TABLE, PolyBase almacena las filas que se recuperan del origen de datos externo como datos permanentes en la tabla SQL. La tabla se crea durante la ejecución de la consulta cuando PolyBase recupera los datos externos.

PolyBase puede insertar algunos de los cálculos de la consulta en Hadoop para mejorar el rendimiento. Esta acción se denomina "aplicación de predicado". Para habilitarla, especifique la opción de ubicación del administrador de recursos de Hadoop en [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

Puede crear varias tablas externas que hagan referencia a los mismos orígenes de datos externos o a otros.

## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones

Como los datos de una tabla externa no están bajo el control de administración directo de Azure Synapse, un proceso externo puede modificarlos o quitarlos en cualquier momento. Como resultado, no se garantiza que los resultados de la consulta sobre una tabla externa sean deterministas. La misma consulta puede devolver resultados diferentes cada vez que se ejecute en una tabla externa. Del mismo modo, es posible que se produzca un error en una consulta si los datos externos se mueven o se quitan.

Puede crear varias tablas externas que hagan referencia a diferentes orígenes de datos externos.

En tablas externas solo se admiten estas instrucciones de lenguaje de definición de datos (DDL):

- CREATE TABLE y DROP TABLE
- CREATE STATISTICS y DROP STATISTICS
- CREATE VIEW y DROP VIEW

Construcciones y operaciones no admitidas:

- Restricción DEFAULT en columnas de tabla externa
- Operaciones de lenguaje de manipulación de datos (DML) delete, insert y update

### <a name="query-limitations"></a>Limitaciones de las consultas

Se recomienda no superar un máximo de 30 000 archivos por carpeta. Si hay referencias a demasiados archivos, podría producirse una excepción de memoria insuficiente de Máquina virtual Java (JVM), o el rendimiento podría verse degradado.

### <a name="table-width-limitations"></a>Limitaciones de ancho de tabla

PolyBase en Azure Data Warehouse tiene un límite de ancho de fila de 1 MB, en función del tamaño máximo de una sola fila válida por definición de tabla. Si la suma del esquema de columnas es superior a 1 MB, PolyBase no puede consultar los datos.

### <a name="data-type-limitations"></a>Limitaciones de tipos de datos

Los tipos de datos siguientes no se pueden usar en las tablas externas de PolyBase:

- `geography`
- `geometry`
- `hierarchyid`
- `image`
- `text`
- `nText`
- `xml`
- Cualquier tipo definido por el usuario

## <a name="locking"></a>Bloqueo

Bloqueo compartido en el objeto SCHEMARESOLUTION.

## <a name="examples"></a>Ejemplos

### <a name="a-importing-data-from-adls-gen-2-into-azure-ssdw"></a>A. Importación de datos de ADLS Gen 2 en Azure [!INCLUDE[ssDW](../../includes/ssdw-md.md)]. 

Para ver ejemplos de ADLS Gen 1, consulte [Crear origen de datos externos](create-external-data-source-transact-sql.md).

```sql

-- These values come from your Azure Active Directory Application used to authenticate to ADLS Gen 2. 
CREATE DATABASE SCOPED CREDENTIAL ADLUser
WITH IDENTITY = '<clientID>@\<OAuth2.0TokenEndPoint>',
SECRET = '<KEY>' ;

CREATE EXTERNAL DATA SOURCE AzureDataLakeStore
WITH (TYPE = HADOOP,
      LOCATION = 'abfss://data@pbasetr.azuredatalakestore.net'
)

CREATE EXTERNAL FILE FORMAT TextFileFormat
WITH
(
    FORMAT_TYPE = DELIMITEDTEXT
    , FORMAT_OPTIONS ( FIELD_TERMINATOR = '|'
       , STRING_DELIMITER = ''
      , DATE_FORMAT = 'yyyy-MM-dd HH:mm:ss.fff'
      , USE_TYPE_DEFAULT = FALSE
      )
)

CREATE EXTERNAL TABLE [dbo].[DimProductexternal]
( [ProductKey] [int] NOT NULL,
  [ProductLabel] nvarchar NULL,
  [ProductName] nvarchar NULL )
WITH
(
    LOCATION='/DimProduct/' ,
    DATA_SOURCE = AzureDataLakeStore ,
    FILE_FORMAT = TextFileFormat ,
    REJECT_TYPE = VALUE ,
    REJECT_VALUE = 0
) ;

CREATE TABLE [dbo].[DimProduct]
WITH (DISTRIBUTION = HASH([ProductKey] ) )
AS SELECT * FROM
[dbo].[DimProduct_external] ;
```

## <a name="see-also"></a>Consulte también

- [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md)
- [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md)
- [CREATE EXTERNAL TABLE AS SELECT](../../t-sql/statements/create-external-table-as-select-transact-sql.md)
- [CREATE TABLE AS SELECT &#40;Azure Synapse Analytics&#41;](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md)

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](create-external-table-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Database](create-external-table-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-external-table-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Analytics<br />Platform System (PDW) \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="overview-analytics-platform-system"></a>Introducción: Sistema de la plataforma de análisis

Use una tabla externa para:

- Consultar datos de Hadoop o Azure Blob Storage con instrucciones de [!INCLUDE[tsql](../../includes/tsql-md.md)].
- Importar datos de Hadoop o Azure Blob Storage y almacenarlos en Analytics Platform System.

Vea también [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) y [DROP EXTERNAL TABLE](../../t-sql/statements/drop-external-table-transact-sql.md).

## <a name="syntax"></a>Sintaxis

```syntaxsql
CREATE EXTERNAL TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( <column_definition> [ ,...n ] )  
    WITH (
        LOCATION = 'hdfs_folder_or_filepath',  
        DATA_SOURCE = external_data_source_name,  
        FILE_FORMAT = external_file_format_name  
        [ , <reject_options> [ ,...n ] ]  
    )  
[;]  

<column_definition> ::=
column_name <data_type>
    [ COLLATE collation_name ]
    [ NULL | NOT NULL ]

<reject_options> ::=  
{  
    | REJECT_TYPE = value | percentage,  
    | REJECT_VALUE = reject_value,  
    | REJECT_SAMPLE_VALUE = reject_sample_value,
  
}  
```

## <a name="arguments"></a>Argumentos

*{ nombre_base_de_datos.nombre_de_esquema.nombre_de_tabla | nombre_de_esquema.nombre_de_tabla | nombre_de_tabla }* El nombre de uno a tres elementos de la tabla que se va a crear. En una tabla externa, Analytics Platform System solo almacena los metadatos de tabla junto con estadísticas básicas sobre el archivo o carpeta a los que se hace referencia en Hadoop o Azure Blob Storage. Ningún dato real se mueve o se almacena en Analytics Platform System.

> [!IMPORTANT]
> Para obtener el mejor rendimiento, si el controlador de origen de datos externo admite un nombre de tres partes, se recomienda encarecidamente que lo proporcione.

\<column_definition> [ ,...*n* ] CREATE EXTERNAL TABLE admite la capacidad de configurar el nombre de columna, el tipo de datos, la nulabilidad y la intercalación. No se puede usar DEFAULT CONSTRAINT en tablas externas.

Las definiciones de columna, incluidos los tipos de datos y el número de columnas, deben coincidir con los datos de los archivos externos. Si hay algún error de coincidencia, se rechazarán las filas de archivo al consultar los datos reales.

LOCATION = "*carpeta_o_ruta_de_archivo*" Especifica la carpeta o la ruta de archivo y el nombre de archivo de los datos reales en Hadoop o Azure Blob Storage. La ubicación empieza desde la carpeta raíz. La carpeta raíz es la ubicación de datos especificada en el origen de datos externo.

En Analytics Platform System, la instrucción [CREATE EXTERNAL TABLE AS SELECT](create-external-table-as-select-transact-sql.md) crea la ruta de acceso y la carpeta si no existen. `CREATE EXTERNAL TABLE` no crea la ruta de acceso y la carpeta.

Si se especifica LOCATION para que sea una carpeta, una consulta de PolyBase que seleccione en la tabla externa recuperará los archivos de la carpeta y todas sus subcarpetas. Al igual que Hadoop, PolyBase no devuelve carpetas ocultas. Tampoco devuelve los archivos cuyo nombre comienza con un carácter de subrayado (_) o un punto (.).

En este ejemplo, si LOCATION='/webdata/', una consulta de PolyBase devolverá filas de mydata.txt y mydata2.txt. No devolverá mydata3.txt porque es una subcarpeta de una carpeta oculta. Y no devolverá _hidden.txt porque es un archivo oculto.

![Datos recursivos para tablas externas](../../t-sql/statements/media/aps-polybase-folder-traversal.png "Datos recursivos para tablas externas")

Para cambiar el valor predeterminado y leer solo de la carpeta raíz, establezca el atributo \<polybase.recursive.traversal> en "false" en el archivo de configuración core-site.xml. Este archivo se encuentra en `<SqlBinRoot>\PolyBase\Hadoop\Conf with SqlBinRoot the bin root of SQl Server`. Por ejemplo, `C:\\Program Files\\Microsoft SQL Server\\MSSQL13.XD14\\MSSQL\\Binn`.

DATA_SOURCE = *nombre_del_origen_de_datos_externo* Especifica el nombre del origen de datos externo que contiene la ubicación de los datos externos. Esta ubicación es Hadoop o Azure Blob Storage. Para crear un origen de datos externo, use[CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

FILE_FORMAT = *nombre_formato_de_archivo_externo* Especifica el nombre del objeto de formato de archivo externo que almacena el tipo de archivo y el método de compresión de los datos externos. Para crear un formato de archivo externo, use [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

Opciones de Reject Puede especificar parámetros de Reject que determinen cómo va a administrar PolyBase los registros *desfasados* que recupera del origen de datos externo. Un registro de datos se considera "desfasado" si los tipos de datos reales o el número de columnas no coinciden con las definiciones de columna de la tabla externa.

Si no se especifican ni se cambian los valores de Reject, PolyBase usa los valores predeterminados. Esta información sobre los parámetros de Reject se almacena como metadatos adicionales al crear una tabla externa con la instrucción CREATE EXTERNAL TABLE. Cuando una futura instrucción SELECT o SELECT INTO SELECT selecciona datos de la tabla externa, PolyBase usa las opciones de rechazo para determinar el número o porcentaje de filas que se pueden rechazar antes de que se produzca un error en la consulta real. La consulta devolverá resultados (parciales) hasta que se supere el umbral de rechazo. Después, se produce un error con el mensaje de error correspondiente.

REJECT_TYPE = **valor** | porcentaje Aclara si la opción REJECT_VALUE se especifica como un valor literal o como un porcentaje.

valor REJECT_VALUE es un valor literal, no un porcentaje. Si el número de filas rechazadas supera el valor *reject_value*, se produce un error en la consulta de PolyBase.

Por ejemplo, si REJECT_VALUE = 5 y REJECT_TYPE = value, se producirá un error en la consulta SELECT de PolyBase después de que se hayan rechazado cinco filas.

porcentaje REJECT_VALUE es un porcentaje, no un valor literal. Si el *porcentaje* de filas con errores supera el valor *reject_value*, se produce un error en la consulta de PolyBase. El porcentaje de filas con errores se calcula a intervalos.

REJECT_VALUE = *valor_de_rechazo* Especifica el valor o el porcentaje de filas que se pueden rechazar antes de que se produzca un error en la consulta.

Para REJECT_TYPE = value, *reject_value* debe ser un entero comprendido entre 0 y 2.147.483.647.

Para REJECT_TYPE = percentage, *reject_value* debe ser un valor float comprendido entre 0 y 100.

REJECT_SAMPLE_VALUE = *valor_de_muestra_de_rechazo* Este atributo es necesario cuando se especifica REJECT_TYPE = percentage. Determina el número de filas que se intentan recuperar antes de que PolyBase vuelva a calcular el porcentaje de filas rechazadas.

El parámetro *reject_sample_value* debe ser un entero comprendido entre 0 y 2.147.483.647.

Por ejemplo, si REJECT_SAMPLE_VALUE = 1000, PolyBase calcula el porcentaje de filas con errores después de haber intentado importar 1000 filas desde el archivo de datos externos. Si el porcentaje de filas con errores es inferior al valor de *reject_value*, PolyBase intenta recuperar otras 1000 filas. Sigue recalculando el porcentaje de filas con errores después de intentar importar cada 1000 filas más.

> [!NOTE]
> Puesto que PolyBase calcula el porcentaje de filas con errores a intervalos, el porcentaje real de filas con errores puede superar el valor de *reject_value*.

Ejemplo:

En este ejemplo se muestra cómo interactúan entre sí las tres opciones REJECT. Por ejemplo, si REJECT_TYPE = percentage, REJECT_VALUE = 30 y REJECT_SAMPLE_VALUE = 100, sucederá lo siguiente:

- PolyBase intenta recuperar las 100 primeras filas; 25 no se importan y 75 sí.
- El porcentaje de las filas con errores se calcula en un 25 %, que es menor que el valor de rechazo de 30 %. Por tanto, PolyBase seguirá recuperando datos del origen de datos externo.
- PolyBase intenta cargar las siguientes 100 filas; esta vez, 25 lo hacen y 75 no.
- El porcentaje de filas con errores se recalcula en un 50 %. El porcentaje de filas con errores supera pues el valor de rechazo de 30 %.
- Se produce un error en la consulta de PolyBase con un 50 % de filas rechazadas después de intentar devolver las 200 primeras filas. Tenga en cuenta que se han devuelto filas coincidentes antes de que la consulta de PolyBase detecte que se ha superado el umbral de rechazo.

## <a name="permissions"></a>Permisos

Requiere estos permisos de usuario:

- **CREATE TABLE**
- **ALTER ANY SCHEMA**
- **ALTER ANY EXTERNAL DATA SOURCE**
- **ALTER ANY EXTERNAL FILE FORMAT**
- **CONTROL DATABASE**

Tenga en cuenta que el inicio de sesión que crea el origen de datos externo debe tener permiso para leer y escribir en el origen de datos externo, ubicado en Hadoop o Azure Blob Storage.

> [!IMPORTANT]
> El permiso ALTER ANY EXTERNAL DATA SOURCE concede a cualquier entidad de seguridad la capacidad de crear y modificar cualquier objeto de origen de datos externo y, por tanto, también permite acceder a todas las credenciales con ámbito de base de datos de la base de datos. Debe considerarse como un permiso con muchos privilegios, por lo que solo debe concederse a las entidades de seguridad de confianza del sistema.

## <a name="error-handling"></a>Tratamiento de errores

Al ejecutar la instrucción CREATE EXTERNAL TABLE, PolyBase intenta conectarse al origen de datos externo. Si se produce un error en el intento de conexión, se producirá un error en la instrucción y no se creará la tabla externa. El error en el comando puede tardar un minuto o más en producirse, ya que PolyBase vuelve a intentar conectar hasta que al final da como errónea la consulta.

## <a name="general-remarks"></a>Notas generales

En escenarios de consulta "ad-hoc", como SELECT FROM EXTERNAL TABLE, PolyBase almacena en una tabla temporal las filas que se recuperan del origen de datos externo. Una vez finalizada la consulta, PolyBase quita y elimina la tabla temporal. En las tablas SQL no se almacenan datos permanentes.

En cambio, en el escenario de importación, como SELECT INTO FROM EXTERNAL TABLE, PolyBase almacena las filas que se recuperan del origen de datos externo como datos permanentes en la tabla SQL. La tabla se crea durante la ejecución de la consulta cuando PolyBase recupera los datos externos.

PolyBase puede insertar algunos de los cálculos de la consulta en Hadoop para mejorar el rendimiento. Esta acción se denomina "aplicación de predicado". Para habilitarla, especifique la opción de ubicación del administrador de recursos de Hadoop en [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

Puede crear varias tablas externas que hagan referencia a los mismos orígenes de datos externos o a otros.

## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones

Como los datos de una tabla externa no están bajo el control de administración directo del dispositivo, un proceso externo puede modificarlos o quitarlos en cualquier momento. Como resultado, no se garantiza que los resultados de la consulta sobre una tabla externa sean deterministas. La misma consulta puede devolver resultados diferentes cada vez que se ejecute en una tabla externa. Del mismo modo, es posible que se produzca un error en una consulta si los datos externos se mueven o se quitan.

Puede crear varias tablas externas que hagan referencia a diferentes orígenes de datos externos. Si ejecuta consultas de forma simultánea sobre otros orígenes de datos de Hadoop, cada uno tendrá que usar el mismo valor de configuración de servidor "hadoop connectivity". Por ejemplo, no se puede ejecutar simultáneamente una consulta en un clúster de Cloudera Hadoop y un clúster de Hortonworks Hadoop, ya que usan valores de configuración diferentes. Para obtener los valores de configuración y las combinaciones admitidas, vea [Configuración de conectividad de PolyBase](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md).

En tablas externas solo se admiten estas instrucciones de lenguaje de definición de datos (DDL):

- CREATE TABLE y DROP TABLE
- CREATE STATISTICS y DROP STATISTICS
- CREATE VIEW y DROP VIEW

Construcciones y operaciones no admitidas:

- Restricción DEFAULT en columnas de tabla externa
- Operaciones de lenguaje de manipulación de datos (DML) delete, insert y update

### <a name="query-limitations"></a>Limitaciones de las consultas

PolyBase puede consumir un máximo de 33 000 archivos por carpeta cuando se ejecutan 32 consultas simultáneas de PolyBase. Esta cifra máxima engloba los archivos y las subcarpetas de cada carpeta de HDFS. Si el grado de simultaneidad es inferior a 32, un usuario puede ejecutar consultas de PolyBase en carpetas de HDFS que contengan más de 33 000 archivos. Se recomienda tener unas rutas de acceso de archivos externos cortas y no usar más de 30.000 archivos por carpeta de HDFS. Si hay referencias a demasiados archivos, podría producirse una excepción de memoria insuficiente de Máquina virtual Java (JVM).

### <a name="table-width-limitations"></a>Limitaciones de ancho de tabla

PolyBase en SQL Server 2016 tiene un límite de ancho de fila de 32 KB, en función del tamaño máximo de una sola fila válida por definición de tabla. Si la suma del esquema de columnas es superior a 32 KB, PolyBase no puede consultar los datos.

En Azure Synapse Analytics, esta limitación se ha elevado a 1 MB.

### <a name="data-type-limitations"></a>Limitaciones de tipos de datos

Los tipos de datos siguientes no se pueden usar en las tablas externas de PolyBase:

- `geography`
- `geometry`
- `hierarchyid`
- `image`
- `text`
- `nText`
- `xml`
- Cualquier tipo definido por el usuario

## <a name="locking"></a>Bloqueo

Bloqueo compartido en el objeto SCHEMARESOLUTION.

## <a name="security"></a>Seguridad

Los archivos de datos de una tabla externa se almacenan en Hadoop o Azure Blob Storage. Son sus propios procesos los que crean y administran estos archivos de datos. Es responsabilidad suya administrar la seguridad de los datos externos.

## <a name="examples"></a>Ejemplos

### <a name="a-join-hdfs-data-with-analytics-platform-system-data"></a>A. Combinar datos HDFS con datos de Analytics Platform System

```sql
SELECT cs.user_ip FROM ClickStream cs
JOIN User u ON cs.user_ip = u.user_ip
WHERE cs.url = 'www.microsoft.com'
;
```

### <a name="b-import-row-data-from-hdfs-into-a-distributed-analytics-platform-system-table"></a>B. Importar datos de filas de HDFS en una tabla distribuida de Analytics Platform System

```sql
CREATE TABLE ClickStream_PDW
WITH ( DISTRIBUTION = HASH (url) )
AS SELECT url, event_date, user_ip FROM ClickStream
;
```

### <a name="c-import-row-data-from-hdfs-into-a-replicated-analytics-platform-system-table"></a>C. Importar datos de filas de HDFS en una tabla replicada de Analytics Platform System

```sql
CREATE TABLE ClickStream_PDW
WITH ( DISTRIBUTION = REPLICATE )
AS SELECT url, event_date, user_ip
FROM ClickStream
;
```

## <a name="see-also"></a>Consulte también

- [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md)
- [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md)
- [CREATE EXTERNAL TABLE AS SELECT](../../t-sql/statements/create-external-table-as-select-transact-sql.md)
- [CREATE TABLE AS SELECT &#40;Azure Synapse Analytics&#41;](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md)

::: moniker-end
