---
description: ALTER INDEX (Transact-SQL)
title: ALTER INDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/21/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER INDEX
- ALTER_INDEX_TSQL
dev_langs:
- t-sql
helpviewer_keywords:
- indexes [SQL Server], reorganizing
- ALTER INDEX statement
- indexes [SQL Server], disabling
- online index operations
- index reorganization [SQL Server]
- ALLOW_ROW_LOCKS option
- ALL keyword
- reorganizing indexes
- constraints [SQL Server], indexes
- row locks [SQL Server]
- index rebuilding [SQL Server]
- rebuilding indexes
- locking [SQL Server], indexes
- partitioned indexes [SQL Server], rebuilding
- defragmenting indexes
- disabling indexes
- XML indexes [SQL Server], modifying
- index modifications [SQL Server]
- indexes [SQL Server], modifying
- index options [SQL Server]
- modifying indexes
- index disabling [SQL Server]
- MAXDOP index option, ALTER INDEX statement
- spatial indexes [SQL Server], modifying
- indexes [SQL Server], options
- ALLOW_PAGE_LOCKS option
- page locks [SQL Server]
- index rebuild [SQL Server]
- index reorganize [SQL Server]
ms.assetid: b796c829-ef3a-405c-a784-48286d4fb2b9
author: pmasl
ms.author: pelopes
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2ec8737d01b52db828dbe95cd1252d8913128c90
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339310"
---
# <a name="alter-index-transact-sql"></a>ALTER INDEX (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Modifica un índice existente de una tabla o una vista (almacén de filas, almacén de columnas o XML) mediante su deshabilitación, regeneración o reorganización, o mediante el establecimiento de sus opciones.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database
  
ALTER INDEX { index_name | ALL } ON <object>  
{  
      REBUILD {  
            [ PARTITION = ALL ] [ WITH ( <rebuild_index_option> [ ,...n ] ) ]   
          | [ PARTITION = partition_number [ WITH ( <single_partition_rebuild_index_option> ) [ ,...n ] ]  
      }  
    | DISABLE  
    | REORGANIZE  [ PARTITION = partition_number ] [ WITH ( <reorganize_option>  ) ]  
    | SET ( <set_index_option> [ ,...n ] )   
    | RESUME [WITH (<resumable_index_options>,[...n])]
    | PAUSE
    | ABORT
}  
[ ; ]  
  
<object> ::=   
{  
    { database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }  
}  
  
<rebuild_index_option > ::=  
{  
      PAD_INDEX = { ON | OFF }  
    | FILLFACTOR = fillfactor   
    | SORT_IN_TEMPDB = { ON | OFF }  
    | IGNORE_DUP_KEY = { ON | OFF }  
    | STATISTICS_NORECOMPUTE = { ON | OFF }  
    | STATISTICS_INCREMENTAL = { ON | OFF }  
    | ONLINE = {   
          ON [ ( <low_priority_lock_wait> ) ]   
        | OFF } 
    | RESUMABLE = { ON | OFF } 
    | MAX_DURATION = <time> [MINUTES}     
    | ALLOW_ROW_LOCKS = { ON | OFF }  
    | ALLOW_PAGE_LOCKS = { ON | OFF }  
    | MAXDOP = max_degree_of_parallelism  
    | DATA_COMPRESSION = { NONE | ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE }   
        [ ON PARTITIONS ( {<partition_number> [ TO <partition_number>] } [ , ...n ] ) ]  
}  
  
<single_partition_rebuild_index_option> ::=  
{  
      SORT_IN_TEMPDB = { ON | OFF }  
    | MAXDOP = max_degree_of_parallelism  
    | RESUMABLE = { ON | OFF } 
    | MAX_DURATION = <time> [MINUTES}     
    | DATA_COMPRESSION = { NONE | ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE } }  
    | ONLINE = { ON [ ( <low_priority_lock_wait> ) ] | OFF }  
}  
  
<reorganize_option>::=  
{  
       LOB_COMPACTION = { ON | OFF }  
    |  COMPRESS_ALL_ROW_GROUPS =  { ON | OFF}  
}  
  
<set_index_option>::=  
{  
      ALLOW_ROW_LOCKS = { ON | OFF }  
    | ALLOW_PAGE_LOCKS = { ON | OFF }  
    | OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | OFF }
    | IGNORE_DUP_KEY = { ON | OFF }  
    | STATISTICS_NORECOMPUTE = { ON | OFF }  
    | COMPRESSION_DELAY= { 0 | delay [Minutes] }  
}  

<resumable_index_option> ::=
 { 
    MAXDOP = max_degree_of_parallelism
    | MAX_DURATION =<time> [MINUTES]
    | <low_priority_lock_wait>  
 }
 
<low_priority_lock_wait>::=  
{  
    WAIT_AT_LOW_PRIORITY ( MAX_DURATION = <time> [ MINUTES ] ,   
                          ABORT_AFTER_WAIT = { NONE | SELF | BLOCKERS } )  
}  

