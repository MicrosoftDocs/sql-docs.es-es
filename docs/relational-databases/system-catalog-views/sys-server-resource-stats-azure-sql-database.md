---
description: sys.server_resource_stats (Azure SQL Database)
title: sys.server_resource_stats (Azure SQL Database) | Microsoft Docs
ms.custom: ''
ms.date: 06/28/2018
ms.service: sql-database
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
ms.assetid: ''
author: jovanpop-msft
ms.author: jovanpop
monikerRange: =azuresqldb-current
ms.openlocfilehash: e1fe6592c4962499d5f02f1f076f49eb05402d54
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206827"
---
# <a name="sysserver_resource_stats-azure-sql-database"></a>sys.server_resource_stats (Azure SQL Database)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

Devuelve los datos de uso de CPU, e/s y almacenamiento de Azure SQL Instancia administrada. Los datos se recopilan y se agregan en intervalos de cinco minutos. Hay una fila por cada 15 segundos de informes. Los datos devueltos incluyen el uso de CPU, el tamaño de almacenamiento, el uso de e/s y la SKU. Los datos históricos se conservan durante 14 días aproximadamente.

La vista **Sys.server_resource_stats** tiene definiciones diferentes en función de la versión de Azure SQL instancia administrada con la que está asociada la base de datos. Tenga en cuenta estas diferencias y cualquier modificación que requiera la aplicación al actualizar a una nueva versión de servidor.
 
  
 En la tabla siguiente se describen las columnas disponibles en un servidor v12:  
  
|Columnas|Tipo de datos|Descripción|  
|----------------------------|---------------|-----------------|  
|start_time|**datetime2**|Hora UTC que indica el inicio del intervalo de informes de quince segundos|  
|end_time|**datetime**|Hora UTC que indica el final del intervalo de informes de quince segundos|
|resource_type|Nvarchar(128)|Tipo del recurso para el que se proporcionan las métricas|
|resource_name|nvarchar(128)|Nombre del recurso.|
|sku|nvarchar(128)|Instancia administrada nivel de servicio de la instancia. Los posibles valores son los siguientes: <br><ul><li>De uso general</li></ul><ul><li>Crítico para la empresa</li></ul>|
|hardware_generation|nvarchar(128)|Identificador de generación de hardware: como gen 4 o gen 5|
|virtual_core_count|int|Representa el número de núcleos virtuales por instancia (8, 16 o 24 en versión preliminar pública)|
|avg_cpu_percent|decimal (5, 2)|Uso de proceso promedio en porcentaje del límite del nivel de servicio Instancia administrada utilizado por la instancia de. Se calcula como suma del tiempo de CPU de todos los grupos de recursos para todas las bases de datos de la instancia y dividido por el tiempo de CPU disponible para ese nivel en el intervalo especificado.|
|reserved_storage_mb|bigint|Almacenamiento reservado por instancia (cantidad de espacio de almacenamiento que el cliente compró para la instancia administrada)|
|storage_space_used_mb|decimal (18, 2)|Almacenamiento utilizado por todos los archivos de base de datos en una instancia administrada (incluidas las bases de datos de usuario y del sistema)|
|io_request|bigint|Número total de operaciones físicas de e/s dentro del intervalo|
|io_bytes_read|bigint|Número de bytes físicos leídos dentro del intervalo|
|io_bytes_written|bigint|Número de bytes físicos escritos en el intervalo|

 
> [!TIP]  
>  Para más información sobre estos límites y los niveles de servicio, consulte los temas [instancia administrada niveles de servicio](/azure/sql-database/sql-database-managed-instance#managed-instance-service-tiers).  
    
## <a name="permissions"></a>Permisos  
 Esta vista está disponible para todos los roles de usuario con permisos para conectarse a la base de datos **maestra** .  
  
## <a name="remarks"></a>Observaciones  
 Los datos devueltos por **Sys.server_resource_stats** se expresan como el total que se usa en bytes o megabytes (indicados en nombres de columna) distintos de avg_cpu, que se expresa como un porcentaje de los límites máximos permitidos para el nivel de servicio/nivel de rendimiento que se está ejecutando.  
 
## <a name="examples"></a>Ejemplos  
En el ejemplo siguiente se devuelve el uso promedio de la CPU en los últimos siete días.  
  
```sql  
DECLARE @s datetime;  
DECLARE @e datetime;  
SET @s= DateAdd(d,-7,GetUTCDate());  
SET @e= GETUTCDATE();  
SELECT AVG(avg_cpu_percent) AS Average_Compute_Utilization   
FROM sys.server_resource_stats   
WHERE start_time BETWEEN @s AND @e  
GO;
```  
    
## <a name="see-also"></a>Consulte también  
 [Instancia administrada niveles de servicio](/azure/sql-database/sql-database-managed-instance#managed-instance-service-tiers)
