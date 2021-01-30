---
description: sys.dm_xe_session_object_columns (Transact-SQL)
title: sys.dm_xe_session_object_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_xe_session_object_columns_TSQL
- sys.dm_xe_session_object_columns_TSQL
- dm_xe_session_object_columns
- sys.dm_xe_session_object_columns
dev_langs:
- TSQL
helpviewer_keywords:
- xe
- sys.dm_xe_session_object_columns dynamic management view
ms.assetid: e97f3307-2da6-4c54-b818-a474faec752e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e8a7cf766e172d0cb62a3051894be390b176361c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99128621"
---
# <a name="sysdm_xe_session_object_columns-transact-sql"></a>sys.dm_xe_session_object_columns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Muestra los valores de configuración de los objetos enlazados a una sesión.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|event_session_address|**varbinary(8**|La dirección de memoria de la sesión de eventos. Tiene una relación de varios a uno con sys.dm_xe_sessions.address. No admite valores NULL.|  
|column_name|**nvarchar(256)**|El nombre del valor de configuración. No admite valores NULL.|  
|column_id|**int**|IDENTIFICADOR de la columna. Es único en el objeto. No admite valores NULL.|  
|column_value|**nvarchar (a.**|El valor configurado de la columna. Acepta valores NULL.|  
|object_type|**nvarchar(60)**|Tipo del objeto. No admite valores NULL. object_type es uno de los siguientes:<br /><br /> event<br /><br /> Destino|  
|object_name|**nvarchar(256)**|Nombre del objeto al que pertenece esta columna. No admite valores NULL.|  
|object_package_guid|**uniqueidentifier**|GUID del paquete que contiene el objeto. No admite valores NULL.|  
  
## <a name="permissions"></a>Permisos  
 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
  
### <a name="relationship-cardinalities"></a>Cardinalidades de relación  
  
|From|En|Relación|  
|----------|--------|------------------|  
|dm_xe_session_object_columns dm_xe_session_object_columns.object_name,<br /><br /> dm_xe_session_object_columns.object_package_guid|Sys.dm_xe_objects sys.dm_xe_objects.package_guid,<br /><br /> sys.dm_xe_objects.name|Varios a uno|  
|dm_xe_session_object_columns dm_xe_session_object_columns.column_name,<br /><br /> dm_xe_session_object_columns.column_id|sys.dm_xe_object_columns. Name,<br /><br /> sys.dm_xe_object_columns.column_id|Varios a uno|  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  

