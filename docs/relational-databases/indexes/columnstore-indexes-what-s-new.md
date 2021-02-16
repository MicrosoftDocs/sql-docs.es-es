---
description: Novedades de los índices de almacén de columnas
title: Novedades de los índices de almacén de columnas | Microsoft Docs
ms.custom: ''
ms.date: 05/11/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4a82a85c87fd1b5c9b2625ed0d8a7c51bee11e21
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351232"
---
# <a name="columnstore-indexes---what39s-new"></a>Novedades de los índices de almacén de columnas
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Resumen de las características de almacén de columnas disponibles en cada versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y en las últimas versiones de [!INCLUDE[ssSDS](../../includes/sssds-md.md)], [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  

 > [!NOTE]
 > Para [!INCLUDE[ssSDS](../../includes/sssds-md.md)], los índices de almacén de columnas están disponibles en los niveles Premium y Standard de [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] (S3 y versiones posteriores) y en todos los niveles de núcleo virtual. Para [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 y versiones posteriores, los índices de almacén de columnas están disponibles en todas las ediciones. Para [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] (antes de SP1) y versiones anteriores, los índices de almacén de columnas solo están disponibles en Enterprise Edition.
 
## <a name="feature-summary-for-product-releases"></a>Resumen de las características para cada versión del producto  
 En esta tabla se resumen las características fundamentales de los índices de almacén de columnas y los productos en los que están disponibles.  

|Característica de índice de almacén de columnas|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]|[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|[!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]|[!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]|[!INCLUDE[ssSDW](../../includes/sssdw-md.md)]|  
|-------------------------------|---------------------------|---------------------------|---------------------------|---------------------------|--------------------------------------------|-------------------------|---|  
|Ejecución del modo por lotes de consultas multiproceso|sí|sí|sí|sí|sí|sí|sí| 
|Ejecución del modo por lotes para las consultas de un solo subproceso|||sí|sí|sí|sí|sí|  
|Opción de compresión de archivos||sí|sí|sí|sí|sí|sí|  
|Aislamiento de instantánea y aislamiento de instantánea de lectura confirmada|||sí|sí|sí|sí|sí| 
|Especificación del índice de almacén de columnas a la hora de crear una tabla|||sí|sí|sí|sí|sí|  
|Compatibilidad de Always On con índices de almacén de columnas|sí|sí|sí|sí|sí|sí|sí| 
|Compatibilidad del elemento secundario legible de Always On con índices de almacén de columnas no agrupados de solo lectura|sí|sí|sí|sí|sí|sí|sí|  
|Compatibilidad del elemento secundario legible de Always On con índices de almacén de columnas actualizables|||sí||sí|||  
|Índice de almacén de columnas no agrupado de solo lectura en árbol B o montón|sí|sí|sí <sup>1</sup>|sí <sup>1</sup>|sí <sup>1</sup>|sí <sup>1</sup>|sí <sup>1</sup>|  
|Índice de almacén de columnas no agrupado actualizable en árbol B o montón|||sí|sí|sí|sí|sí|  
|Índices de árbol B adicionales permitidos en un montón o árbol B con índice de almacén de columnas no agrupado|sí|sí|sí|sí|sí|sí|sí|  
|Índice de almacén de columnas agrupado actualizable||sí|sí|sí||sí|sí|  
|Índice de árbol B en índice de almacén de columnas agrupado|||sí|sí||sí|sí|  
|Índice de almacén de columnas en una tabla optimizada para memoria|||sí|sí||sí|sí|  
|Compatibilidad de la definición del índice de almacén de columnas no agrupado con el uso de una condición filtrada|||sí|sí|sí|sí|sí|  
|Opción de retraso de compresión para los índices de almacén de columnas en `CREATE TABLE` y `ALTER TABLE`|||sí|sí|sí|sí|sí|
|El índice de almacén de columnas puede tener una columna calculada no persistente||||sí|sí|||   
|Compatibilidad con la combinación en segundo plano del motor de tuplas||||||sí|sí|sí|
  
 <sup>1</sup> Para crear un índice de almacén de columnas no agrupado de solo lectura, almacénelo en un grupo de archivos de solo lectura.  
 
> [!NOTE]
> El grado de paralelismo (DOP) de las operaciones del [modo de proceso por lotes](../../relational-databases/query-processing-architecture-guide.md#batch-mode-execution) está limitado a dos para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Standard Edition y a uno para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Web Edition y Express Edition. Se refiere a los índices de almacén de columnas que se crean en tablas basadas en disco y en tablas optimizadas para memoria.

## [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] 
 [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] agrega estas nuevas características.

### <a name="functional"></a>Funcional
- A partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], el motor de tuplas cuenta con la ayuda de una tarea de combinación en segundo plano que comprime automáticamente los grupos de filas delta OPEN que han existido durante algún tiempo, según lo determinado por un umbral interno, o combina los grupos de filas COMPRESSED desde donde se ha eliminado un gran número de filas. Anteriormente, se necesitaba una operación de reorganización de índices para combinar grupos de filas con datos eliminados parcialmente. De este modo, se mejora la calidad del índice de almacén de columnas a lo largo del tiempo. 

## [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 
 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] agrega estas nuevas características.

### <a name="functional"></a>Funcional
- [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] admite columnas calculadas no persistentes en índices de almacén de columnas agrupados. No se admiten columnas persistentes en índices de almacén de columnas agrupados. No se puede crear un índice no agrupado en un índice de almacén columnas con una columna calculada. 

## [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]  
 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] incorpora mejoras esenciales destinadas a mejorar el rendimiento y la flexibilidad de los índices de almacén de columnas. Estas mejoras optimizan los escenarios de almacenamiento de datos y facilitan los análisis operativos en tiempo real.  
  
