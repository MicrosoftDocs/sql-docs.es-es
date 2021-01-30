---
description: sys.dm_continuous_copy_status (Azure SQL Database)
title: sys.dm_continuous_copy_status
titleSuffix: Azure SQL Database
ms.date: 03/03/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sys.dm_continuous_copy_status_TSQL
- dm_continuous_copy_status_TSQL
- dm_continuous_copy_status
- sys.dm_continuous_copy_status
dev_langs:
- TSQL
helpviewer_keywords:
- dm_continuous_copy_status
- sys.dm_continuous_copy_status
ms.assetid: 411b2e71-4421-4ef5-900d-5af068750899
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.custom: seo-dt-2019
ms.openlocfilehash: 7e0dc3ec69f4c35b1361cf12f501f4b23abf29c7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207796"
---
# <a name="sysdm_continuous_copy_status-azure-sql-database"></a>sys.dm_continuous_copy_status (Azure SQL Database)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Devuelve una fila para cada base de datos de usuario (V11) que está ocupada actualmente en una relación de copia continua de replicación geográfica. Si se inicia más de una relación de copia continua para una base de datos principal dada, esta tabla contiene una fila por cada base de datos secundaria activa.  
  
Si usa SQL Database V12, debe usar [Sys.dm_geo_replication_link_status](../../relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database.md) (porque *Sys.dm_continuous_copy_status* solo se aplica a V11).

  
|Nombre de columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**copy_guid**|**uniqueidentifier**|Identificador único de la base de datos de réplica.|  
|**partner_server**|**sysname**|Nombre del servidor de SQL Database vinculado.|  
|**partner_database**|**sysname**|Nombre de la base de datos vinculada en la que reside el servidor de SQL Database vinculado.|  
|**last_replication**|**datetimeoffset**|La marca de tiempo de la última transacción replicada aplicada.|  
|**replication_lag_sec**|**int**|Diferencia de tiempo en segundos entre la hora actual y la marca de tiempo de la última transacción confirmada correctamente en la base de datos principal que la base de datos secundaria activa no ha confirmado.|  
|**replication_state**|**tinyint**|El estado de replicación de copia continua para esta base de datos. A continuación se muestran los valores posibles y sus descripciones.<br /><br /> 1: propagación. El destino de replicación se está inicializando y está en un estado transaccionalmente incoherente. Hasta que la inicialización se completa, no puede conectarse a la base de datos secundaria activa. <br />2: ponerse al día. La base de datos secundaria activa se está poniendo al día con la base de datos principal y está en un estado transaccionalmente coherente.<br />3: reinicialización. La base de datos secundaria activa se está reinicializando automáticamente debido a un error de replicación irrecuperable.<br />4: suspendido. No es una relación activa de copia continua. Este estado suele indicar que el ancho de banda disponible para el interlink es insuficiente para el nivel de actividad de transacción en la base de datos principal. Sin embargo, la relación de copia continua sigue intacta.|  
|**replication_state_desc**|**nvarchar(256)**|Descripción del estado de replication_state, uno de los siguientes:<br /><br /> SEEDING<br /><br /> CATCH_UP<br /><br /> RE_SEEDING<br /><br /> SUSPENDED|  
|**is_rpo_limit_reached**|**bit**|Siempre se establece en 0|  
|**is_target_role**|**bit**|0 = Origen de la relación de copia<br /><br /> 1 = Destino de la relación de copia|  
|**is_interlink_connected**|**bit**|1 = Interlink está conectado.<br /><br /> 0 = Interlink está desconectado.|  
  
## <a name="permissions"></a>Permisos  
 Para recuperar datos, es necesario pertenecer al rol de base de datos **db_owner** . El usuario DBO, los miembros del rol de base de datos **dbmanager** y el inicio de sesión SA también pueden consultar esta vista.  
  
## <a name="remarks"></a>Observaciones  
 La vista de **Sys.dm_continuous_copy_status** se crea en la base de datos de **recursos** y está visible en todas las bases de datos, incluida la maestra lógica. Sin embargo, al consultar esta vista en la maestra lógica se devuelve un conjunto vacío.  
  
 Si se termina la relación de copia continua en una base de datos, la fila de esa base de datos en la vista **Sys.dm_continuous_copy_status** desaparece.  
  
 Al igual que la vista **Sys.dm_database_copies** , **Sys.dm_continuous_copy_status** refleja el estado de la relación de copia continua en la que la base de datos es una base de datos principal o secundaria activa. A diferencia de **Sys.dm_database_copies**, **Sys.dm_continuous_copy_status** contiene varias columnas que proporcionan detalles sobre las operaciones y el rendimiento. Estas columnas incluyen **last_replication** y **replication_lag_sec**.  
  
## <a name="see-also"></a>Consulte también  
 [sys.dm_database_copies &#40;Azure SQL Database&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-database-copies-azure-sql-database.md)   
 [Procedimientos almacenados de Geo-Replication activos &#40;Transact-SQL&#41;](../system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