```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse 
  
ALTER INDEX { index_name | ALL }  
    ON   [ schema_name. ] table_name  
{  
      REBUILD {  
            [ PARTITION = ALL [ WITH ( <rebuild_index_option> ) ] ] 
          | [ PARTITION = partition_number [ WITH ( <single_partition_rebuild_index_option> )] ] 
      }  
    | DISABLE  
    | REORGANIZE [ PARTITION = partition_number ]  
}  
[;]  

<rebuild_index_option > ::=   
{  
    DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }
        [ ON PARTITIONS ( {<partition_number> [ TO <partition_number>] } [ , ...n ] ) ]   
}

<single_partition_rebuild_index_option > ::=   
{  
    DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }  
}  
  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos

 *index_name*  
 Es el nombre del índice. Los nombres de índice deben ser únicos en una tabla o vista, pero no es necesario que sean únicos en una base de datos. Los nombres de índice deben seguir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md).  
  
 ALL  
 Especifica todos los índices asociados a la tabla o vista independientemente del tipo de índice. Si se especifica ALL y uno o más índices se encuentran en un grupo de archivos sin conexión o de solo lectura o la operación especificada no está permitida en uno o más tipos de índices, se produce un error en la instrucción. En la siguiente tabla se enumeran las operaciones de índice y los tipos de índices no permitidos.  
  
|Usar la palabra clave ALL con esta operación|Se produce un error si la tabla tiene uno o más|  
|----------------------------------------|----------------------------------------|  
|REBUILD WITH ONLINE = ON|Índice XML<br /><br /> Índice espacial<br /><br /> Índice de almacén de columnas: **Se aplica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|REBUILD PARTITION = *partition_number*|Índice sin particiones, índice XML, índice espacial o índice deshabilitado|  
|REORGANIZE|Índices que tienen ALLOW_PAGE_LOCKS establecido en OFF|  
|REORGANIZE PARTITION = *partition_number*|Índice sin particiones, índice XML, índice espacial o índice deshabilitado|  
|IGNORE_DUP_KEY = ON|Índice XML<br /><br /> Índice espacial<br /><br /> Índice de almacén de columnas: **Se aplica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|ONLINE = ON|Índice XML<br /><br /> Índice espacial<br /><br /> Índice de almacén de columnas: **Se aplica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|
|RESUMABLE = ON| Índices reanudables no compatibles con la palabra clave **All**. <br /><br /> **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] |   
  
> [!WARNING]
> Para saber más sobre las operaciones de índices que pueden realizarse en línea, vea [Directrices para operaciones de índices en línea](../../relational-databases/indexes/guidelines-for-online-index-operations.md).

 Si se especifica ALL con PARTITION = *partition_number*, es necesario alinear todos los índices. Esto significa que se crean particiones basadas en las funciones de partición equivalentes. Si se utiliza ALL con PARTITION, todas las particiones de índice con el mismo parámetro *partition_number* se vuelven a generar u organizar. Para obtener más información acerca de los índices con particiones, vea [Partitioned Tables and Indexes](../../relational-databases/partitions/partitioned-tables-and-indexes.md).  
  
 *database_name*  
 Es el nombre de la base de datos.  
  
 *schema_name*  
 Es el nombre del esquema al que pertenece la tabla o la vista.  
  
 *table_or_view_name*  
 Es el nombre de la tabla o vista asociada al índice. Para mostrar un informe de los índices en un objeto, use la vista de catálogo [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] admite el formato de nombre de tres partes database_name.[schema_name].table_or_view_name, donde database_name es la base de datos actual o database_name es tempdb y table_or_view_name empieza por #.  
  
 REBUILD [ WITH **(** \<rebuild_index_option> [ **,** ... *n*] **)** ]  
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

Especifica que el índice se volverá a generar con unas columnas, un tipo de índice, un atributo de unicidad y un criterio de ordenación idénticos. Esta cláusula es equivalente a [DBCC DBREINDEX](../../t-sql/database-console-commands/dbcc-dbreindex-transact-sql.md). REBUILD habilita un índice deshabilitado. Cuando se regenera un índice clúster, no se vuelven a generar los índices no clúster asociados, a menos que se especifique la palabra clave ALL. Si no se especifican las opciones de índice, se aplican los valores de las opciones de índice existentes que hay almacenados en [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md). Para las opciones de índice cuyos valores no estén almacenados en **sys.indexes**, se aplica el valor predeterminado indicado en la definición del argumento de la opción.  
  
 Si se especifica ALL y la tabla base es un montón, la operación de regeneración no tiene ningún efecto sobre la tabla. Se regeneran los índices no clúster asociados a la tabla.  
  
 Si el modelo de recuperación de base de datos está establecido como simple u optimizado para cargas masivas de registros, la operación de regeneración se puede registrar mínimamente.  
  
> [!NOTE]
> Cuando se regenera un índice XML principal, la tabla de usuario subyacente no está disponible mientras dura esta operación.  
 
Para los índices de almacén de columnas, la operación de regeneración:  
  
-  No usa el criterio de ordenación.  
-  Adquiere un bloqueo exclusivo en la tabla o la partición mientras se produce la regeneración.  Los datos están desconectados y no disponibles durante la recompilación, aunque se use NOLOCK, RCSI o SI.  
-  Vuelve a comprimir todos los datos del almacén de columnas. Hay dos copias del índice de almacén de columnas mientras se está produciendo la regeneración. Cuando se finaliza la recopilación, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] elimina el índice original del almacén de columnas.  

Para obtener más información, vea [Reorganizar y volver a generar índices](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md). 
  
PARTITION  

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Especifica que solamente se va a volver a generar o a reorganizar una partición de un índice. No es posible especificar PARTITION si *index_name* no es un índice con particiones.  
  
 PARTITION = ALL vuelve a generar todas las particiones.  
  
> [!WARNING]
> La creación y regeneración de índices no alineados en una tabla con más de 1.000 particiones es posible, pero no se admite. Si se hace, se puede degradar el rendimiento o consumir excesiva memoria durante estas operaciones. Microsoft recomienda usar solo índices alineados cuando el número de particiones sea superior a 1000.  
  
 *partition_number*  
   
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Es el número de partición de un índice con particiones que se va a volver a generar o a reorganizar. *partition_number* es una expresión constante que puede hacer referencia a variables. Estas incluyen variables o funciones de tipo definido por el usuario y funciones definidas por el usuario, pero no pueden hacer referencia a una instrucción de [!INCLUDE[tsql](../../includes/tsql-md.md)]. *partition_number* debe existir; de lo contrario, se producirá un error en la instrucción.  
  
 WITH **(** \<single_partition_rebuild_index_option> **)**  
   
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 `SORT_IN_TEMPDB`, `MAXDOP` y `DATA_COMPRESSION` son las opciones que se pueden especificar cuando se vuelve a generar una partición única (PARTITION = *partition_number*). No es posible especificar índices XML en una operación para volver a generar una partición única.  
  
 DISABLE  
 Marca el índice como deshabilitado y no disponible para el [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Cualquier índice puede estar deshabilitado. La definición de índice de un índice deshabilitado se conserva en el catálogo del sistema sin datos del índice subyacente. La deshabilitación de un índice clúster evita que los usuarios obtengan acceso a los datos de la tabla subyacente. Para habilitar un índice, utilice ALTER INDEX REBUILD o CREATE INDEX WITH DROP_EXISTING. Para más información, vea [Habilitar índices y restricciones](../../relational-databases/indexes/disable-indexes-and-constraints.md) y [Deshabilitar índices y restricciones](../../relational-databases/indexes/enable-indexes-and-constraints.md).  
  
 REORGANICE un índice de **almacén de filas**  
 Para los índices de almacén de filas, REORGANIZE especifica que se reorganizará el nivel hoja del índice. La operación REORGANIZE:  
  
-   Siempre se realiza en línea. Esto significa que los bloqueos de tabla a largo plazo no se mantienen y que las consultas o actualizaciones en la tabla subyacente pueden continuar durante la transacción ALTER INDEX REORGANIZE.
-   No se permite para un índice deshabilitado.
-   No se permite cuando ALLOW_PAGE_LOCKS está desactivado.
-   No se revierte cuando se realiza como parte de una transacción y la transacción se revierte.

> [!NOTE]
> Cuando ALTER INDEX REORGANIZE usa transacciones explícitas (por ejemplo, ALTER INDEX dentro de una instrucción BEGIN TRAN... COMMIT/ROLLBACK) en lugar del modo de transacción implícita predeterminado, el comportamiento de REORGANIZE es más restrictivo, lo que puede provocar un bloqueo. Para más información sobre las transacciones implícitas, vea [SET IMPLICIT_TRANSACTIONS (Transact-SQL)](../../t-sql/statements/set-implicit-transactions-transact-sql.md).

Para obtener más información, vea [Reorganizar y volver a generar índices](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md). 

REORGANIZE WITH **(** LOB_COMPACTION = { **ON** | OFF } **)**  
 Se aplica a índices de almacén de filas.  
  
LOB_COMPACTION = ON  
  
-   Especifica que se deben compactar todas las páginas que contienen datos de estos tipos de datos de objetos grandes (LOB): image, text, ntext, varchar(max), nvarchar(max), varbinary(max) y xml. Compactar estos datos puede reducir el tamaño de los datos en disco.  
-   Para un índice agrupado, se compactan todas las columnas LOB que figuran en la tabla.  
-   Para un índice no agrupado, se compactan todas las columnas LOB que son columnas sin clave (incluidas) en el índice.  
-   REORGANIZE ALL realiza LOB_COMPACTION en todos los índices. Para cada índice, se compactan todas las columnas LOB en el índice agrupado, la tabla subyacente o las columnas incluidas en un índice no agrupado.  
  
LOB_COMPACTION = OFF  
  
-   Las páginas que contienen datos de objetos grandes no se compactan.  
-   OFF no tiene ningún efecto sobre un montón.  
  
 REORGANICE un índice de **almacén de columnas**  
 Para índices de almacén de columnas, REORGANIZE comprime cada grupo de filas delta con el estado CLOSED en el almacén de columnas como un grupo de filas comprimido. La operación REORGANIZE siempre se efectúa en línea. Esto significa que los bloqueos de tabla a largo plazo no se mantienen y que las consultas o actualizaciones en la tabla subyacente pueden continuar durante la transacción ALTER INDEX REORGANIZE. Para obtener más información, vea [Reorganizar y volver a generar índices](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md). 
  
-   No es necesario usar REORGANIZE para mover grupos de filas delta con el estado CLOSED a los grupos de filas comprimidos. El proceso de motor de tupla (TM) en segundo plano se activa periódicamente para comprimir los grupos de filas delta con el estado CLOSED. Se recomienda usar REORGANIZE cuando el motor de tupla se retrase. REORGANIZE puede comprimir grupos de filas de una forma más agresiva.  
-   Para comprimir todos los grupos de filas con los estados OPEN y CLOSED, vea la opción `REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS)` en esta sección.  
  
Para los índices de almacén de columnas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], REORGANIZE realiza estas otras optimizaciones de desfragmentación en línea:  
  
-   Quita físicamente las filas de un grupo de filas cuando el 10 % o más de las filas se hayan eliminado lógicamente. Los bytes eliminados se reclaman en los medios físicos. Por ejemplo, si en un grupo de filas comprimido que contiene un millón de filas se eliminan 100 000 filas, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] quita las filas eliminadas y vuelve a comprimir el grupo de filas con 900 000 filas. Ahorra almacenamiento mediante la eliminación de filas eliminadas.  
  
-   Combina uno o varios grupos de filas comprimidos para aumentar las filas por grupo de filas, hasta alcanzar el máximo de 1 048 576 filas. Por ejemplo, si importa de forma masiva 5 lotes de 102 400 filas, obtendrá 5 grupos de filas comprimidos. Si ejecuta REORGANIZE, estos grupos de filas se combinarán en un grupo de filas comprimido del tamaño de 512 000 filas. Se supone que no había ninguna limitación de memoria o de tamaño de diccionario.  
  
-   Para los grupos de filas en los que un 10 % o más de las filas se han eliminado lógicamente, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intentará combinar este grupo de filas con uno o varios grupos de filas. Por ejemplo, el grupo de filas 1 se comprimió con 500 000 filas y el grupo de filas 21 se comprimió con el máximo de 1 048 576 filas. El grupo de filas 21 tiene el 60 % de las filas eliminadas, lo que deja 409 830 filas. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] favorece la opción de combinar estos dos grupos de filas para comprimir un nuevo grupo de filas que tenga 909 830 filas.  
  
REORGANIZE WITH ( COMPRESS_ALL_ROW_GROUPS = { ON | **OFF** } )  
 Se aplica a los índices de almacén de columnas. 

 **Se aplica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

COMPRESS_ALL_ROW_GROUPS ofrece una manera de forzar a los grupos de filas delta con el estado OPEN o CLOSED hacia el almacén de columnas. Con esta opción, no es necesario regenerar el índice de almacén de columnas para vaciar los grupos de filas delta.  Al combinar esta opción con las otras características de eliminación y combinación de desfragmentación, ya no es necesario regenerar el índice en la mayoría de los casos.    

-   ON fuerza todos los grupos de filas del almacén de columnas, independientemente de su tamaño y estado (CLOSED u OPEN).  
-   OFF fuerza todos los grupos de filas con el estado CLOSED hacia el almacén de columnas.  

Para obtener más información, vea [Reorganizar y volver a generar índices](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md). 

SET **(** \<set_index option> [ **,** ... *n*] **)**  
 Especifica las opciones del índice sin volver a generar ni organizar el índice. No es posible especificar SET para un índice deshabilitado.  
  
PAD_INDEX = { ON | OFF }  
   
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  

 Especifica el relleno del índice. El valor predeterminado es OFF.  
  
 ACTIVAR  
 El porcentaje de espacio disponible especificado por FILLFACTOR se aplica a páginas de nivel intermedio del índice. Si no se especifica FILLFACTOR al mismo tiempo que PAD_INDEX se establece en ON, se usa el valor de factor de relleno almacenado en [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
 No se especifica OFF ni *fillfactor*.  
 Las páginas de nivel intermedio se rellenan casi al máximo. Esto deja suficiente espacio para al menos una fila del tamaño máximo que puede tener el índice, según el conjunto de claves de las páginas intermedias.  
  
 Para obtener más información, vea [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
FILLFACTOR = *fillfactor*  
 
 **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Especifica un porcentaje que indica cuánto debe llenar el [!INCLUDE[ssDE](../../includes/ssde-md.md)] el nivel hoja de cada página de índice durante la creación o modificación de los índices. *fillfactor* debe ser un valor entero comprendido entre 1 y 100. El valor predeterminado es 0. Los valores de fill factor 0 y 100 son idénticos.  
  
 Un valor FILLFACTOR explícito solo se aplica la primera vez que se crea o se vuelve a generar el índice. [!INCLUDE[ssDE](../../includes/ssde-md.md)] no mantiene dinámicamente el porcentaje especificado de espacio disponible de las páginas. Para obtener más información, vea [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
 Para ver el valor de factor de relleno, use **sys.indexes**.  
  
> [!IMPORTANT]
> La creación o modificación de un índice clúster con un valor FILLFACTOR afecta a la cantidad de espacio de almacenamiento que ocupan los datos, dado que el [!INCLUDE[ssDE](../../includes/ssde-md.md)] vuelve a distribuir los datos cuando crea el índice clúster.  
  
 SORT_IN_TEMPDB = { ON | **OFF** }  
 
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Especifica si los resultados de ordenación se almacenan en **tempdb**. El valor predeterminado es desactivado, excepto para la Hiperescala de Azure SQL Database. Para todas las operaciones de recompilación de índices en Hiperescala, SORT_IN_TEMPDB está siempre activado, independientemente de la opción especificada, a menos que se use la recompilación de índices reanudables.  
  
 ACTIVAR  
 Los resultados de ordenación intermedios utilizados para generar el índice se almacenan en **tempdb**. Si **tempdb** se encuentra en un conjunto de discos distinto al de la base de datos de usuario, se puede reducir el tiempo necesario para crear un índice. Sin embargo, esto aumenta la cantidad de espacio en disco utilizado durante la generación del índice.  
  
 Apagado  
 Los resultados de orden intermedios se almacenan en la misma base de datos que el índice.  
  
 Si no se necesita una operación de ordenación o si la ordenación se puede realizar en la memoria, se omite la opción SORT_IN_TEMPDB.  
  
 Para más información, vea [Opción SORT_IN_TEMPDB para índices](../../relational-databases/indexes/sort-in-tempdb-option-for-indexes.md).  
  
 IGNORE_DUP_KEY **=** { ON | OFF }  
 Especifica la respuesta de error cuando una operación de inserción intenta insertar valores de clave duplicados en un índice único. La opción IGNORE_DUP_KEY se aplica solamente a operaciones de inserción realizadas tras crear o volver a generar el índice. El valor predeterminado es OFF.  
  
 ACTIVAR  
 Se producirá un mensaje de advertencia cuando se inserten valores de clave duplicados en un índice único. Solo las filas que infrinjan la restricción de unicidad darán error.  
  
 Apagado  
 Se producirá un mensaje de error cuando se inserten valores de clave duplicados en un índice único. Toda la operación INSERT se revertirá.  
  
 IGNORE_DUP_KEY no se puede establecer en ON para los índices creados en una vista, los índices que no sean únicos, los índices XML, los índices espaciales y los índices filtrados.  
  
 Para ver IGNORE_DUP_KEY, use [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
 En la sintaxis compatible con versiones anteriores, WITH IGNORE_DUP_KEY es equivalente a WITH IGNORE_DUP_KEY = ON.  
  
 STATISTICS_NORECOMPUTE **=** { ON | OFF }  
 Especifica si se vuelven a calcular las estadísticas de distribución. El valor predeterminado es OFF.  
  
 ACTIVAR  
 Las estadísticas obsoletas no se vuelven a calcular automáticamente.  
  
 Apagado  
 Se habilita la actualización automática de las estadísticas.  
  
 Para restaurar la actualización automática de estadísticas, establezca STATISTICS_NORECOMPUTE en OFF o ejecute UPDATE STATISTICS sin la cláusula NORECOMPUTE.  
  
> [!IMPORTANT]
> La deshabilitación del cálculo automático de estadísticas de distribución puede impedir que el optimizador de consultas elija los planes de ejecución óptimos para las consultas relativas a la tabla.  
  
STATISTICS_INCREMENTAL = { ON | **OFF** }  

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Cuando se establece en **ON**, se crean estadísticas por cada partición. Cuando se establece en **OFF**, se quita el árbol de estadísticas y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recalcula las estadísticas. El valor predeterminado es **OFF**.  
  
 Si no se admiten las estadísticas por partición, la opción se omite y se genera una advertencia. Las estadísticas incrementales no se admiten para los siguientes tipos de estadísticas:  
  
-   Estadísticas creadas con índices que no están alineados por partición con la tabla base  
-   Estadísticas creadas sobre bases de datos secundarias legibles Always On  
-   Estadísticas creadas sobre bases de datos de solo lectura  
-   Estadísticas creadas sobre índices filtrados  
-   Estadísticas creadas sobre vistas  
-   Estadísticas creadas sobre tablas internas  
-   Estadísticas creadas con índices espaciales o índices XML  
  
 ONLINE **=** { ON | **OFF** } \<as applies to rebuild_index_option>  
 Especifica si las tablas subyacentes y los índices asociados están disponibles para realizar consultas y modificar datos durante la operación de índice. El valor predeterminado es OFF.  
  
 Para un índice XML o un índice espacial, solo se admite `ONLINE = OFF` y, si ONLINE se establece en ON, se produce un error.  
  
> [!IMPORTANT]
> Las operaciones de índices en línea no están disponibles en todas las ediciones de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Ediciones y características admitidas de SQL Server 2017[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]](../../sql-server/editions-and-supported-features-for-sql-server-2016.md) y [Ediciones y características admitidas de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).  
  
 ACTIVAR  
 Los bloqueos de tabla de larga duración no se mantienen durante la operación de indización. Durante la fase principal de la operación de índice, solo se mantiene un bloqueo preventivo en la tabla de origen. De esta forma, las consultas o actualizaciones realizadas en la tabla y los índices subyacentes pueden continuar. Al principio de la operación, se mantiene un bloqueo compartido (S) sobre el objeto de origen durante un breve período. Al final de la operación, durante un breve período, se adquiere un bloqueo S sobre el origen si se está creando un índice no clúster; o se adquiere un bloqueo SCH-M (modificación del esquema) cuando se crea o se quita un índice clúster en línea o cuando se regenera un índice clúster o no clúster. ONLINE no se puede establecer en ON cuando se crea un índice en una tabla temporal local.  
  
 Apagado  
 Los bloqueos de tabla se aplican durante la operación de índice. Una operación de índice sin conexión que crea, vuelve a generar o quita un índice clúster, un índice espacial o un índice XML, o vuelve a generar o quita un índice no clúster, adquiere un bloqueo de modificación del esquema (Sch-M) en la tabla. Esto evita que todos los usuarios tengan acceso a la tabla subyacente durante la operación. Una operación de índice sin conexión que crea un índice no clúster adquiere un bloqueo compartido (S) en la tabla. Esto evita que se realicen actualizaciones en la tabla subyacente, pero permite la realización de operaciones de lectura, como instrucciones SELECT.  
  
Para más información, consulte [Perform Index Operations Online](../../relational-databases/indexes/perform-index-operations-online.md).  
  
Los índices, incluidos los índices de las tablas temp globales, se pueden volver a generar en línea, salvo en los casos siguientes:
- Índice XML
- Índice de una tabla temporal local
- Índice clúster único inicial en una vista
- Índices de almacén de columnas
- Índice clúster, si la tabla subyacente contiene tipos de datos LOB (**image**, **ntext**, **text**) y tipos de datos espaciales
- Las columnas **varchar(max)** y **varbinary(max)** no pueden formar parte de un índice. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], cuando una tabla contiene columnas **varchar(max)** o **varbinary(max)** , puede crearse o volver a crearse un índice agrupado que contiene otras columnas con la opción **ONLINE**. [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] no permite la opción **ONLINE** cuando la tabla contiene columnas **varchar(max)** o **varbinary(max)**

Para más información, vea [Cómo funcionan las operaciones de índice en línea](../../relational-databases/indexes/how-online-index-operations-work.md).

RESUMABLE **=** { ON | **OFF**}

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]   

 Especifica si una operación de índice en línea se puede reanudar.

 ON: la operación de índice se puede reanudar.

 OFF: la operación de índice no se puede reanudar.

MAX_DURATION **=** *time* [**MINUTES**] se usa con **RESUMABLE = ON** (requiere **ONLINE = ON**).

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

Indica el tiempo (valor entero especificado en minutos) durante el cual se ejecuta una operación de índice en línea reanudable antes de ponerse en pausa. 

> [!IMPORTANT]
> Para saber más sobre las operaciones de índices que pueden realizarse en línea, vea [Directrices para operaciones de índices en línea](../../relational-databases/indexes/guidelines-for-online-index-operations.md).

> [!NOTE] 
> No se admiten las recompilaciones de índices en línea reanudables en los índices de almacén de columnas.

ALLOW_ROW_LOCKS **=** { **ON** | OFF }  

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Especifica si se permiten los bloqueos de fila. El valor predeterminado es ON.  
  
 ACTIVAR  
 Los bloqueos de fila se admiten al obtener acceso al índice. El [!INCLUDE[ssDE](../../includes/ssde-md.md)] determina cuándo se usan los bloqueos de fila.  
  
 Apagado  
 No se usan los bloqueos de fila.  
  
ALLOW_PAGE_LOCKS **=** { **ON** | OFF }  
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Especifica si se permiten bloqueos de página. El valor predeterminado es ON.  
  
 ACTIVAR  
 Se permiten bloqueos de página cuando se tiene acceso al índice. [!INCLUDE[ssDE](../../includes/ssde-md.md)] determina el momento en que se usan los bloqueos de página.  
  
 Apagado  
 No se utilizan bloqueos de página.  
  
> [!NOTE]
> No es posible reorganizar un índice cuando ALLOW_PAGE_LOCKS está establecido en OFF.  

 OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | **OFF** }

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Especifica si se deben optimizar la contención de inserción de la última página. El valor predeterminado es OFF. Consulte la sección [Claves secuenciales](./create-index-transact-sql.md#sequential-keys) de la página CREATE INDEX para obtener más información.

 MAXDOP **=** max_degree_of_parallelism  
 
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Reemplaza la opción de configuración de **max_degree_of_parallelism** mientras dure la operación de índice. Para obtener más información, vea [Establecer la opción de configuración del servidor Grado máximo de paralelismo](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md). Utilice MAXDOP para establecer un límite para el número de procesadores utilizados en la ejecución de un plan paralelo. El máximo es 64 procesadores.  
  
> [!IMPORTANT]
>  Aunque la opción MAXDOP se admite sintácticamente para todos los índices XML, ALTER INDEX utiliza en la actualidad un solo procesador para un índice espacial o un índice XML principal.  
  
 *max_degree_of_parallelism* puede tener estos valores:  
  
 1  
 Suprime la generación de planes paralelos.  
  
 \>1  
 Restringe el número máximo de procesadores utilizados en una operación de índice paralelo para el número especificado.  
  
 0 (predeterminado)  
 Usa el número real de procesadores o menos, según la carga de trabajo actual del sistema.  
  
 Para obtener más información, vea [Configurar operaciones de índice en paralelo](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!NOTE]
> Las operaciones de índices en paralelo no están disponibles en todas las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Características compatibles con las ediciones de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
COMPRESSION_DELAY **=** { **0** |*duration [Minutes]* }  

**Se aplica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)])  
  
 Para las tablas basadas en disco, el retraso especifica el número mínimo de minutos que debe permanecer un grupo de filas delta con estado CLOSED en el grupo de filas delta antes de que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pueda comprimirlo en el grupo de filas comprimido. Puesto que las tablas basadas en disco no realizan el seguimiento de los tiempos de inserción y actualización de filas individuales, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aplica el retraso a los grupos de filas delta en el estado CLOSED.  
  
 El valor predeterminado es 0 minutos.  
  
 Para obtener recomendaciones sobre cuándo usar COMPRESSION_DELAY, vea [Introducción al almacén de columnas para análisis operativos en tiempo real](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md).  
  
 DATA_COMPRESSION  
 Especifica la opción de compresión de datos para el índice, número de partición o intervalo de particiones especificado. Las opciones son las siguientes:  
  
 Ninguno  
 No se comprimen el índice ni las particiones especificadas. Esto no se aplica a los índices de almacén de columnas.  
  
 ROW  
 El índice o las particiones especificadas se comprimen mediante la compresión de fila. Esto no se aplica a los índices de almacén de columnas.  
  
 PAGE  
 El índice o las particiones especificadas se comprimen mediante la compresión de página. Esto no se aplica a los índices de almacén de columnas.  
  
 COLUMNSTORE  
   
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Solo se aplica a los índices de almacén de columnas, incluidos los clúster y no clúster. COLUMNSTORE especifica que se descomprima el índice o las particiones especificadas comprimidas con la opción COLUMNSTORE_ARCHIVE. Cuando se restablecen los datos, seguirán estando comprimidos con la compresión de almacén de columnas que se usa para todos los índices de almacén de columnas.  
  
 COLUMNSTORE_ARCHIVE  
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Solo se aplica a los índices de almacén de columnas, incluidos los clúster y no clúster. COLUMNSTORE_ARCHIVE comprimirá aún más la partición especificada a un tamaño mínimo. Esto se puede usar para el archivado o para otras situaciones que requieran un tamaño de almacenamiento mínimo y pueda permitirse más tiempo para el almacenamiento y recuperación.  
  
 Para más información sobre la compresión, vea [Compresión de datos](../../relational-databases/data-compression/data-compression.md).  
  
 ON PARTITIONS **(** { \<partition_number_expression> | \<range> } [ **,** ...n] **)**  
    
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
 Especifica las particiones a las que se aplica el valor DATA_COMPRESSION. Si el índice no tiene particiones, el argumento ON PARTITIONS generará un error. Si no se proporciona la cláusula ON PARTITIONS, la opción DATA_COMPRESSION se aplica a todas las particiones de un índice con particiones.  
  
 \<partition_number_expression> se puede especificar de estas maneras:  
  
-   Proporcionar el número de una partición, por ejemplo: `ON PARTITIONS (2)`.  
-   Proporcionar los números de partición para varias particiones individuales separadas por comas, por ejemplo: `ON PARTITIONS (1, 5)`.  
-   Proporcionar particiones individuales y de intervalos: `ON PARTITIONS (2, 4, 6 TO 8)`.  
  
 \<range> se puede especificar como números de partición separados por la palabra TO, como por ejemplo: `ON PARTITIONS (6 TO 8)`.  
  
 Para establecer diferentes tipos de compresión de datos para distintas particiones, especifique la opción DATA_COMPRESSION más de una vez, por ejemplo:  
  
```sql  
REBUILD WITH   
(  
DATA_COMPRESSION = NONE ON PARTITIONS (1),   
DATA_COMPRESSION = ROW ON PARTITIONS (2, 4, 6 TO 8),   
DATA_COMPRESSION = PAGE ON PARTITIONS (3, 5)  
);  
```  
  
 ONLINE **=** { ON  | **OFF** } \<as applies to single_partition_rebuild_index_option>  
 Especifica si un índice o una partición de índice de una tabla subyacente se puede regenerar en línea o sin conexión. Si **REBUILD** se realiza en línea (**ON**), los datos de esta tabla están disponibles para las consultas y la modificación de datos durante la operación de índice.  El valor predeterminado es **OFF**.  
  
 ACTIVAR  
 Los bloqueos de tabla de larga duración no se mantienen durante la operación de indización. Durante la fase principal de la operación de índice, solo se mantiene un bloqueo preventivo en la tabla de origen. Se necesita un bloqueo S en la tabla al inicio de la regeneración del índice y un bloqueo Sch-M en la tabla al final de la regeneración del índice en línea. Aunque ambos bloqueos son bloqueos de metadatos cortos, especialmente el bloqueo Sch-M debe esperar a que todas las transacciones de bloqueo se completen. Durante el tiempo de espera, bloqueo Sch-M bloquea las demás transacciones que esperen a este bloqueo al tener acceso a la misma tabla.  
  
> [!NOTE]
>  La regeneración de índice en línea puede establecer las opciones *low_priority_lock_wait* que se describen más adelante en esta sección.  
  
 Apagado  
 Los bloqueos de tabla se aplican durante la operación de índice. Esto evita que todos los usuarios tengan acceso a la tabla subyacente durante la operación.  
  
 WAIT_AT_LOW_PRIORITY solo se usa con **ONLINE=ON**.  
 
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Una recompilación de índices en línea tiene que esperar a las operaciones de bloqueo en esta tabla. **WAIT_AT_LOW_PRIORITY** indica que la operación de regeneración de índice en línea esperará a los bloqueos de prioridad baja, de forma que otras operaciones podrán continuar mientras la operación de generación de índice en línea está en espera. La omisión de la opción **WAIT AT LOW PRIORITY** equivale a WAIT_AT_LOW_PRIORITY (MAX_DURATION = 0 minutos, ABORT_AFTER_WAIT = NONE). Para más información, vea [WAIT_AT_LOW_PRIORITY](alter-index-transact-sql.md). 
  
 MAX_DURATION = *time* [**MINUTES**]  
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 El tiempo de espera (valor entero especificado en minutos) que esperarán con prioridad baja los bloqueos de la operación de recompilación de índices en línea cuando se ejecuta el comando DDL. Si la operación se bloquea durante el tiempo de **MAX_DURATION**, se ejecutará una de las acciones **ABORT_AFTER_WAIT**. El tiempo de **MAX_DURATION** siempre es en minutos y la palabra **MINUTES** puede omitirse.  
 
 ABORT_AFTER_WAIT = [**NONE** | **SELF** | **BLOCKERS** } ]  
   
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Ninguno  
 Se continúa esperando al bloqueo con prioridad normal.  
  
 SELF  
 Cierra la operación DDL de regeneración del índice en línea que se está ejecutando actualmente sin realizar ninguna acción.  
  
 BLOCKERS  
 Elimine todas las transacciones de usuario que bloqueen la operación DDL de regeneración de índice en línea de forma que la operación pueda continuar. Para la opción **BLOCKERS**, es necesario que el inicio de sesión tenga el permiso **ALTER ANY CONNECTION**.  
 
 RESUME 
 
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  

Reanudar una operación de índice que se haya pausado manualmente o debido a un error.

MAX_DURATION se usa con **RESUMABLE=ON**
 
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

El tiempo (valor entero especificado en minutos) durante el cual se ejecuta la operación de índice en línea reanudable después de reanudarse. Cuando este tiempo expira, se pone en pausa la operación reanudable si todavía se está ejecutando.

WAIT_AT_LOW_PRIORITY se usa con **RESUMABLE=ON** y **ONLINE = ON**.  
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
 Para reanudar una regeneración de índice en línea después de una pausa es necesario esperar a las operaciones de bloqueo de esta tabla. **WAIT_AT_LOW_PRIORITY** indica que la operación de regeneración de índice en línea esperará a los bloqueos de prioridad baja, de forma que otras operaciones podrán continuar mientras la operación de generación de índice en línea está en espera. La omisión de la opción **WAIT AT LOW PRIORITY** equivale a WAIT_AT_LOW_PRIORITY (MAX_DURATION = 0 minutos, ABORT_AFTER_WAIT = NONE). Para más información, vea [WAIT_AT_LOW_PRIORITY](alter-index-transact-sql.md). 

PAUSE
 
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
Pausa una operación de regeneración de índice en línea reanudable.

ABORT

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]   

Anular una operación de índice que está ejecutándose o en pausa y que se había declarado como reanudable. Tendrá que ejecutar explícitamente un comando **ABORT** para terminar una operación de regeneración de índice reanudable. La ejecución de una operación de índice reanudable no termina cuando esta da error o se pone en pausa, sino que la operación se queda en un estado de pausa indefinido.
  
## <a name="remarks"></a>Observaciones  
No es posible utilizar ALTER INDEX para volver a crear particiones en un índice o moverlo a un grupo de archivos distinto. No se puede usar esta instrucción para modificar la definición de índice; por ejemplo, para agregar o eliminar columnas o cambiar su orden. Utilice CREATE INDEX con la cláusula DROP_EXISTING para realizar estas operaciones.  
  
Cuando no se especifica una opción de forma explícita, se aplica el valor actual. Por ejemplo, si no se especifica un valor FILLFACTOR en la cláusula REBUILD, se utilizará el valor de factor de relleno almacenado en el catálogo del sistema durante el proceso de regeneración. Para ver la configuración de las opciones de índice actuales, use [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
Los valores de `ONLINE`, `MAXDOP` y `SORT_IN_TEMPDB` no se almacenan en el catálogo del sistema. A menos que se especifiquen en la instrucción de índice, se utiliza el valor predeterminado de la opción.
  
En los equipos con varios procesadores, `ALTER INDEX REBUILD`, al igual que otras consultas, usa automáticamente más procesadores para realizar las operaciones de examen y orden asociadas a la modificación del índice. Al ejecutar `ALTER INDEX REORGANIZE`, con o sin LOB_COMPACTION, el valor de **Grado máximo de paralelismo** es una operación de un solo subproceso. Para obtener más información, vea [Configurar operaciones de índice en paralelo](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!IMPORTANT]
> No es posible volver a organizar o generar un índice si el grupo de archivos en el que se encuentra está sin conexión o está definido como de solo lectura. Cuando se especifica la palabra clave ALL y hay uno o más índices en un grupo de archivos sin conexión o de solo lectura, se produce un error en la instrucción.  
  
## <a name="rebuilding-indexes"></a><a name="rebuilding-indexes"></a> Regenerar índices  
El proceso de volver a crear un índice quita y vuelve a crear el índice. Quita la fragmentación, utiliza espacio en disco al compactar las páginas según el valor de factor de relleno especificado o existente y vuelve a ordenar las filas del índice en páginas contiguas. Cuando se especifica ALL, todos los índices de la tabla se quitan y se vuelven a generar en una única transacción. No es necesario quitar las restricciones FOREIGN KEY por adelantado. Cuando se regeneran índices con 128 extensiones o más, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] difiere las cancelaciones de asignación de página y sus bloqueos asociados hasta después de la confirmación de la transacción.  
 
Para obtener más información, vea [Reorganizar y volver a generar índices](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md). 
  
## <a name="reorganizing-indexes"></a><a name="reorganizing-indexes"></a> Reorganizar índices
La reorganización de un índice usa muy pocos recursos del sistema. Desfragmenta el nivel hoja de los índices agrupados y no clúster de las tablas y las vistas al volver a ordenar físicamente las páginas de nivel hoja para que coincidan con el orden lógico de los nodos hoja, de izquierda a derecha. La reorganización también compacta las páginas de índice. La compactación se basa en el valor de factor de relleno existente. 
  
Cuando se especifica `ALL`, se reorganizan los índices relacionales, tanto agrupados como no clúster, y los índices XML. Cuando se especifica ALL, se aplican algunas restricciones; vea la definición de ALL en la sección Argumentos de este artículo.  
  
Para obtener más información, vea [Reorganizar y volver a generar índices](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).  

> [!IMPORTANT]
> En una tabla de [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] con un índice de almacén de columnas agrupadas ordenado, `ALTER INDEX REORGANIZE` no reordenará los datos. Para reordenar los datos, use `ALTER INDEX REBUILD`.
  
## <a name="disabling-indexes"></a><a name="disabling-indexes"></a> Deshabilitar índices  
Al deshabilitar un índice, se impide que el usuario tenga acceso al mismo y, en el caso de los índices clúster, a los datos de la tabla subyacente. La definición de índice permanece en el catálogo del sistema. La deshabilitación de un índice no clúster o agrupado en una vista elimina físicamente los datos del índice. La deshabilitación de un índice clúster evita el acceso a los datos, aunque éstos permanecen en el árbol b hasta que el índice se quita o se vuelve a generar. Para ver el estado de un índice habilitado o deshabilitado, realice una consulta en la columna **is_disabled** de la vista de catálogo **sys.indexes**.  
  
Si una tabla se encuentra en una publicación de replicación transaccional, no puede deshabilitar ningún índice que esté asociado con las columnas de clave principal. Estos índices son necesarios para la replicación. Para deshabilitar un índice, primero debe quitar la tabla de la publicación. Para obtener más información sobre la creación de publicaciones, vea [Publicar datos y objetos de base de datos](../../relational-databases/replication/publish/publish-data-and-database-objects.md).  
  
Use las instrucciones `ALTER INDEX REBUILD` o `CREATE INDEX WITH DROP_EXISTING` para habilitar el índice. No es posible volver a generar un índice clúster deshabilitado si la opción ONLINE está establecida en ON. Para obtener más información, vea [Deshabilitar índices y restricciones](../../relational-databases/indexes/disable-indexes-and-constraints.md).  
  
## <a name="setting-options"></a>Configurar opciones  
Puede establecer las opciones `ALLOW_ROW_LOCKS`, `ALLOW_PAGE_LOCKS`, `OPTIMIZE_FOR_SEQUENTIAL_KEY`, `IGNORE_DUP_KEY` y `STATISTICS_NORECOMPUTE` para un índice especificado sin volver a generar o reorganizar ese índice. Los valores modificados se aplican inmediatamente al índice. Para ver estos valores, use **sys.indexes**. Para obtener más información, consulte [Establecer opciones de índice](../../relational-databases/indexes/set-index-options.md).  
  
### <a name="row-and-page-locks-options"></a>Opciones de bloqueo de fila y página  
Si `ALLOW_ROW_LOCKS = ON` y `ALLOW_PAGE_LOCK = ON`, se permiten los bloqueos de nivel de fila, página y tabla al obtener acceso al índice. [!INCLUDE[ssDE](../../includes/ssde-md.md)] elige el bloqueo apropiado y puede cambiar de escala el bloqueo: de un bloqueo de fila o página a un bloqueo de tabla.  
  
Si `ALLOW_ROW_LOCKS = OFF` y `ALLOW_PAGE_LOCK = OFF`, solo se permite un bloqueo de nivel de tabla al obtener acceso al índice.  
  
Si se especifica ALL al establecer las opciones de bloqueo de fila o página, la configuración se aplica a todos los índices. Cuando la tabla subyacente es un montón, la configuración se aplica de las siguientes formas:  
  
|Opción|Detalles|
|------|-------|
|ALLOW_ROW_LOCKS = ON u OFF|Al montón y a cualquier índice no clúster asociado.|  
|ALLOW_PAGE_LOCKS = ON|Al montón y a cualquier índice no clúster asociado.|  
|ALLOW_PAGE_LOCKS = OFF|Completamente a los índices no clúster. Esto significa que no se permite ningún bloqueo de página en los índices no clúster. En el montón, los únicos bloqueos no permitidos para la página son los bloqueos compartidos (S), de actualización (U) y exclusivos (X). El [!INCLUDE[ssDE](../../includes/ssde-md.md)] aún puede adquirir un bloqueo de página de intención (IS, IU o IX) por motivos internos.|  
  
## <a name="online-index-operations"></a><a name="online-index-operations"></a> Operaciones de índice en línea  
Cuando se vuelve a generar un índice y la opción ONLINE está establecida en ON, los objetos subyacentes, las tablas y los índices asociados están disponibles para las consultas y la modificación de datos. También puede regenerar en línea una parte de un índice que resida en una sola partición. Los bloqueos de tabla exclusivos solo se mantienen un espacio de tiempo muy reducido durante el proceso de modificación.  
  
La reorganización de un índice siempre se realiza en línea. El proceso no mantiene bloqueos a largo plazo y, por ello, no bloquea las consultas o las actualizaciones en ejecución.  
  
Únicamente se pueden realizar operaciones de índice simultáneas en línea en la misma tabla o partición de tabla si se hace lo siguiente:  
  
-   Crear varios índices no clúster.  
-   Reorganizar diferentes índices en la misma tabla.  
-   Reorganizar diferentes índices mientras se vuelven a generar índices que no se superponen en la misma tabla.  
  
Se producirá un error en todas las operaciones de índice en línea que se realizan al mismo momento. Por ejemplo, no es posible volver a generar dos o más índices en la misma tabla de forma simultánea ni crear un índice nuevo mientras se regenera un índice existente en la misma tabla.  

### <a name="resumable-index-operations"></a><a name="resumable-indexes"></a> Operaciones de índice reanudable

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

ONLINE INDEX REBUILD se especifica como reanudable mediante la opción RESUMABLE = ON. 
-  La opción RESUMABLE no se conserva en los metadatos de un índice dado y solo se aplica a la duración de una instrucción DDL actual. Por tanto, la cláusula RESUMABLE = ON debe especificarse explícitamente para habilitar la capacidad de reanudación.
-  La opción MAX_DURATION es compatible con la opción RESUMABLE = ON o la opción del argumento **low_priority_lock_wait**. 
   -  MAX_DURATION para la opción RESUMABLE especifica el intervalo de tiempo para regenerar un índice. Una vez pasado este tiempo, la operación de regeneración de índice se pausa o completa su ejecución. El usuario decide cuándo se puede reanudar una recompilación de un índice en pausa. El valor **time** en minutos para MAX_DURATION debe ser mayor que 0 minutos o menor o igual que una semana (7 \* 24 \* 60 = 10080 minutos). Si se hace una pausa larga en una operación de índice puede afectar al rendimiento de DML en una tabla específica, así como a la capacidad de disco de base de datos, dado que ambos índices, el original y el que se acaba de crear, necesitan espacio en disco y deben actualizarse durante las operaciones de DML. Si se omite la opción MAX_DURATION, la operación de índice continuará hasta su finalización o hasta que se produzca un error. 
   -  La opción del argumento \<low_priority_lock_wait> le permite decidir cómo puede continuar la operación de índice cuando se bloquea en el bloqueo SCH-M.
 
-  Al volver a ejecutar la instrucción ALTER INDEX REBUILD original con los mismos parámetros, se reanuda una operación de regeneración de índice en pausa. También se puede reanudar una operación de regeneración de índice en pausa mediante la instrucción ALTER INDEX RESUME.
-  La opción SORT_IN_TEMPDB=ON no es compatible con el índice reanudable. 
-  El comando DDL con RESUMABLE=ON no se puede ejecutar dentro de una transacción explícita (no puede formar parte del bloque begin tran ... commit).
-  Solo se pueden reanudar aquellas operaciones de índice que estén en pausa.
-  Cuando se reanuda una operación de índice que está en pausa, se puede cambiar el valor de MAXDOP a un nuevo valor.  Si MAXDOP no se especifica cuando se reanuda una operación de índice que está en pausa, se toma el último valor MAXDOP. Si la opción MAXDOP no se especifica en absoluto para la operación de regeneración de índice, se toma el valor predeterminado.
- Para pausar inmediatamente la operación de índice, puede detener el comando en curso (Ctrl+C) o puede ejecutar el comando ALTER INDEX PAUSE o el comando KILL *session_id*. Una vez que el comando está en pausa, se puede reanudar mediante la opción RESUME.
-  El comando ABORT termina la sesión que hospedaba la regeneración de índice original y anula la operación de índice.  
-  No se necesita ningún recurso adicional para la regeneración de índice reanudable, excepto para:
   -    el espacio adicional necesario para mantener el índice que se está generando, incluida la hora en que se pausó el índice;
   -    un estado DDL que impida cualquier modificación de DDL.
-  La limpieza de registros fantasma se ejecutará durante la fase de pausa del índice, pero se pondrá en pausa durante la ejecución del índice.   
Abajo se detallan las funciones que se deshabilitan para las operaciones de regeneración de índice reanudables:
   -    Regenerar un índice que está deshabilitado no es compatible con RESUMABLE=ON
   -    Comando ALTER INDEX REBUILD ALL
   -    ALTER TABLE mediante regeneración de índice  
   -    El comando DDL con “RESUMABLE = ON” no se puede ejecutar dentro de una transacción explícita (no puede formar parte del bloque begin tran ... commit)
   -    Regenerar un índice que tiene columnas TIMESTAMP o calculadas como columnas de clave.
-   En caso de que la tabla base contenga columnas LOB reanudables, para la regeneración de índice agrupado reanudable se necesita un bloqueo Sch-M al inicio de esta operación. 

> [!NOTE]
> El comando DDL se ejecuta hasta que se completa, se pone en pausa o genera un error. En caso de que el comando se ponga en pausa, se emite un error que indica que se pausó la operación y que no se completó la creación de índice. Para más información sobre el estado actual del índice, vea [sys.index_resumable_operations](../../relational-databases/system-catalog-views/sys-index-resumable-operations.md). Como ocurrió antes, en caso de fallo se generará también un error. 

 Para más información, consulte [Perform Index Operations Online](../../relational-databases/indexes/perform-index-operations-online.md).  
  
 ### <a name="wait_at_low_priority-with-online-index-operations"></a>WAIT_AT_LOW_PRIORITY con operaciones de índice en línea  
  
 Para ejecutar la instrucción DDL para regenerar el índice en línea, todas las transacciones activas de bloqueo que se ejecutan en una tabla determinada deben completarse. Cuando la regeneración de índice en línea se ejecuta, bloquea todas las nuevas transacciones que están listas para iniciar la ejecución en esta tabla. Aunque la vigencia del bloqueo para volver a generar el índice en línea es muy corta, si se espera a que las transacciones abiertas en una tabla dada se completen y se bloquean las nuevas transacciones que se inician, se podría afectar significativamente al rendimiento, produciendo un retraso o un tiempo de espera en la carga de trabajo y se limitaría mucho el acceso a la tabla base. Con la opción **WAIT_AT_LOW_PRIORITY**, el DBA puede administrar los bloqueos S-lock y Sch-M necesarios para las regeneraciones de índice en línea y seleccionar una de las tres opciones. En los tres casos, si durante el tiempo de espera ( (MAX_DURATION = n [minutos]) ) no hay actividades de bloqueo, la regeneración de índice en línea se ejecuta inmediatamente sin esperar y la instrucción DDL se completa.  
  
## <a name="spatial-index-restrictions"></a>Restricciones de los índices espaciales  
 Cuando se regenera un índice espacial, la tabla de usuario subyacente no está disponible mientras dura esta operación, porque el índice espacial tiene un bloqueo del esquema.  
  
 No se puede modificar la restricción PRIMARY KEY de la tabla de usuario mientras se define un índice espacial en una columna de esa tabla. Para cambiar la restricción PRIMARY KEY, quite primero todos los índices espaciales de la tabla. Después de modificar la restricción PRIMARY KEY, puede volver a crear cada uno de los índices espaciales.  
  
 No se puede especificar ningún índice espacial en una operación para volver a generar una partición única. Sin embargo, se pueden especificar índices espaciales cuando se regeneran todas las particiones.  
  
 Para cambiar opciones específicas de un índice espacial, como BOUNDING_BOX o GRID, puede usar una instrucción CREATE SPATIAL INDEX que especifica DROP_EXISTING = ON, o quitar el índice espacial y crear uno nuevo. Para ver un ejemplo, consulte [CREATE SPATIAL INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-spatial-index-transact-sql.md).  
  
## <a name="data-compression"></a>Data Compression  
 Para obtener más información sobre la compresión de datos, vea [Compresión de datos](../../relational-databases/data-compression/data-compression.md).  
  
 Para evaluar el modo en que el cambio de compresión de PAGE y ROW afectará a una tabla, índice o partición, use el procedimiento almacenado [sp_estimate_data_compression_savings](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md).  
  
Las restricciones siguientes se aplican a los índices con particiones:  
  
-   Al utilizar ALTER INDEX ALL..., no puede cambiar la configuración de compresión de una única partición si la tabla tiene índices no alineados.  
-   La sintaxis ALTER INDEX \<index> ... REBUILD PARTITION ... vuelve a generar la partición especificada del índice.  
-   La sintaxis ALTER INDEX \<index> ... REBUILD WITH ... vuelve a generar todas las particiones del índice.  
  
## <a name="statistics"></a>Estadísticas  
 Cuando se ejecuta **ALTER INDEX ALL ...** en una tabla, solo se actualizan las estadísticas asociadas a índices. Las estadísticas automáticas o manuales creadas en la tabla (en lugar de en un índice) no se actualizan.  
  
## <a name="permissions"></a>Permisos  
 Para ejecutar ALTER INDEX, se necesita, como mínimo, el permiso ALTER en la tabla o en la vista.  
  
## <a name="version-notes"></a>Notas de la versión  
  
-  [!INCLUDE[ssSDS](../../includes/sssds-md.md)] no usa opciones de grupo de archivos ni secuencia de archivos.  
-  Los índices de almacén de columnas no están disponibles en versiones anteriores a [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]. 
-  Las operaciones de índice reanudables están disponibles con [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   
  
## <a name="basic-syntax-example"></a>Ejemplo de sintaxis básica:   
  
```sql 
ALTER INDEX index1 ON table1 REBUILD;  
  
