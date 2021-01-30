---
description: sys.dm_os_loaded_modules (Transact-SQL)
title: sys.dm_os_loaded_modules (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_loaded_modules
- dm_os_loaded_modules
- sys.dm_os_loaded_modules_TSQL
- dm_os_loaded_modules_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_loaded_modules dynamic management view
ms.assetid: 56c7743a-b568-4943-bd3b-73c57d9d641c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 5d807593527ea08586aaaefdbf46c0fa9ee874ef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184876"
---
# <a name="sysdm_os_loaded_modules-transact-sql"></a>sys.dm_os_loaded_modules (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila por cada módulo cargado en el espacio de direcciones del servidor.  
  
> [!NOTE]  
>  Para llamarlo desde [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , utilice el nombre **Sys.dm_pdw_nodes_os_loaded_modules**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**base_address**|**varbinary(8**|Dirección del módulo en proceso.|  
|**file_version**|**VARCHAR (23)**|Versión del archivo. Aparece en el siguiente formato:<br /><br /> x.x:x.x|  
|**product_version**|**VARCHAR (23)**|Versión del producto. Aparece en el siguiente formato:<br /><br /> x.x:x.x|  
|**depura**|**bit**|1 = El módulo es una versión de depuración del módulo cargado.|  
|**patched**|**bit**|1 = El módulo se ha revisado.|  
|**versión preliminar**|**bit**|1 = El módulo es una versión preliminar del módulo cargado.|  
|**private_build**|**bit**|1 = El módulo es una versión privada del módulo cargado.|  
|**special_build**|**bit**|1 = El módulo es una versión especial del módulo cargado.|  
|**language**|**int**|Información del idioma de la versión del módulo.|  
|**recíproca**|**nvarchar(256)**|Nombre de la compañía que ha creado el módulo.|  
|**description**|**nvarchar(256)**|Descripción del módulo.|  
|**name**|**nvarchar(255)**|Nombre del módulo. Incluye la ruta de acceso completa del módulo.|  
|**pdw_node_id**|**int**|**Se aplica a**: [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
## <a name="permissions"></a>Permisos  
 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [SQL Server vistas de administración dinámica relacionadas con el sistema operativo &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  
