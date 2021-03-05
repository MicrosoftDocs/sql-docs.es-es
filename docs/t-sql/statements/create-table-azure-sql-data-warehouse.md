---
description: CREATE TABLE (Azure Synapse Analytics)
title: CREATE TABLE (Azure Synapse Analytics) | Microsoft Docs
ms.custom: ''
ms.date: 07/03/2019
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: ea21c73c-40e8-4c54-83d4-46ca36b2cf73
author: julieMSFT
ms.author: jrasnick
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 3358f3d7393b1afa250cfc5f0a953c1ef178f820
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186575"
---
# <a name="create-table-azure-synapse-analytics"></a>CREATE TABLE (Azure Synapse Analytics)

[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Crea una tabla en [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  

Para entender las tablas y cómo usarlas, vea el artículo sobre [tablas en [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-overview/).

> [!NOTE]
>  En este artículo, las descripciones de [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] se aplican tanto a [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] como a [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], a menos que se indique lo contrario.

[!INCLUDE[synapse-analytics-od-supported-tables](../../includes/synapse-analytics-od-supported-tables.md)]

 ![Icono de vínculo de artículo](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de artículo") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

<a name="Syntax"></a>

## <a name="syntax"></a>Sintaxis
  
```syntaxsql
-- Create a new table.
CREATE TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( 
      { column_name <data_type>  [ <column_options> ] } [ ,...n ]
    )  
    [ WITH ( <table_option> [ ,...n ] ) ]  
[;]  

<column_options> ::=
    [ COLLATE Windows_collation_name ]  
    [ NULL | NOT NULL ] -- default is NULL  
    [ <column_constraint> ]

<column_constraint>::=
    {
        DEFAULT DEFAULT constant_expression
        | PRIMARY KEY NONCLUSTERED  NOT ENFORCED -- Applies to Azure Synapse Analytics only
        | UNIQUE NOT ENFORCED -- Applies to Azure Synapse Analytics only
    }

<table_option> ::=
    {
       CLUSTERED COLUMNSTORE INDEX --default for Azure Synapse Analytics 
      | CLUSTERED COLUMNSTORE INDEX ORDER (column [,...n])  
      | HEAP --default for Parallel Data Warehouse
      | CLUSTERED INDEX ( { index_column_name [ ASC | DESC ] } [ ,...n ] ) -- default is ASC
    }  
    {
        DISTRIBUTION = HASH ( distribution_column_name )
      | DISTRIBUTION = ROUND_ROBIN -- default for Azure Synapse Analytics
      | DISTRIBUTION = REPLICATE -- default for Parallel Data Warehouse
    }
    | PARTITION ( partition_column_name RANGE [ LEFT | RIGHT ] -- default is LEFT  
        FOR VALUES ( [ boundary_value [,...n] ] ) )

<data type> ::=
      datetimeoffset [ ( n ) ]  
    | datetime2 [ ( n ) ]  
    | datetime  
    | smalldatetime  
    | date  
    | time [ ( n ) ]  
    | float [ ( n ) ]  
    | real [ ( n ) ]  
    | decimal [ ( precision [ , scale ] ) ]   
    | numeric [ ( precision [ , scale ] ) ]   
    | money  
    | smallmoney  
    | bigint  
    | int   
    | smallint  
    | tinyint  
    | bit  
    | nvarchar [ ( n | max ) ]  -- max applies only to Azure Synapse Analytics 
    | nchar [ ( n ) ]  
    | varchar [ ( n | max )  ] -- max applies only to Azure Synapse Analytics  
    | char [ ( n ) ]  
    | varbinary [ ( n | max ) ] -- max applies only to Azure Synapse Analytics  
    | binary [ ( n ) ]  
    | uniqueidentifier  
```  

<a name="Arguments"></a>
## <a name="arguments"></a>Argumentos

 *database_name*  
 Nombre de la base de datos que contendrá la nueva tabla. El valor predeterminado es la base de datos actual.  
  
 *schema_name*  
 El esquema para la tabla. La especificación de *esquema* es opcional. Si se deja vacío, se usará el esquema predeterminado.  
  
 *table_name*  
 Nombre de la nueva tabla. Para crear una tabla temporal local, anteponga # al nombre de tabla.  Para obtener instrucciones y explicaciones sobre las tablas temporales, vea [Tablas temporales en [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-temporary/). 

 *column_name*  
 El nombre de una columna de la tabla.

### <a name="column-options"></a><a name="ColumnOptions"></a> Opciones de columna

 `COLLATE` *Windows_collation_name*  
 Especifica la intercalación para la expresión. La intercalación debe ser una de las intercalaciones de Windows que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite. Para obtener una lista de las intercalaciones de Windows que admite [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Nombre de intercalación de Windows (Transact-SQL)](windows-collation-name-transact-sql.md)/).  
  
 `NULL` | `NOT NULL`  
 Especifica si se permiten valores `NULL` en la columna. El valor predeterminado es `NULL`.  
  
 [ `CONSTRAINT` *constraint_name* ] `DEFAULT` *constant_expression*  
 Especifica el valor de columna predeterminado.  
  
 | Argumento | Explicación |
 | -------- | ----------- |
 | *constraint_name* | El nombre opcional para la restricción. El nombre de la restricción es único dentro de la base de datos. El nombre se puede volver a usar en otras bases de datos. |
 | *constant_expression* | El valor predeterminado de la columna. La expresión debe ser un valor literal o una constante. Por ejemplo, se permiten estas expresiones constantes: `'CA'`, `4`. Estas expresiones constantes no se permiten: `2+3`, `CURRENT_TIMESTAMP`. |
  
### <a name="table-structure-options"></a><a name="TableOptions"></a> Opciones de estructura de tabla

Para obtener instrucciones sobre cómo elegir el tipo de tabla, vea [Indexación de tablas en [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-index/).
  
 `CLUSTERED COLUMNSTORE INDEX` 
 
Almacena la tabla como un índice de almacén de columnas en clúster. El índice de almacén de columnas en clúster se aplica a todos los datos de tabla. Este es el comportamiento predeterminado para [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].
 
 `HEAP` Almacena la tabla como un montón. Este es el comportamiento predeterminado para [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  
  
 `CLUSTERED INDEX` ( *index_column_name* [ ,...*n* ] )  
 Almacena la tabla como un índice agrupado con una o varias columnas de clave. Este comportamiento almacena los datos por fila. Use *index_column_name* para especificar el nombre de una o varias columnas de clave en el índice.  Para obtener más información, vea Tablas de almacén de filas en la sección Comentarios generales.
 
 `LOCATION = USER_DB` Esta opción está en desuso. Se acepta sintácticamente, pero ya no es necesaria y no afecta al comportamiento.   
  
### <a name="table-distribution-options"></a><a name="TableDistributionOptions"></a> Opciones de distribución de tabla

Para entender cómo elegir el mejor método de distribución y usar tablas distribuidas, vea el artículo sobre la [distribución de tablas en [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-distribute/).

`DISTRIBUTION = HASH` (*distribution_column_name*) Asigna cada fila a una distribución aplicando un algoritmo hash al valor almacenado en *distribution_column_name*. El algoritmo es determinista, lo que significa que siempre se aplica el algoritmo hash al mismo valor para la misma distribución.  La columna de distribución se debe definir como NOT NULL porque todas las filas que tengan NULL se asignan a la misma distribución.

`DISTRIBUTION = ROUND_ROBIN` Distribuye uniformemente las filas entre todas las distribuciones de un modo Round Robin. Este es el comportamiento predeterminado para [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].

`DISTRIBUTION = REPLICATE` Almacena una copia de la tabla en cada nodo de ejecución. Para [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], la tabla se almacena en una base de datos de distribución en cada nodo de ejecución. Para [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], la tabla se almacena en un grupo de archivos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que abarca el nodo de ejecución. Este es el comportamiento predeterminado para [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].
  
### <a name="table-partition-options"></a><a name="TablePartitionOptions"></a> Opciones de partición de tabla
Para obtener instrucciones sobre cómo usar las particiones de tabla, vea el artículo sobre la [partición de tablas en [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-partition/).

 `PARTITION` ( *partition_column_name* `RANGE` [ `LEFT` | `RIGHT` ] `FOR VALUES` ( [ *boundary_value* [,...*n*] ] ))   
Crea una o varias particiones de tabla. Estas particiones son segmentos de tabla horizontales que permiten aplicar operaciones a subconjuntos de filas, independientemente de si la tabla se almacena como un montón, índice agrupado o índice de almacén de columnas en clúster. A diferencia de la columna de distribución, las particiones de tabla no determinan la distribución donde se almacena cada fila. En su lugar, las particiones de tabla determinan cómo se agrupan las filas y se almacenan dentro de cada distribución.  

| Argumento | Explicación |
| -------- | ----------- |
|*partition_column_name*| Especifica la columna que va a usar [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] para crear particiones de las filas. Esta columna puede ser de cualquier tipo de datos. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] ordena los valores de columna de partición en orden ascendente. El orden de bajo a alto va de `LEFT` a `RIGHT` en la especificación de `RANGE`. |  
| `RANGE LEFT` | Especifica que el valor de límite pertenece a la partición de la izquierda (los valores más bajos). El valor predeterminado es LEFT. |
| `RANGE RIGHT` | Especifica que el valor de límite pertenece a la partición de la derecha (los valores más altos). | 
| `FOR VALUES` ( *boundary_value* [,...*n*] ) | Especifica los valores de límite para la partición. *valor_de_límite* es una expresión constante. No puede ser NULL. Debe coincidir o ser implícitamente convertible al tipo de datos de *partition_column_name*. No se puede truncar durante la conversión implícita para que el tamaño y la escala del valor no coincidan con el tipo de datos de *partition_column_name*.<br></br><br></br>Si se especifica la cláusula `PARTITION`, pero no se especifica un valor de límite, [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] creará una tabla con particiones con una partición. Si procede, más adelante se puede dividir la tabla en dos particiones.<br></br><br></br>Si se especifica un valor de límite, la tabla resultante tendrá dos particiones; una para los valores inferiores al valor de límite y otra para los valores superiores al valor de límite. Si se mueve una partición a una tabla sin particiones, la tabla sin particiones recibirá los datos, pero no tendrá los límites de partición en sus metadatos.| 

 Vea [Crear una tabla con particiones](#PartitionedTable) en la sección Ejemplos.

### <a name="ordered-clustered-columnstore-index-option"></a>Opción de índice de almacén de columnas agrupado ordenado 

El índice de almacén de columnas agrupado (CCI) es el valor predeterminado para crear tablas en [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)].  Los datos de un CCI no se ordenan antes de comprimirse en segmentos de almacén de columnas.  Al crear un CCI con ORDER, los datos se ordenan antes de agregarse a los segmentos de índice y el rendimiento de las consultas se puede mejorar. Consulte [Optimización del rendimiento con el índice de almacén de columnas agrupado ordenado](/azure/sql-data-warehouse/performance-tuning-ordered-cci?view=azure-sqldw-latest&preserve-view=true) para obtener más información.  

Se puede crear un CCI ordenado en columnas de cualquier tipo de datos que se admita en [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], excepto en las columnas de cadena.  

Los usuarios pueden consultar la columna **column_store_order_ordinal** en **sys.index_columns** para obtener las columnas en las que se ordena una tabla y la secuencia del orden.  

Consulte [Optimización del rendimiento con el índice de almacén de columnas agrupado ordenado](/azure/sql-data-warehouse/performance-tuning-ordered-cci) para obtener más información.   

### <a name="data-type"></a><a name="DataTypes"></a> Tipo de datos

[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] admite los tipos de datos más usados habitualmente. A continuación se muestra una lista de los tipos de datos admitidos junto con sus detalles y los bytes de almacenamiento. Para entender mejor los tipos de datos y cómo usarlos, vea el artículo sobre los [tipos de datos de tabla en [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-data-types).

Para obtener una tabla de conversiones de tipos de datos, vea la sección Conversiones implícitas de [CAST y CONVERT (Transact-SQL)](../functions/cast-and-convert-transact-sql.md).

>[!NOTE]
>Para más información, consulte [Tipos de datos y funciones de fecha y hora &#40;Transact-SQL&#41;](../functions/date-and-time-data-types-and-functions-transact-sql.md).

`datetimeoffset` [ ( *n* ) ]  
 El valor predeterminado de *n* es 7.  
  
 `datetime2` [ ( *n* ) ]  
Igual que `datetime`, salvo que puede se especificar el número de fracciones de segundo. El valor predeterminado de *n* es `7`.  
  
|Valor *n*|Precision|Escala|  
|--:|--:|-:|  
|`0`|19|0|  
|`1`|21|1|  
|`2`|22|2|  
|`3`|23|3|  
|`4`|24|4|  
|`5`|25|5|  
|`6`|26|6|  
|`7`|27|7|  
  
 `datetime`  
 Almacena la fecha y hora del día con 19 y 23 caracteres según el calendario gregoriano. La fecha puede contener el año, el mes y el día. La hora contiene la hora, los minutos y los segundos. Como opción, se pueden mostrar tres dígitos para fracciones de segundo. El tamaño de almacenamiento es de 8 bytes.  
  
 `smalldatetime`  
 Almacena una fecha y una hora. El tamaño de almacenamiento es de 4 bytes.  
  
 `date`  
 Almacena una fecha con un máximo de 10 caracteres para el año, el mes y el día según el calendario gregoriano. El tamaño de almacenamiento es de 3 bytes. La fecha se almacena como un entero.  
  
 `time` [ ( *n* ) ]  
 El valor predeterminado de *n* es `7`.  
  
 `float` [ ( *n* ) ]  
 Tipo de datos numérico aproximado que se usa con datos numéricos de punto flotante. Los datos de punto flotante son aproximados, lo que significa que no todos los valores del rango del tipo de datos se pueden representar con exactitud. *n* especifica el número de bits que se usa para almacenar la mantisa del `float` en notación científica. *n* determina el tamaño de almacenamiento y la precisión. Si se especifica *n*, debe ser un valor entre `1` y `53`. El valor predeterminado de *n* es `53`.  
  
| Valor *n* | Precision | Tamaño de almacenamiento |  
| --------: | --------: | -----------: |  
| 1-24   | 7 dígitos  | 4 bytes      |  
| 25-53  | 15 dígitos | 8 bytes      |  
  
 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] trata *n* como uno de dos valores posibles. Si `1`<= *n* <= `24`, *n* se trata como `24`. Si `25` <= *n* <= `53`, *n* se trata como `53`.  
  
 El tipo de datos [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] de `float` cumple con el estándar ISO para todos los valores de *n* desde `1` hasta `53`. El sinónimo para doble precisión es `float(53)`.  
  
 `real` [ ( *n* ) ]  
 La definición de real es la misma que la de float. El sinónimo de `real` en ISO es `float(24)`.  
  
 `decimal` [ ( *precisión* [ *, escala* ] ) ] | `numeric` [ ( *precisión* [ *, escala* ] ) ]  
 Almacena números de precisión y escala fijas.  
  
 *precisión*  
 El número total máximo de dígitos decimales que se puede almacenar, tanto a la izquierda como a la derecha del separador decimal. La precisión debe ser un valor comprendido entre `1` y la precisión máxima de `38`. La precisión predeterminada es `18`.  
  
 *scale*  
 El número máximo de dígitos decimales que se puede almacenar a la derecha del separador decimal. La *escala* debe ser un valor comprendido entre `0` y *precisión*. Solo se puede especificar *escala* si se especifica *precisión*. La escala predeterminada es `0` y, por tanto, `0` <= *escala* <= *precisión*. Los tamaños de almacenamiento máximo varían según la precisión.  
  
| Precision | Bytes de almacenamiento  |  
| ---------: |-------------: |  
|  1-9       |             5 |  
| 10-19      |             9 |  
| 20-28      |            13 |  
| 29-38      |            17 |  
  
 `money` | `smallmoney`  
 Tipos de datos que representan valores de moneda.  
  
| Tipo de datos | Bytes de almacenamiento |  
| --------- | ------------: |  
| `money`|8|  
| `smallmoney` |4|  
  
 `bigint` \| `int` \| `smallint` \| `tinyint`  
 Tipos de datos numéricos exactos que utilizan datos enteros. El almacenamiento se muestra en la tabla siguiente.  
  
| Tipo de datos | Bytes de almacenamiento |  
| --------- | ------------: |  
| `bigint`|8|  
| `int` |4|  
| `smallint` |2|  
| `tinyint` |1|  
  
 `bit`  
 Tipo de datos entero que puede aceptar los valores `1`, `0` o `NULL. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] optimiza el almacenamiento de las columnas de tipo bit. Si una tabla contiene ocho columnas o menos de tipo bit, se almacenan como 1 byte. Si hay entre 9-16 columnas de tipo bit, se almacenan como 2 bytes, y así sucesivamente.  
  
 `nvarchar` [ ( *n* | `max` ) ]  -- `max` solo se aplica a [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].  
 Datos de caracteres Unicode de longitud variable. *n* puede ser un valor de 1 a 4000. `max` indica que el tamaño máximo de almacenamiento es de 2^31-1 bytes (2 GB). El tamaño de almacenamiento en bytes es dos veces el número de caracteres especificado + 2 bytes. Los datos especificados pueden tener una longitud de cero caracteres.  
  
 `nchar` [ ( *n* ) ]  
 Datos de caracteres Unicode de longitud fija, con una longitud de *n* caracteres. *n* debe ser un valor entre `1` y `4000`. El tamaño de almacenamiento es dos veces *n* bytes.  
  
 `varchar` [ ( *n*  | `max` ) ]  -- `max` solo se aplica a [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].   
 Datos de caracteres no Unicode de longitud variable, con una longitud de *n* bytes. *n* debe ser un valor entre `1` y `8000`. `max` indica que el tamaño máximo de almacenamiento es 2^31-1 bytes (2 GB). El tamaño de almacenamiento es la longitud real de los datos especificados + 2 bytes.  
  
 `char` [ ( *n* ) ]  
 Datos de caracteres no Unicode de longitud fija, con una longitud de *n* bytes. *n* debe ser un valor entre `1` y `8000`. El tamaño de almacenamiento es de *n* bytes. El valor predeterminado de *n* es `1`.  
  
 `varbinary` [ ( *n*  | `max` ) ]  -- `max` solo se aplica a [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].  
 Datos binarios de longitud variable. *n* puede ser un valor entre `1` y `8000`. `max` indica que el tamaño máximo de almacenamiento es de 2^31-1 bytes (2 GB). El tamaño de almacenamiento es la longitud real de los datos especificados + 2 bytes. El valor predeterminado de *n* es 7.  
  
 `binary` [ ( *n* ) ]  
 Datos binarios de longitud fija con una longitud de *n* bytes. *n* puede ser un valor entre `1` y `8000`. El tamaño de almacenamiento es de *n* bytes. El valor predeterminado de *n* es `7`.  
  
 `uniqueidentifier`  
 Es un GUID de 16 bytes.  
   
<a name="Permissions"></a>  
## <a name="permissions"></a>Permisos  
 Crear una tabla requiere un permiso en el rol fijo de base de datos `db_ddladmin`, o bien:
 - El permiso `CREATE TABLE` en la base de datos.
 - El permiso `ALTER SCHEMA` en el esquema que va a contener la tabla. 

Crear una tabla con particiones requiere un permiso en el rol fijo de base de datos `db_ddladmin`, o bien

- el permiso `ALTER ANY DATASPACE`.
  
 El inicio de sesión que crea una tabla temporal local recibe los permisos `CONTROL`, `INSERT`, `SELECT` y `UPDATE` en la tabla.  
 
<a name="GeneralRemarks"></a>  
## <a name="general-remarks"></a>Notas generales  
 
Para obtener información sobre los límites mínimo y máximo, vea [Límites de capacidad de [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-service-capacity-limits/). 
 
### <a name="determining-the-number-of-table-partitions"></a>Determinar el número de particiones de tabla
Cada tabla definida por el usuario se divide en varias tablas más pequeñas que se almacenan en ubicaciones independientes llamadas distribuciones. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] usa 60 distribuciones. En [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], el número de distribuciones depende del número de nodos de ejecución.
 
Cada distribución contiene todas las particiones de tabla. Por ejemplo, si hay 60 distribuciones y 4 particiones de tabla, además de una partición vacía, habrá 300 particiones (5 x 60 = 300). Si la tabla es un índice de almacén de columnas en clúster, habrá un índice de almacén de columnas por partición, lo que significa que tendrá 300 índices de almacén de columnas.

Se recomienda el uso de menos particiones de tabla para asegurar que cada índice de almacén de columnas tiene suficientes filas para aprovechar las ventajas de los índices de almacén de columnas. Para obtener más información, vea [Creación de particiones de tablas en [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-partition/) e [lndexación de tablas en [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-index/).  

### <a name="rowstore-table-heap-or-clustered-index"></a>Tabla de almacén de filas (montón o índice agrupado)

Una tabla de almacén de filas es una tabla almacenada en el orden de fila por fila. Es un montón o índice agrupado. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] crea todas las tablas de almacén de filas con compresión de página; este comportamiento no es configurable por el usuario.

### <a name="columnstore-table-columnstore-index"></a>Tabla de almacén de columnas (índice de almacén de columnas)

Una tabla de almacén de columnas es una tabla almacenada en el orden de columna por columna. El índice de almacén de columnas es la tecnología que administra los datos almacenados en una tabla de almacén de columnas.  El índice de almacén de columnas agrupado no afecta a cómo se distribuyen los datos, si no que afecta a cómo se almacenan dichos datos dentro de cada distribución.

Para cambiar una tabla de almacén de filas a una tabla de almacén de columnas, quite todos los índices existentes en la tabla y cree un índice de almacén de columnas agrupado. Para obtener un ejemplo, vea [CREATE COLUMNSTORE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md).

Para obtener más información, consulte estos artículos:
- [Resumen de las características de los índices de almacén de columnas para cada versión](../../relational-databases/indexes/columnstore-indexes-what-s-new.md)
- [Indexación de tablas en [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-index/)
- [Descripción de los índices de almacén de columnas](~/relational-databases/indexes/columnstore-indexes-overview.md) 

<a name="LimitationsRestrictions"></a>  
## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones  
 No puede definir una restricción DEFAULT en una columna de distribución.  
  
### <a name="partitions"></a>Particiones
Al usar particiones, la columna de partición no puede tener una intercalación exclusiva de Unicode. Por ejemplo, se produce un error en la instrucción siguiente.  
  
 ```sql
CREATE TABLE t1 ( c1 varchar(20) COLLATE Divehi_90_CI_AS_KS_WS) WITH (PARTITION (c1 RANGE FOR VALUES (N'')))
```  
 
 Si *valor_de_límite* es un valor literal que se debe convertir implícitamente al tipo de datos en *partition_column_name*, se producirá una discrepancia. El valor literal se muestra a través de las vistas del sistema de [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], pero el valor convertido se usa para operaciones de [!INCLUDE[tsql](../../includes/tsql-md.md)]. 

### <a name="temporary-tables"></a>Tablas temporales

 Las tablas temporales globales que comienzan por ## no se admiten.  
  
 Las tablas temporales locales tienen las siguientes limitaciones y restricciones:  
  
-   Solo son visibles para la sesión actual. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] las elimina automáticamente al final de la sesión. Para quitarlas de forma explícita, use la instrucción DROP TABLE.   
-   No se pueden cambiar de nombre. 
-   No pueden tener particiones ni vistas.  
-   No se pueden cambiar sus permisos. Las instrucciones `GRANT`, `DENY` y `REVOKE` no se pueden usar con tablas temporales locales.   
-   Los comandos de consola de base de datos se bloquean para las tablas temporales.   
-   Si se usa más de una tabla temporal local dentro de un lote, cada una debe tener un nombre único. Si varias sesiones ejecutan el mismo lote y crean la misma tabla temporal local, [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] anexa de forma interna un sufijo numérico al nombre de la tabla temporal local para mantener un nombre único para cada tabla temporal local.  
    
<a name="LockingBehavior"></a>   
## <a name="locking-behavior"></a>Comportamiento de bloqueo  
 Toma un bloqueo exclusivo en la tabla. Toma un bloqueo compartido en los objetos DATABASE, SCHEMA y SCHEMARESOLUTION.  
 

<a name="ExamplesColumn"></a>   
## <a name="examples-for-columns"></a>Ejemplos para columnas

### <a name="a-specify-a-column-collation"></a><a name="ColumnCollation"></a> A. Especificar una intercalación 
 En el ejemplo siguiente, se crea la tabla `MyTable` con dos intercalaciones de columna diferentes. De forma predeterminada, la columna `mycolumn1` tiene la intercalación predeterminada Latin1_General_100_CI_AS_KS_WS. La columna `mycolumn2` tiene la intercalación Frisian_100_CS_AS.  
  
```sql
CREATE TABLE MyTable   
  (  
    mycolumnnn1 nvarchar,  
    mycolumn2 nvarchar COLLATE Frisian_100_CS_AS )  
WITH ( CLUSTERED COLUMNSTORE INDEX )  
;  
```  
  
### <a name="b-specify-a-default-constraint-for-a-column"></a><a name="DefaultConstraint"></a> B. Especificación de una restricción DEFAULT para una columna

 En el ejemplo siguiente se muestra la sintaxis para especificar un valor predeterminado para una columna. La columna colA tiene una restricción predeterminada denominada constraint_colA y un valor predeterminado de 0.  
  
```sql
CREATE TABLE MyTable
  (  
    colA int CONSTRAINT constraint_colA DEFAULT 0,  
    colB nvarchar COLLATE Frisian_100_CS_AS
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX )  
;  
```

<a name="ExamplesTemporaryTables"></a> 
## <a name="examples-for-temporary-tables"></a>Ejemplos de tablas temporales

### <a name="c-create-a-local-temporary-table"></a><a name="TemporaryTable"></a> C. Creación de una tabla temporal local  
 En el ejemplo siguiente se crea una tabla temporal local denominada #myTable. La tabla se especifica con un nombre de tres partes, que comienza con #.
  
```sql
CREATE TABLE AdventureWorks.dbo.#myTable
  (  
   id int NOT NULL,  
   lastName varchar(20),  
   zipCode varchar(6)  
  )  
WITH  
  (   
    DISTRIBUTION = HASH (id),  
    CLUSTERED COLUMNSTORE INDEX
  )  
;  
```

<a name="ExTableStructure"></a>  
## <a name="examples-for-table-structure"></a>Ejemplos de estructura de tabla

### <a name="d-create-a-table-with-a-clustered-columnstore-index"></a><a name="ClusteredColumnstoreIndex"></a> D. Creación de una tabla con un índice de almacén de columnas en clúster  
 En el ejemplo siguiente se crea una tabla distribuida con un índice de almacén de columnas en clúster. Cada distribución se almacenará como un almacén de columnas.  
  
 El índice de almacén de columnas en clúster no afecta a cómo se distribuyen los datos; los datos siempre se distribuyen por fila. El índice de almacén de columnas en clúster afecta a cómo se almacenan los datos dentro de cada distribución.  
  
```sql
  CREATE TABLE MyTable
  (  
    colA int CONSTRAINT constraint_colA DEFAULT 0,  
    colB nvarchar COLLATE Frisian_100_CS_AS
  )  
WITH   
  (   
    DISTRIBUTION = HASH ( colB ),  
    CLUSTERED COLUMNSTORE INDEX
  )  
;  
```  

### <a name="e-create-an-ordered-clustered-columnstore-index"></a><a name="OrderedClusteredColumnstoreIndex"></a> E. Creación de un índice ordenado de almacén de columnas agrupado

En el ejemplo siguiente muestra cómo crear un índice de almacén de columnas agrupado ordenado. El índice está ordenado por el valor SHIPDATE.

```sql
CREATE TABLE Lineitem  
WITH (DISTRIBUTION = ROUND_ROBIN, CLUSTERED COLUMNSTORE INDEX ORDER(SHIPDATE))  
AS  
SELECT * FROM ext_Lineitem
```

<a name="ExTableDistribution"></a> 
## <a name="examples-for-table-distribution"></a>Ejemplos de distribución de la tabla

### <a name="f-create-a-round_robin-table"></a><a name="RoundRobin"></a> F. Creación de una tabla ROUND_ROBIN  
 En el ejemplo siguiente se crea una tabla ROUND_ROBIN con tres columnas y sin particiones. Los datos se reparten entre todas las distribuciones. La tabla se crea con un CLUSTERED COLUMNSTORE INDEX, lo que proporciona mejor rendimiento y compresión de datos que un montón o un índice de almacén de filas en clúster.  
  
```sql
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX );  
```  
  
### <a name="g-create-a-hash-distributed-table"></a><a name="HashDistributed"></a> G. Creación de una tabla distribuida de hash

 En el ejemplo siguiente se crea la misma tabla que el ejemplo anterior. Pero para esta tabla, las filas se distribuyen (en la columna `id`) en lugar de propagarse de forma aleatoria como una tabla ROUND_ROBIN. La tabla se crea con un CLUSTERED COLUMNSTORE INDEX, lo que proporciona mejor rendimiento y compresión de datos que un montón o un índice de almacén de filas en clúster.  
  
```sql
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH  
  (   
    DISTRIBUTION = HASH (id),   
    CLUSTERED COLUMNSTORE INDEX  
  );  
```  
  
### <a name="h-create-a-replicated-table"></a><a name="Replicated"></a> H. Creación de una carpeta replicada  
 En el ejemplo siguiente se crea una tabla replicada similar a los ejemplos anteriores. Las tablas replicadas se copian por completo en cada nodo de ejecución. Con esta copia en cada nodo de ejecución, se reduce el movimiento de datos para las consultas. Este ejemplo se crea con un ÍNDICE AGRUPADO, lo que proporciona una mejor compresión de datos que un montón. Un montón no puede contener filas suficientes para lograr una buena compresión del ÍNDICE DE ALMACÉN DE COLUMNAS AGRUPADO.  
  
```sql
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH  
  (   
    DISTRIBUTION = REPLICATE,
    CLUSTERED INDEX (lastName)  
  );  
```  

<a name="ExTablePartitions"></a> 
## <a name="examples-for-table-partitions"></a>Ejemplos de particiones de tabla

###  <a name="i-create-a-partitioned-table"></a><a name="PartitionedTable"></a> I. Creación de una tabla con particiones

 En el ejemplo siguiente se crea la misma tabla que en el ejemplo A, con la adición de la partición RANGE LEFT en la columna `id`. Especifica cuatro valores de límite de partición, lo que da como resultado cinco particiones.  
  
```sql
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode int)  
WITH
  (
  
    PARTITION ( id RANGE LEFT FOR VALUES (10, 20, 30, 40 )),  
    CLUSTERED COLUMNSTORE INDEX
  )  
;  
```  
  
 En este ejemplo, los datos se ordenarán en las particiones siguientes:  
  
- Partición 1: col <= 10
- Partición 2: 10 < col <= 20
- Partición 3: 20 < col <= 30
- Partición 4: 30 < col <= 40
- Partición 5: 40 < col  
  
 Si se crearan particiones RANGE RIGHT en lugar de RANGE LEFT (el valor predeterminado) en esta misma tabla, los datos se ordenarían en las particiones siguientes:  
  
- Partición 1: col < 10  
- Partición 2: 10 <= col < 20
- Partición 3: 20 <= col < 30
- Partición 4: 30 <= col < 40
- Partición 5: 40 <= col  
  
### <a name="j-create-a-partitioned-table-with-one-partition"></a><a name="OnePartition"></a> J. Creación de una tabla con particiones con una partición

 En el ejemplo siguiente se crea una tabla con particiones con una partición. No especifica ningún valor de límite, lo que da como resultado una partición.  
  
```sql
CREATE TABLE myTable (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode int)  
WITH
    (
      PARTITION ( id RANGE LEFT FOR VALUES ( )),  
      CLUSTERED COLUMNSTORE INDEX  
    )  
;  
```  
  
### <a name="k-create-a-table-with-date-partitioning"></a><a name="DatePartition"></a> K. Creación de una tabla con particiones de fecha

 En el ejemplo siguiente se crea una tabla denominada `myTable`, con la partición en una columna `date`. Al usar RANGE RIGHT y fechas para los valores de límite, en cada partición se coloca un mes de datos.  
  
```sql
CREATE TABLE myTable (  
    l_orderkey      bigint,
    l_partkey       bigint,
    l_suppkey       bigint,
    l_linenumber    bigint,
    l_quantity      decimal(15,2),  
    l_extendedprice decimal(15,2),  
    l_discount      decimal(15,2),  
    l_tax           decimal(15,2),  
    l_returnflag    char(1),  
    l_linestatus    char(1),  
    l_shipdate      date,  
    l_commitdate    date,  
    l_receiptdate   date,  
    l_shipinstruct  char(25),  
    l_shipmode      char(10),  
    l_comment       varchar(44))  
WITH
  (
    DISTRIBUTION = HASH (l_orderkey),  
    CLUSTERED COLUMNSTORE INDEX,  
    PARTITION ( l_shipdate  RANGE RIGHT FOR VALUES
      (  
        '1992-01-01','1992-02-01','1992-03-01','1992-04-01','1992-05-01',
        '1992-06-01','1992-07-01','1992-08-01','1992-09-01','1992-10-01',
        '1992-11-01','1992-12-01','1993-01-01','1993-02-01','1993-03-01',
        '1993-04-01','1993-05-01','1993-06-01','1993-07-01','1993-08-01',
        '1993-09-01','1993-10-01','1993-11-01','1993-12-01','1994-01-01',
        '1994-02-01','1994-03-01','1994-04-01','1994-05-01','1994-06-01',
        '1994-07-01','1994-08-01','1994-09-01','1994-10-01','1994-11-01',
        '1994-12-01'  
      ))
  );  
```  
  
<a name="SeeAlso"></a>
## <a name="see-also"></a>Consulte también
 
[CREATE TABLE AS SELECT &#40;Azure Synapse Analytics&#41;](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md)   
[DROP TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-table-transact-sql.md)   
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
[sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md) 