ALTER INDEX ALL ON table1 REBUILD;  
  
ALTER INDEX ALL ON dbo.table1 REBUILD;  
```

## <a name="examples-columnstore-indexes"></a>Ejemplos: Índices de almacén de columnas  
 Estos ejemplos se aplican a índices de almacén de columnas.  
  
### <a name="a-reorganize-demo"></a>A. Demo de REORGANIZE  
 En este ejemplo se muestra cómo funciona el comando ALTER INDEX REORGANIZE.  Se crea una tabla que tiene varios grupos de filas y, después, se muestra cómo REORGANIZE combina los grupos de filas.  
  
```sql  
-- Create a database   
CREATE DATABASE [ columnstore ];  
GO  
  
-- Create a rowstore staging table  
CREATE TABLE [ staging ] (  
     AccountKey              int NOT NULL,  
     AccountDescription      nvarchar (50),  
     AccountType             nvarchar(50),  
     AccountCodeAlternateKey     int  
     )  
  
-- Insert 10 million rows into the staging table.   
DECLARE @loop int  
DECLARE @AccountDescription varchar(50)  
DECLARE @AccountKey int  
DECLARE @AccountType varchar(50)  
DECLARE @AccountCode int  
  
SELECT @loop = 0  
BEGIN TRAN  
    WHILE (@loop < 300000)   
      BEGIN  
        SELECT @AccountKey = CAST (RAND()*10000000 as int);  
        SELECT @AccountDescription = 'accountdesc ' + CONVERT(varchar(20), @AccountKey);  
        SELECT @AccountType = 'AccountType ' + CONVERT(varchar(20), @AccountKey);  
        SELECT @AccountCode =  CAST (RAND()*10000000 as int);  
  
        INSERT INTO  staging VALUES (@AccountKey, @AccountDescription, @AccountType, @AccountCode);  
  
        SELECT @loop = @loop + 1;  
    END  
