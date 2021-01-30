---
description: sys.dm_pdw_resource_waits (Transact-SQL)
title: sys.dm_pdw_resource_waits (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/26/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: a43ce9a2-5261-41e3-97f0-555ba05ebed9
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: f6685853de3fed6397886d654135bb6ce499e7da
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203395"
---
# <a name="sysdm_pdw_resource_waits-transact-sql"></a>sys.dm_pdw_resource_waits (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Muestra información de espera para todos los tipos de recursos de [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] .  
  
|Nombre de columna|Tipo de datos|Descripción|Intervalo|  
|-----------------|---------------|-----------------|-----------|  
|wait_id|**bigint**|Posición de la solicitud en la lista de espera.|ordinal basado en 0. Esto no es único en todas las entradas de espera.|  
|session_id|**nvarchar(32)**|IDENTIFICADOR de la sesión en la que se ha producido el estado de espera.|Vea session_id en [sys.dm_pdw_exec_sessions &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-sessions-transact-sql.md).|  
|tipo|**nvarchar(255)**|Tipo de espera que esta entrada representa.|Valores posibles:<br /><br /> Conexión<br /><br /> Simultaneidad de consultas locales<br /><br /> Simultaneidad de consultas distribuidas<br /><br /> Simultaneidad de DMS<br /><br /> Simultaneidad de copias de seguridad|  
|object_type|**nvarchar(255)**|Tipo de objeto que se ve afectado por la espera.|Valores posibles:<br /><br /> **OBJETO**<br /><br /> **DATABASE**<br /><br /> **INTEGRADO**<br /><br /> **SCHEMA**<br /><br /> **APLICACIÓN**|  
|object_name|**nvarchar (386)**|Nombre o GUID del objeto especificado que se ha visto afectado por la espera.|Las tablas y vistas se muestran con nombres de tres partes.<br /><br /> Los índices y las estadísticas se muestran con nombres de cuatro partes.<br /><br /> Los nombres, entidades de seguridad y bases de datos son nombres de cadena.|  
|request_id|**nvarchar(32)**|IDENTIFICADOR de la solicitud en la que se produjo el estado de espera.|Identificador de QID de la solicitud.<br /><br /> Identificador GUID para las solicitudes de carga.|  
|request_time|**datetime**|Hora a la que se solicitó el bloqueo o el recurso.||  
|acquire_time|**datetime**|Hora a la que se adquirió el bloqueo o el recurso.||  
|state|**nvarchar(50)**|Estado del estado de espera.|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|  
|priority|**int**|Prioridad del elemento de espera.|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|  
|concurrency_slots_used|**int**|Interno|Vea las [esperas de recursos de monitor](#monitor-resource-waits) a continuación|  
|resource_class|**nvarchar (20)**|Interno |Vea las [esperas de recursos de monitor](#monitor-resource-waits) a continuación|  
  
## <a name="monitor-resource-waits"></a>Supervisar esperas de recursos 
Con la introducción de [grupos de cargas de trabajo](/azure/sql-data-warehouse/sql-data-warehouse-workload-isolation), los espacios de simultaneidad ya no son aplicables.  Utilice la consulta siguiente y la `resources_requested` columna para comprender los recursos necesarios para ejecutar la solicitud.

```sql
select rw.wait_id
      ,rw.session_id
      ,rw.type
      ,rw.object_type
      ,rw.object_name
      ,rw.request_id
      ,rw.request_time
      ,rw.acquire_time
      ,rw.state
      ,resources_requested = s.effective_request_min_resource_grant_percent
      ,r.group_name
  from sys.dm_workload_management_workload_groups_stats s
  join sys.dm_pdw_exec_requests r on r.group_name = s.name collate SQL_Latin1_General_CP1_CI_AS
  join sys.dm_pdw_resource_waits rw on rw.request_id = r.request_id
```

## <a name="see-also"></a>Consulte también  
 [Azure Synapse Analytics y vistas de administración dinámica de almacenamiento de datos paralelos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
