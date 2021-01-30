---
description: sp_spaceused (Transact-SQL)
title: sp_spaceused (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_spaceused_TSQL
- sp_spaceused
dev_langs:
- TSQL
helpviewer_keywords:
- sp_spaceused
ms.assetid: c6253b48-29f5-4371-bfcd-3ef404060621
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 480b61493dc38ea91679e590f3abe63bde3a48c0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189312"
---
# <a name="sp_spaceused-transact-sql"></a>sp_spaceused (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Muestra el número de filas, el espacio de disco reservado y el espacio de disco utilizado por una tabla, vista indizada o cola de [!INCLUDE[ssSB](../../includes/sssb-md.md)] de la base de datos actual, o bien muestra el espacio de disco reservado y el que utiliza la base de datos completa.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
sp_spaceused [[ @objname = ] 'objname' ]   
[, [ @updateusage = ] 'updateusage' ]  
[, [ @mode = ] 'mode' ]  
[, [ @oneresultset = ] oneresultset ]  
[, [ @include_total_xtp_storage = ] include_total_xtp_storage ]
```  
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="arguments"></a>Argumentos  

Para [!INCLUDE[sssdw-md](../../includes/sssdw-md.md)] y [!INCLUDE[sspdw-md](../../includes/sspdw-md.md)] , `sp_spaceused` debe especificar parámetros con nombre (por ejemplo, `sp_spaceused (@objname= N'Table1');` en lugar de confiar en la posición ordinal de los parámetros. 

`[ @objname = ] 'objname'`
   
 Se trata del nombre completo o incompleto de la tabla, vista indizada o cola para la que se solicita información de uso del espacio. Las comillas solo son necesarias si se especifica un nombre de objeto completo. Si se proporciona un nombre de objeto completo, incluido el nombre de una base de datos, el nombre de la base de datos debe ser el nombre de la base de datos actual.  
Si no se especifica *objName* , se devuelven resultados para toda la base de datos.  
*objName* es de tipo **nvarchar (776)** y su valor predeterminado es NULL.  
> [!NOTE]  
> [!INCLUDE[sssdw-md](../../includes/sssdw-md.md)] y [!INCLUDE[sspdw-md](../../includes/sspdw-md.md)] solo admiten objetos de base de datos y tabla.
  
`[ @updateusage = ] 'updateusage'` Indica que se debe ejecutar DBCC UPDATEUSAGE para actualizar la información de uso del espacio. Cuando no se especifica *objName* , la instrucción se ejecuta en toda la base de datos; de lo contrario, la instrucción se ejecuta en *objName*. Los valores pueden ser **true** o **false**. *UPDATEUSAGE* es de tipo **VARCHAR (5)** y su valor predeterminado es **false**.  
  
`[ @mode = ] 'mode'` Indica el ámbito de los resultados. En una tabla o base de datos extendida, el parámetro *mode* permite incluir o excluir la parte remota del objeto. Para obtener más información, vea [Stretch Database](../../sql-server/stretch-database/stretch-database.md).  
  
 El argumento *mode* puede tener los valores siguientes:  
  
|Value|Descripción|  
|-----------|-----------------|  
|ALL|Devuelve las estadísticas de almacenamiento del objeto o la base de datos, incluida la parte local y la parte remota.|  
|LOCAL_ONLY|Devuelve las estadísticas de almacenamiento de solo la parte local del objeto o la base de datos. Si el objeto o la base de datos no están habilitados para Stretch, devuelve las mismas estadísticas que @mode = ALL.|  
|REMOTE_ONLY|Devuelve las estadísticas de almacenamiento de solo la parte remota del objeto o la base de datos. Esta opción genera un error cuando se cumple una de las condiciones siguientes:<br /><br /> La tabla no está habilitada para Stretch.<br /><br /> La tabla está habilitada para Stretch, pero nunca ha habilitado la migración de datos. En este caso, la tabla remota todavía no tiene un esquema.<br /><br /> El usuario ha quitado manualmente la tabla remota.<br /><br /> El aprovisionamiento del archivo de datos remotos devolvió el estado correcto, pero en realidad se produjo un error.|  
  
 el *modo* es **VARCHAR (11)** y su valor predeterminado es **N'ALL '**.  
  
`[ @oneresultset = ] oneresultset` Indica si se va a devolver un único conjunto de resultados. El argumento *oneresultset* puede tener los siguientes valores:  
  
|Value|Descripción|  
|-----------|-----------------|  
|0|Cuando *\@ objName* es null o no se especifica, se devuelven dos conjuntos de resultados. Dos conjuntos de resultados son el comportamiento predeterminado.|  
|1|Cuando *\@ objName* = null o no se especifica, se devuelve un conjunto de resultados único.|  
  
 *oneresultset* es de **bit** y su valor predeterminado es **0**.  

`[ @include_total_xtp_storage] 'include_total_xtp_storage'`
**Se aplica a:** [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] , [!INCLUDE[sssds-md](../../includes/sssds-md.md)] .  
  
 Cuando @oneresultset = 1, el parámetro @include_total_xtp_storage determina si el conjunto de resultados único incluye columnas para MEMORY_OPTIMIZED_DATA almacenamiento. El valor predeterminado es 0, es decir, de forma predeterminada (si se omite el parámetro), las columnas XTP no se incluyen en el conjunto de resultados.  

## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Si se omite *objName* y el valor de *oneresultset* es 0, se devuelven los conjuntos de resultados siguientes para proporcionar la información de tamaño de la base de datos actual.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**database_name**|**nvarchar(128)**|Nombre de la base de datos actual.|  
|**database_size**|**VARCHAR (18)**|Tamaño de la base de datos actual en megabytes. **database_size** incluye archivos de datos y de registro.|  
|**espacio sin asignar**|**VARCHAR (18)**|Espacio de la base de datos que no se ha reservado para objetos de base de datos.|  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**sector**|**VARCHAR (18)**|Espacio total asignado por los objetos de la base de datos.|  
|**data**|**VARCHAR (18)**|Cantidad total de espacio utilizado por los datos.|  
|**index_size**|**VARCHAR (18)**|Cantidad total de espacio utilizado por índices.|  
|**sin usar**|**VARCHAR (18)**|Espacio total reservado para los objetos de la base de datos, pero no utilizado todavía.|  
  
 Si se omite *objName* y el valor de *oneresultset* es 1, se devuelve el siguiente conjunto de resultados único para proporcionar la información de tamaño de la base de datos actual.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**database_name**|**nvarchar(128)**|Nombre de la base de datos actual.|  
|**database_size**|**VARCHAR (18)**|Tamaño de la base de datos actual en megabytes. **database_size** incluye archivos de datos y de registro.|  
|**espacio sin asignar**|**VARCHAR (18)**|Espacio de la base de datos que no se ha reservado para objetos de base de datos.|  
|**sector**|**VARCHAR (18)**|Espacio total asignado por los objetos de la base de datos.|  
|**data**|**VARCHAR (18)**|Cantidad total de espacio utilizado por los datos.|  
|**index_size**|**VARCHAR (18)**|Cantidad total de espacio utilizado por índices.|  
|**sin usar**|**VARCHAR (18)**|Espacio total reservado para los objetos de la base de datos, pero no utilizado todavía.|  
  
 Si se especifica *objName* , se devuelve el siguiente conjunto de resultados para el objeto especificado.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**name**|**nvarchar(128)**|Nombre del objeto del que se solicitó la información de utilización de espacio.<br /><br /> El nombre del esquema del objeto no se devuelve. Si se requiere el nombre de esquema, use las vistas de administración dinámica [Sys.dm_db_partition_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md) o [Sys.dm_db_index_physical_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md) para obtener información de tamaño equivalente.|  
|**rows**|**Char (20)**|Número de filas de la tabla. Si el objeto especificado es una cola de [!INCLUDE[ssSB](../../includes/sssb-md.md)], esta columna indica el número de mensajes de la misma.|  
|**sector**|**VARCHAR (18)**|Cantidad total de espacio reservado para *objName*.|  
|**data**|**VARCHAR (18)**|Cantidad total de espacio utilizado por los datos en *objName*.|  
|**index_size**|**VARCHAR (18)**|Cantidad total de espacio utilizado por los índices en *objName*.|  
|**sin usar**|**VARCHAR (18)**|Cantidad total de espacio reservado para *objName* pero que todavía no se ha usado.|  
 
Este es el modo predeterminado cuando no se especifica ningún parámetro. Se devuelven los siguientes conjuntos de resultados que detallan la información de tamaño de base de datos en disco. 

|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**database_name**|**nvarchar(128)**|Nombre de la base de datos actual.|  
|**database_size**|**VARCHAR (18)**|Tamaño de la base de datos actual en megabytes. **database_size** incluye archivos de datos y de registro. Si la base de datos tiene un grupo de archivos MEMORY_OPTIMIZED_DATA, esto incluye el tamaño total en disco de todos los archivos de punto de comprobación del grupo de archivos.|  
|**espacio sin asignar**|**VARCHAR (18)**|Espacio de la base de datos que no se ha reservado para objetos de base de datos. Si la base de datos tiene un grupo de archivos MEMORY_OPTIMIZED_DATA, esto incluye el tamaño total en disco de los archivos de punto de comprobación con el estado creado en el grupo de archivos.|  

Espacio usado por las tablas en la base de datos: (este conjunto de resultados no refleja las tablas optimizadas para memoria, ya que no hay ninguna contabilidad por tabla del uso de disco) 

|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**sector**|**VARCHAR (18)**|Espacio total asignado por los objetos de la base de datos.|  
|**data**|**VARCHAR (18)**|Cantidad total de espacio utilizado por los datos.|  
|**index_size**|**VARCHAR (18)**|Cantidad total de espacio utilizado por índices.|  
|**sin usar**|**VARCHAR (18)**|Espacio total reservado para los objetos de la base de datos, pero no utilizado todavía.|

Solo se devuelve el siguiente conjunto de resultados **si** la base de datos tiene un grupo de archivos MEMORY_OPTIMIZED_DATA con al menos un contenedor: 

|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**xtp_precreated**|**VARCHAR (18)**|Tamaño total de los archivos de punto de comprobación con estado precreated, en KB. Cuenta el espacio sin asignar en la base de datos en conjunto. [Por ejemplo, si hay 600.000 KB de archivos de punto de comprobación creados de forma precreada, esta columna contiene ' 600000 KB ']|  
|**xtp_used**|**VARCHAR (18)**|Tamaño total de los archivos de punto de comprobación con Estados en construcción, activo y de combinación destino, en KB. Este es el espacio en disco que se usa activamente para los datos de las tablas optimizadas para memoria.|  
|**xtp_pending_truncation**|**VARCHAR (18)**|Tamaño total de los archivos de punto de comprobación con WAITING_FOR_LOG_TRUNCATION de estado, en KB. Es el espacio en disco usado para los archivos de punto de comprobación que esperan la limpieza, cuando se produce el truncamiento del registro.|

Si se omite *objName* , el valor de oneresultset es 1 y *include_total_xtp_storage* es 1, se devuelve el siguiente conjunto de resultados único para proporcionar la información de tamaño de la base de datos actual. Si `include_total_xtp_storage` es 0 (valor predeterminado), se omiten las últimas tres columnas. 

|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**database_name**|**nvarchar(128)**|Nombre de la base de datos actual.|  
|**database_size**|**VARCHAR (18)**|Tamaño de la base de datos actual en megabytes. **database_size** incluye archivos de datos y de registro. Si la base de datos tiene un grupo de archivos MEMORY_OPTIMIZED_DATA, esto incluye el tamaño total en disco de todos los archivos de punto de comprobación del grupo de archivos.|
|**espacio sin asignar**|**VARCHAR (18)**|Espacio de la base de datos que no se ha reservado para objetos de base de datos. Si la base de datos tiene un grupo de archivos MEMORY_OPTIMIZED_DATA, esto incluye el tamaño total en disco de los archivos de punto de comprobación con el estado creado en el grupo de archivos.|  
|**sector**|**VARCHAR (18)**|Espacio total asignado por los objetos de la base de datos.|  
|**data**|**VARCHAR (18)**|Cantidad total de espacio utilizado por los datos.|  
|**index_size**|**VARCHAR (18)**|Cantidad total de espacio utilizado por índices.|  
|**sin usar**|**VARCHAR (18)**|Espacio total reservado para los objetos de la base de datos, pero no utilizado todavía.|
|**xtp_precreated**|**VARCHAR (18)**|Tamaño total de los archivos de punto de comprobación con estado precreated, en KB. Esto cuenta en el espacio sin asignar en la base de datos en conjunto. Devuelve NULL si la base de datos no tiene un grupo de archivos de memory_optimized_data con al menos un contenedor. *Esta columna solo se incluye si @include_total_xtp_storage = 1*.| 
|**xtp_used**|**VARCHAR (18)**|Tamaño total de los archivos de punto de comprobación con Estados en construcción, activo y de combinación destino, en KB. Este es el espacio en disco que se usa activamente para los datos de las tablas optimizadas para memoria. Devuelve NULL si la base de datos no tiene un grupo de archivos de memory_optimized_data con al menos un contenedor. *Esta columna solo se incluye si @include_total_xtp_storage = 1*.| 
|**xtp_pending_truncation**|**VARCHAR (18)**|Tamaño total de los archivos de punto de comprobación con WAITING_FOR_LOG_TRUNCATION de estado, en KB. Es el espacio en disco usado para los archivos de punto de comprobación que esperan la limpieza, cuando se produce el truncamiento del registro. Devuelve NULL si la base de datos no tiene un grupo de archivos de memory_optimized_data con al menos un contenedor. Esta columna solo se incluye si `@include_total_xtp_storage=1` .|

## <a name="remarks"></a>Observaciones  
 **database_size** suele ser mayor que la suma del espacio **reservado** sin  +  **asignar** porque incluye el tamaño de los archivos de registro, pero **reservado** y **unallocated_space** considerar solo las páginas de datos. En algunos casos con Azure Synapse Analytics, es posible que esta instrucción no sea verdadera. 
  
 Las páginas que se usan en los índices XML y los índices de texto completo se incluyen en **index_size** para ambos conjuntos de resultados. Cuando se especifica *objName* , las páginas de los índices XML y los índices de texto completo del objeto también se cuentan en el total de resultados **reservados** y **index_size** .  
  
 Si se calcula el uso de espacio para una base de datos o un objeto que tiene un índice espacial, las columnas de tamaño de espacio, como **database_size**, **reserved** y **index_size**, incluyen el tamaño del índice espacial.  
  
 Cuando se especifica *UPDATEUSAGE* , [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] examina las páginas de datos de la base de datos y realiza las correcciones necesarias en las vistas de catálogo **Sys.allocation_units** y **Sys. partitions** en relación con el espacio de almacenamiento utilizado por cada tabla. Existen algunas situaciones, como por ejemplo después de quitar un índice, en las que la información de espacio para la tabla podría no estar actualizada. *UPDATEUSAGE* puede tardar algún tiempo en ejecutarse en tablas o bases de datos de gran tamaño. Use *UPDATEUSAGE* solo cuando sospeche que se devuelven valores incorrectos y cuando el proceso no tendrá ningún efecto adverso en otros usuarios o procesos de la base de datos. Si se prefiere, DBCC UPDATEUSAGE puede ejecutarse por separado.  
  
> [!NOTE]  
>  Al quitar o volver a generar índices grandes, o al quitar o truncar tablas grandes, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] difiere las cancelaciones de asignación de páginas, así como sus bloqueos asociados, hasta que se confirma la transacción. Las operaciones de eliminación diferidas no liberan inmediatamente el espacio asignado. Por lo tanto, es posible que los valores devueltos por **sp_spaceused** inmediatamente después de quitar o truncar un objeto grande no reflejen el espacio en disco real disponible.  
  
## <a name="permissions"></a>Permisos  
 El permiso para ejecutar **sp_spaceused** se otorga al rol **public** . Solo los miembros del rol fijo de base de datos **db_owner** pueden especificar el parámetro **\@updateusage**.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-displaying-disk-space-information-about-a-table"></a>A. Mostrar información de espacio en disco acerca de una tabla  
 El siguiente ejemplo muestra información de espacio en disco para la tabla `Vendor` y sus índices.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_spaceused N'Purchasing.Vendor';  
GO  
```  
  
### <a name="b-displaying-updated-space-information-about-a-database"></a>B. Mostrar información de espacio actualizada acerca de una base de datos  
 En este ejemplo se resume el espacio utilizado en la base de datos actual y se utiliza el parámetro opcional `@updateusage` para garantizar que se devuelvan los valores actuales.  
  
```sql  
USE AdventureWorks008R2;  
GO  
EXEC sp_spaceused @updateusage = N'TRUE';  
GO  
```  
  
### <a name="c-displaying-space-usage-information-about-the-remote-table-associated-with-a-stretch-enabled-table"></a>C. Mostrar información de uso de espacio sobre la tabla remota asociada a una tabla habilitada para Stretch  
 En el ejemplo siguiente se resume el espacio utilizado por la tabla remota asociada a una tabla habilitada para Stretch mediante el argumento **\@ mode** para especificar el destino remoto. Para obtener más información, vea [Stretch Database](../../sql-server/stretch-database/stretch-database.md).  
  
```sql  
USE StretchedAdventureWorks2016  
GO  
EXEC sp_spaceused N'Purchasing.Vendor', @mode = 'REMOTE_ONLY'  
```  
  
### <a name="d-displaying-space-usage-information-for-a-database-in-a-single-result-set"></a>D. Mostrar información de uso de espacio para una base de datos en un único conjunto de resultados  
 En el ejemplo siguiente se resume el uso de espacio para la base de datos actual en un conjunto de resultados único.  
  
```sql  
USE AdventureWorks2016  
GO  
EXEC sp_spaceused @oneresultset = 1  
```  

### <a name="e-displaying-space-usage-information-for-a-database-with-at-least-one-memory_optimized-file-group-in-a-single-result-set"></a>E. Mostrar información de uso de espacio para una base de datos con al menos un grupo de archivos MEMORY_OPTIMIZED en un único conjunto de resultados 
 En el ejemplo siguiente se resume el uso de espacio para la base de datos actual con al menos un grupo de archivos MEMORY_OPTIMIZED en un único conjunto de resultados.
 
```sql
USE WideWorldImporters
GO
EXEC sp_spaceused @updateusage = 'FALSE', @mode = 'ALL', @oneresultset = '1', @include_total_xtp_storage = '1';
GO
``` 

### <a name="f-displaying-space-usage-information-for-a-memory_optimized-table-object-in-a-database"></a>F. Mostrar información de uso de espacio para un objeto de tabla de MEMORY_OPTIMIZED en una base de datos.
 En el ejemplo siguiente se resume el uso de espacio para un objeto de tabla de MEMORY_OPTIMIZED en la base de datos actual con al menos un grupo de archivos MEMORY_OPTIMIZED.
 
```sql
USE WideWorldImporters
GO
EXEC sp_spaceused
@objname = N'VehicleTemparatures',
@updateusage = 'FALSE',
@mode = 'ALL',
@oneresultset = '0',
@include_total_xtp_storage = '1';
GO
```  

## <a name="see-also"></a>Vea también  
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [DBCC UPDATEUSAGE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-updateusage-transact-sql.md)   
 [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)   
 [sys.allocation_units &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
