---
description: 'Índices de almacén de columnas: Información general'
title: 'Índices de almacén de columnas: Introducción | Microsoft Docs'
ms.custom: ''
ms.date: 05/08/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- indexes creation, columnstore
- indexes [SQL Server], columnstore
- columnstore index
- batch mode execution
- columnstore index, described
- xVelocity, columnstore indexes
ms.assetid: f98af4a5-4523-43b1-be8d-1b03c3217839
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f4ac369f42fccd251cffb5b06f3603f8349f857a
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "100351219"
---
# <a name="columnstore-indexes-overview"></a>Índices de almacén de columnas: Información general
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Los índices de almacén de columnas son el estándar para almacenar y consultar las tablas de hechos de almacenamiento de datos de gran tamaño. Este índice usa el almacenamiento de datos basado en columnas y el procesamiento de consultas para lograr ganancias de hasta 10 veces el rendimiento de las consultas en el almacenamiento de datos sobre el almacenamiento tradicional orientado a filas. También puede lograr ganancias de hasta 10 veces la compresión de datos sobre el tamaño de los datos sin comprimir. Desde [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1, los índices de almacén de columnas permiten los análisis operativos, es decir, ejecutar análisis de rendimiento en tiempo real en una carga de trabajo transaccional.  
  
Obtenga información sobre un escenario relacionado:  
  
-   [Índices de almacén de columnas para almacenamiento de datos](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)  
-   [Introducción al almacén de columnas para el análisis operativo en tiempo real](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)  
  
## <a name="what-is-a-columnstore-index"></a>¿Qué es un índice de almacén de columnas?  
Un índice de almacén de columnas es una tecnología de almacenamiento, recuperación y administración de datos que emplea un formato de datos en columnas denominado *almacén de columnas*.  
  
### <a name="key-terms-and-concepts"></a>Términos y conceptos clave  
Los términos y conceptos clave siguientes están asociados a los índices de almacén de columnas.  
  
#### <a name="columnstore"></a>columnstore
Un almacén de columnas son datos organizados lógicamente como una tabla con filas y columnas, y almacenados físicamente en un formato de columnas.  
  
#### <a name="rowstore"></a>Almacén de filas
Un almacén de filas son datos organizados lógicamente como una tabla con filas y columnas, y almacenados físicamente en un formato de filas. Este formato es la forma tradicional de almacenar los datos de una tabla relacional. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], "almacén de filas" hace referencia a una tabla en la que el formato de almacenamiento de datos subyacente es un montón, un índice agrupado o una tabla optimizada para memoria.  
  
> [!NOTE]  
> Al tratar los índices de almacén de columnas, usamos los términos almacén de filas y almacén de columnas para hacer hincapié en el formato del almacenamiento de datos.  
  
#### <a name="rowgroup"></a>Grupo de filas
En un grupo de filas, las filas se comprimen al mismo tiempo con el formato del almacén de columnas. Un grupo de filas suele contener el número máximo de filas por grupo, que es 1 048 576 filas.  
  
Para conseguir unas tasas elevadas de rendimiento y compresión, el índice de almacén de columnas segmenta la tabla en grupos de filas y luego comprime cada grupo de filas a modo de columna. El número de filas del grupo de filas debe ser suficientemente grande como para mejorar las tasas de compresión y suficientemente pequeño como para beneficiarse de las operaciones en memoria.    

Un grupo de filas del que se han eliminado todos los datos pasa del estado COMPRESSED al estado TOMBSTONE, y luego se elimina mediante un proceso en segundo plano denominado "motor de tupla". Para obtener más información sobre los estados de filas, vea [sys.dm_db_column_store_row_group_physical_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md).

> [!TIP]
> Tener demasiados grupos de filas pequeños reduce la calidad del índice de almacén de columnas. Hasta [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], se requiere una operación de reorganización para combinar grupos de filas COMPRESSED más pequeños, siguiendo una directiva de umbral interno que determina cómo quitar las filas eliminadas y combinar los grupos de filas comprimidos.    
> A partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], una tarea de combinación en segundo plano también funciona para combinar filas de grupos COMPRESSED de donde se ha eliminado un gran número de filas.     
> Después de combinar grupos de filas más pequeños, se debe mejorar la calidad del índice. 

