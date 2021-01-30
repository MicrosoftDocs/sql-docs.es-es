---
description: sys.dm_database_copies (Azure SQL Database)
title: sys.dm_database_copies (Azure SQL Database) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- dm_database_copies_TSQL
- sys.dm_database_copies
- dm_database_copies
- sys.dm_database_copies_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dm_database_copies
- sys.dm_database_copies
ms.assetid: d03d4657-86d1-4496-97e6-cc3bc292e0b1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.openlocfilehash: a9a6e570b6c535620152f49947b4d4e289a6ff2a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195098"
---
# <a name="sysdm_database_copies-azure-sql-database"></a>sys.dm_database_copies (Azure SQL Database)
[!INCLUDE[Azure SQL Database](../../includes/applies-to-version/asdb.md)]

  Devuelve información acerca de la copia de la base de datos.  
  
Para devolver información acerca de los vínculos de replicación geográfica, use las vistas [Sys.geo_replication_links](../../relational-databases/system-dynamic-management-views/sys-geo-replication-links-azure-sql-database.md) o [Sys.dm_geo_replication_link_status](../../relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database.md) (disponibles en SQL Database V12).
  
  
|Nombre de columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|El identificador de la base de datos actual en la vista `sys.databases`.|  
|**start_date**|**datetimeoffset**|La hora UTC en un centro de datos regional de [!INCLUDE[ssSDS](../../includes/sssds-md.md)] a la que se inició la copia de la base de datos.|  
|**modify_date**|**datetimeoffset**|La hora UTC en un centro de datos regional de [!INCLUDE[ssSDS](../../includes/sssds-md.md)] a la que se ha completado la copia de la base de datos. La nueva base de datos es transaccionalmente coherente con la base de datos principal en este momento. La información de finalización se actualiza cada 1 minuto.<br /><br />Hora UTC que refleja la última actualización del campo percent_complete.|  
|**percent_complete**|**real**|Porcentaje de bytes que se han copiado. Los valores pueden oscilar entre 0 y 100. [!INCLUDE[ssSDS](../../includes/sssds-md.md)] puede recuperarse automáticamente de algunos errores, como la conmutación por error, y reiniciar la copia de la base de datos. En este caso, percent_complete se reiniciaría desde 0.|  
|**error_code**|**int**|Cuando es mayor que 0, el código que indica el error que ha aparecido mientras se realizaba la copia. El valor es igual a 0 si no se ha producido ningún error.|  
|**error_desc**|**nvarchar (4096)**|Descripción del error que se produjo durante la copia.|  
|**error_severity**|**int**|Devuelve 16 si se produjo un error en la copia de la base de datos.|  
|**error_state**|**int**|Devuelve 1 si se produjo un error en la copia.|  
|**copy_guid**|**uniqueidentifier**|IDENTIFICADOR único de la operación de copia.|  
|**partner_server**|**sysname**|Nombre del servidor de SQL Database en el que se crea la copia.|  
|**partner_database**|**sysname**|Nombre de la copia de la base de datos en el servidor asociado.|  
|**replication_state**|**tinyint**|El estado de replicación de copia continua para esta base de datos. Los valores son:<br /><br /> 0 = pendiente. La creación de la copia de la base de datos está programada, pero los pasos de preparación necesarios aún no se han completado o están bloqueados temporalmente por la cuota de propagación.<br /><br /> 1 = propagación. La base de datos de copia que se va a inicializar todavía no está totalmente sincronizada con la base de datos de origen. En este estado, no se puede conectar a la copia. Para cancelar la operación de propagación en curso, se debe quitar la base de datos de copia.|  
|**replication_state_desc**|**nvarchar(256)**|Descripción del estado de replication_state, uno de los siguientes:<br /><br /> PENDING<br /><br /> SEEDING<br />|  
|**maximum_lag**|**int**|Campo reservado.|  
|**is_continuous_copy**|**bit**|0 = devuelve 0|  
|**is_target_role**|**bit**|0 = base de datos de origen<br /><br /> 1 = copiar base de datos|  
|**is_interlink_connected**|bit|Campo reservado.|  
|**is_offline_secondary**|bit|Campo reservado.|  
  
## <a name="permissions"></a>Permisos  
 Esta vista solo está disponible en la base de datos **maestra** para el inicio de sesión principal de nivel de servidor.  
  
## <a name="remarks"></a>Observaciones  
 Puede usar la vista de **Sys.dm_database_copies** en la base de datos **maestra** del servidor de origen o de destino [!INCLUDE[ssSDS](../../includes/sssds-md.md)] . Cuando la copia de la base de datos se completa correctamente y la nueva base de datos se vuelve a poner en línea, la fila de la vista **Sys.dm_database_copies** se quita automáticamente.  
  
  
