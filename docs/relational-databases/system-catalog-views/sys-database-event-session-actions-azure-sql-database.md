---
description: sys.database_event_session_actions (Azure SQL Database)
title: sys.database_event_session_actions (Azure SQL Database) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
ms.assetid: 32494df1-7ab7-4b88-a858-6b1021d67433
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2613739822160969f070413f2d87a661878ae3ff
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210347"
---
# <a name="sysdatabase_event_session_actions-azure-sql-database"></a>sys.database_event_session_actions (Azure SQL Database)
[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Devuelve una fila por cada acción en cada evento de una sesión de eventos.  
  
||  
|-|  
|**Se aplica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] V12 y a las versiones posteriores.|  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|event_session_id|**int**|Identificador de la sesión de eventos. No admite valores NULL.|  
|event_id|**int**|Id. del evento. Este Id. es único dentro del objeto de sesión de eventos. No admite valores NULL.|  
|name|**sysname**|Nombre de la acción. Acepta valores NULL.|  
|paquete|**sysname**|Nombre del paquete de eventos que contiene el evento. Acepta valores NULL.|  
|module|**sysname**|Nombre del módulo que contiene el evento. Acepta valores NULL.|  
  
## <a name="permissions"></a>Permisos  
 Necesita el permiso VIEW DATABASE STATE en el servidor.  
  
## <a name="remarks"></a>Observaciones  
 Esta vista tiene las siguientes cardinalidades de relación.  
  
| From | En | Relación |
| ---- | -- | ------------ |
|Sys.database_event_session_actions sys.database_event_session_actions.event_session_id|sys.sys. database_event_sessions. event_session_id|Varios a uno|  
|Sys.database_event_session_actions sys.database_event_session_actions.event_id<br /><br /> Sys.database_event_session_actions sys.database_event_session_actions.event_session_id|Sys.database_event_session_events sys.database_event_session_events.event_session_id<br /><br /> Sys.database_event_session_events sys.database_event_session_events.event_id|Varios a uno|  
  
  