> [!NOTE]
> A partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], el motor de tupla cuenta con la ayuda de una tarea de combinación en segundo plano que comprime automáticamente los grupos de filas delta OPEN que han existido durante algún tiempo, según lo determinado por un umbral interno, o combina los grupos de filas COMPRESSED desde donde se ha eliminado un gran número de filas. De este modo, se mejora la calidad del índice de almacén de columnas a lo largo del tiempo.         

#### <a name="column-segment"></a>segmento de columna
Un segmento de columna es una columna de datos perteneciente al grupo de filas.  
  
-   Cada grupo de filas contiene un segmento de cada columna de la tabla.  
-   Cada sector de columna se comprime junto y se almacena en un medio físico.  
  
![Segmento de columna](../../relational-databases/indexes/media/sql-server-pdw-columnstore-columnsegment.gif "segmento de columna")  
  
#### <a name="clustered-columnstore-index"></a>Índice de almacén de columnas agrupado
Un índice de almacén de columnas agrupado es el almacenamiento físico de toda la tabla.    
  
![Índice de almacén de columnas agrupado](../../relational-databases/indexes/media/sql-server-pdw-columnstore-physicalstorage.gif "Índice de almacén de columnas agrupado")  
  
Para reducir la fragmentación de los segmentos de columna y mejorar el rendimiento, el índice de almacén de columnas puede almacenar temporalmente algunos datos en un índice agrupado (denominado *almacén delta*), así como una lista en forma de árbol b de los identificadores de las filas eliminadas. Las operaciones del almacén delta se administran en segundo plano. Para devolver los resultados correctos de la consulta, el índice clúster de almacén de columnas combina los resultados de la consulta tanto del almacén de columnas como del almacén delta.  
  
#### <a name="delta-rowgroup"></a>Grupo de filas delta
Un grupo de filas delta es un índice de árbol B agrupado que solo se usa con índices de almacén de columnas. Mejora el rendimiento y la compresión de almacén de columnas mediante el almacenamiento de filas hasta que el número de filas alcanza un umbral (1 048 576 filas) y luego se mueve al almacén de columnas.  

Cuando un grupo de filas delta alcanza el número máximo de filas, pasa de un estado OPEN a un estado CLOSED. Un proceso en segundo plano denominado "motor de tupla" comprueba si hay grupos de filas cerrados. Si el proceso encuentra un grupo de filas cerrado, comprime el grupo de filas delta y lo almacena en el almacén de columnas como un grupo de filas COMPRESSED. 

Cuando se ha comprimido un grupo de filas delta, el grupo de filas delta existente pasa al estado TOMBSTONE para que lo quite más adelante el motor de tupla cuando no haya referencias a él. 

Para obtener más información sobre los estados de filas, vea [sys.dm_db_column_store_row_group_physical_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md). 

> [!NOTE]
> A partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], el motor de tupla cuenta con la ayuda de una tarea de combinación en segundo plano que comprime automáticamente los grupos de filas delta OPEN que han existido durante algún tiempo, según lo determinado por un umbral interno, o combina los grupos de filas COMPRESSED desde donde se ha eliminado un gran número de filas. De este modo, se mejora la calidad del índice de almacén de columnas a lo largo del tiempo.         
  
#### <a name="deltastore"></a>Almacén delta
Un índice de almacén de columnas puede tener más de un grupo de filas delta. Todos los grupos de filas delta se denominan colectivamente almacén delta.   

Durante una gran carga masiva, la mayoría de las filas van directamente al almacén de columnas sin pasar por el almacén delta. Al final de la carga masiva, podría quedar un número de filas menor que el tamaño mínimo de un grupo de filas, que es 102 400 filas. Como resultado, las últimas filas van al almacén delta en lugar de al almacén de columnas. En el caso de cargas masivas pequeñas con menos de 102.400 filas, todas las filas van directamente al almacén delta.  
  
#### <a name="nonclustered-columnstore-index"></a>índice no clúster de almacén de columnas
Un índice de almacén de columnas no agrupado y un índice de almacén de columnas agrupado funcionan del mismo modo. La diferencia es que un índice no agrupado es un índice secundario creado en una tabla de almacén de filas, pero un índice de almacén de columnas agrupado es el almacenamiento principal de toda la tabla.  
  
