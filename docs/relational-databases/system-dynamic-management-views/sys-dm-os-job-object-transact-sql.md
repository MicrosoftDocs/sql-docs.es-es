---
description: sys.dm_os_job_object (Azure SQL Database)
title: sys.dm_os_job_object (Azure SQL Database) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2020
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sys.dm_db_resource_stats
- sys.dm_db_resource_stats_TSQL
- dm_db_resource_stats
- dm_db_resource_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_resource_stats
- dm_db_resource_stats
ms.assetid: 6e76b39f-236e-4bbf-b0b5-38be190d81e8
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current
ms.openlocfilehash: 5115f543953d549769f1894530baba1f41eac42a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190159"
---
# <a name="sysdm_os_job_object-azure-sql-database"></a>sys.dm_os_job_object (Azure SQL Database)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

Devuelve una sola fila que describe la configuración del objeto de trabajo que administra el proceso de SQL Server, así como ciertas estadísticas de consumo de recursos en el nivel de objeto de trabajo. Devuelve un conjunto vacío si SQL Server no se está ejecutando en un objeto de trabajo.

Un objeto de trabajo es una construcción de Windows que implementa la CPU, la memoria y el gobierno de recursos de e/s en el nivel del sistema operativo. Para obtener más información sobre los objetos de trabajo, vea [objetos de trabajo](/windows/desktop/ProcThread/job-objects).
  
|Columnas|Tipo de datos|Descripción|  
|-------------|---------------|-----------------|  
|cpu_rate|**int**|Especifica la parte de los ciclos del procesador que SQL Server subprocesos pueden utilizar durante cada intervalo de programación. El valor se muestra como un porcentaje de los ciclos disponibles dentro de un intervalo de programación del ciclo 10000, multiplicado por el número de CPU lógicas. Por ejemplo, el valor 800 en una instancia de SQL Server con 8 CPU lógicas significa que los subprocesos pueden usar CPU tienen toda su capacidad.|
|cpu_affinity_mask|**bigint**|Máscara de bits que describe qué procesadores lógicos puede utilizar el SQL Server proceso en el grupo de procesadores. Por ejemplo, cpu_affinity_mask 255 (1111 1111 en binario) significa que se pueden usar los primeros ocho procesadores lógicos. <br /><br />Esta columna se proporciona por compatibilidad con versiones anteriores. No notifica el grupo de procesadores y el valor notificado puede ser incorrecto cuando un grupo de procesadores contiene más de 64 procesadores lógicos. Use la `process_physical_affinity` columna para determinar la afinidad del procesador en su lugar.|
|cpu_affinity_group|**int**|El número del grupo de procesadores que usa SQL Server.|
|memory_limit_mb|**bigint**|Cantidad máxima de memoria asignada, en MB, que todos los procesos del objeto de trabajo, incluido SQL Server, pueden usar de manera acumulativa.| 
|process_memory_limit_mb |**bigint**|La cantidad máxima de memoria asignada, en MB, que un único proceso en el objeto de trabajo, como SQL Server, puede usar.|
|workingset_limit_mb |**bigint**|Cantidad máxima de memoria, en MB, que puede usar el espacio de trabajo de SQL Server.|
|non_sos_mem_gap_mb|**bigint**|Cantidad de memoria, en MB, reservada para pilas de subprocesos, archivos dll y otras asignaciones de memoria no SOS. La memoria de destino de SOS es la diferencia entre `process_memory_limit_mb` y `non_sos_mem_gap_mb` .| 
|low_mem_signal_threshold_mb|**bigint**|Umbral de memoria, en MB. Cuando la cantidad de memoria disponible para el objeto de trabajo está por debajo de este umbral, se envía una señal de notificación de memoria baja al proceso de SQL Server. |
|total_user_time|**bigint**|El número total de 100 pasos NS que los subprocesos del objeto de trabajo han pasado en modo de usuario, desde que se creó el objeto de trabajo. |
|total_kernel_time |**bigint**|El número total de 100 pasos NS que los subprocesos del objeto de trabajo han pasado en modo kernel, desde que se creó el objeto de trabajo. |
|write_operation_count |**bigint**|El número total de operaciones de e/s de escritura en los discos locales emitidos por SQL Server desde que se creó el objeto de trabajo. |
|read_operation_count |**bigint**|El número total de operaciones de e/s de lectura en discos locales emitidos por SQL Server desde que se creó el objeto de trabajo. |
|peak_process_memory_used_mb|**bigint**|Cantidad máxima de memoria, en MB, que un único proceso en el objeto de trabajo, como SQL Server, ha usado desde que se creó el objeto de trabajo.| 
|peak_job_memory_used_mb|**bigint**|Cantidad máxima de memoria, en MB, que todos los procesos del objeto de trabajo han utilizado de manera acumulativa desde que se creó el objeto de trabajo.|
|process_physical_affinity|**nvarchar (a.**|Máscaras de bits que describen qué procesadores lógicos puede utilizar el SQL Server proceso en cada grupo de procesadores. El valor de esta columna está formado por uno o varios pares de valores, cada uno entre llaves. En cada par, el primer valor es el número de grupo del procesador y el segundo valor es la máscara de bits de afinidad para ese grupo de procesadores. Por ejemplo, el valor `{{0,a}{1,2}}` significa que la máscara de afinidad para el grupo de procesadores `0` es `a` ( `1010` en binario, lo que indica que se usan los procesadores 2 y 4) y la máscara de afinidad para el grupo de procesadores `1` es `2` ( `10` en binario, lo que indica que se usa el procesador 2).|
  
## <a name="permissions"></a>Permisos  
En SQL Instancia administrada, requiere el `VIEW SERVER STATE` permiso. En SQL Database, se requiere el permiso `VIEW DATABASE STATE` en la base de datos.  
 
## <a name="see-also"></a>Consulte también  

Para obtener información acerca de las instancias administradas, vea [SQL instancia administrada](/azure/sql-database/sql-database-managed-instance).
