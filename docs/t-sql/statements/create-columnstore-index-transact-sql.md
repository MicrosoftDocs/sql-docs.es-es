---
description: CREATE COLUMNSTORE INDEX (Transact-SQL)
title: CREATE COLUMNSTORE INDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/25/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_COLUMNSTORE_INDEX_TSQL
- COLUMNSTORE INDEX
- COLUMNSTORE_INDEX_TSQL
- CREATE CLUSTERED COLUMNSTORE INDEX
- COLUMNSTORE_TSQL
- CREATE NONCLUSTERED COLUMNSTORE INDEX
- CREATE_NONCLUSTERED_COLUMNSTORE_INDEX_TSQL
- CREATE COLUMNSTORE INDEX
- CREATE_CLUSTERED_COLUMNSTORE_INDEX_TSQL
- COLUMNSTORE
dev_langs:
- TSQL
helpviewer_keywords:
- index creation [SQL Server], columnstore indexes
- columnstore index, creating
- CREATE COLUMNSTORE INDEX statement
- CREATE INDEX statement
ms.assetid: 7e1793b3-5383-4e3d-8cef-027c0c8cb5b1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 47759238673fa5e88c02421e27d0e12177072605
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347047"
---
# <a name="create-columnstore-index-transact-sql"></a>CREATE COLUMNSTORE INDEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Convierta una tabla de almacén de filas en un índice de almacén de columnas o cree un índice no clúster de almacén de columnas. Use un índice de almacén de columnas para ejecutar de forma eficaz los análisis operativos en tiempo real en una carga de trabajo OLTP o para mejorar la compresión de los datos y el rendimiento de las consultas de las cargas de trabajo de almacenamiento de datos.  
  
> [!NOTE]
> A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], puede crear la tabla como un índice clúster de almacén de columnas.   Ya no es necesario crear una tabla de almacén de filas y luego convertirla en un índice clúster de almacén de columnas.  

> [!TIP]
> Para obtener más información sobre las directrices de diseño de índices, vea la [Guía de diseño de índices de SQL Server](../../relational-databases/sql-server-index-design-guide.md).