COMMIT  
  
-- Create a table for the clustered columnstore index  
  
CREATE TABLE cci_target (  
     AccountKey              int NOT NULL,  
     AccountDescription      nvarchar (50),  
     AccountType             nvarchar(50),  
     AccountCodeAlternateKey int  
     )  
  
-- Convert the table to a clustered columnstore index named inxcci_cci_target;  
CREATE CLUSTERED COLUMNSTORE INDEX idxcci_cci_target ON cci_target;  
```  
  
 Use la opción TABLOCK para insertar filas en paralelo. A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], la operación INSERT INTO se puede ejecutar en paralelo cuando se usa TABLOCK.  
  
```sql  
INSERT INTO cci_target WITH (TABLOCK) 
SELECT TOP 300000 * FROM staging;  
```  
  
 Ejecute este comando para ver los grupos de filas delta con estado OPEN. El número de grupos de filas depende del grado de paralelismo.  
  
```sql  
SELECT *   
FROM sys.dm_db_column_store_row_group_physical_stats   
WHERE object_id  = object_id('cci_target');  
```  
  
 Ejecute este comando para forzar todos los grupos de filas con estado CLOSED y OPEN hacia el almacén de columnas.  
  
```sql  
ALTER INDEX idxcci_cci_target ON cci_target REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS = ON);  
```  
  
 Vuelva a ejecutar este comando y verá que los grupos de filas más pequeños se combinan en un grupo de filas comprimido.  
  
```sql  
ALTER INDEX idxcci_cci_target ON cci_target REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS = ON);  
```  
  
### <a name="b-compress-closed-delta-rowgroups-into-the-columnstore"></a>B. Comprimir grupos de filas delta con estado CLOSED en el almacén de columnas  
 En este ejemplo se usa la opción REORGANIZE para comprimir cada grupo de filas delta con el estado CLOSED en el almacén de columnas como un grupo de filas comprimido.   Esta operación no es necesaria, pero es útil cuando el motor de tupla no comprime los grupos de filas CLOSED lo suficientemente rápido.  
  
```sql  
-- Uses AdventureWorksDW  
-- REORGANIZE all partitions  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE;  
  