El índice no agrupado contiene una copia de parte o la totalidad de las filas y columnas de la tabla subyacente. El índice se define como una o varias columnas de la tabla y tiene una condición opcional que filtra las filas.  
  
Un índice de almacén de columnas no agrupado permite análisis operativos en tiempo real donde la carga de trabajo OLTP usa el índice agrupado subyacente mientras los análisis se ejecutan simultáneamente en el índice de almacén de columnas. Para más información, consulte [Introducción al almacén de columnas para análisis operativos en tiempo real](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md).  
  
#### <a name="batch-mode-execution"></a>Ejecución del modo por lotes
La ejecución del modo por lotes es un método de procesamiento de consultas que se usa para procesar varias filas a la vez. La ejecución del modo por lotes está estrechamente integrada con el formato de almacenamiento de almacén de columnas y optimizada alrededor del mismo. La ejecución en modo por lotes se conoce en ocasiones como ejecución *basada en vectores* o *vectorizada*. Las consultas de los índices de almacén de columnas usan la ejecución en modo por lotes, ya que suele duplicar o hasta cuadruplicar el rendimiento de las consultas. Para más información, consulte la [Guía de arquitectura de procesamiento de consulta](../query-processing-architecture-guide.md#execution-modes). 
  
##  <a name="why-should-i-use-a-columnstore-index"></a><a name="benefits"></a> ¿Por qué debo usar un índice de almacén de columnas?  
Un índice de almacén de columnas puede proporcionar un nivel muy alto de compresión de datos, normalmente hasta 10 veces superior, para reducir considerablemente el costo de almacenamiento del almacén de datos. Para el análisis, un índice de almacén ofrece un mejor rendimiento de orden de magnitud que un índice de árbol b. Los índices de almacén de columnas son el formato de almacenamiento de datos preferido para cargas de trabajo de análisis y almacenamiento de datos. Desde [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], puede usar índices de almacén de columnas para llevar a cabo análisis operativos en tiempo real en la carga de trabajo operativa.  
  
Motivos por los que los índices de almacén de columnas son tan rápidos:  
  
-   Las columnas almacenan valores del mismo dominio y normalmente tienen valores similares, lo que resulta en altas tasas de compresión. Los cuellos de botella de E/S del sistema se minimizan o eliminan y la superficie de memoria se disminuye considerablemente.  
  
-   Los factores de compresión altos mejoran el rendimiento de las consultas mediante la utilización de una superficie en memoria menor. A su vez, el rendimiento de las consultas puede mejorar porque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede realizar más consultas y operaciones de datos en memoria.  
  
-   La ejecución por lotes mejora el rendimiento de las consultas, hasta llegar a duplicarlo o cuadruplicarlo, ya que procesa varias filas juntas.  
  
-   Con frecuencia, las consultas seleccionan únicamente unas pocas columnas de una tabla, lo que reduce la E/S total desde los medios físicos.  
  
## <a name="when-should-i-use-a-columnstore-index"></a>¿Cuándo debo usar un índice de almacén de columnas?  
Casos de uso recomendados:  
  
-   Use un índice de almacén de columnas agrupado para almacenar tablas de hechos y tablas de dimensiones grandes de cargas de trabajo de almacenamiento de datos. Este método mejora el rendimiento de las consultas y la compresión de datos, hasta multiplicarlos por diez. Para más información, consulte [Índices de almacén de columnas para el almacenamiento de datos](~/relational-databases/indexes/columnstore-indexes-data-warehouse.md).  
  
-   Use un índice de almacén de columnas no agrupado para realizar análisis en tiempo real en una carga de trabajo OLTP. Para más información, consulte [Introducción al almacén de columnas para análisis operativos en tiempo real](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md).  
  
### <a name="how-do-i-choose-between-a-rowstore-index-and-a-columnstore-index"></a>¿Cómo elegir entre un índice de almacén de filas y un índice de almacén de columnas?  
Los índices de almacén de filas tienen un mejor rendimiento en las consultas que buscan en datos, cuando se busca un valor determinado o en las consultas realizadas en un rango de valores muy acotado. Use índices de almacén de filas en las cargas de trabajo transaccionales, porque tienden a necesitar búsquedas de tabla, más que recorridos de tabla.  
  
Los índices de almacén de columnas ofrecen un alto rendimiento en consultas analíticas en las que se analizan grandes cantidades de datos, especialmente en tablas grandes. Use índices de almacén de columnas en cargas de trabajo de análisis y almacenamiento de datos, especialmente en las tablas de hechos, porque tienden a necesitar recorridos de tabla completos, más que búsquedas de tabla.  
  
### <a name="can-i-combine-rowstore-and-columnstore-on-the-same-table"></a>¿Puedo combinar un almacén de filas y un almacén de columnas en la misma tabla?  
Sí. Desde [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], puede crear un índice de almacén de columnas no agrupado actualizable en una tabla de almacén de filas. El índice de almacén de columnas almacena una copia de las columnas seleccionadas, por lo que necesita espacio adicional para estos datos, pero los datos seleccionados se comprimen 10 veces en promedio. Puede ejecutar análisis en el índice de almacén de columnas y realizar transacciones en el índice de almacén de filas al mismo tiempo. El almacén de columnas se actualiza cuando cambian los datos de la tabla de almacén de filas, de modo que ambos índices trabajan con los mismos datos.  
  
Desde [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], puede tener uno o varios índices de almacén de filas en un índice de almacén de columnas y realizar búsquedas eficientes de tabla en el almacén de columnas subyacente. También habrá disponibles otras opciones. Por ejemplo, podrá aplicar una restricción de clave principal mediante una restricción UNIQUE en la tabla de almacén de filas. Como un valor que no es único no se inserta en la tabla de almacén de filas, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede insertar ese valor en el almacén de columnas.  
  
## <a name="metadata"></a>Metadatos  
Todas las columnas de un índice de almacén de columnas se almacenan en los metadatos como columnas incluidas. El índice de almacén de columnas no tiene columnas de clave.  

:::row:::
    :::column:::
        [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.internal_partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-internal-partitions-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.column_store_segments &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-segments-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.column_store_dictionaries &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-dictionaries-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.column_store_row_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-row-groups-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_column_store_row_group_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-operational-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_column_store_row_group_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_column_store_object_pool &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-column-store-object-pool-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_column_store_row_group_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-operational-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_index_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
 

## <a name="related-tasks"></a>Tareas relacionadas  
Todas las tablas relacionales (a menos que se especifiquen como un índice de almacén de columnas no agrupado) usan el almacén de filas como formato de datos subyacente. `CREATE TABLE` crea una tabla de almacén de filas a menos que especifique la opción `WITH CLUSTERED COLUMNSTORE INDEX`.  
  
Cuando se crea una tabla con la instrucción `CREATE TABLE`, puede crearla como un almacén de columnas especificando la opción `WITH CLUSTERED COLUMNSTORE INDEX`. Si ya tiene una tabla de almacén de filas y quiere convertirla en un almacén de columnas, puede usar la instrucción `CREATE COLUMNSTORE INDEX`.  
  
|Tarea|Temas de referencia|Notas|  
|----------|----------------------|-----------|  
|Crear una tabla como un almacén de columnas.|[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)|Desde [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], puede crear la tabla como un índice de almacén de columnas agrupado. No es necesario crear primero una tabla de almacén de filas y, luego, convertirla en almacén de columnas.|  
|Crear una tabla de memoria con un índice de almacén de columnas.|[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)|Desde [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], puede crear una tabla optimizada para memoria con un índice de almacén de columnas. El índice de almacén de columnas también se puede agregar una vez creada la tabla mediante el uso de la sintaxis de `ALTER TABLE ADD INDEX`.|  
|Convertir una tabla de almacén de filas en un almacén de columnas.|[CREATE COLUMNSTORE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md)|Convierta un árbol binario o un montón existentes en un almacén de columnas. Los ejemplos muestran cómo tratar los índices existentes, así como el nombre del índice, al realizar esta conversión.|  
|Convertir una tabla de almacén de columnas en un almacén de filas.|[CREATE CLUSTERED INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md#d-convert-a-columnstore-table-to-a-rowstore-table-with-a-clustered-index) o [Volver a convertir una tabla de almacén de columnas en un montón de almacén de filas](../../t-sql/statements/create-columnstore-index-transact-sql.md#e-convert-a-columnstore-table-back-to-a-rowstore-heap) |Habitualmente, esta conversión no es necesaria pero puede haber ocasiones en las que necesite realizarla. Los ejemplos muestran cómo convertir un almacén de columnas en un montón o un índice agrupado.|  
|Crear un índice de almacén de columnas en una tabla de almacén de filas.|[CREATE COLUMNSTORE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md)|Una tabla de almacén de filas puede tener un índice de almacén de columnas. Desde [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], los índices de almacén de columnas pueden tener una condición de filtrado. En los ejemplos se usa la sintaxis básica.|  
|Crear índices de rendimiento para análisis operativos.|[Introducción al almacén de columnas para el análisis operativo en tiempo real](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)|Se describe cómo crear índices de almacén de columnas y de árbol b complementarios para que las consultas OLTP usen los índices de árbol b y, las consultas de análisis, los índices de almacén de columnas.|  
|Crear índices de almacén de columnas de rendimiento para el almacenamiento de datos.|[Índices de almacén de columnas para almacenamiento de datos](~/relational-databases/indexes/columnstore-indexes-data-warehouse.md)|Se describe cómo usar índices de árbol b en las tablas de almacén de columnas para crear consultas de almacenamiento de datos de rendimiento.|  
|Usar un índice de árbol b para aplicar una restricción de clave principal en un índice de almacén de columnas.|[Índices de almacén de columnas para almacenamiento de datos](~/relational-databases/indexes/columnstore-indexes-data-warehouse.md)|Se muestra cómo combinar índices de árbol b y de almacén de columnas para aplicar restricciones de clave principal en el índice de almacén de columnas.|  
|Eliminar un índice de almacén de columnas.|[DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md)|Para eliminar un índice de almacén de columnas, se usa la sintaxis de `DROP INDEX` estándar que usan los índices de árbol b. Si se elimina un índice de almacén de columnas agrupado, la tabla de almacén de columnas se convierte en un montón.|  
|Eliminar una fila de un índice de almacén de columnas.|[DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)|Use [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md) para eliminar una fila.<br /><br /> **fila de almacén de columna**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] marca la fila como eliminada lógicamente, pero no recupera el almacenamiento físico de la fila hasta que se vuelva a generar el índice.<br /><br /> **fila de almacén delta**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] elimina la fila lógica y físicamente.|  
|Actualizar una fila en el índice de almacén de columnas.|[UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)|Use [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md) para actualizar una fila.<br /><br /> **fila de almacén de columna**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] marca la fila como eliminada lógicamente y, después, inserta la fila actualizada en el almacén delta.<br /><br /> **fila almacén delta**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] actualiza la fila en el almacén delta.|  
|Cargar datos en un índice de almacén de columnas.|[Carga de datos de índices de almacén de columnas](~/relational-databases/indexes/columnstore-indexes-data-loading-guidance.md)||  
|Forzar que todas las filas del almacén delta vayan al almacén de columnas.|[ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md) ... `REBUILD`<br /><br /> [Reorganizar y volver a generar índices](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)|`ALTER INDEX` con la opción `REBUILD` hace que todas las filas vayan al almacén de columnas.|  
|Desfragmentar un índice de almacén de columnas.|[ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)|`ALTER INDEX ... REORGANIZE` desfragmenta los índices de almacén de columnas en línea.|  
|Combinar tablas con índices de almacén de columnas.|[MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md)||  
  
## <a name="see-also"></a>Consulte también  
 [Carga de datos de índices de almacén de columnas](~/relational-databases/indexes/columnstore-indexes-data-loading-guidance.md)   
 [Resumen de las características de los índices de almacén de columnas para cada versión](~/relational-databases/indexes/columnstore-indexes-what-s-new.md)   
 [Rendimiento de las consultas de índices de almacén de columnas](~/relational-databases/indexes/columnstore-indexes-query-performance.md)   
 [Introducción al almacén de columnas para análisis operativos en tiempo real](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)   
 [Índices de almacén de columnas para almacenamiento de datos](~/relational-databases/indexes/columnstore-indexes-data-warehouse.md)   
 [Desfragmentación de índices de almacén de columnas](~/relational-databases/indexes/columnstore-indexes-defragmentation.md)   
 [ Guía de diseño de índices de SQL Server](../../relational-databases/sql-server-index-design-guide.md)   
 [Diseño de los índices de almacén de columnas](../../relational-databases/sql-server-index-design-guide.md#columnstore_index)   
  
  