Ejemplos:  
-   [Ejemplos para convertir una tabla de almacén de filas en almacén de columnas](../../t-sql/statements/create-columnstore-index-transact-sql.md#convert)  
-   [Ejemplos de índices no clúster de almacén de columnas](../../t-sql/statements/create-columnstore-index-transact-sql.md#nonclustered)  
  
Escenarios:  
-   [Índices de almacén de columnas para análisis operativos en tiempo real](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)  
-   [Índices de almacén de columnas para almacenamiento de datos](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)  
  
Más información:  
-   [Guía de índices de almacén de columnas](../../relational-databases/indexes/columnstore-indexes-overview.md)  
-   [Resumen de características de índices de almacén de columnas](../../relational-databases/indexes/columnstore-indexes-what-s-new.md)  
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
-- Create a clustered columnstore index on disk-based table.  
CREATE CLUSTERED COLUMNSTORE INDEX index_name  
    ON { database_name.schema_name.table_name | schema_name.table_name | table_name }  
    [ WITH ( < with_option> [ ,...n ] ) ]  
    [ ON <on_option> ] 
[ ; ]  
  
--Create a nonclustered columnstore index on a disk-based table.  
CREATE [NONCLUSTERED]  COLUMNSTORE INDEX index_name   
    ON { database_name.schema_name.table_name | schema_name.table_name | table_name }
        ( column  [ ,...n ] )  
    [ WHERE <filter_expression> [ AND <filter_expression> ] ]
    [ WITH ( < with_option> [ ,...n ] ) ]  
    [ ON <on_option> ]
[ ; ]  
  
<with_option> ::=  
      DROP_EXISTING = { ON | OFF } -- default is OFF  
    | MAXDOP = max_degree_of_parallelism 
    | ONLINE = { ON | OFF } 
    | COMPRESSION_DELAY  = { 0 | delay [ Minutes ] }  
    | DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }  
      [ ON PARTITIONS ( { partition_number_expression | range } [ ,...n ] ) ]  
  
<on_option>::=  
      partition_scheme_name ( column_name )
    | filegroup_name
    | "default"
  
<filter_expression> ::=  
      column_name IN ( constant [ ,...n ]  
    | column_name { IS | IS NOT | = | <> | != | > | >= | !> | < | <= | !< } constant  
  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
CREATE CLUSTERED COLUMNSTORE INDEX index_name
    ON { database_name.schema_name.table_name | schema_name.table_name | table_name } 
    [ORDER (column [,...n] ) ]  
    [ WITH ( DROP_EXISTING = { ON | OFF } ) ] --default is OFF  
[;]  

```
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos

Algunas de las opciones no están disponibles en todas las versiones del motor de base de datos. En la siguiente tabla se muestran las versiones cuando las opciones se introducen en los índices CLUSTERED COLUMNSTORE y NONCLUSTERED COLUMNSTORE:

|Opción| CLUSTERED | NONCLUSTERED |
|---|---|---|
| COMPRESSION_DELAY | [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] | [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] |
| DATA_COMPRESSION | [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] | [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] | 
| ONLINE | [!INCLUDE[ssSQLv15_md](../../includes/sssql19-md.md)] | [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] |
| WHERE, cláusula | N/D | [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] |

Todas las opciones están disponibles en Azure SQL Database.

### <a name="create-clustered-columnstore-index"></a>CREATE CLUSTERED COLUMNSTORE INDEX

Crea un índice clúster de almacén de columnas en el que todos los datos están comprimidos y almacenados por columna. El índice incluye todas las columnas de la tabla y almacena toda la tabla. Si la tabla existente es un montón o un índice clúster, la tabla se convierte en un índice clúster de almacén de columnas. Si la tabla ya está almacenada como un índice clúster de almacén de columnas, el índice existente se quita y se vuelve a compilar.  
  
*index_name*  
Especifica el nombre del nuevo índice.  
  
Si la tabla ya tiene un índice clúster de almacén de columnas, puede especificar el mismo nombre que el índice existente o puede usar la opción DROP EXISTING para especificar uno nuevo.  
  
ON [*database_name*. [*schema_name* ] . | *schema_name* . ] *table_name*

Especifica el nombre de una, dos o tres partes de la tabla que se almacenará como un índice clúster de almacén de columnas. Si la tabla es un montón o un índice clúster, se convierte de almacén de filas en un almacén de columnas. Si la tabla ya es un almacén de columnas, esta instrucción vuelve a compilar el índice clúster de almacén de columnas. Para convertir a un índice ordenado de almacén de columnas agrupado, el índice existente debe ser un índice de almacén de columnas agrupado.
  
#### <a name="with-options"></a>Opciones de WITH

##### <a name="drop_existing--off--on"></a>DROP_EXISTING = [OFF] | ON

   `DROP_EXISTING = ON` especifica que se quite el índice existente y se cree un nuevo índice de almacén de columnas.  
```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH (DROP_EXISTING = ON);
```
   El valor predeterminado, DROP_EXISTING = OFF, espera que el nombre del índice sea el mismo que el nombre existente. Se muestra un error si ya existe el nombre de índice especificado.  
  
##### <a name="maxdop--max_degree_of_parallelism"></a>MAXDOP = *max_degree_of_parallelism*  
   Reemplaza la configuración del servidor existente para el grado máximo de paralelismo mientras dura la operación de índice. Utilice MAXDOP para establecer un límite para el número de procesadores utilizados en la ejecución de un plan paralelo. El máximo es 64 procesadores.  
  
   Los valores de *max_degree_of_parallelism* pueden ser:  
   - 1: suprime la generación de planes paralelos.  
   - \>1: restringe el número máximo de procesadores usados en una operación de índice paralelo al número especificado o a un número inferior en función de la actual carga de trabajo del sistema. Por ejemplo, si MAXDOP = 4, el número de procesadores usados es de 4 o menos.  
   - 0 (predeterminado): usa el número real de procesadores o menos, según la carga de trabajo actual del sistema.  
  
```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH (MAXDOP = 2);
```

   Para obtener más información, vea [Establecer la opción de configuración del servidor Grado máximo de paralelismo](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) y [Configurar operaciones de índice en paralelo](../../relational-databases/indexes/configure-parallel-index-operations.md).  
 
###### <a name="compression_delay--0--delay--minutes-"></a>COMPRESSION_DELAY = **0** | *delay* [ Minutes ]  
   En una tabla basada en disco, *delay* especifica el número mínimo de minutos que debe permanecer un grupo de filas delta en estado CLOSED en el grupo de filas delta antes de que SQL Server pueda comprimirlo en el grupo de filas comprimido. Puesto que las tablas basadas en disco no realizan el seguimiento de los tiempos de inserción y actualización de filas individuales, SQL Server aplica el retraso a los grupos de filas delta en el estado CLOSED.  
   El valor predeterminado es 0 minutos.  
   
```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH ( COMPRESSION_DELAY = 10 Minutes );
```

   Para obtener recomendaciones sobre cuándo usar COMPRESSION_DELAY, vea [Introducción al almacén de columnas para análisis operativos en tiempo real](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md).  
  
##### <a name="data_compression--columnstore--columnstore_archive"></a>DATA_COMPRESSION = COLUMNSTORE | COLUMNSTORE_ARCHIVE  
   Especifica la opción de compresión de datos para la tabla, el número de partición o el intervalo de particiones especificados. Las opciones son las siguientes:   
- `COLUMNSTORE` es el valor predeterminado y especifica que se comprima con la compresión de almacén de columnas que ofrezca el mejor rendimiento. Es la elección habitual.  
- `COLUMNSTORE_ARCHIVE` comprime la tabla o la partición a un tamaño menor. Use esta opción para situaciones como un archivo que requiera un tamaño menor de almacenamiento y pueda permitirse usar más tiempo para el almacenamiento y la recuperación.  
  
```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH ( DATA_COMPRESSION = COLUMNSTORE_ARCHIVE );
```
   Para más información sobre la compresión, vea [Compresión de datos](../../relational-databases/data-compression/data-compression.md).  

###### <a name="online--on--off"></a>ONLINE = [ON | OFF]
- `ON` especifica que el índice de almacén de columnas permanece en línea y disponible mientras se compila la nueva copia del índice.
- `OFF` especifica que el índice no está disponible mientras se compila la nueva copia.

```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
       WITH ( ONLINE = ON );
```

#### <a name="on-options"></a>Opciones de ON 
   Con las opciones ON puede especificar opciones para el almacenamiento de datos, como un esquema de partición, un grupo de archivos específico o el grupo de archivos predeterminado. Si no se especifica la opción ON, el índice usa la configuración de partición o de grupo de archivos de la tabla existente.  
  
   *partition_scheme_name* **(** _column_name_ **)**  
   Especifica el esquema de partición de la tabla. El esquema de partición ya debe existir en la base de datos. Para crear el esquema de partición, vea [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md).  
 
   *column_name* especifica la columna en la que se van a crear las particiones de un índice con particiones. Esta columna debe coincidir con el tipo de datos, la longitud y la precisión del argumento de la función de partición que *partition_scheme_name* emplea.  

   *filegroup_name*  
   Especifica el grupo de archivos para almacenar el índice clúster de almacén de columnas. Si no se especifica ninguna ubicación y la tabla no tiene particiones, el índice usa el mismo grupo de archivos que la tabla o la vista subyacente. El grupo de archivos debe existir previamente.  

   **"** default **"**  
   Para crear el índice en el grupo de archivos predeterminado, use "default" o [ default ].  
  
   Si se especifica "default", la opción QUOTED_IDENTIFIER debe tener el valor ON para la sesión actual. QUOTED_IDENTIFIER es ON de forma predeterminada. Para obtener más información, vea [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
### <a name="create-nonclustered-columnstore-index"></a>CREATE [NONCLUSTERED] COLUMNSTORE INDEX  
Crea un índice no clúster de almacén de columnas en memoria en una tabla de almacén de filas almacenada como un montón o un índice clúster. El índice puede tener una condición de filtrado y no necesita incluir todas las columnas de la tabla subyacente. El índice de almacén de columnas requiere suficiente espacio para almacenar una copia de los datos. Se puede actualizar y se actualiza a medida que se modifica la tabla subyacente. El índice no clúster de almacén de columnas de un índice clúster permite el análisis en tiempo real.  
  
*index_name*  
   Especifica el nombre del índice. *index_name* debe ser único en la tabla, pero no es necesario que sea único en la base de datos. Los nombres de índice deben seguir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md).  
  
 **(** _column_  [ **,** ...*n* ] **)**  
    Especifica las columnas que se van a almacenar. Un índice no clúster de almacén de columnas está limitado a 1024 columnas.  
   Cada columna debe ser de un tipo de datos compatible con los índices de almacén de columnas. Vea [Limitaciones y restricciones](../../t-sql/statements/create-columnstore-index-transact-sql.md#LimitRest) para obtener una lista de los tipos de datos admitidos.  

ON [*database_name*. [*schema_name* ] . | *schema_name* . ] *table_name*  
   Especifica el nombre de una, dos o tres partes de la tabla que contiene el índice.  

#### <a name="with-options"></a>Opciones de WITH
##### <a name="drop_existing--off--on"></a>DROP_EXISTING = [OFF] | ON  
   DROP_EXISTING = ON El índice existente se quita y se vuelve a compilar. El nombre de índice especificado debe ser el mismo que el de un índice actualmente existente; sin embargo, la definición se puede modificar. Por ejemplo, puede especificar distintas columnas u opciones de índice.
  
   DROP_EXISTING = OFF Se muestra un error si ya existe el nombre de índice especificado. El tipo de índice no puede cambiarse utilizando DROP_EXISTING. En la sintaxis compatible con versiones anteriores, WITH DROP_EXISTING es equivalente a WITH DROP_EXISTING = ON.  

###### <a name="maxdop--max_degree_of_parallelism"></a>MAXDOP = *max_degree_of_parallelism*  
   Reemplaza la opción [Establecer la opción de configuración del servidor Grado máximo de paralelismo](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) durante la operación de índice. Utilice MAXDOP para establecer un límite para el número de procesadores utilizados en la ejecución de un plan paralelo. El máximo es 64 procesadores.  
  
   Los valores de *max_degree_of_parallelism* pueden ser:  
   - 1: suprime la generación de planes paralelos.  
   - \>1: restringe el número máximo de procesadores usados en una operación de índice paralelo al número especificado o a un número inferior en función de la actual carga de trabajo del sistema. Por ejemplo, si MAXDOP = 4, el número de procesadores usados es de 4 o menos.  
   - 0 (predeterminado): usa el número real de procesadores o menos, según la carga de trabajo actual del sistema.  
  
   Para obtener más información, vea [Configurar operaciones de índice en paralelo](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!NOTE]
>  Las operaciones de índices en paralelo no están disponibles en todas las ediciones de [!INCLUDE[msC](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Características compatibles con las ediciones de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
###### <a name="online--on--off"></a>ONLINE = [ON | OFF]   
- `ON` especifica que el índice de almacén de columnas permanece en línea y disponible mientras se compila la nueva copia del índice.
- `OFF` especifica que el índice no está disponible mientras se compila la nueva copia. En índices no agrupados, la tabla base permanece disponible, solo el índice de almacén de columnas no agrupado no se usa para responder a consultas hasta que finaliza el nuevo índice. 

```sql
CREATE COLUMNSTORE INDEX ncci ON Sales.OrderLines (StockItemID, Quantity, UnitPrice, TaxRate) WITH ( ONLINE = ON );
```

##### <a name="compression_delay--0--delayminutes"></a>COMPRESSION_DELAY = **0** | \<delay>[Minutos]  
   Especifica un límite inferior para el tiempo que una fila debe permanecer en el grupo de filas delta antes de que se pueda migrar a un grupo de filas comprimido. Por ejemplo, un cliente puede indicar que si una fila no se ha modificado durante 120 minutos, se pueda comprimir en formato de almacenamiento en columnas. En el índice de almacén de columnas de las tablas basadas en disco, no se realiza el seguimiento del tiempo de inserción o actualización de una fila; en su lugar, se usa el tiempo de cierre del grupo de filas de diferencial como elemento intermedio de la fila. La duración predeterminada es 0 minutos. Una fila se migra al almacenamiento en columnas una vez que en el grupo de filas delta se han acumulado 1 millón de filas y se ha marcado como cerrado.  
  
###### <a name="data_compression"></a>DATA_COMPRESSION  
   Especifica la opción de compresión de datos para la tabla, el número de partición o el intervalo de particiones especificados. Solo se aplica a los índices de almacén de columnas, incluidos los clúster y no clúster. Las opciones son las siguientes:
   
- `COLUMNSTORE` es el valor predeterminado y especifica que se comprima con la compresión de almacén de columnas que ofrezca el mejor rendimiento. Es la elección habitual.  
- `COLUMNSTORE_ARCHIVE` comprime la tabla o la partición a un tamaño menor. Esto se puede usar para el archivado o para otras situaciones que requieran un tamaño de almacenamiento mínimo y pueda permitirse más tiempo para el almacenamiento y recuperación.  
  
 Para más información sobre la compresión, vea [Compresión de datos](../../relational-databases/data-compression/data-compression.md).  
  
##### <a name="where-filter_expression--and-filter_expression-"></a>WHERE \<filter_expression> [ AND \<filter_expression> ]
  
   Si se ha llamado a un predicado de filtro, especifica qué filas se va a incluir en el índice. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crea estadísticas filtradas sobre las filas de datos en el índice filtrado.  
  
   El predicado de filtro usa lógica de comparación simple. Las comparaciones que utilizan literales NULL no se admiten con los operadores de comparación. En su lugar, use los operadores IS NULL e IS NOT NULL.  
  
   A continuación, se muestran algunos ejemplos de predicados de filtro para la tabla `Production.BillOfMaterials`:  
   `WHERE StartDate > '20000101' AND EndDate <= '20000630'`    
   `WHERE ComponentID IN (533, 324, 753)`  
   `WHERE StartDate IN ('20000404', '20000905') AND EndDate IS NOT NULL`  
   
   Para obtener orientación sobre índices filtrados, vea [Crear índices filtrados](../../relational-databases/indexes/create-filtered-indexes.md).  
  
#### <a name="on-options"></a>Opciones de ON  
   Estas opciones especifican los grupos de archivos en los que se crea el índice.  
  
*partition_scheme_name* **(** _column_name_ **)**  
   Especifica el esquema de partición que define los grupos de archivos a los que se asignan las particiones de un índice con particiones. El esquema de partición debe existir dentro de la base de datos mediante la ejecución de [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md). 
   *column_name* especifica la columna en la que se van a crear las particiones de un índice con particiones. Esta columna debe coincidir con el tipo de datos, la longitud y la precisión del argumento de la función de partición que *partition_scheme_name* emplea. *column_name* no está limitado a las columnas de la definición de índice. Al crear particiones en un índice de almacén de columnas, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] agrega la columna de partición como una columna del índice, si no se especificó todavía.  
   Si no se especificó *partition_scheme_name* o *filegroup* y se crearon particiones en la tabla, el índice se coloca en el mismo esquema de partición y usa la misma columna de partición que en la tabla subyacente.  
   Un índice de almacén de columnas de una tabla con particiones debe estar alineado.  
   Para obtener más información sobre los índices con particiones, vea [Partitioned Tables and Indexes](../../relational-databases/partitions/partitioned-tables-and-indexes.md) (Tablas e índices con particiones).  

*filegroup_name*  
   Especifica el nombre de un grupo de archivos en el que se va a crear el índice. Si no se especifica *filegroup_name* y la tabla no tiene particiones, el índice usa el mismo grupo de archivos que la tabla subyacente. El grupo de archivos debe existir previamente.  
 
**"** default **"**  
Crea el índice especificado en el grupo de archivos predeterminado.  
  
El término predeterminado (default), en este contexto, no es una palabra clave. Es un identificador para el grupo de archivos predeterminado y debe delimitarse, como en ON **"** default **"** u ON **[** default **]** . Si se especifica "default", la opción QUOTED_IDENTIFIER debe tener el valor ON para la sesión actual. Esta es la configuración predeterminada. Para obtener más información, vea [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
##  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Requiere el permiso ALTER en la tabla.  
  
##  <a name="general-remarks"></a><a name="GenRemarks"></a> Notas generales  
Se puede crear un índice de almacén de columnas en una tabla temporal. Cuando se quita la tabla o finaliza la sesión, también se quita el índice.  

Se puede crear un índice de almacén de columnas agrupadas y ordenado en columnas de cualquier tipo de datos que se admita en [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], excepto en columnas de cadena.  
 
## <a name="filtered-indexes"></a>Índices filtrados  
Un índice filtrado es un índice no clúster optimizado, adecuado para las consultas que seleccionan un porcentaje pequeño de las filas de una tabla. Utiliza un predicado de filtro para indizar una parte de los datos de la tabla. Un índice filtrado bien diseñado puede mejorar el rendimiento de las consultas, reducir los costos de almacenamiento y de mantenimiento.  
  
### <a name="required-set-options-for-filtered-indexes"></a>Opciones SET requeridas para los índices filtrados  
Las opciones SET de la columna de valor requerido son necesarias siempre que se dé alguna de las condiciones siguientes:  
- Se crea un índice filtrado.  
- La operación INSERT, UPDATE, DELETE o MERGE modifica los datos de un índice filtrado.  
- El optimizador de consultas usa el índice filtrado para crear el plan de consulta.  
  
    |Opciones de Set|Valor requerido|Valor de servidor predeterminado|Valor predeterminado<br /><br /> Valor de OLE DB y ODBC|Valor predeterminado<br /><br /> predeterminado|  
    |-----------------|--------------------|--------------------------|---------------------------------------|-----------------------------------|  
    |ANSI_NULLS|ACTIVAR|ACTIVAR|ACTIVAR|Apagado|  
    |ANSI_PADDING|ACTIVAR|ACTIVAR|ACTIVAR|Apagado|  
    |ANSI_WARNINGS*|ACTIVAR|ACTIVAR|ACTIVAR|Apagado|  
    |ARITHABORT|ACTIVAR|ACTIVAR|Apagado|Apagado|  
    |CONCAT_NULL_YIELDS_NULL|ACTIVAR|ACTIVAR|ACTIVAR|Apagado|  
    |NUMERIC_ROUNDABORT|Apagado|Apagado|Apagado|Apagado|  
    |QUOTED_IDENTIFIER|ACTIVAR|ACTIVAR|ACTIVAR|Apagado|   
  
     *Al establecer ANSI_WARNINGS en ON, ARITHABORT se establece de forma implícita en ON cuando el nivel de compatibilidad de base de datos está establecido en 90 o un valor superior. Si el nivel de compatibilidad de la base de datos está establecido en 80 o en un nivel inferior, debe configurarse explícitamente la opción ARITHABORT en ON.  
  
 Si las opciones SET son incorrectas, se pueden producir las condiciones siguientes:  
  
-   El índice filtrado no se crea.  
  
-   El [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera un error y revierte cualquier instrucción INSERT, UPDATE, DELETE o MERGE que cambia los datos del índice.  
  
-   El optimizador de consultas no tiene en cuenta el índice en el plan de ejecución de ninguna instrucción Transact-SQL.  
  
 Para obtener más información sobre los índices filtrados, vea [Crear índices filtrados](../../relational-databases/indexes/create-filtered-indexes.md). 
  
##  <a name="limitations-and-restrictions"></a><a name="LimitRest"></a> Limitaciones y restricciones  

**Cada columna de un índice de almacén de columnas debe ser de uno de los tipos de datos empresariales comunes siguientes:** 
-   datetimeoffset [ ( *n* ) ]  
-   datetime2 [ ( *n* ) ]  
-   datetime  
-   smalldatetime  
-   date  
-   time [ ( *n* ) ]  
-   float [ ( *n* ) ]  
-   real [ ( *n* ) ]  
-   decimal [ ( *precision* [ *, scale* ] **)** ]
-   numeric [ ( *precision* [ *, scale* ] **)** ]    
-   money  
-   SMALLMONEY  
-   bigint  
-   int  
-   SMALLINT  
-   TINYINT  
-   bit  
-   nvarchar [ ( *n* ) ] 
-   nvarchar (max) [se aplica a [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] y al nivel Premium, al nivel estándar (S3 y versiones posteriores) y a todos los niveles de ofertas de núcleo virtual, solo en los índices de almacén de columnas agrupado]   
-   nchar [ ( *n* ) ]  
-   varchar [ ( *n* ) ]  
-   varchar (max) [se aplica a [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] y al nivel Premium, al nivel estándar (S3 y versiones posteriores) y a todos los niveles de ofertas de núcleo virtual, solo en los índices de almacén de columnas agrupado]
-   char [ ( *n* ) ]  
-   varbinary [ ( *n* ) ] 
-   varbinary (max) [se aplica a [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] y a Azure SQL Database en el nivel Premium, en el nivel estándar (S3 y versiones posteriores) y en todos los niveles de ofertas de núcleo virtual, solo en los índices de almacén de columnas agrupado]
-   binary [ ( *n* ) ]  
-   uniqueidentifier (Se aplica a [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores)
  
Si la tabla subyacente tiene una columna con un tipo de datos no admitido para los índices de almacén de columnas, debe omitir esa columna del índice no clúster de almacén de columnas.  
  
**Las columnas que usan alguno de los siguientes tipos de datos no pueden incluirse en un índice de almacén de columnas:**
-   ntext, text e image  
-   nvarchar(max), varchar(max) y varbinary(max) (Se aplica a [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones anteriores e índices no clúster de almacén de columnas) 
-   rowversion (y timestamp)  
-   sql_variant  
-   Tipos CLR (hierarchyid y tipos espaciales)  
-   Xml  
-   uniqueidentifier (Se aplica a [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])  

**Índices no clúster de almacén de columnas:**
-   No puede tener más de 1024 columnas.
-   No se pueden crear como índice basado en restricciones. Se pueden tener restricciones únicas, restricciones de clave principal y restricciones de clave externa en una tabla con un índice de almacén de columnas. Las restricciones se aplican siempre con un índice de almacén de filas. Las restricciones no se pueden aplicar con un índice de almacén de columnas (agrupado o no agrupado).
-   No puede incluir ninguna columna dispersa.  
-   No se pueden modificar mediante la instrucción **ALTER INDEX**. Para cambiar el índice no clúster, debe quitar y volver a crear el índice de almacén de columnas en su lugar. Puede usar **ALTER INDEX** para deshabilitar y volver a compilar un índice de almacén de columnas.  
-   No se pueden crear mediante la palabra clave **INCLUDE**.  
-   No pueden incluir las palabras clave **ASC** ni **DESC** para ordenar el índice. Los índices de almacén de columnas se ordenan de acuerdo con los algoritmos de compresión. La ordenación eliminaría muchas mejoras de rendimiento.  
-   No se pueden incluir columnas de objetos grandes (LOB) de tipo nvarchar(max), varchar(max) y varbinary(max) en índices no clúster de almacén de columnas. Solo los índices clúster de almacén de columnas admiten tipos LOB, a partir de la versión [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] y Azure SQL Database configurados en el nivel Premium, nivel estándar (S3 y posteriores) y en todos los niveles de ofertas de núcleo virtual. Tenga en cuenta que las versiones anteriores no admiten tipos LOB en los índices clúster y no clúster de almacén de columnas.


> [!NOTE]  
> A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], se puede crear un índice de almacén de columnas no agrupado en una vista indexada.  


 **Los índices de almacén de columnas no se pueden combinar con las siguientes características:**  
-   Columnas calculadas A partir de SQL Server 2017, un índice clúster de almacén de columnas puede contener una columna calculada no persistente. Pero en SQL Server 2017, los índices clúster de almacén de columnas no pueden contener columnas calculadas persistentes y no se pueden crear índices no clúster en columnas calculadas. 
-   Compresión de página y fila, y formato de almacenamiento **vardecimal** (un índice de almacén de columnas ya está comprimido en otro formato).  
-   Replicación  
-   Secuencia de archivos

No puede usar cursores ni desencadenadores en una tabla con un índice clúster de almacén de columnas. Esta restricción no se aplica a los índices no clúster de almacén de columnas; puede usar cursores y desencadenadores en una tabla con un índice no clúster de almacén de columnas.

Limitaciones específicas de **[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]**  
Estas limitaciones solo se aplican a [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]. En esta versión se presentaron los índices clúster de almacén de columnas actualizables. Los índices no clúster de almacén de consultas seguían siendo de solo lectura.  

-   Seguimiento de cambios. No se puede usar el seguimiento de cambios con índices de almacén de columnas.  
-   Captura de datos modificados. La captura de datos modificados no se puede habilitar en las tablas con un índice de almacén de columnas agrupado. A partir de SQL Server 2016, se puede habilitar en las tablas con un índice de almacén de columnas no agrupado.  
-   Secundario legible. No se puede acceder a un índice clúster de almacén de columnas (CCI) desde un secundario legible de un grupo de disponibilidad Always On legible.  Puede acceder a un índice no clúster de almacén de columnas (NCCI) desde un secundario legible.  
-   Conjuntos de resultados activos múltiples (MARS). [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] usa MARS para las conexiones de solo lectura a las tablas con un índice de almacén de columnas. Pero [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] no es compatible con MARS para operaciones simultáneas de lenguaje de manipulación de datos (DML) en una tabla con un índice de almacén de columnas. Cuando ocurre esto, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] termina las conexiones y anula las transacciones.  
-  Los índices de almacén de columnas no agrupados no se pueden crear en una vista o vista indexada.
  
 Para obtener más información sobre las ventajas de rendimiento y las limitaciones de los índices de almacén de columnas, vea [Introducción a los índices de almacén de columnas](../../relational-databases/indexes/columnstore-indexes-overview.md).
  
##  <a name="metadata"></a><a name="Metadata"></a> Metadatos  
 Todas las columnas de un índice de almacén de columnas se almacenan en los metadatos como columnas incluidas. El índice de almacén de columnas no tiene columnas de clave. Estas vistas del sistema proporcionan información sobre los índices de almacén de columnas.  
  
-   [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)  
-   [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
-   [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)  
-   [sys.column_store_segments &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-segments-transact-sql.md)  
-   [sys.column_store_dictionaries &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-dictionaries-transact-sql.md)  
-   [sys.column_store_row_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-row-groups-transact-sql.md)  

##  <a name="examples-for-converting-a-rowstore-table-to-columnstore"></a><a name="convert"></a> Ejemplos para convertir una tabla de almacén de filas en almacén de columnas  
  
### <a name="a-convert-a-heap-to-a-clustered-columnstore-index"></a>A. Convertir un montón en un índice clúster de almacén de columnas  
 En este ejemplo se crea una tabla como un montón y después se convierte en un índice clúster de almacén de columnas denominado cci_Simple. Esto cambia el almacenamiento de la tabla de un almacén de filas a un almacén de columnas.  
  
```sql  
CREATE TABLE SimpleTable(  
    ProductKey [INT] NOT NULL,   
    OrderDateKey [INT] NOT NULL,   
    DueDateKey [INT] NOT NULL,   
    ShipDateKey [INT] NOT NULL);  
GO  
CREATE CLUSTERED COLUMNSTORE INDEX cci_Simple ON SimpleTable;  
GO  
```  
  
### <a name="b-convert-a-clustered-index-to-a-clustered-columnstore-index-with-the-same-name"></a>B. Convertir un índice clúster en un índice clúster de almacén de columnas con el mismo nombre.  
 En este ejemplo se crea la tabla con un índice clúster y después se muestra la sintaxis para convertir el índice clúster en un índice clúster de almacén de columnas. Esto cambia el almacenamiento de la tabla de un almacén de filas a un almacén de columnas.  
  
```sql  
CREATE TABLE SimpleTable (  
    ProductKey [INT] NOT NULL,   
    OrderDateKey [INT] NOT NULL,   
    DueDateKey [INT] NOT NULL,   
    ShipDateKey [INT] NOT NULL);  
GO  
CREATE CLUSTERED INDEX cl_simple ON SimpleTable (ProductKey);  
GO  
CREATE CLUSTERED COLUMNSTORE INDEX cl_simple ON SimpleTable  
WITH (DROP_EXISTING = ON);  
GO  
```  
  
### <a name="c-handle-nonclustered-indexes-when-converting-a-rowstore-table-to-a-columnstore-index"></a>C. Administrar índices no clúster al convertir una tabla de almacén de filas en un índice de almacén de columnas.  
 En este ejemplo se muestra cómo administrar índices no clúster al convertir una tabla de almacén de filas en un índice de almacén de columnas. En realidad, a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], no es necesaria ninguna acción especial; [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] define automáticamente y vuelve a compilar los índices no clúster en el nuevo índice clúster de almacén de columnas.  
  
 Si quiere quitar los índices no clúster, use la instrucción DROP INDEX antes de crear el índice de almacén de columnas. La opción DROP EXISTING solo quita el índice clúster que se va a convertir. No quita los índices no clúster.  
  
 En [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], no se podía crear un índice no clúster en un índice de almacén de columnas. Este ejemplo muestra cómo en versiones anteriores es necesario quitar los índices no clúster antes de crear el índice de almacén de columnas.  
  
```sql  
--Create the table for use with this example.  
CREATE TABLE SimpleTable (  
    ProductKey [INT] NOT NULL,   
    OrderDateKey [INT] NOT NULL,   
    DueDateKey [INT] NOT NULL,   
    ShipDateKey [INT] NOT NULL);  
GO  
  
--Create two nonclustered indexes for use with this example  
CREATE INDEX nc1_simple ON SimpleTable (OrderDateKey);  
CREATE INDEX nc2_simple ON SimpleTable (DueDateKey);   
GO  
  
--SQL Server 2012 and SQL Server 2014: you need to drop the nonclustered indexes  
--in order to create the columnstore index.   
  
DROP INDEX SimpleTable.nc1_simple;  
DROP INDEX SimpleTable.nc2_simple;  
  
--Convert the rowstore table to a columnstore index.  
CREATE CLUSTERED COLUMNSTORE INDEX cci_simple ON SimpleTable;   
GO  
```  
  
### <a name="d-convert-a-large-fact-table-from-rowstore-to-columnstore"></a>D. Convertir una tabla de hechos grande de almacén de filas en almacén de columnas  
 En este ejemplo se explica cómo convertir una tabla de hechos grande desde una tabla de almacén de filas en una tabla de almacén de columnas.  
  
 Para convertir una tabla de almacén de filas en una tabla de almacén de columnas.  
  
1.  En primer lugar, cree una tabla pequeña para usar en este ejemplo.  
  
    ```sql  
    --Create a rowstore table with a clustered index and a nonclustered index.  
    CREATE TABLE MyFactTable (  
        ProductKey [INT] NOT NULL,  
        OrderDateKey [INT] NOT NULL,  
         DueDateKey [INT] NOT NULL,  
         ShipDateKey [INT] NOT NULL )  
    )  
    WITH (  
        CLUSTERED INDEX ( ProductKey )  
    );  
  
    --Add a nonclustered index.  
    CREATE INDEX my_index ON MyFactTable ( ProductKey, OrderDateKey );  
    ```  
  
2.  Quite todos los índices no agrupados de la tabla de almacén de filas.  
  
    ```sql  
    --Drop all nonclustered indexes  
    DROP INDEX my_index ON MyFactTable;  
    ```  
  
3.  Quitar el índice clúster.  
  
    -   Haga esto solo si desea especificar un nuevo nombre para el índice cuando se convierta en un índice clúster de almacén de columnas. Si no quita el índice clúster, el nuevo índice clúster de almacén de columnas tiene el mismo nombre.  
  
        > [!NOTE]  
        > El nombre del índice será más fácil de recordar si usa su propio nombre. Todos los índices agrupados de almacén de filas usan el nombre predeterminado, "ClusteredIndex_\<GUID>".  
  
    ```sql  
    --Process for dropping a clustered index.  
    --First, look up the name of the clustered rowstore index.  
    --Clustered rowstore indexes always use the DEFAULT name 'ClusteredIndex_<GUID>'.  
    SELECT i.name   
    FROM sys.indexes i   
    JOIN sys.tables t  
    ON ( i.type_desc = 'CLUSTERED' ) WHERE t.name = 'MyFactTable';  
  
    --Drop the clustered rowstore index.  
    DROP INDEX ClusteredIndex_d473567f7ea04d7aafcac5364c241e09 ON MyDimTable;  
    ```  
  
4.  Convierta la tabla de almacén de filas en una tabla de almacén de columnas con un índice clúster de almacén de columnas.  
  
    ```sql  
    --Option 1: Convert to columnstore and name the new clustered columnstore index MyCCI.  
    CREATE CLUSTERED COLUMNSTORE INDEX MyCCI ON MyFactTable;  
  
    --Option 2: Convert to columnstore and use the rowstore clustered   
    --index name for the columnstore clustered index name.  
    --First, look up the name of the clustered rowstore index.  
    SELECT i.name   
    FROM sys.indexes i  
    JOIN sys.tables t   
    ON ( i.type_desc = 'CLUSTERED' )  
    WHERE t.name = 'MyFactTable';  
  
    --Second, create the clustered columnstore index and   
    --Replace ClusteredIndex_d473567f7ea04d7aafcac5364c241e09  
    --with the name of your clustered index.  
    CREATE CLUSTERED COLUMNSTORE INDEX   
    ClusteredIndex_d473567f7ea04d7aafcac5364c241e09  
     ON MyFactTable  
    WITH DROP_EXISTING = ON;  
    ```  
  
### <a name="e-convert-a-columnstore-table-to-a-rowstore-table-with-a-clustered-index"></a>E. Convertir una tabla de almacén de columnas en una tabla de almacén de filas con un índice clúster  
 Para convertir una tabla de almacén de columnas en una tabla de almacén de filas con un índice clúster, use la instrucción CREATE INDEX con la opción DROP_EXISTING.  
  
```sql  
CREATE CLUSTERED INDEX ci_MyTable   
ON MyFactTable  
WITH ( DROP EXISTING = ON );  
```  
  
### <a name="f-convert-a-columnstore-table-to-a-rowstore-heap"></a>F. Convertir una tabla de almacén de columnas en un montón de almacenes de filas  
 Para convertir una tabla de almacén de columnas en un montón de almacenes de filas, simplemente quite el índice clúster de almacén de columnas.  
  
```sql  
DROP INDEX MyCCI   
ON MyFactTable;  
```  
  

### <a name="g-defragment-by-rebuilding-the-entire-clustered-columnstore-index"></a>G. Desfragmentar mediante la recompilación de todo el índice clúster de almacén de columnas  
   Se aplica a: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
 Hay dos maneras de volver a generar todo el índice clúster de almacén de columnas. Puede usar CREATE CLUSTERED COLUMNSTORE INDEX o [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md) y la opción REBUILD. Con ambos métodos se obtienen los mismos resultados.  
  
> [!NOTE]  
> A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], use `ALTER INDEX...REORGANIZE` en lugar de recompilar con los métodos que se describen en este ejemplo.  
  
```sql  
--Determine the Clustered Columnstore Index name of MyDimTable.  
SELECT i.object_id, i.name, t.object_id, t.name   
FROM sys.indexes i   
JOIN sys.tables t  
ON (i.type_desc = 'CLUSTERED COLUMNSTORE')  
WHERE t.name = 'RowstoreDimTable';  
  
--Rebuild the entire index by using CREATE CLUSTERED INDEX.  
CREATE CLUSTERED COLUMNSTORE INDEX my_CCI   
ON MyFactTable  
WITH ( DROP_EXISTING = ON );  
  
--Rebuild the entire index by using ALTER INDEX and the REBUILD option.  
ALTER INDEX my_CCI  
ON MyFactTable  
REBUILD PARTITION = ALL  
WITH ( DROP_EXISTING = ON );  
```  
  
##  <a name="examples-for-nonclustered-columnstore-indexes"></a><a name="nonclustered"></a> Ejemplos de índices no clúster de almacén de columnas  
  
### <a name="a-create-a-columnstore-index-as-a-secondary-index-on-a-rowstore-table"></a>A. Crear un índice de almacén de columnas en un índice secundario de una tabla de almacén de filas  
 En este ejemplo se crea un índice no clúster de almacén de columnas en una tabla de almacén de filas. En esta situación solo se puede crear un índice de almacén de columnas. El índice de almacén de columnas necesita almacenamiento adicional, ya que contiene una copia de los datos de la tabla de almacén de filas. En el ejemplo se crean una tabla simple y un índice clúster y luego se muestra la sintaxis para crear un índice no clúster de almacén de columnas.  
  
```sql  
CREATE TABLE SimpleTable  
(ProductKey [INT] NOT NULL,   
OrderDateKey [INT] NOT NULL,   
DueDateKey [INT] NOT NULL,   
ShipDateKey [INT] NOT NULL);  
GO  
CREATE CLUSTERED INDEX cl_simple ON SimpleTable (ProductKey);  
GO  
CREATE NONCLUSTERED COLUMNSTORE INDEX csindx_simple  
ON SimpleTable  
(OrderDateKey, DueDateKey, ShipDateKey);  
GO  
```  
  
### <a name="b-create-a-simple-nonclustered-columnstore-index-using-all-options"></a>B. Crear un índice no clúster de almacén de columnas simple con todas las opciones  
 En el ejemplo siguiente se muestra la sintaxis para crear un índice no clúster de almacén de columnas usando todas las opciones.  
  
```sql  
CREATE NONCLUSTERED COLUMNSTORE INDEX csindx_simple  
ON SimpleTable  
(OrderDateKey, DueDateKey, ShipDateKey)  
WITH (DROP_EXISTING =  ON,   
    MAXDOP = 2)  
ON "default"  
GO  
```  
  
 Para obtener un ejemplo más complejo con tablas con particiones, vea [Introducción a los índices de almacén de columnas](../../relational-databases/indexes/columnstore-indexes-overview.md).  
  
### <a name="c-create-a-nonclustered-columnstore-index-with-a-filtered-predicate"></a>C. Crear un índice no clúster de almacén de columnas con un predicado filtrado  
 En el ejemplo siguiente se crea un índice no clúster de almacén de columnas filtrado en la tabla Production.BillOfMaterials de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. El predicado de filtro puede incluir columnas que no son columnas de clave en el índice filtrado. El predicado de este ejemplo selecciona solo las filas en que EndDate no es NULL.  
  
```sql  
IF EXISTS (SELECT name FROM sys.indexes  
    WHERE name = N'FIBillOfMaterialsWithEndDate'   
    AND object_id = OBJECT_ID(N'Production.BillOfMaterials'))  
DROP INDEX FIBillOfMaterialsWithEndDate  
    ON Production.BillOfMaterials;  
GO  
CREATE NONCLUSTERED COLUMNSTORE INDEX "FIBillOfMaterialsWithEndDate"  
    ON Production.BillOfMaterials (ComponentID, StartDate)  
    WHERE EndDate IS NOT NULL;  
```  
  
###  <a name="d-change-the-data-in-a-nonclustered-columnstore-index"></a><a name="ncDML"></a> D. Modificar los datos de un índice no clúster de almacén de columnas  
   Se aplica a: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] a [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].
  
 Una vez creado un índice no clúster de almacén de columnas en una tabla, no puede modificar directamente los datos de esa tabla. Una consulta con INSERT, UPDATE, DELETE o MERGE genera un error y devuelve un mensaje de error. Para agregar o modificar los datos de la tabla, puede hacer lo siguiente:  
  
-   Deshabilitar o quitar el índice de almacén de columnas. Después puede actualizar los datos de la tabla. Si deshabilita el índice de almacén de columnas, puede regenerar el índice de almacén de columnas cuando termine de actualizar los datos. Por ejemplo,  
  
    ```sql  
    ALTER INDEX mycolumnstoreindex ON mytable DISABLE;  
    -- update mytable --  
    ALTER INDEX mycolumnstoreindex on mytable REBUILD  
    ```  
  
-   Cargar datos en una tabla de ensayo que no tenga un índice de almacén de columnas. Genere un índice de almacén de columnas en la tabla de ensayo. Cambie la tabla de ensayo a una partición vacía de la tabla principal.  
  
-   Cambiar una partición de la tabla con el índice de almacén de columnas a una tabla de ensayo vacía. Si hay un índice de almacén de columnas en la tabla de ensayo, deshabilítelo. Realice las actualizaciones que desee. Genere (o regenere) el índice de almacén de columnas. Vuelva a cambiar la tabla de ensayo a la partición (ahora vacía) de la tabla principal.  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-change-a-clustered-index-to-a-clustered-columnstore-index"></a>A. Convertir un índice clúster en un índice clúster de almacén de columnas  
 Mediante la instrucción CREATE CLUSTERED COLUMNSTORE INDEX con DROP_EXISTING = ON, puede:  
  
-   Convertir un índice clúster en un índice clúster de almacén de columnas.  
  
-   Volver a compilar un índice clúster de almacén de columnas.  
  
 En este ejemplo se crea la tabla xDimProduct como una tabla de almacén de filas con un índice clúster y luego se usa CREATE CLUSTERED COLUMNSTORE INDEX para convertir la tabla de almacén de filas en una tabla de almacén de columnas.  
  
```sql  
-- Uses AdventureWorks  
  
IF EXISTS (SELECT name FROM sys.tables  
    WHERE name = N'xDimProduct'  
    AND object_id = OBJECT_ID (N'xDimProduct'))  
DROP TABLE xDimProduct;  
  
--Create a distributed table with a clustered index.  
CREATE TABLE xDimProduct (ProductKey, ProductAlternateKey, ProductSubcategoryKey)  
WITH ( DISTRIBUTION = HASH(ProductKey),  
    CLUSTERED INDEX (ProductKey) )  
AS SELECT ProductKey, ProductAlternateKey, ProductSubcategoryKey FROM DimProduct;  
  
--Change the existing clustered index   
--to a clustered columnstore index with the same name.  
--Look up the name of the index before running this statement.  
CREATE CLUSTERED COLUMNSTORE INDEX <index_name>   
ON xdimProduct   
WITH ( DROP_EXISTING = ON );  
```  
  
### <a name="b-rebuild-a-clustered-columnstore-index"></a>B. Volver a compilar un índice clúster de almacén de columnas  
 A partir del ejemplo anterior, en este ejemplo se usa CREATE CLUSTERED COLUMNSTORE INDEX para volver a compilar el índice clúster de almacén de columnas existente denominado cci_xDimProduct.  
  
```sql  
--Rebuild the existing clustered columnstore index.  
CREATE CLUSTERED COLUMNSTORE INDEX cci_xDimProduct   
ON xdimProduct   
WITH ( DROP_EXISTING = ON );  
```  
  
### <a name="c-change-the-name-of-a-clustered-columnstore-index"></a>C. Cambiar el nombre de un índice clúster de almacén de columnas  
 Para cambiar el nombre de un índice clúster de almacén de columnas, quite el índice clúster de almacén de columnas existente y luego vuelva a crear el índice con un nuevo nombre.  
  
 Se recomienda realizar esta operación solo con una tabla pequeña o una tabla vacía. Se tarda mucho en quitar un índice clúster de almacén de columnas grande y en volver a compilarlo con otro nombre.  
  
 Con el índice clúster de almacén de columnas cci_xDimProduct del ejemplo anterior, en este ejemplo se quita el índice clúster de almacén de columnas cci_xDimProduct y luego se vuelve a crear el índice clúster de almacén de columnas con el nombre mycci_xDimProduct.  
  
```sql  
--For illustration purposes, drop the clustered columnstore index.   
--The table continues to be distributed, but changes to a heap.  
DROP INDEX cci_xdimProduct ON xDimProduct;  
  
--Create a clustered index with a new name, mycci_xDimProduct.  
CREATE CLUSTERED COLUMNSTORE INDEX mycci_xDimProduct  
ON xdimProduct  
WITH ( DROP_EXISTING = OFF );  
```  
  
### <a name="d-convert-a-columnstore-table-to-a-rowstore-table-with-a-clustered-index"></a>D. Convertir una tabla de almacén de columnas en una tabla de almacén de filas con un índice clúster  
 Puede haber una situación en la que quiera quitar un índice clúster de almacén de columnas y crear un índice clúster. Así se almacena la tabla en formato de almacén de filas. En este ejemplo se convierte una tabla de almacén de columnas en una tabla de almacén de filas con un índice clúster con el mismo nombre. No se pierde ningún dato. Todos los datos van a la tabla de almacén de filas y las columnas enumeradas se convierten en las columnas de clave del índice clúster.  
  
```sql  
--Drop the clustered columnstore index and create a clustered rowstore index.   
--All of the columns are stored in the rowstore clustered index.   
--The columns listed are the included columns in the index.  
CREATE CLUSTERED INDEX cci_xDimProduct    
ON xdimProduct (ProductKey, ProductAlternateKey, ProductSubcategoryKey, WeightUnitMeasureCode)  
WITH ( DROP_EXISTING = ON);  
```  
  
### <a name="e-convert-a-columnstore-table-back-to-a-rowstore-heap"></a>E. Volver a convertir una tabla de almacén de columnas en un montón de almacén de filas  
 Use [DROP INDEX (SQL Server PDW)](drop-index-transact-sql.md) para quitar el índice clúster de almacén de columnas y convertir la tabla en un montón de almacén de filas. En este ejemplo se convierte la tabla cci_xDimProduct en un montón de almacén de filas. La tabla se sigue distribuyendo, pero se almacena como un montón.  
  
```sql  
--Drop the clustered columnstore index. The table continues to be distributed, but changes to a heap.  
DROP INDEX cci_xdimProduct ON xdimProduct;  
```  

### <a name="f-create-an-ordered-clustered-columnstore-index-on-a-table-with-no-index"></a>F. Crear un índice de almacén de columnas agrupado ordenado en una tabla sin índice

```sql
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
ORDER ( SHIPDATE );
```

### <a name="g-convert-a-clustered-columnstore-index-to-an-ordered-clustered-columnstore-index"></a>G. Convertir un índice de almacén de columnas agrupado en un índice de almacén de columnas agrupado ordenado

```sql  
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
ORDER ( SHIPDATE );
WITH (DROP_EXISTING = ON)
```

### <a name="h-add-a-column-to-the-ordering-of-an-ordered-clustered-columnstore-index"></a>H. Agregar una columna a la ordenación de un índice de almacén de columnas agrupado ordenado

```sql
-- The original ordered clustered columnstore index was ordered on SHIPDATE column only.  Add PRODUCTKEY column to the ordering.
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
ORDER ( SHIPDATE, PRODUCTKEY );
WITH (DROP_EXISTING = ON)
```
### <a name="i-change-the-ordinal-of-ordered-columns"></a>I. Cambiar el ordinal de las columnas ordenadas  
```sql
-- The original ordered clustered columnstore index was ordered on SHIPDATE, PRODUCTKEY.  Change the ordering to PRODUCTKEY, SHIPDATE.  
CREATE CLUSTERED COLUMNSTORE INDEX cci ON Sales.OrderLines
ORDER ( PRODUCTKEY,SHIPDATE );
WITH (DROP_EXISTING = ON)
```