### <a name="functional"></a>Funcional  
  
-   Una tabla de almacén de filas puede contar con un índice de almacén de columnas no agrupado actualizable. Antes, el índice de almacén de columnas no agrupado era de solo lectura.  
  
-   La definición del índice de almacén de columnas no agrupado admite el uso de una condición de filtrado. Para minimizar el impacto de rendimiento que tiene agregar un índice de almacén de columnas a una tabla OLTP, use una condición de filtrado para crear un índice de almacén de columnas no agrupado únicamente en los datos inactivos de la carga de trabajo operativa. 
  
-   Las tablas en memoria pueden tener un índice de almacén de columnas. Puede crearlo cuando se genere la tabla o agregarlo en otro momento con [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md). Antes, solo las tablas basadas en disco podían contar con un índice de almacén de columnas.  
  
-   Un índice de almacén de columnas agrupado puede tener uno o varios índices de almacén de filas no agrupados. Antes, el índice de almacén de columnas no admitía índices no agrupados. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mantiene automáticamente los índices no agrupados para las operaciones DML.  
  
-   Compatibilidad con las claves principales y claves externas mediante un índice de árbol B para aplicar estas restricciones en un índice de almacén de columnas agrupado.  
  
-   Los índices de almacén de columnas tienen una opción de retraso de compresión que minimiza el impacto de la carga de trabajo transaccional en los análisis operativos en tiempo real.  Esta opción permite cambiar con frecuencia filas para estabilizarlas antes de comprimirlas en el almacén de columnas. Para más información, vea [CREATE COLUMNSTORE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md) e [Introducción al almacén de columnas para análisis operativos en tiempo real](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md).  
  
### <a name="performance-for-database-compatibility-level-120-or-130"></a>Rendimiento del nivel de compatibilidad de base de datos 120 o 130  
  
-   Los índices de almacén de columnas admiten el nivel de aislamiento de instantánea de lectura confirmada (RCSI) y el aislamiento de instantánea (SI). De esta forma, se pueden realizar consultas de análisis homogéneas transaccionales sin ningún bloqueo.  
  
-   El almacén de columnas admite con la desfragmentación de índices mediante la eliminación de filas suprimidas sin necesidad de volver a generar el índice explícitamente. La instrucción `ALTER INDEX ... REORGANIZE` quita las filas eliminadas, según una directiva definida internamente, del almacén de columnas como una operación en línea  
  