-- REORGANIZE a specific partition  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE PARTITION = 0;  
```  
  
### <a name="c-compress-all-open-and-closed-delta-rowgroups-into-the-columnstore"></a>C. Comprimir todos los grupos de filas delta con estado OPEN y CLOSED en el almacén de columnas  
 **Se aplica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
 El comando REORGANIZE WITH ( COMPRESS_ALL_ROW_GROUPS = ON ) comprime cada grupo de filas delta con los estados OPEN y CLOSED en el almacén de columnas como un grupo de filas comprimido. Esto vacía el almacén delta y obliga a todas las filas a comprimirse en el almacén de columnas. Esto es útil sobre todo después de realizar muchas operaciones de inserción, ya que estas operaciones almacenan las filas en uno o varios grupos de filas delta.  
  
 REORGANIZE combina grupos de filas para rellenar los grupos de filas hasta un número máximo de filas \<= 1 024 576. Por tanto, al comprimir todos los grupos de filas OPEN y CLOSED, no terminará con una gran cantidad de grupos de filas comprimidos que solo tengan algunas filas en ellos. Es preferible que los grupos de filas estén lo más llenos posible para reducir el tamaño comprimido y mejorar el rendimiento de las consultas.  
  
```sql  
-- Uses AdventureWorksDW2016  
-- Move all OPEN and CLOSED delta rowgroups into the columnstore.  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS = ON);  
  
