---
description: sys.resource_stats (Azure SQL Database)
title: sys.resource_stats (Azure SQL Database) | Microsoft Docs
ms.custom: ''
ms.date: 09/13/2018
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- resource_stats
- sys.resource_stats
- sys.resource_stats_TSQL
- resource_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.resource_stats
- resource_stats
ms.assetid: 02379a1b-3622-4578-8c59-a1b8f1a17914
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current
ms.openlocfilehash: 14785888953906f42b2196dc2cb06ca35cfc3a8b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210023"
---
# <a name="sysresource_stats-azure-sql-database"></a>sys.resource_stats (Azure SQL Database)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Devuelve datos de almacenamiento y uso de CPU para una base de datos de Azure SQL. Los datos se recopilan y se agregan en intervalos de cinco minutos. Para cada base de datos de usuario, hay una fila por cada ventana de informes de cinco minutos en la que hay un cambio en el consumo de recursos. Los datos devueltos incluyen el uso de CPU, el cambio de tamaño de almacenamiento y la modificación de la SKU de base de datos. Las bases de datos inactivas sin cambios no pueden tener filas por cada intervalo de cinco minutos. Los datos históricos se conservan durante 14 días aproximadamente.  
  
 La vista **Sys.resource_stats** tiene definiciones diferentes en función de la versión del servidor de Azure SQL Database al que está asociada la base de datos. Tenga en cuenta estas diferencias y cualquier modificación que requiera la aplicación al actualizar a una nueva versión de servidor.  
  
 En la tabla siguiente se describen las columnas disponibles en un servidor v12:  
  
|Columnas|Tipo de datos|Descripción|  
|----------------------------|---------------|-----------------|  
|start_time|**datetime**|Hora UTC que indica el inicio del intervalo de informes de cinco minutos.|  
|end_time|**datetime**|Hora UTC que indica el final del intervalo de informes de cinco minutos.|  
|database_name|**nvarchar(128)**|Nombre de la base de datos del usuario.|  
|sku|**nvarchar(128)**|Nivel de servicio de la base de datos. Los posibles valores son los siguientes:<br /><br /> Básico<br /><br /> Estándar<br /><br /> Premium<br /><br />De uso general<br /><br />Crítico para la empresa|  
|storage_in_megabytes|**float**|Tamaño de almacenamiento máximo en megabytes para el período de tiempo, incluidos los datos de base de datos, los índices, los procedimientos almacenados y los metadatos.|  
|avg_cpu_percent|**decimal (5, 2)**|Uso de proceso promedio como porcentaje del límite del nivel de servicio.|  
|avg_data_io_percent|**decimal (5, 2)**|Uso de E/S promedio como porcentaje según el límite del nivel de servicio. Para las bases de datos de hiperescala, consulte [e/s de datos en estadísticas de uso de recursos](/azure/sql-database/sql-database-hyperscale-performance-diagnostics#data-io-in-resource-utilization-statistics).|  
|avg_log_write_percent|**decimal (5, 2)**|Uso de recursos de escritura promedio como porcentaje del límite del nivel de servicio.|  
|max_worker_percent|**decimal (5, 2)**|Número máximo de trabajos simultáneos (solicitudes) en porcentaje según el límite del nivel de servicio de la base de datos.<br /><br /> El máximo se calcula actualmente para el intervalo de cinco minutos basado en las muestras de 15 segundos de los recuentos de trabajo simultáneos.|  
|max_session_percent|**decimal (5, 2)**|Número máximo de sesiones simultáneas en porcentaje según el límite del nivel de servicio de la base de datos.<br /><br /> El valor máximo se calcula actualmente para el intervalo de cinco minutos basado en las muestras de 15 segundos de los recuentos de sesiones simultáneas.|  
|dtu_limit|**int**|Valor actual máximo de DTU de base de datos para esta base de datos durante este intervalo. |
|xtp_storage_percent|**decimal (5, 2)**|Uso del almacenamiento para In-Memory OLTP en porcentaje del límite del nivel de servicio (al final del intervalo de generación de informes). Esto incluye la memoria usada para el almacenamiento de los siguientes In-Memory objetos OLTP: tablas optimizadas para memoria, índices y variables de tabla. También incluye la memoria usada para procesar las operaciones de ALTER TABLE.<br /><br /> Devuelve 0 si no se utiliza In-Memory OLTP en la base de datos.|
|avg_login_rate_percent|**decimal (5, 2)**|Solamente se identifica con fines informativos. No se admite. La compatibilidad con versiones posteriores no está garantizada.|
|avg_instance_cpu_percent|**decimal (5, 2)**|Uso promedio de la CPU de la base de datos como porcentaje del proceso de SQL Database.|
|avg_instance_memory_percent|**decimal (5, 2)**|Promedio de uso de memoria de base de datos como porcentaje del proceso de SQL Database.|
|cpu_limit|**decimal (5, 2)**|Número de núcleos virtuales para esta base de datos durante este intervalo. En el caso de las bases de datos que usan el modelo basado en DTU, esta columna es NULL.|
|allocated_storage_in_megabytes|**float**|La cantidad de espacio de archivo con formato en MB disponible para almacenar los datos de la base de datos. El espacio de archivo con formato también se conoce como espacio de datos asignado.  Para obtener más información, vea [Administración del espacio de archivo en SQL Database](/azure/sql-database/sql-database-file-space-management)|
  
> [!TIP]  
>  Para más información sobre estos límites y los niveles de servicio, consulte los temas sobre los [niveles de servicio](/azure/azure-sql/database/purchasing-models).  
    
## <a name="permissions"></a>Permisos  
 Esta vista está disponible para todos los roles de usuario con permisos para conectarse a la base de datos **maestra** virtual.  
  
## <a name="remarks"></a>Observaciones  
 Los datos devueltos por **Sys.resource_stats** se expresan como un porcentaje de los límites máximos permitidos para el nivel de servicio/nivel de rendimiento que se está ejecutando.  
  
 Cuando una base de datos es miembro de un grupo elástico, las estadísticas de recursos que se presentan como valores porcentuales se expresan como el porcentaje del límite máximo de las bases de datos establecidas en la configuración del grupo elástico.  
  
 Para obtener una vista más detallada de estos datos, use **Sys.dm_db_resource_stats** vista de administración dinámica en una base de datos de usuario. Esta vista captura datos cada 15 segundos u mantiene datos históricos durante 1 hora.  Para obtener más información, vea [sys.dm_db_resource_stats &#40;Azure SQL Database&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database.md).  

## <a name="examples"></a>Ejemplos  
 El ejemplo siguiente devuelve todas las bases de datos cuyo promedio de uso de proceso fue al menos del 80 % durante la semana pasada.  
  
```sql  
DECLARE @s datetime;  
DECLARE @e datetime;  
SET @s= DateAdd(d,-7,GetUTCDate());  
SET @e= GETUTCDATE();  
SELECT database_name, AVG(avg_cpu_percent) AS Average_Compute_Utilization   
FROM sys.resource_stats   
WHERE start_time BETWEEN @s AND @e  
GROUP BY database_name  
HAVING AVG(avg_cpu_percent) >= 80  
```  
    
## <a name="see-also"></a>Consulte también  
 [Niveles de servicio](/azure/azure-sql/database/purchasing-models)   
 [Límites y capacidades de nivel de servicio](/azure/azure-sql/database/performance-guidance)  
  
