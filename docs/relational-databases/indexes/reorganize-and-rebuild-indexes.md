---
title: Detección y resolución de índices fragmentados | Microsoft Docs
description: En este artículo se describe cómo se produce la fragmentación de índices, cómo se detecta cuánta fragmentación existe y cómo se determina la mejor opción para resolver la fragmentación de índices mediante T-SQL y SQL Server Management Studio.
ms.custom: ''
ms.date: 03/19/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.technology: table-view-index
ms.topic: conceptual
f1_keywords:
- sql13.swb.index.rebuild.f1
- sql13.swb.indexproperties.fragmentation.f1
- sql13.swb.index.reorg.f1
helpviewer_keywords:
- large object defragmenting
- indexes [SQL Server], reorganizing
- index reorganization [SQL Server]
- reorganizing indexes
- defragmenting large object data types
- index fragmentation [SQL Server]
- index rebuilding [SQL Server]
- rebuilding indexes
- indexes [SQL Server], rebuilding
- defragmenting indexes
- nonclustered indexes [SQL Server], defragmenting
- fragmentation [SQL Server]
- index defragmenting [SQL Server]
- LOB data [SQL Server], defragmenting
- clustered indexes, defragmenting
ms.assetid: a28c684a-c4e9-4b24-a7ae-e248808b31e9
author: pmasl
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 340160c64a64479fe8d194887c838dab3bb7468c
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "100353677"
---
# <a name="resolve-index-fragmentation-by-reorganizing-or-rebuilding-indexes"></a>Solución de la fragmentación de índices mediante la reorganización o recompilación de índices

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

