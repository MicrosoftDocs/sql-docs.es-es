---
description: sys.database_event_session_events (Azure SQL Database)
title: sys.database_event_session_events (Azure SQL Database) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
ms.assetid: f4c9eb0a-173c-4c66-8dd8-6f7176b2657f
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 29cdcc2ebdb8069209b3f919e9d1f01b1aa24507
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210342"
---
# <a name="sysdatabase_event_session_events-azure-sql-database"></a>sys.database_event_session_events (Azure SQL Database)
[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Devuelve una fila por cada evento de una sesión de eventos.  
  
||  
|-|  
|**Se aplica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] V12 y a las versiones posteriores.|  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|event_session_id|**int**|Identificador de la sesión de eventos. No admite valores NULL.|  
|event_id|**int**|Id. del evento. Este Id. es único dentro de un objeto de sesión de eventos. No admite valores NULL.|  
|name|**sysname**|Nombre del evento. No admite valores NULL.|  
|paquete|**sysname**|Nombre del paquete de eventos que contiene el evento. No admite valores NULL.|  
|module|**sysname**|Nombre del módulo que contiene el evento. No admite valores NULL.|  
|predicate|**nvarchar (3000)**|La expresión de predicado aplicada al evento. Acepta valores NULL.|  
|predicate_xml|**nvarchar (3000)**|La expresión de predicado XML aplicada al evento. Acepta valores NULL.|  
  
## <a name="permissions"></a>Permisos  
 Necesita el permiso VIEW DATABASE STATE en el servidor.  
  
## <a name="remarks"></a>Observaciones  
 Esta vista tiene las siguientes cardinalidades de relación.  
  
| From | En | Relación |
| ---- | -- | ------------ |
|Sys.database_event_session_events sys.database_event_session_events.event_session_id|Sys.database_event_sessions sys.database_event_sessions.event_session_id|Varios a uno|  
  
## <a name="see-also"></a>Consulte también  
 [Eventos extendidos](../../relational-databases/extended-events/extended-events.md)  
  
  
