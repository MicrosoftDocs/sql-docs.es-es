---
title: sys.dm_exec_query_optimizer_memory_gateways (Transact-SQL)
description: Devuelve el estado actual de los semáforos de recursos usados para limitar la optimización de consultas simultáneas.
ms.custom: seo-dt-2019
ms.date: 04/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: reference
f1_keywords:
- dm_exec_query_optimizer_memory_gateways_TSQL
- dm_exec_query_optimizer_memory_gateways
- sys.dm_exec_query_optimizer_memory_gateways_TSQL
- sys.dm_exec_query_optimizer_memory_gateways
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_optimizer_memory_gateways dynamic management view
author: josack
ms.author: josack
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 66e5d3e4509b8593a41a53348e466054ee01b4f4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342912"
---
# <a name="sysdm_exec_query_optimizer_memory_gateways-transact-sql"></a>sys.dm_exec_query_optimizer_memory_gateways (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Devuelve el estado actual de los semáforos de recursos usados para limitar la optimización de consultas simultáneas.

|Columna|Tipo|Descripción|  
|----------|---------------|-----------------|  
|**pool_id**|**int**|IDENTIFICADOR del grupo de recursos en Resource Governor|  
|**name**|**sysname**|Nombre de la puerta de compilación (puerta de enlace pequeña, puerta de enlace mediana, Big Gateway)|
|**max_count**|**int**|El recuento máximo configurado de compilaciones simultáneas|
|**active_count**|**int**|El recuento activo actualmente de compilaciones en esta puerta|
|**waiter_count**|**int**|Número de esperas en esta puerta|
|**threshold_factor**|**bigint**|Factor de umbral que define la parte de memoria máxima utilizada por la optimización de consultas.  En el caso de la puerta de enlace pequeña, threshold_factor indica el uso máximo de memoria del optimizador en bytes para una consulta antes de que sea necesaria para obtener acceso a la puerta de enlace pequeña.  En la puerta de enlace mediana y grande, threshold_factor muestra la parte de la memoria total del servidor disponible para esta puerta. Se usa como divisor al calcular el umbral de uso de memoria para la puerta.|
|**threshold**|**bigint**|Siguiente umbral de memoria en bytes.  La consulta es necesaria para obtener acceso a esta puerta de enlace si el consumo de memoria alcanza este umbral.  "-1" si la consulta no es necesaria para obtener acceso a esta puerta de enlace.|
|**is_active**|**bit**|Indica si la consulta debe pasar o no la puerta actual.|


## <a name="permissions"></a>Permisos  
SQL Server requiere el permiso VIEW SERVER STATE en el servidor.

Azure SQL Database requiere el permiso VIEW DATABASE STATE en la base de datos.


## <a name="remarks"></a>Comentarios  
SQL Server usa un enfoque de puerta de enlace en capas para limitar el número de compilaciones simultáneas permitidas.  Se usan tres puertas de enlace, como pequeño, mediano y grande. Las puertas de enlace ayudan a evitar el agotamiento de los recursos de memoria generales por la memoria de compilación más grande, que requiere consumidores.

Espera en una puerta de enlace como resultado de la compilación diferida. Además de los retrasos en la compilación, las solicitudes limitadas tendrán un asociado RESOURCE_SEMAPHORE_QUERY_COMPILE la acumulación del tipo de espera. El tipo de espera de RESOURCE_SEMAPHORE_QUERY_COMPILE puede indicar que las consultas usan una gran cantidad de memoria para la compilación y que se ha agotado la memoria, o que, como alternativa, hay suficiente memoria disponible en general, pero se han agotado las unidades disponibles en una puerta de enlace específica. La salida de **Sys.dm_exec_query_optimizer_memory_gateways** se puede usar para solucionar problemas de escenarios en los que no había memoria suficiente para compilar un plan de ejecución de consulta.  

## <a name="examples"></a>Ejemplos  

### <a name="a-viewing-statistics-on-resource-semaphores"></a>A. Ver estadísticas en semáforos de recursos  
¿Cuáles son las estadísticas actuales de la puerta de enlace de memoria del optimizador para esta instancia de SQL Server?

```  
SELECT [pool_id], [name], [max_count], [active_count],
       [waiter_count], [threshold_factor], [threshold],
       [is_active]
FROM sys.dm_exec_query_optimizer_memory_gateways;   

```  

## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](./system-dynamic-management-views.md)   
 [Funciones y vistas de administración dinámica relacionadas con ejecuciones &#40;Transact-SQL&#41;](./execution-related-dynamic-management-views-and-functions-transact-sql.md)  
[Cómo usar el comando DBCC MEMORYSTATUS para supervisar el uso de memoria en SQL Server 2005](https://support.microsoft.com/help/907877/how-to-use-the-dbcc-memorystatus-command-to-monitor-memory-usage-on-sql-server-2005) 
 La [compilación de consultas de gran tamaño espera RESOURCE_SEMAPHORE_QUERY_COMPILE en SQL Server 2014](https://support.microsoft.com/help/3024815/large-query-compilation-waits-on-resource-semaphore-query-compile-in-sql-server-2014)