En este artículo se describe cómo se produce la desfragmentación de los índices y se describe su impacto en el rendimiento de las consultas. Una vez que determine la [cantidad de fragmentación que existe para un índice](#detecting-the-amount-of-fragmentation), puede desfragmentarlo si lo [reorganiza](#reorganize-an-index) o lo [vuelve a generar](#rebuild-an-index) mediante la ejecución de comandos de Transact-SQL en la herramienta que prefiera o con SQL Server Management Studio.

## <a name="index-fragmentation-overview"></a>Introducción a la fragmentación de índices

Qué es la fragmentación de índices y por qué le debería interesar:

- La fragmentación se produce cuando los índices tienen páginas en las que la ordenación lógica dentro del índice, basada en su valor de clave, no coincide con la ordenación física dentro de las páginas de índice.
- [!INCLUDE[ssde_md](../../includes/ssde_md.md)] modifica los índices automáticamente cada vez que se realizan operaciones de inserción, actualización o eliminación en los datos subyacentes. Por ejemplo, la adición de filas en una tabla puede hacer que las páginas existentes en los índices de almacén de filas se dividan para dejar espacio para la inserción de nuevos valores de clave. Con el tiempo, estas modificaciones pueden hacer que la información del índice se disperse por la base de datos (se fragmente). La fragmentación ocurre cuando los índices tienen páginas en las que la ordenación lógica, basada en el valor de clave, no coincide con la ordenación física dentro del archivo de datos.
- Los índices muy fragmentados pueden reducir el rendimiento de las consultas porque se necesita E/S adicional para localizar los datos a los que señala el índice. Más E/S hace que la aplicación responda con lentitud, sobre todo cuando se trata de operaciones de examen.

## <a name="detecting-the-amount-of-fragmentation"></a>Detección de la cantidad de fragmentación

El primer paso para decidir qué método de desfragmentación de índice se va a usar consiste en analizar el índice a fin de determinar la magnitud de la fragmentación. La fragmentación se detecta de manera diferente para los índices de almacén de filas y los de almacén de columnas.

> [!NOTE]
> Es especialmente importante revisar la fragmentación de índices o montones después de eliminar grandes cantidades de datos. En el caso de los montones, si hay actualizaciones frecuentes, también puede ser necesario revisar la fragmentación para evitar la proliferación de registros de reenvío. Para más información sobre los montones, vea [Montones (tablas sin índices agrupados)](../../relational-databases/indexes/heaps-tables-without-clustered-indexes.md#heap-structures). 

### <a name="detecting-fragmentation-of-rowstore-indexes"></a>Detección de la fragmentación de índices de almacén de filas

Con [sys.dm_db_index_physical_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md), puede detectar la fragmentación de un índice específico, de todos los índices de una tabla o vista indexada, de todos los índices de una base de datos o de todos los índices de todas las bases de datos. Para los índices con particiones, **sys.dm_db_index_physical_stats** también proporciona información de la fragmentación para cada partición.

El conjunto de resultados que devuelve **sys.dm_db_index_physical_stats** incluye las columnas siguientes:

|Columna|Descripción|
|------------|-----------------|
|**avg_fragmentation_in_percent**|Porcentaje de fragmentación lógica (páginas de un índice que no funcionan correctamente).|
|**fragment_count**|Número de fragmentos (páginas hoja físicamente consecutivas) en el índice.|
|**avg_fragment_size_in_pages**|Número promedio de páginas en un fragmento del índice.|

Después de determinar la magnitud de la fragmentación, use la tabla siguiente para determinar el mejor método para quitarla: [INDEX REORGANIZE](#reorganize-an-index) o [INDEX](#rebuild-an-index).

|Valor de **avg_fragmentation_in_percent**|Instrucción correctiva|
|-----------------------------------------------|--------------------------|
|> 5 % y < = 30 % <sup>1</sup>|ALTER INDEX REORGANIZE|
|> 30 % <sup>1</sup>|ALTER INDEX REBUILD WITH (ONLINE = ON) <sup>2</sup>|

<sup>1</sup> Estos valores proporcionan directrices generales para la determinación del punto en el que debe cambiar entre `ALTER INDEX REORGANIZE` y `ALTER INDEX REBUILD`. No obstante, los valores reales pueden variar de un caso a otro. Es importante que experimente la determinación del mejor umbral para su entorno.      

> [!TIP] 
> Por ejemplo, si se usa un índice determinado principalmente para operaciones de examen, la eliminación de la fragmentación puede mejorar el rendimiento de estas operaciones. Es posible que la ventaja de rendimiento no sea perceptible en los índices que se usan principalmente con operaciones de búsqueda.    
Del mismo modo, la eliminación de la fragmentación en un montón (una tabla sin índice agrupado) es especialmente útil para las operaciones de examen de índices no agrupados, pero tiene poco efecto en las operaciones de búsqueda.

<sup>2</sup> La regeneración de un índice se puede ejecutar en línea o sin conexión. La reorganización de un índice siempre se ejecuta en línea. Para lograr una disponibilidad similar a la opción de reorganización, debe volver a generar los índices en línea. Para obtener más información, vea [INDEX](#rebuild-an-index) y [Realización de operaciones de índice en línea](../../relational-databases/indexes/perform-index-operations-online.md).

Los índices con una fragmentación inferior al 5 % no se deben desfragmentar, ya que el beneficio de quitar una cantidad de fragmentación tan pequeña es casi siempre menor que el costo de CPU que implica la reorganización o regeneración del índice. Además, cuando se vuelven a generar o se reorganizan índices pequeños de almacén de filas, por lo general la fragmentación real no se reduce. Hasta [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], inclusive, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] asigna espacio mediante extensiones mixtas. Por consiguiente, las páginas de índices pequeños a veces se almacenan en extensiones mixtas. Las extensiones mixtas pueden estar compartidas por hasta ocho objetos, de modo que es posible que no se pueda reducir la fragmentación en un índice pequeño después de reorganizar o volver a generar dicho índice. Vea también [Consideraciones específicas para volver a generar índices de almacén de filas](#considerations-specific-to-rebuilding-rowstore-indexes). Para más información sobre las extensiones, vea la [Guía de arquitectura de páginas y extensiones](../../relational-databases/pages-and-extents-architecture-guide.md#extents).

### <a name="detecting-fragmentation-of-columnstore-indexes"></a>Detección de la fragmentación en índices de almacén de columnas

Mediante [sys.dm_db_column_store_row_group_physical_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md), puede determinar el porcentaje de filas eliminadas en un índice, que es una medida razonable para la fragmentación en un grupo de filas de un índice de almacén de columnas. Use esta información para calcular la fragmentación en un índice específico, todos los índices de una tabla, todos los índices de una base de datos o todos los índices de todas las bases de datos.

El conjunto de resultados devuelto por **sys.dm_db_column_store_row_group_physical_stats** incluye las columnas siguientes:

|Columna|Descripción|
|------------|-----------------|
|**total_rows**|Número de filas almacenadas físicamente en el grupo de filas. En el caso de los grupos de filas comprimidos, incluye las filas marcadas como eliminadas.|
|**deleted_rows**|Número de filas almacenadas físicamente en un grupo de filas comprimido que se han marcado para su eliminación. Es 0 en el caso de los grupos de filas que se encuentran en el almacén delta.|

Use esta información devuelta para calcular la fragmentación del índice mediante esta fórmula:

```
100*(ISNULL(deleted_rows,0))/NULLIF(total_rows,0)
```

Una vez que se conoce el grado de la fragmentación del índice, use la tabla siguiente para determinar el mejor método para eliminarla: [INDEX REORGANIZE](#reorganize-an-index) o [INDEX](#rebuild-an-index).

|Valor de la **fragmentación calculada en porcentaje**|Se aplica a la versión|Instrucción correctiva|
|-----------------------------------------------|--------------------------|--------------------------|
|> = 20 %|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|ALTER INDEX REBUILD|
|> = 20 %|A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]|ALTER INDEX REORGANIZE|

### <a name="to-check-the-fragmentation-of-a-rowstore-index-using-tsql"></a>Para comprobar la fragmentación de un índice de almacén de filas con [!INCLUDE[tsql](../../includes/tsql-md.md)]

En el ejemplo siguiente, se encuentra el porcentaje de fragmentación promedio de todos los índices de la tabla `HumanResources.Employee` de la base de datos `AdventureWorks2016`.

```sql
SELECT a.object_id, object_name(a.object_id) AS TableName,
    a.index_id, name AS IndedxName, avg_fragmentation_in_percent
FROM sys.dm_db_index_physical_stats
    (DB_ID (N'AdventureWorks2016_EXT')
        , OBJECT_ID(N'HumanResources.Employee')
        , NULL
        , NULL
        , NULL) AS a
INNER JOIN sys.indexes AS b
    ON a.object_id = b.object_id
    AND a.index_id = b.index_id;
GO
```

La instrucción anterior devuelve un conjunto de resultados similar al siguiente.

```
object_id   TableName    index_id    IndexName                                             avg_fragmentation_in_percent
----------- ------------ ----------- ----------------------------------------------------- ------------------------------
1557580587  Employee     1           PK_Employee_BusinessEntityID                          0
1557580587  Employee     2           IX_Employee_OrganizationalNode                        0
1557580587  Employee     3           IX_Employee_OrganizationalLevel_OrganizationalNode    0
1557580587  Employee     5           AK_Employee_LoginID                                   66.6666666666667
1557580587  Employee     6           AK_Employee_NationalIDNumber                          50
1557580587  Employee     7           AK_Employee_rowguid                                   0

(6 row(s) affected)
```

Para más información, consulte [sys.dm_db_index_physical_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md).

### <a name="to-check-the-fragmentation-of-a-columnstore-index-using-transact-sql"></a>Para comprobar la fragmentación de un índice de almacén de columnas con Transact-SQL

En el ejemplo siguiente, se encuentra el porcentaje de fragmentación promedio de todos los índices de la tabla `dbo.FactResellerSalesXL_CCI` de la base de datos `AdventureWorksDW2016`.

```sql
SELECT i.object_id,
    object_name(i.object_id) AS TableName,
    i.index_id,
    i.name AS IndexName,
    100*(ISNULL(SUM(CSRowGroups.deleted_rows),0))/NULLIF(SUM(CSRowGroups.total_rows),0) AS 'Fragmentation'
FROM sys.indexes AS i  
INNER JOIN sys.dm_db_column_store_row_group_physical_stats AS CSRowGroups
    ON i.object_id = CSRowGroups.object_id
    AND i.index_id = CSRowGroups.index_id
WHERE object_name(i.object_id) = 'FactResellerSalesXL_CCI'
GROUP BY i.object_id, i.index_id, i.name
ORDER BY object_name(i.object_id), i.name;
```

La instrucción anterior devuelve un conjunto de resultados similar al siguiente.

```
object_id   TableName                   index_id    IndexName                       Fragmentation
----------- --------------------------- ----------- ------------------------------- ---------------
114099447   FactResellerSalesXL_CCI     1           IndFactResellerSalesXL_CCI      0

(1 row(s) affected)
```

### <a name="check-index-fragmentation-using-sql-server-management-studio"></a>Comprobación de la fragmentación de un índice con SQL Server Management Studio

> [!NOTE]
> No se puede usar [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)] para calcular la fragmentación de los índices de almacén de columnas en SQL Server y tampoco para calcular la fragmentación de los índices en Azure SQL Database. Use el [ejemplo](#to-check-the-fragmentation-of-a-columnstore-index-using-transact-sql) de [!INCLUDE[tsql](../../includes/tsql-md.md)] anterior.

1. En el Explorador de objetos, expanda la base de datos que contiene la tabla en la que quiera comprobar la fragmentación de un índice.
2. Expanda la carpeta **Tablas** .
3. Expanda la tabla en la que quiera comprobar la fragmentación de un índice.
4. Expanda la carpeta **Índices** .
5. Haga clic con el botón derecho en el índice en el que quiere comprobar la fragmentación y seleccione **Propiedades**.
6. Bajo **Seleccionar una página**, seleccione **Fragmentación**.

La siguiente información está disponible en la página **Fragmentación** :

|Value|Descripción|
|---|---|
|**Llenado de página**|Indica el promedio de llenado de las páginas de índice como un porcentaje. 100% indica que las páginas de índice están completamente llenas. 50% indica que, como promedio, las páginas de índice están llenas a la mitad.|
|**Fragmentación total**|Porcentaje de fragmentación lógica. Indica el número de páginas de un índice que no están almacenadas en orden.|
|**Promedio de tamaño de fila**|Tamaño medio de una fila de nivel hoja.|
|**Profundidad**|Número de niveles del índice, incluido el nivel hoja.|
|**Registros reenviados**|Número de registros de un montón que han reenviado punteros a otra ubicación de datos. Este estado se produce durante una actualización, cuando no existe suficiente espacio para almacenar la nueva fila en la ubicación original.|
|**Filas fantasma**|Número de filas marcadas como eliminadas que todavía no se han quitado. Estas filas se quitarán en un subproceso de limpieza, cuando el servidor no esté ocupado. Este valor no incluye las filas que se retienen debido a una transacción pendiente de aislamiento de instantáneas.|
|**Tipo de índice**|Tipo de índice. Los valores posibles son **Índice clúster**, **Índice no clúster** y **XML principal**. Las tablas también se pueden almacenar como un montón (sin índices), pero en tal caso la página Propiedades del índice no puede abrirse.|
|**Filas de nivel de hoja**|Número de filas de nivel hoja.|
|**Tamaño máximo de la fila**|Tamaño máximo de la fila de nivel de hoja.|
|**Tamaño mínimo de la fila**|Tamaño mínimo de la fila de nivel de hoja.|
|**Páginas**|Número total de páginas de datos.|
|**Identificador de la partición**|Id. de partición del árbol b que contiene el índice.|
|**Filas fantasma de la versión**|Número de registros fantasma que se retienen debido a una transacción de aislamiento de instantánea pendiente.|

## <a name="defragmenting-indexes-by-rebuilding-or-reorganizing-the-index"></a>Desfragmentación de índices mediante la recompilación o reorganización del índice

Para desfragmentar un índice fragmentado, use uno de los métodos siguientes:

- Reorganización de índices
- Recompilación de índices

> [!NOTE]
> Para los índices con particiones creados a partir de un esquema de partición, puede usar cualquiera de los métodos siguientes en un índice completo o en una sola partición de un índice.

### <a name="reorganize-an-index"></a>Reorganización de un índice

La reorganización de un índice usa muy pocos recursos del sistema y es una operación en línea. Esto significa que los bloqueos de tabla a largo plazo no se mantienen y que las consultas o actualizaciones en la tabla subyacente pueden continuar durante la transacción `ALTER INDEX REORGANIZE`.

- Para los [índices de almacén de filas](clustered-and-nonclustered-indexes-described.md), el [!INCLUDE[ssde_md](../../includes/ssde_md.md)] desfragmenta el nivel hoja de los índices agrupados y no agrupados de las tablas y las vistas mediante la reordenación física de las páginas de nivel hoja para que coincidan con el orden lógico de los nodos hoja (de izquierda a derecha). La reorganización también compacta las páginas de índice en función del valor de factor de relleno del índice. Para ver el valor de factor de relleno, use [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md). Para obtener ejemplos de sintaxis, vea [Ejemplos: Reorganización del almacén de filas](../../t-sql/statements/alter-index-transact-sql.md#examples-rowstore-indexes).

- Al usar [índices de almacén de columnas](columnstore-indexes-overview.md), el almacén delta puede acabar con múltiples grupos filas de pequeño tamaño después de insertar, actualizar y eliminar datos a lo largo del tiempo. La reorganización de un índice de almacén de columnas fuerza a todos los grupos de filas al almacén de columnas y, luego, los combina en menos grupos de filas con más filas. La operación de reorganización también quita las filas que se hayan eliminado del almacén de columnas. Inicialmente, el proceso de reorganización requiere recursos de CPU adicionales para comprimir los datos, lo que puede reducir el rendimiento general del sistema. Pero tan pronto como se comprimen los datos, el rendimiento de las consultas mejora. Para obtener ejemplos de sintaxis, vea [Ejemplos: Reorganización de ColumnStore](../../t-sql/statements/alter-index-transact-sql.md#examples-columnstore-indexes).

### <a name="rebuild-an-index"></a>Volver a generar un índice

El proceso de volver a crear un índice quita y vuelve a crear el índice. En función del tipo de índice y de la versión del [!INCLUDE[ssde_md](../../includes/ssde_md.md)], una operación de recompilación puede realizarse en línea o sin conexión. Para obtener la sintaxis de T-SQL, vea [ALTER INDEX REBUILD](../../t-sql/statements/alter-index-transact-sql.md#rebuilding-indexes)

- Para los [índices de almacén de filas](clustered-and-nonclustered-indexes-described.md), la recompilación quita la fragmentación, reclama el espacio en disco mediante la compactación de las páginas según el valor de factor de relleno especificado o existente, y reordena las filas del índice en páginas contiguas. Cuando se especifica `ALL`, todos los índices de la tabla se quitan y se vuelven a generar en una única transacción. No es necesario quitar las restricciones FOREIGN KEY por adelantado. Cuando se regeneran índices con 128 extensiones o más, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] difiere las cancelaciones de asignación de página y sus bloqueos asociados hasta después de la confirmación de la transacción. Para obtener ejemplos de sintaxis, vea [Ejemplos: Reorganización del almacén de filas](../../t-sql/statements/alter-index-transact-sql.md#examples-rowstore-indexes).

- Para los [índices de almacén de columnas](columnstore-indexes-overview.md), la recompilación quita la fragmentación, mueve todas las filas al almacén de columnas y recupera el espacio en disco mediante la eliminación física de las filas que se han quitado de forma lógica de la tabla. 
  
  > [!TIP]
  > A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], normalmente no es necesario volver a generar el índice de almacén de columnas, ya que `REORGANIZE` realiza las operaciones básicas de una regeneración en segundo plano como una operación en línea. 
  
  Para obtener ejemplos de sintaxis, vea [Ejemplos: recompilación de almacén de columnas](../../t-sql/statements/alter-index-transact-sql.md#examples-columnstore-indexes).

### <a name="permissions"></a><a name="Permissions"></a> Permisos

Debe tener un permiso de `ALTER` sobre la tabla o vista. El usuario debe ser miembro de al menos uno de los siguientes roles:

- Rol de base de datos **db_ddladmin**<sup>1</sup>
- Rol de base de datos **db_owner**
- Rol de servidor **sysadmin**

<sup>1</sup>El rol de base de datos **db_ddladmin** es el que tiene [menos privilegios](/windows-server/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models).

### <a name="remove-fragmentation-using-ssmanstudiofull"></a><a name="SSMSProcedureReorg"></a> Eliminación de la fragmentación con [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]

#### <a name="to-reorganize-or-rebuild-an-index"></a>Para reorganizar o volver a generar un índice

1. En el Explorador de objetos, expanda la base de datos que contiene la tabla en la que desea reorganizar un índice.
2. Expanda la carpeta **Tablas** .
3. Expanda la tabla en la que desea reorganizar un índice.
4. Expanda la carpeta **Índices** .
5. Haga clic con el botón derecho en el índice que quiera reorganizar y seleccione **Reorganizar**.
6. En el cuadro de diálogo **Reorganizar índices** , compruebe que el índice correcto se encuentra en la cuadrícula **Índices que se van a reorganizar** y haga clic en **Aceptar**.
7. Active la casilla **Compactar datos de columnas de objetos de gran tamaño** para especificar que se compacten también todas las páginas que contengan datos de objetos grandes (LOB).
8. Haga clic en **Aceptar**.

#### <a name="to-reorganize-all-indexes-in-a-table"></a>Para reorganizar todos los índices de una tabla

1. En el Explorador de objetos, expanda la base de datos que contiene la tabla en la que desea reorganizar los índices.
2. Expanda la carpeta **Tablas** .
3. Expanda la tabla en la que desea reorganizar los índices.
4. Haga clic con el botón derecho en la carpeta **Índices** y seleccione **Reorganizar todo**.
5. En el cuadro de diálogo **Reorganizar índices** , compruebe que los índices adecuados están en **Índices que se van a reorganizar**. Para quitar un índice de la cuadrícula **Índices que se van a reorganizar** , seleccione el índice y, a continuación, presione la tecla SUPR.
6. Active la casilla **Compactar datos de columnas de objetos de gran tamaño** para especificar que se compacten también todas las páginas que contengan datos de objetos grandes (LOB).
7. Haga clic en **Aceptar**.

#### <a name="to-rebuild-an-index"></a>Para volver a generar un índice

1. En el Explorador de objetos, expanda la base de datos que contiene la tabla en la que desea reorganizar un índice.
2. Expanda la carpeta **Tablas** .
3. Expanda la tabla en la que desea reorganizar un índice.
4. Expanda la carpeta **Índices** .
5. Haga clic con el botón derecho en el índice que quiera reorganizar y seleccione **Volver a generar**.
6. En el cuadro de diálogo **Volver a generar índices** , compruebe que el índice correcto se encuentra en la cuadrícula **Índices que se van a volver a generar** y haga clic en **Aceptar**.
7. Active la casilla **Compactar datos de columnas de objetos de gran tamaño** para especificar que se compacten también todas las páginas que contengan datos de objetos grandes (LOB).
8. Haga clic en **Aceptar**.

### <a name="remove-fragmentation-using-tsql"></a><a name="TsqlProcedureReorg"></a> Eliminación de la fragmentación con [!INCLUDE[tsql](../../includes/tsql-md.md)]

> [!NOTE]
> Para obtener más ejemplos sobre el uso de [!INCLUDE[tsql](../../includes/tsql-md.md)] para volver a generar o reorganizar los índices, vea [Ejemplos de ALTER INDEX: índices de almacén de columnas](../../t-sql/statements/alter-index-transact-sql.md#examples-columnstore-indexes) y [Ejemplos de ALTER INDEX: índices de almacén de filas](../../t-sql/statements/alter-index-transact-sql.md#examples-rowstore-indexes).

#### <a name="to-reorganize-a-fragmented-index"></a>Para reorganizar un índice fragmentado

En el siguiente ejemplo se reorganiza el índice `IX_Employee_OrganizationalLevel_OrganizationalNode` en la tabla `HumanResources.Employee` de la base de datos `AdventureWorks2016`.

```sql
ALTER INDEX IX_Employee_OrganizationalLevel_OrganizationalNode
    ON HumanResources.Employee
    REORGANIZE;
```

En el siguiente ejemplo se reorganiza el índice de almacén de columnas `IndFactResellerSalesXL_CCI` en la tabla `dbo.FactResellerSalesXL_CCI` de la base de datos `AdventureWorksDW2016`.

```sql
-- This command will force all CLOSED and OPEN rowgroups into the columnstore.
ALTER INDEX IndFactResellerSalesXL_CCI
    ON FactResellerSalesXL_CCI
    REORGANIZE WITH (COMPRESS_ALL_ROW_GROUPS = ON);
```

#### <a name="to-reorganize-all-indexes-in-a-table"></a>Para reorganizar todos los índices de una tabla

En el siguiente ejemplo se reorganizan todos los índices en la tabla `HumanResources.Employee` de la base de datos `AdventureWorks2016`.

```sql
ALTER INDEX ALL ON HumanResources.Employee
   REORGANIZE;
```

#### <a name="to-rebuild-a-fragmented-index"></a>Para volver a generar un índice fragmentado

En el siguiente ejemplo se regenera un único índice en la tabla `Employee` de la base de datos `AdventureWorks2016`.

[!code-sql[IndexDDL#AlterIndex1](../../relational-databases/indexes/codesnippet/tsql/reorganize-and-rebuild-i_1.sql)]

#### <a name="to-rebuild-all-indexes-in-a-table"></a>Para volver a generar todos los índices de una tabla

En el ejemplo siguiente se vuelven a generar todos los índices asociados con la tabla de la base de datos de `AdventureWorks2016` mediante la palabra clave `ALL`. Se especifican tres opciones.

[!code-sql[IndexDDL#AlterIndex2](../../relational-databases/indexes/codesnippet/tsql/reorganize-and-rebuild-i_2.sql)]

Para obtener más información, vea [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).

#### <a name="automatic-index-and-statistics-management"></a>Administración automática de índice y estadísticas

Aproveche soluciones como la [desfragmentación de índice adaptable](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag) para administrar automáticamente las actualizaciones de estadísticas y la desfragmentación de índices para una o varias bases de datos. Este procedimiento elige automáticamente si se debe volver a generar o reorganizar un índice según su nivel de fragmentación, entre otros parámetros y actualiza las estadísticas con un umbral lineal.

## <a name="considerations-specific-to-rebuilding-rowstore-indexes"></a>Consideraciones específicas para volver a generar índices de almacén de filas

Al volver a generar un índice agrupado se vuelven a generar de forma automática los índices no agrupados que hagan referencia a la clave de agrupación en clústeres, si es necesario cambiar los identificadores físicos o lógicos contenidos en los registros de índices no agrupados.

En los escenarios siguiente se obliga a que todos los índices no agrupados de almacén de filas de una tabla se vuelvan a generar de forma automática:

- Creación de un índice agrupado en una tabla
- Eliminación de un índice agrupado, lo que hace que la tabla se almacene como un montón
- Cambio de la clave de agrupación en clústeres para incluir o excluir columnas

En los escenarios siguientes no es necesario que todos los índices no agrupados de almacén de filas se vuelvan a generar automáticamente en una tabla:

- Recompilación de un índice agrupado único
- Recompilación de un índice agrupado que no es único
- Cambio del esquema de índice, como al aplicar un esquema de partición a un índice agrupado o al mover el índice agrupado a un grupo de archivos diferente

> [!IMPORTANT]
> No es posible volver a organizar o generar un índice si el grupo de archivos en el que se encuentra está sin conexión o está definido como de solo lectura. Cuando se especifica la palabra clave ALL y hay uno o más índices en un grupo de archivos sin conexión o de solo lectura, se produce un error en la instrucción.
>
> Mientras se vuelve a generar un índice, el medio físico debe tener espacio suficiente para almacenar dos copias del índice. Cuando finaliza la recompilación, el [!INCLUDE[ssde_md](../../includes/ssde_md.md)] elimina el índice original.

Cuando se especifica `ALL` en la instrucción `ALTER INDEX`, se reorganizan los índices relacionales, tanto agrupados como no agrupados, y los índices XML.

## <a name="considerations-specific-to-rebuilding-a-columnstore-index"></a>Consideraciones específicas para volver a generar un índice de almacén de columnas

Cuando se vuelve a generar un índice de almacén de columnas, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] lee todos los datos del índice original de almacén de columnas, incluido el almacén delta. Combina los datos en grupos de filas nuevos y comprime los grupos de filas en el almacén de columnas. El [!INCLUDE[ssde_md](../../includes/ssde_md.md)] desfragmenta el almacén de columnas mediante la eliminación física de las filas que se han eliminado lógicamente de la tabla. Los bytes eliminados se reclaman en el disco.

> [!NOTE]
> La reorganización de un índice de almacén de columnas mediante [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)] combinará grupos de filas COMPRESSED, pero no obligará a que todos los grupos de filas se compriman en el almacén de columnas. Se comprimirán los grupos de filas con el estado CLOSED, mientras que los grupos de filas con el estado OPEN no se comprimirán en el almacén de columnas. Para comprimir forzosamente todos los grupos de filas, use el ejemplo de [!INCLUDE[tsql](../../includes/tsql-md.md)] [siguiente](#TsqlProcedureReorg).

> [!NOTE]
> A partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], el motor de tupla cuenta con la ayuda de una tarea de combinación en segundo plano que comprime automáticamente los grupos de filas delta OPEN que han existido durante algún tiempo, según lo determinado por un umbral interno, o combina los grupos de filas COMPRESSED desde donde se ha eliminado un gran número de filas. De este modo, se mejora la calidad del índice de almacén de columnas a lo largo del tiempo.    
> Para obtener más información sobre los términos y conceptos de almacén de columnas, vea [Índices de almacén de columnas: Información general](../../relational-databases/indexes/columnstore-indexes-overview.md).

### <a name="rebuild-a-partition-instead-of-the-entire-table"></a>Recompilación de una partición en lugar de la tabla completa

- Volver a generar la tabla completa tarda mucho si el índice es grande y requiere el espacio en disco suficiente para almacenar una copia adicional del índice durante la regeneración. Por lo general, solo es necesario volver a generar la partición que se ha usado más recientemente.
- Para las tablas con particiones, no es necesario que vuelva a generar el índice de almacén de columnas completo, ya que es probable que se produzca la fragmentación solo en las particiones que se han modificado recientemente. Las tablas de hechos y las tablas de dimensiones de gran tamaño normalmente tienen particiones para poder realizar operaciones de copia de seguridad y de administración con los fragmentos de la tabla.

### <a name="rebuild-a-partition-after-heavy-dml-operations"></a>Recompilación de una partición después de haber realizado operaciones DML intensivas

La recompilación de una partición la desfragmenta y reduce el almacenamiento de disco. La recompilación elimina todas las filas del almacén de columnas que están marcadas para la eliminación y traslada todos los grupos de filas del almacén delta al de columnas. Puede haber varios grupos de filas en el almacén delta con menos de un millón de filas.

### <a name="rebuild-a-partition-after-loading-data"></a>Recompilación de una partición después de cargar los datos

La recompilación de una partición después de cargar datos permite asegurarse de que todos los datos se almacenan en el almacén de columnas. Cuando cada proceso simultáneo carga al mismo tiempo menos de 100 000 filas en la misma partición, esta puede acabar con varios almacenes delta. La recompilación traslada todas las filas del almacén delta al almacén de columnas.

## <a name="considerations-specific-to-reorganizing-a-columnstore-index"></a>Consideraciones específicas para reorganizar un índice de almacén de columnas

Al reorganizar un índice de almacén de columnas, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] comprime cada grupo de filas delta con el estado CLOSED en el almacén de columnas como un grupo de filas comprimido. A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], el comando `REORGANIZE` realiza estas otras optimizaciones de desfragmentación en línea:

- Quita físicamente las filas de un grupo de filas cuando el 10 % o más de las filas se hayan eliminado lógicamente. Los bytes eliminados se reclaman en los medios físicos. Por ejemplo, si en un grupo de filas comprimido que contiene un millón de filas se eliminan 100 000 filas, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] quita las filas eliminadas y vuelve a comprimir el grupo de filas con 900 000 filas. Ahorra almacenamiento mediante la eliminación de filas eliminadas.

- Combina uno o varios grupos de filas comprimidos para aumentar las filas por grupo de filas, hasta alcanzar el máximo de 1 048 576 filas. Por ejemplo, si importa de forma masiva 5 lotes de 102 400 filas, obtendrá 5 grupos de filas comprimidos. Si ejecuta REORGANIZE, estos grupos de filas se combinarán en un grupo de filas comprimido del tamaño de 512 000 filas. Se supone que no había ninguna limitación de memoria o de tamaño de diccionario.

- Para los grupos de filas en los que un 10 % o más de las filas se han eliminado lógicamente, el [!INCLUDE[ssde_md](../../includes/ssde_md.md)] intenta combinar este grupo de filas con uno o varios grupos de filas. Por ejemplo, el grupo de filas 1 se comprimió con 500 000 filas y el grupo de filas 21 se comprimió con el máximo de 1 048 576 filas. El grupo de filas 21 tiene el 60 % de las filas eliminadas, lo que deja 409 830 filas. [!INCLUDE[ssde_md](../../includes/ssde_md.md)] favorece la opción de combinar estos dos grupos de filas para comprimir un nuevo grupo de filas que tenga 909 830 filas.

Después de realizar cargas de datos, puede haber varios grupos de filas pequeños en el almacén delta. Puede usar `ALTER INDEX REORGANIZE` para forzar a todos los grupos de filas al almacén de columnas y luego combinar los grupos de filas en menos grupos de filas con más filas. La operación de reorganización también quitará las filas que se hayan eliminado del almacén de columnas.

## <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones

Los índices de almacén de filas que tienen más de 128 extensiones se vuelven a generar en dos fases independientes: lógica y física. En la fase lógica, las unidades de asignación existentes que utiliza el índice están señaladas para cancelación de asignación las filas de datos se copian y ordenan y luego se mueven a las nuevas unidades de asignación creadas para almacenar el índice recompilado. En la fase física, las unidades de asignación previamente señaladas para cancelación de asignación se quitan físicamente de las transacciones breves que se realizan en segundo plano y no requieren demasiados bloqueos. Para obtener más información sobre las extensiones, vea [Guía de arquitectura de páginas y extensiones](../../relational-databases/pages-and-extents-architecture-guide.md).

La instrucción `ALTER INDEX REORGANIZE` requiere que el archivo de datos que contiene el índice tenga espacio disponible, ya que la operación solo puede asignar páginas de trabajo temporales en el mismo archivo, no en otro del grupo de archivos. Debido a esto, aunque el grupo de archivos tenga páginas libres disponibles, puede que al usuario le siga apareciendo el error 1105: `Could not allocate space for object '###' in database '###' because the '###' filegroup is full. Create disk space by deleting unneeded files, dropping objects in the filegroup, adding additional files to the filegroup, or setting autogrowth on for existing files in the filegroup.`

> [!WARNING]
> La creación y regeneración de índices no alineados en una tabla con más de 1.000 particiones es posible, pero no se admite. Si se hace, se puede degradar el rendimiento o consumir excesiva memoria durante estas operaciones. Microsoft recomienda usar solo [índices alineados](../partitions/partitioned-tables-and-indexes.md#aligned-index) cuando el número de particiones sea superior a 1000.

No es posible volver a organizar o generar un índice si el grupo de archivos en el que se encuentra está **sin conexión** o está definido como de **solo lectura**. Cuando se especifica la palabra clave `ALL` y hay uno o más índices en un grupo de archivos sin conexión o de solo lectura, se produce un error en la instrucción.

Estadísticas:

- Cuando se **crea** o se **vuelve a generar** un índice, las estadísticas se crean o actualizan mediante el examen de todas las filas de la tabla. Pero a partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], las estadísticas no se crean o actualizan examinando todas las filas de la tabla cuando se crea o se vuelve a generar un índice con particiones. En su lugar, el optimizador de consultas usa el algoritmo de muestreo predeterminado para generar estas estadísticas. Para obtener estadísticas sobre índices con particiones mediante el examen de todas las filas de la tabla, use [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) o [UPDATE STATISTICS](../../t-sql/statements/update-statistics-transact-sql.md) con la cláusula `FULLSCAN`.

- Cuando se **reorganiza** un índice, las estadísticas no se actualizan.

No es posible reorganizar un índice cuando `ALLOW_PAGE_LOCKS` está establecido en OFF.

Hasta [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], la regeneración de un índice de almacén de columnas agrupado es una operación sin conexión. El motor de base de datos tiene que adquirir un bloqueo exclusivo en la tabla o la partición mientras se produce la regeneración. Los datos están sin conexión y no se encuentran disponibles durante el proceso, incluso si se usa `NOLOCK`, el aislamiento de instantánea de lectura confirmada (RCSI) o el aislamiento de instantánea.
A partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], se puede volver a generar un índice de almacén de columnas agrupado mediante la opción `ONLINE = ON`.

Para una tabla de Azure Synapse Analytics (anteriormente [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]) con un índice de almacén de columnas agrupado ordenado, `ALTER INDEX REBUILD` reordenará los datos mediante TempDB. Supervise TempDB durante las operaciones de regeneración. Si necesita más espacio en TempDB, escale verticalmente el almacenamiento de datos. Vuelva a reducirlo verticalmente una vez completada la recompilación del índice.

Para una tabla de Azure Synapse Analytics (anteriormente [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]) con un índice de almacén de columnas agrupado ordenado, `ALTER INDEX REORGANIZE` no reordena los datos. Para reordenar los datos, use `ALTER INDEX REBUILD`.

## <a name="using-index-rebuild-to-recover-from-hardware-failures"></a>Uso de INDEX REBUILD para recuperarse de errores de hardware

En versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], a veces se podía volver a generar un índice no agrupado de almacén de filas para corregir incoherencias provocadas por errores de hardware.
A partir de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)], aún es posible reparar estas incoherencias entre el índice y el índice agrupado al volver a generar un índice no agrupado sin conexión. Pero no es posible reparar las incoherencias de índices no agrupados mediante la regeneración del índice en línea, ya que el mecanismo de regeneración en línea usa el índice no agrupado existente como base para la regeneración y, por tanto, la incoherencia persiste. La regeneración del índice sin conexión hará que se examine el índice clúster (o montón) y eliminará la incoherencia. Para garantizar una regeneración del índice agrupado, quite y vuelva a crear el índice no agrupado. Al igual que en las versiones anteriores, para recuperar incoherencias se recomienda restaurar los datos afectados desde una copia de seguridad. No obstante, es posible que pueda reparar las incoherencias del índice mediante la regeneración del índice no clúster sin conexión. Para obtener más información, vea [DBCC CHECKDB &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md).

## <a name="see-also"></a>Consulte también

- [Guía de diseño y de arquitectura de índices de SQL Server](../../relational-databases/sql-server-index-design-guide.md)
- [Realizar operaciones de índice en línea](../../relational-databases/indexes/perform-index-operations-online.md)
- [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)
- [Desfragmentación de índice adaptable](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag)
- [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md)
- [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)
- [Rendimiento de las consultas de índices de almacén de columnas](../../relational-databases/indexes/columnstore-indexes-query-performance.md)
- [Introducción al almacén de columnas para análisis operativos en tiempo real](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)
- [Índices de almacén de columnas para el almacenamiento de datos](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)
- [Índices de almacén de columnas y directiva de combinación de grupos de filas](/archive/blogs/sqlserverstorageengine/columnstore-index-merge-policy-for-reorganize)