-- For a specific partition, move all OPEN AND CLOSED delta rowgroups into the columnstore  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE PARTITION = 0 WITH (COMPRESS_ALL_ROW_GROUPS = ON);  
```  
  
### <a name="d-defragment-a-columnstore-index-online"></a>D. Desfragmentar un índice de almacén de columnas en línea  
 No se aplica a: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].  
  
 A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], con REORGANIZE se consigue hacer mucho más que comprimir grupos de filas delta en el almacén de columnas: también realiza desfragmentación en línea. Primero reduce el tamaño del almacén de columnas. Para ello, quita físicamente las filas eliminadas cuando se ha eliminado al menos un 10 % o más de las filas de un grupo de filas.  Después, combina grupos de filas para formar grupos de filas de mayor tamaño que tengan hasta un máximo de 1 024 576 filas por grupos de filas.  Todos los grupos de filas que se cambian se vuelven a comprimir.  
  
> [!NOTE]
> A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], ya no será necesario volver a regenerar un índice de almacén de columnas en la mayoría de los casos, ya que REORGANIZE quita físicamente las filas eliminadas y combina los grupos de filas. La opción COMPRESS_ALL_ROW_GROUPS fuerza todos los grupos de filas delta con el estado OPEN y CLOSED hacia el almacén de columnas, lo que antes solo se podía realizar mediante una regeneración. REORGANIZE se ejecuta en línea y en segundo plano, por lo que las consultas pueden seguir haciéndose mientras se realiza la operación.  
  
```sql  
-- Uses AdventureWorks  
-- Defragment by physically removing rows that have been logically deleted from the table, and merging rowgroups.  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REORGANIZE;  
```  
  
### <a name="e-rebuild-a-clustered-columnstore-index-offline"></a>E. Regenerar un índice de almacén de columnas agrupado sin conexión  
Se aplica a: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])   
  
> [!TIP]
> A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], se recomienda usar ALTER INDEX REORGANIZE en lugar de ALTER INDEX REBUILD.  
  
> [!NOTE]
> En [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], REORGANIZE solo se usa para comprimir los grupos de filas CLOSED en el almacén de columnas. La única manera de realizar operaciones de desfragmentación y forzar todos los grupos de filas delta hacia el almacén de columnas es regenerar el índice.  
  
 En este ejemplo se muestra cómo regenerar un índice de almacén de columnas agrupado y forzar todos los grupos de filas delta hacia el almacén de columnas. Este primer paso prepara una tabla FactInternetSales2 con un índice de almacén de columnas clúster e inserta los datos de las primeras cuatro columnas.  
  
```sql  
-- Uses AdventureWorksDW  
  
