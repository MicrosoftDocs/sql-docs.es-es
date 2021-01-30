---
description: sys.dm_xe_session_targets (Transact-SQL)
title: sys.dm_xe_session_targets (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_xe_session_targets
- dm_xe_session_targets_TSQL
- dm_xe_session_targets
- sys.dm_xe_session_targets_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_xe_session_targets dynamic management view
- extended events [SQL Server], views
ms.assetid: 76fbc3e1-ad88-4a47-8bf1-471c3bee5ad8
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6723fe0085ac07d00ed325f0202e4175a4d77c4a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99127832"
---
# <a name="sysdm_xe_session_targets-transact-sql"></a>sys.dm_xe_session_targets (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información acerca de los destinos de la sesión.  
  
  |Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|event_session_address|**varbinary(8**|La dirección de memoria de la sesión de eventos. Tiene una relación de varios a uno con sys.dm_xe_sessions.address. No admite valores NULL.|  
|target_name|**nvarchar(60)**|El nombre del destino dentro de una sesión. No admite valores NULL.|  
|target_package_guid|**uniqueidentifier**|GUID del paquete que contiene el destino. No admite valores NULL.|  
|execution_count|**bigint**|El número de veces que se ha ejecutado el destino para la sesión. No admite valores NULL.|  
|execution_duration_ms|**bigint**|El tiempo total, en milisegundos, que se ha estado ejecutando el destino. No admite valores NULL.|  
|target_data|**nvarchar(max)**|Datos que mantiene el destino como, por ejemplo, información de agregación de eventos. Acepta valores NULL.|  
  
## <a name="permissions"></a>Permisos  
 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
  
### <a name="relationship-cardinalities"></a>Cardinalidades de relación  
  
|From|En|Relación|  
|----------|--------|------------------|  
|sys.dm_xe_session_targets.event_session_address|sys.dm_xe_sessions.address|Varios a uno|  
  
## <a name="change-history"></a>Historial de cambios  
  
|Contenido actualizado|  
|---------------------|  
|Se corrige el tipo de datos de la columna target_data.|  
|Se corrige la descripción de la columna target_data  para que indique que el valor admite valores NULL.|  
|Se corrige la tabla "Cardinalidades de relación".|  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  