-   Es posible acceder a los índices de almacén de columnas en una réplica secundaria legible de AlwaysOn. Se puede mejorar el rendimiento de los análisis operativos descargando consultas de análisis en una réplica secundaria de AlwaysOn.  
  
-   La Aplicación de agregados calcula las funciones de agregado `MIN`, `MAX`, `SUM`, `COUNT` y `AVG` durante los recorridos de tabla cuando el tipo de datos usa un máximo de ocho bytes y no es de cadena. Se admite la Aplicación de agregados, con la cláusula `GROUP BY` o sin esta, tanto para los índices de almacén de columnas agrupados como para los no agrupados. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], esta mejora está reservada para la Enterprise Edition.
  
-   La Aplicación de predicado de la cadena acelera las consultas que comparan cadenas del tipo VARCHAR/CHAR o NVARCHAR/NCHAR. Esto se aplica a los operadores de comparación habituales y se incluyen operadores que utilizan filtros de mapa de bits, como `LIKE`. Esto funciona con todas las intercalaciones admitidas. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], esta mejora está reservada para Enterprise Edition. 

-   Mejoras para las operaciones en modo de proceso por lotes mediante el uso de las capacidades de hardware basadas en vectores. [!INCLUDE[ssde_md](../../includes/ssde_md.md)] detecta el nivel de compatibilidad de la CPU para las extensiones de hardware AVX 2 (Extensiones de vector avanzadas) y SSE 4 (Extensiones SIMD de streaming 4) y las usa si son compatibles. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], esta mejora está reservada para Enterprise Edition.
  
### <a name="performance-for-database-compatibility-level-130"></a>Rendimiento del nivel de compatibilidad de base de datos 130  
  
-   El modo de ejecución por lotes admite ahora consultas con cualquiera de estas operaciones:  
    -   `SORT`  
    -   Agregados con varias funciones distintas. Algunos ejemplos son los siguientes: `COUNT/COUNT`, `AVG/SUM`, `CHECKSUM_AGG` y `STDEV/STDEVP`.  
    -   Funciones de agregado de ventana: `COUNT`, `COUNT_BIG`, `SUM`, `AVG`, `MIN`, `MAX` y `CLR`.  
    -   Agregados definidos por el usuario de ventana: `CHECKSUM_AGG`, `STDEV`, `STDEVP`, `VAR`, `VARP` y `GROUPING`.  
    -   Funciones analíticas de agregado de ventana: `LAG`, `LEAD`, `FIRST_VALUE`, `LAST_VALUE`, `PERCENTILE_CONT`, `PERCENTILE_DISC`, `CUME_DIST` y `PERCENT_RANK`.  

-   Las consultas de un solo proceso que se ejecuten con `MAXDOP 1` o un plan de consulta en serie se ejecutarán en el modo por lotes. Antes, solo las consultas multiproceso se ejecutaban por lotes.  

-   Las consultas de tabla optimizada para memoria pueden tener planes paralelos en el modo de interoperabilidad de SQL a la hora de acceder a datos de índices de almacén de columnas o almacén de filas.
  
### <a name="supportability"></a>Compatibilidad  
Las siguientes vistas del sistema son nuevas para el almacén de columnas:  
  