CREATE TABLE dbo.FactInternetSales2 (  
    ProductKey [int] NOT NULL,   
    OrderDateKey [int] NOT NULL,   
    DueDateKey [int] NOT NULL,   
    ShipDateKey [int] NOT NULL);  
  
CREATE CLUSTERED COLUMNSTORE INDEX cci_FactInternetSales2  
ON dbo.FactInternetSales2;  
  
INSERT INTO dbo.FactInternetSales2  
SELECT ProductKey, OrderDateKey, DueDateKey, ShipDateKey  
FROM dbo.FactInternetSales;  
  
SELECT * FROM sys.column_store_row_groups;  
```  
  
 Los resultados muestran que hay un grupo de filas OPEN, lo que significa que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esperará a que se agreguen más filas antes de cerrar el grupo de filas y moverá los datos al almacén de columnas. La siguiente instrucción regenera el índice de almacén de columnas agrupado, lo cual fuerza todas las filas hacia el almacén de columnas.  
  
```sql  
ALTER INDEX cci_FactInternetSales2 ON FactInternetSales2 REBUILD;  
SELECT * FROM sys.column_store_row_groups;  
```  
  
 Los resultados de la instrucción SELECT muestran que el grupo de filas es COMPRESSED, lo que significa que los segmentos de la columna del grupo de filas ahora están comprimidos y .almacenados en el almacén de columnas.  
  
### <a name="f-rebuild-a-partition-of-a-clustered-columnstore-index-offline"></a>F. Regenerar una partición de un índice de almacén de columnas agrupado sin conexión  
 **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])  
 
 Para regenerar una partición de un índice de almacén de columnas agrupado de gran tamaño, use ALTER INDEX REBUILD con la opción de partición. En este ejemplo se regenera la partición 12. A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], se recomienda reemplazar REBUILD con REORGANIZE.  
  
```sql  
ALTER INDEX cci_fact3   
ON fact3  
REBUILD PARTITION = 12;  
```  
  
### <a name="g-change-a-clustered-columstore-index-to-use-archival-compression"></a>G. Cambiar un índice de almacén de columnas agrupado para usar la compresión de archivo  
 No se aplica a: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
 Puede optar por reducir aún más el tamaño de un índice de almacén de columnas agrupado mediante la opción de compresión de datos COLUMNSTORE_ARCHIVE. Esto resulta práctico para datos más antiguos que prefiere mantener en un almacenamiento más económico. Se recomienda usarlo solamente en datos a los que no se acceda con frecuencia, ya que la descompresión es más lenta que la compresión de COLUMNSTORE normal.  
  
 En el ejemplo siguiente se vuelve a generar un índice de almacén de columnas clúster para usar la compresión archivada y, a continuación, se muestra cómo quitar la compresión de archivo. El resultado final usará solo la compresión de almacén de columnas.  
  
```sql  
--Prepare the example by creating a table with a clustered columnstore index.  
CREATE TABLE SimpleTable (  
    ProductKey [int] NOT NULL,   
    OrderDateKey [int] NOT NULL,   
    DueDateKey [int] NOT NULL,   
    ShipDateKey [int] NOT NULL  
);  
  
CREATE CLUSTERED INDEX cci_SimpleTable ON SimpleTable (ProductKey);  
  
