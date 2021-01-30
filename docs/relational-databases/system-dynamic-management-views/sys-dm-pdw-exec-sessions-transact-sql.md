---
description: sys.dm_pdw_exec_sessions (Transact-SQL)
title: sys.dm_pdw_exec_sessions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/22/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 31c262b3-7e4d-44c4-af71-aaef0fd1a980
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 829eb5d53fc4e60c7c975a75a2e3159885b85171
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99142448"
---
# <a name="sysdm_pdw_exec_sessions-transact-sql"></a>sys.dm_pdw_exec_sessions (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Contiene información acerca de todas las sesiones que se han abierto actualmente o recientemente en el dispositivo. Muestra una fila por sesión.  
  
|Nombre de columna|Tipo de datos|Descripción|Intervalo|  
|-----------------|---------------|-----------------|-----------|  
|session_id|**nvarchar(32)**|Identificador de la consulta actual o de la última ejecución de la consulta (si se termina la sesión y la consulta se estaba ejecutando en el momento de la finalización). Clave para esta vista.|Único en todas las sesiones del sistema.|  
|status|**nvarchar(10**|En el caso de las sesiones actuales, identifica si la sesión está activa o inactiva. En el caso de las sesiones anteriores, el estado de la sesión puede mostrar cerrado o eliminado (si la sesión se cerró de manera forzada).|' ACTIVE ', ' CLOSED ', ' IDLE ', ' TERMINATED '|  
|request_id|**nvarchar(32)**|Identificador de la consulta actual o de la última ejecución de la consulta.|Único en todas las solicitudes del sistema. Es NULL si no se ha ejecutado ninguno.|  
|security_id|**varbinary(85)**|IDENTIFICADOR de seguridad de la entidad de seguridad que ejecuta la sesión.||  
|login_name|**nvarchar(128)**|Nombre de inicio de sesión de la entidad de seguridad que ejecuta la sesión.|Cualquier cadena que se ajuste a las convenciones de nomenclatura de usuario.|  
|login_time|**datetime**|Fecha y hora en que se creó el usuario que inició sesión y esta sesión.|**DateTime** válido antes de la hora actual.|  
|query_count|**int**|Captura el número de consultas/la sesión requeststhis se ha ejecutado desde la creación.|Mayor o igual que 0.|  
|is_transactional|**bit**|Captura si una sesión está actualmente dentro de una transacción o no.|0 para la confirmación automática, 1 para transaccional.|  
|client_id|**nvarchar(255)**|Captura la información de cliente para la sesión.|Cualquier cadena válida.|  
|app_name|**nvarchar(255)**|Captura la información del nombre de la aplicación opcionalmente establecida como parte del proceso de conexión.|Cualquier cadena válida.|  
|sql_spid|**int**|Número de ID. del SPID. Use `session_id` esta sesión. Use la `sql_spid` columna para unirse a **Sys.dm_pdw_nodes_exec_sessions**.<br /><br /> ADVERTENCIA: esta columna contiene SPID cerrados. **\* \* \* \***||  
  
 Para obtener información acerca de las filas máximas retenidas en esta vista, consulte la sección de metadatos en el tema [límites de capacidad](/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits#metadata) .  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso `VIEW SERVER STATE`.  
  
## <a name="see-also"></a>Consulte también  
 [Azure Synapse Analytics y vistas de administración dinámica de almacenamiento de datos paralelos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
