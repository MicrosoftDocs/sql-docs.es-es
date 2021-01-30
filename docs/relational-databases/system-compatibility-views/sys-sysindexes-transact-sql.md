---
description: sys.sysindexes (Transact-SQL)
title: Índices de sys.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysindexes
- sysindexes_TSQL
- sys.sysindexes
- sys.sysindexes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysindexes system table
- sys.sysindexes compatibility view
ms.assetid: f483d89c-35c4-4a08-8f8b-737fd80d13f5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: bda40e2ddc1858857bc6ae3f1616d05987c708c0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158533"
---
# <a name="syssysindexes-transact-sql"></a>sys.sysindexes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila por cada índice y tabla de la base de datos actual. Los índices XML no se admiten en esta vista. Las tablas e índices con particiones no son totalmente compatibles con esta vista; Use la vista de catálogo [Sys. Indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) en su lugar.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Id. de la tabla a la que pertenece el índice.|  
|**status**|**int**|Información de estado del sistema.<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**first**|**binario (6)**|Puntero a la primera página o página raíz.<br /><br /> Sin usar cuando **indid** = 0.<br /><br /> NULL = el índice tiene particiones cuando **indid** > 1.<br /><br /> NULL = la tabla tiene particiones cuando **indid** es 0 o 1.|  
|**indid**|**smallint**|Id. del índice:<br /><br /> 0 = Montón<br /><br /> 1 = Índice clúster<br /><br /> >1 = índice no clúster|  
|**root**|**binario (6)**|Para **indid** >= 1, la **raíz** es el puntero a la página raíz.<br /><br /> Sin usar cuando **indid** = 0.<br /><br /> NULL = el índice tiene particiones cuando **indid** > 1.<br /><br /> NULL = la tabla tiene particiones cuando **indid** es 0 o 1.|  
|**minlen**|**smallint**|Tamaño mínimo de una fila.|  
|**keycnt**|**smallint**|Número de claves.|  
|**GROUPID**|**smallint**|Id. del grupo de archivos en el que se creó el objeto.<br /><br /> NULL = el índice tiene particiones cuando **indid** > 1.<br /><br /> NULL = la tabla tiene particiones cuando **indid** es 0 o 1.|  
|**DPAGES**|**int**|Para **indid** = 0 o **indid** = 1, **DPAGES** es el número de páginas de datos que se usan.<br /><br /> En el caso de **indid** > 1, **DPAGES** es el número de páginas de índice usadas.<br /><br /> 0 = el índice tiene particiones cuando **indid** > 1.<br /><br /> 0 = la tabla tiene particiones cuando **indid** es 0 o 1.<br /><br /> No producen resultados precisos en caso de desbordamiento de fila.|  
|**sector**|**int**|Para **indid** = 0 o **indid** = 1, **Reserved** es el número de páginas asignadas para todos los índices y datos de tabla.<br /><br /> En el caso de **indid** > 1, **Reserved** es el número de páginas asignadas para el índice.<br /><br /> 0 = el índice tiene particiones cuando **indid** > 1.<br /><br /> 0 = la tabla tiene particiones cuando **indid** es 0 o 1.<br /><br /> No producen resultados precisos en caso de desbordamiento de fila.|  
|**usa**|**int**|Para **indid** = 0 o **indid** = 1, **utilizado** es el número total de páginas utilizadas para todos los datos de índice y de tabla.<br /><br /> En el caso de **indid** > 1, **utilizado** es el recuento de páginas utilizadas para el índice.<br /><br /> 0 = el índice tiene particiones cuando **indid** > 1.<br /><br /> 0 = la tabla tiene particiones cuando **indid** es 0 o 1.<br /><br /> No producen resultados precisos en caso de desbordamiento de fila.|  
|**rowcnt**|**bigint**|Recuento de filas de nivel de datos basado en **indid** = 0 e **indid** = 1.<br /><br /> 0 = el índice tiene particiones cuando **indid** > 1.<br /><br /> 0 = la tabla tiene particiones cuando **indid** es 0 o 1.|  
|**rowmodctr**|**int**|Cuenta el número total de filas insertadas, eliminadas o actualizadas desde la última vez que se actualizaron las estadísticas de la tabla.<br /><br /> 0 = el índice tiene particiones cuando **indid** > 1.<br /><br /> 0 = la tabla tiene particiones cuando **indid** es 0 o 1.<br /><br /> En [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] y versiones posteriores, **rowmodctr** no es totalmente compatible con las versiones anteriores. Para obtener más información, vea la sección Comentarios.|  
|**reserved3**|**int**|Devuelve 0.<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**reserved4**|**int**|Devuelve 0.<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**xmaxlen**|**smallint**|Tamaño máximo de una fila|  
|**maxirow**|**smallint**|Tamaño máximo de una fila de índice no hoja.<br /><br /> En [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] y versiones posteriores, **maxirow** no es totalmente compatible con las versiones anteriores.|  
|**OrigFillFactor**|**tinyint**|Valor de factor de relleno original empleado cuando se creó el índice. Este valor no se mantiene, aunque puede ser útil si necesita volver a crear un índice y no recuerda el valor del factor de relleno que se utilizó originalmente.|  
|**StatVersion**|**tinyint**|Devuelve 0.<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**reserved2**|**int**|Devuelve 0.<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**FirstIAM**|**binario (6)**|NULL = Índice con particiones.<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**impid**|**smallint**|Marca de implementación de índice.<br /><br /> Devuelve 0.<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**lockflags**|**smallint**|Se utiliza para restringir las granularidades de bloqueo que se tienen en cuenta para un índice. Por ejemplo, una tabla de búsqueda que es esencialmente de solo lectura se puede configurar de modo que solo imponga bloqueos en la tabla para minimizar el costo de bloqueo.|  
|**pgmodctr**|**int**|Devuelve 0.<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**mykeys**|**varbinary (816)**|Lista de los Id. de columna para las columnas que forman la clave de índice.<br /><br /> Devuelve NULL.<br /><br /> Para mostrar las columnas de clave de índice, use [sys.sysindexkeys](../../relational-databases/system-compatibility-views/sys-sysindexkeys-transact-sql.md).|  
|**name**|**sysname**|Nombre del índice o estadística. Devuelve NULL cuando **indid** = 0. Modifique la aplicación para que busque un nombre de montón NULL.|  
|**statblob**|**image**|Objeto binario grande de estadística (BLOB).<br /><br /> Devuelve NULL.|  
|**maxlen**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**rows**|**int**|Recuento de filas de nivel de datos basado en **indid** = 0 e **indid** = 1, y el valor se repite para **indid** >1.|  
  