:::row:::
    :::column:::
        [sys.column_store_row_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-row-groups-transact-sql.md)
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
        [sys.dm_db_column_store_row_group_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_index_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.internal_partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-internal-partitions-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
  
Estas DMV basadas en OLTP en memoria contienen actualizaciones para el almacén de columnas:  

:::row:::
    :::column:::
        [sys.dm_db_xtp_hash_index_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-hash-index-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_xtp_index_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-index-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_xtp_memory_consumers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-memory-consumers-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_xtp_nonclustered_index_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-nonclustered-index-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_xtp_object_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-object-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_xtp_table_memory_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-table-memory-stats-transact-sql.md)
    :::column-end:::
:::row-end:::

### <a name="limitations"></a>Limitaciones    
-   En el caso de las tablas en memoria, los índices de almacén de columnas deben incluir todas las columnas; estos no pueden tener una condición de filtrado.  
-   Para las tablas en memoria, las consultas de índices de almacén de columnas se ejecutan únicamente en el modo de interoperabilidad y no en el modo nativo en memoria. Se admite la ejecución en paralelo.  
  
## [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] introdujo el índice de almacén de columnas agrupadas como el formato de almacenamiento principal. Este permitía cargas regulares, así como operaciones de actualización, eliminación e inserción.  
  
-   La tabla puede utilizar un índice de almacén de columnas agrupadas como el almacenamiento de tabla principal. No se permite ningún otro índice en la tabla, pero el índice de almacén de columnas agrupado se puede actualizar. De este modo, se pueden realizar cargas regulares y efectuar cambios en filas individuales.  
-   Los índices de almacén de columnas no agrupados siguen presentando la misma funcionalidad que en [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], excepto en lo concerniente a los operadores adicionales que se pueden ejecutar en el modo por lotes. Aún no se pueden actualizar, a menos que se reconstruyan o se utilice la modificación de las particiones. Los índices de almacén de columnas solo son compatibles en las tablas basadas en disco y no en las tablas en memoria.  
-   Los índices de almacén de columnas agrupados y no agrupados cuentan con una opción de compresión de archivos que comprimen aún más los datos. La opción de archivos resulta útil para reducir el tamaño de los datos en memoria y en disco, pero repercute negativamente en el rendimiento de las consultas. Funciona bien para los datos a los que se accede con poca frecuencia.  
-   Los índices de almacén de columnas agrupados y no agrupados funcionan de una manera muy similar; utilizan el mismo formato de almacenamiento en columnas, el mismo motor de procesamiento de consultas y el mismo conjunto de vistas de administración dinámica. La diferencia radica en los tipos de índices primarios frente a los secundarios; además, el índice de almacén de columnas no agrupado es de solo lectura.  
-   Estos operadores se ejecutan en modo por lotes de consultas multiproceso: SCAN, FILTER, PROJECT, JOIN, GROUP BY y UNION ALL.  
  
## [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] introdujo el índice de almacén de columnas no agrupado como otro tipo de índice en las tablas de almacén de filas y el procesamiento por lotes para las consultas de los datos de almacén de columnas.  
  
-   Una tabla de almacén de filas puede contar con un índice de almacén de columnas no agrupado.  
-   El índice de almacén de columnas es de solo lectura. Después de crear el índice de almacén, no se puede actualizar la tabla mediante las operaciones `INSERT`, `DELETE` y `UPDATE`; para realizar estas operaciones debe quitar el índice, actualizar la tabla y reconstruir el índice de almacén de columnas. Puede cargar datos adicionales en la tabla mediante la modificación de las particiones. La ventaja de la modificación de las particiones es que puede cargar datos sin quitar y reconstruir el índice de almacén de columnas.  
-   El índice de almacén de columnas siempre necesita almacenamiento adicional, normalmente un 10 % más que el almacén de filas, ya que almacena una copia de los datos.  
-   El procesamiento por lotes se traduce en que las consultas rinden el doble de bien o mejor, pero solo está disponible para la ejecución de consultas en paralelo.  
  
## <a name="see-also"></a>Consulte también  
 [Guía de diseño de índices de almacén de columnas](../../relational-databases/indexes/columnstore-indexes-design-guidance.md)   
 [Guía de carga de datos de los índices de almacén de columnas](../../relational-databases/indexes/columnstore-indexes-data-loading-guidance.md)   
 [Rendimiento de las consultas de índices de almacén de columnas](../../relational-databases/indexes/columnstore-indexes-query-performance.md)   
 [Introducción al almacén de columnas para análisis operativos en tiempo real](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)   
 [Índices de almacén de columnas para el almacenamiento de datos](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)   
 [Reorganizar y volver a generar índices](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)
  
