---
description: sys.database_event_session_fields (Azure SQL Database)
title: sys.database_event_session_fields (Azure SQL Database) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
ms.assetid: 9b5c94d6-612c-4e0f-976d-ac6ba55da3ac
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: dafeecb84d666b7899890a75f24c1ecfd8a1cb51
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210321"
---
# <a name="sysdatabase_event_session_fields-azure-sql-database"></a>sys.database_event_session_fields (Azure SQL Database)
[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Devuelve una fila para cada columna personalizable que se estableció explícitamente en los eventos y destinos.  
  
||  
|-|  
|**Se aplica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] V12 y a las versiones posteriores.|  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|event_session_id|**int**|Identificador de la sesión de eventos. No admite valores NULL.|  
|object_id|**int**|Id. del objeto al que está asociado este campo. No admite valores NULL.|  
|name|**sysname**|Nombre del campo. No admite valores NULL.|  
|value|**sql_variant**|Valor del campo. No admite valores NULL.|  
  
## <a name="permissions"></a>Permisos  
 Necesita el permiso VIEW DATABASE STATE en el servidor.  
  
## <a name="remarks"></a>Observaciones  
 Esta vista tiene las siguientes cardinalidades de relación.  
  
| From | En | Relación |
| ---- | -- | ------------ |
|Sys.database_event_session_actions sys.database_event_session_actions.event_session_id|Sys.database_event_sessions sys.database_event_sessions.event_session_id|Varios a uno|  
|Sys.database_event_session_actions sys.database_event_session_actions.event_id<br /><br /> Sys.database_event_session_actions sys.database_event_session_actions.object_id<br /><br /> Sys.database_event_session_actions sys.database_event_session_actions.event_session_id|Sys.database_event_session_events sys.database_event_session_events.event_session_id<br /><br /> Sys.database_event_session_events sys.database_event_session_events.event_id|Varios a uno|  
|Sys.database_event_session_actions sys.database_event_session_actions.event_session_id<br /><br /> Sys.database_event_session_actions sys.database_event_session_actions.object_id|Sys.database_event_session_targets sys.database_event_session_targets.event_session_id<br /><br /> Sys.database_event_session_targets sys.database_event_session_targets.target_id|Varios a uno|  
  
## <a name="see-also"></a>Consulte también  
 [Eventos extendidos](../../relational-databases/extended-events/extended-events.md)  
  
  