CREATE CLUSTERED COLUMNSTORE INDEX cci_SimpleTable  
ON SimpleTable  
WITH (DROP_EXISTING = ON);  
  
--Compress the table further by using archival compression.  
ALTER INDEX cci_SimpleTable ON SimpleTable  
REBUILD  
WITH (DATA_COMPRESSION = COLUMNSTORE_ARCHIVE);  
  
--Remove the archive compression and only use columnstore compression.  
ALTER INDEX cci_SimpleTable ON SimpleTable  
REBUILD  
WITH (DATA_COMPRESSION = COLUMNSTORE);  
GO  
```  
  
## <a name="examples-rowstore-indexes"></a>Ejemplos: Índices de almacén de filas  
  
### <a name="a-rebuilding-an-index"></a>A. Volver a generar un índice  
 En el siguiente ejemplo se regenera un único índice en la tabla `Employee` de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
ALTER INDEX PK_Employee_EmployeeID ON HumanResources.Employee REBUILD;  
```  
  
### <a name="b-rebuilding-all-indexes-on-a-table-and-specifying-options"></a>B. Volver a generar todos los índices de una tabla y especificar opciones  
 En el siguiente ejemplo se especifica la palabra clave ALL. Esto vuelve a generar todos los índices asociados a la tabla Production.Product en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Se especifican tres opciones.  
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
```sql  
ALTER INDEX ALL ON Production.Product  
REBUILD WITH (FILLFACTOR = 80, SORT_IN_TEMPDB = ON, STATISTICS_NORECOMPUTE = ON);  
```  
  
El ejemplo siguiente agrega la opción ONLINE, incluida la opción de bloqueo de prioridad baja, y agrega la opción de compresión de fila.  
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
```sql  
ALTER INDEX ALL ON Production.Product  
REBUILD WITH   
(  
    FILLFACTOR = 80,   
    SORT_IN_TEMPDB = ON,  
    STATISTICS_NORECOMPUTE = ON,  
    ONLINE = ON ( WAIT_AT_LOW_PRIORITY ( MAX_DURATION = 4 MINUTES, ABORT_AFTER_WAIT = BLOCKERS ) ),   
    DATA_COMPRESSION = ROW  
);  
```  
  
### <a name="c-reorganizing-an-index-with-lob-compaction"></a>C. Reorganizar un índice con compactación LOB  
 En el siguiente ejemplo se reorganiza un único índice clúster en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Dado que el índice contiene un tipo de datos LOB en el nivel hoja, la instrucción también compacta todas las páginas que contienen datos de objetos grandes. Observe que no es necesario especificar la opción WITH (LOB_COMPACTION = ON) porque el valor predeterminado es ON.  
  
```sql  
ALTER INDEX PK_ProductPhoto_ProductPhotoID ON Production.ProductPhoto REORGANIZE WITH (LOB_COMPACTION = ON);  
```  
  
### <a name="d-setting-options-on-an-index"></a>D. Establecer opciones en un índice  
 En el siguiente ejemplo se establecen varias opciones en el índice `AK_SalesOrderHeader_SalesOrderNumber` en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
```sql  
ALTER INDEX AK_SalesOrderHeader_SalesOrderNumber ON  
    Sales.SalesOrderHeader  
SET (  
    STATISTICS_NORECOMPUTE = ON,  
    IGNORE_DUP_KEY = ON,  
    ALLOW_PAGE_LOCKS = ON  
    ) ;  
GO
```  
  
### <a name="e-disabling-an-index"></a>E. Deshabilitar un índice  
 En el siguiente ejemplo se deshabilita un índice no clúster de la tabla `Employee` en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
ALTER INDEX IX_Employee_ManagerID ON HumanResources.Employee DISABLE;
```  
  
### <a name="f-disabling-constraints"></a>F. Deshabilitar restricciones  
 En este ejemplo se deshabilita una restricción PRIMARY KEY al deshabilitar el índice PRIMARY KEY en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. La restricción FOREIGN KEY de la tabla subyacente se deshabilita automáticamente y aparece un mensaje de advertencia.  
  
```sql  
ALTER INDEX PK_Department_DepartmentID ON HumanResources.Department DISABLE;  
```  
  
El conjunto de resultados devuelve este mensaje de advertencia.  
  
```  
Warning: Foreign key 'FK_EmployeeDepartmentHistory_Department_DepartmentID'  
on table 'EmployeeDepartmentHistory' referencing table 'Department'  
was disabled as a result of disabling the index 'PK_Department_DepartmentID'.
```  
  
### <a name="g-enabling-constraints"></a>G. Habilitar restricciones  
 En el siguiente ejemplo se habilitan las restricciones PRIMARY KEY y FOREIGN KEY deshabilitadas en el ejemplo F.  
  
La restricción PRIMARY KEY se habilita al volver a generar el índice PRIMARY KEY.  
  
```sql  
ALTER INDEX PK_Department_DepartmentID ON HumanResources.Department REBUILD;  
```  
  
A continuación, se habilita la restricción FOREIGN KEY.  
  
```sql  
ALTER TABLE HumanResources.EmployeeDepartmentHistory  
CHECK CONSTRAINT FK_EmployeeDepartmentHistory_Department_DepartmentID;  
GO  
```  
  
### <a name="h-rebuilding-a-partitioned-index"></a>H. Volver a generar un índice con particiones  
 En el siguiente ejemplo se vuelve a generar una única partición, el número de partición `5`, del índice con particiones `IX_TransactionHistory_TransactionDate` de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. La partición 5 se vuelve a generar en línea y el tiempo de espera de 10 minutos para el bloqueo de prioridad baja se aplica independientemente a cada bloqueo adquirido por la operación de regeneración de índice. Si, durante este tiempo, el bloqueo no se puede obtener para completar la regeneración del índice, la instrucción de operación de la regeneración se anula.  
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
```sql  
-- Verify the partitioned indexes.  
SELECT *  
FROM sys.dm_db_index_physical_stats (DB_ID(),OBJECT_ID(N'Production.TransactionHistory'), NULL , NULL, NULL);  
GO  
--Rebuild only partition 5.  
ALTER INDEX IX_TransactionHistory_TransactionDate  
ON Production.TransactionHistory  
REBUILD Partition = 5   
   WITH (ONLINE = ON (WAIT_AT_LOW_PRIORITY (MAX_DURATION = 10 minutes, ABORT_AFTER_WAIT = SELF)));  
GO  
```  
  
### <a name="i-changing-the-compression-setting-of-an-index"></a>I. Cambiar la configuración de compresión de un índice  
 En el ejemplo siguiente se vuelve a generar un índice en una tabla de almacén de filas sin particiones.  
  
```sql
ALTER INDEX IX_INDEX1   
ON T1  
REBUILD   
WITH (DATA_COMPRESSION = PAGE);  
GO  
```  
  
Para obtener más ejemplos de compresión de datos, vea [Compresión de datos](../../relational-databases/data-compression/data-compression.md).  
 
### <a name="j-online-resumable-index-rebuild"></a>J. Regeneración de índice reanudable en línea

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]   

 En estos ejemplos se muestra cómo usar la regeneración de índice reanudable en línea. 

1. Ejecute una regeneración de índice en línea como una operación reanudable con MAXDOP=1.

   ```sql
   ALTER INDEX test_idx on test_table REBUILD WITH (ONLINE=ON, MAXDOP=1, RESUMABLE=ON) ;
   ```

2. Al volver a ejecutar el mismo comando (véase más arriba) después de pausar una operación de índice, se reanuda automáticamente la operación de regeneración de índice.

3. Ejecute una regeneración de índice en línea como una operación reanudable con MAX_DURATION establecido en 240 minutos.

   ```sql
   ALTER INDEX test_idx on test_table REBUILD WITH (ONLINE=ON, RESUMABLE=ON, MAX_DURATION=240) ; 
   ```
4. Pause una operación de regeneración de índice en línea reanudable que se esté ejecutando.

   ```sql
   ALTER INDEX test_idx on test_table PAUSE ;
   ```   
5. Reanude una regeneración de índice en línea para una regeneración de índice que se ejecutó como una operación reanudable, especificando un nuevo valor de MAXDOP en 4.

   ```sql
   ALTER INDEX test_idx on test_table RESUME WITH (MAXDOP=4) ;
   ```
6. Reanude una operación de regeneración de índice en línea para una regeneración de índice en línea que se ejecutó como reanudable. Establezca MAXDOP en 2, defina el tiempo de ejecución del índice que se está ejecutando como reanudable en 240 minutos y, en el caso de un índice que se está bloqueando, espere 10 minutos y después elimine todos los bloqueadores. 

   ```sql
      ALTER INDEX test_idx on test_table  
         RESUME WITH (MAXDOP=2, MAX_DURATION= 240 MINUTES, 
         WAIT_AT_LOW_PRIORITY (MAX_DURATION=10, ABORT_AFTER_WAIT=BLOCKERS)) ;
   ```      
7. Anular la operación de regeneración del índice reanudables que se está ejecutando o en pausa.

   ```sql
   ALTER INDEX test_idx on test_table ABORT ;
   ``` 
  
## <a name="see-also"></a>Consulte también  
[Guía de diseño y de arquitectura de índices de SQL Server](../../relational-databases/sql-server-index-design-guide.md)     
[Realizar operaciones de índice en línea](../../relational-databases/indexes/perform-index-operations-online.md)    
[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)     
[CREATE SPATIAL INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-spatial-index-transact-sql.md)   
[CREATE XML INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-xml-index-transact-sql.md)   
[DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md)   
[Deshabilitar índices y restricciones](../../relational-databases/indexes/disable-indexes-and-constraints.md)   
[Índices XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-indexes-sql-server.md)   
[Reorganizar y volver a generar índices](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)   
[sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)   
[EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)    
