---
description: sys.dm_server_memory_dumps (Transact-SQL)
title: sys.dm_server_memory_dumps (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_server_memory_dumps_TSQL
- dm_server_memory_dumps_TSQL
- dm_server_memory_dumps
- sys.dm_server_memory_dumps
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_server_memory_dumps dynamic management view
ms.assetid: 41782719-f54d-4e11-941a-c050c7576e23
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 937e73f48f43693d1c976534c2c272f58d4fed14
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99134607"
---
# <a name="sysdm_server_memory_dumps-transact-sql"></a>sys.dm_server_memory_dumps (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila por cada archivo de volcado de memoria generado por [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Use esta vista de administración dinámica para solucionar problemas potenciales.  
 
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**filename**|**nvarchar(256)**|Ruta de acceso y nombre del archivo de volcado de memoria. No puede ser NULL.|  
|**creation_time**|**datetimeoffset(7)**|Fecha y hora en que se creó el archivo. No puede ser NULL.|  
|**size_in_bytes**|**bigint**|Tamaño (en bytes) del archivo. Acepta valores NULL.|  
  
## <a name="general-remarks"></a>Notas generales  
 El tipo de volcado de memoria puede ser un minivolcado de memoria, un volcado de memoria de todos los subprocesos o un volcado de memoria completo. Los archivos tienen una extensión .mdmp.  
  
## <a name="security"></a>Seguridad  
 Los archivos de volcado de memoria pueden contener información confidencial. Para proteger la información confidencial, puede utilizar una lista de control de acceso (ACL) con objeto de restringir el acceso a los archivos o copiarlos en una carpeta con acceso restringido. Por ejemplo, antes de enviar los archivos de depuración a los servicios de soporte técnico de Microsoft, se recomienda que quite cualquier información confidencial o confidencial.  
  
### <a name="permissions"></a>Permisos  
 Requiere el permiso VIEW SERVER STATE.  
  
  