## <a name="remarks"></a>Observaciones  
 Las columnas definidas como reservadas no deben utilizarse.  
  
 Las columnas **DPAGES**, **Reserved** y **Used** no devolverán resultados precisos si la tabla o el índice contienen datos en la unidad de asignación ROW_OVERFLOW. Además, se realiza el seguimiento para el total de páginas de cada índice por separado y no se agregan para la tabla base. Para ver los recuentos de páginas, use las vistas de catálogo [Sys.allocation_units](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md) o [Sys. partitions](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md) , o la vista de administración dinámica [Sys.dm_db_partition_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md) .  
  
 En SQL Server 2000 y versiones anteriores, [!INCLUDE[ssDE](../../includes/ssde-md.md)] mantenía contadores de modificaciones de filas. Este tipo de contadores se mantienen ahora en el nivel de columna. Por lo tanto, la columna **rowmodctr** se calcula y genera resultados similares a los resultados de versiones anteriores, pero no son exactos.  
  
 Si usa el valor de **rowmodctr** para determinar cuándo actualizar las estadísticas, tenga en cuenta las siguientes soluciones:  
  
-   No haga nada. Con frecuencia, el nuevo valor de **rowmodctr** le ayudará a determinar cuándo actualizar las estadísticas, ya que el comportamiento es razonablemente similar a los resultados de las versiones anteriores.  
  
-   Utilice AUTO_UPDATE_STATISTICS. Para obtener más información, vea [Statistics](../../relational-databases/statistics/statistics.md).  
  
-   Utilice un límite de tiempo para determinar cuándo debe actualizar las estadísticas. Por ejemplo, cada hora, cada día o cada semana.  
  
-   Utilice la información de nivel de aplicación para determinar cuándo debe actualizar las estadísticas. Por ejemplo, cada vez que el valor máximo de una columna de **identidad** cambia en más de 10.000, o cada vez que se realiza una operación de inserción masiva.  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Asignar tablas del sistema a vistas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)  
  
  
