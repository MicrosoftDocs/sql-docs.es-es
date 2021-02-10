---
title: sys.fn_PageResCracker (Transact-SQL) | Microsoft Docs
description: Obtenga información acerca de la función del sistema sys.fn_PageResCracker. Vea ejemplos y examine los recursos adicionales disponibles.
ms.custom: ''
ms.date: 09/18/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_PageResCracker
- sys.fn_PageResCracker_TSQL
- fn_PageResCracker_TSQL
- sys.fn_PageResCracker
- sys.dm_db_page_info
dev_langs:
- TSQL
helpviewer_keywords:
- fn_PageResCracker function
- page_resource
- sys.fn_PageResCracker function
- sys.dm_db_page_info
- page info
author: bluefooted
ms.author: pamela
manager: amitban
ms.openlocfilehash: 49e8472353f19bcca00e68969579d4cbf6f0b538
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064830"
---
# <a name="sysfn_pagerescracker-transact-sql"></a>sys.fn_PageResCracker (Transact-SQL)
[!INCLUDE[SQL Server 2019](../../includes/applies-to-version/sqlserver2019.md)]

Devuelve `db_id` , `file_id` y `page_id` para el `page_resource` valor especificado. 
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
```  
sys.fn_PageResCracker ( page_resource )  
```  
  
## <a name="arguments"></a>Argumentos  
*page_resource*    
Es el formato hexadecimal de 8 bytes de un recurso de página de base de datos.
  
## <a name="tables-returned"></a>Tablas devueltas  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|db_id|**int**|Identificador de base de datos|  
|file_id|**int**|Id. de archivo|  
|page_id|**int**|Identificador de página|  
  
## <a name="remarks"></a>Observaciones  
`sys.fn_PageResCracker` se utiliza para convertir la representación hexadecimal de 8 bytes de una página de base de datos en un conjunto de filas que contiene el identificador de base de datos, el identificador de archivo y el ID. de página de la página.   

Puede obtener un recurso de página válido de la `page_resource` columna de la sys.dm_exec_requests &#40;vista de administración dinámica de [TRANSACT-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md) o los procesos desys.sys&#40;vista del sistema [ de Transact-SQL ](../../relational-databases/system-compatibility-views/sys-sysprocesses-transact-sql.md)&#41;. Si se usa un recurso de página no válido, el valor devuelto es NULL.  
El uso principal de `sys.fn_PageResCracker` es facilitar combinaciones entre estas vistas y el sys.dm_db_page_info &#40;función de administración dinámica [&#41;de TRANSACT-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-page-info-transact-sql.md) con el fin de obtener información sobre la página, como el objeto al que pertenece.
  
## <a name="permissions"></a>Permisos  
El usuario necesita el `VIEW SERVER STATE` permiso en el servidor.  
  
## <a name="examples"></a>Ejemplos  
La `sys.fn_PageResCracker` función se puede usar junto con [sys.dm_db_page_info &#40;&#41;de TRANSACT-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-page-info-transact-sql.md) para solucionar problemas de esperas y bloqueos relacionados con las páginas en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  El script siguiente es un ejemplo de cómo puede usar estas funciones para recopilar información de la página de base de datos para todas las solicitudes activas que están esperando actualmente algún tipo de recurso de página. 
  
```sql  
SELECT page_info.* 
FROM sys.dm_exec_requests AS d  
CROSS APPLY sys.fn_PageResCracker (d.page_resource) AS r  
CROSS APPLY sys.dm_db_page_info(r.db_id, r.file_id, r.page_id, 1) AS page_info
```  
  
## <a name="see-also"></a>Consulte también  
 [sys.dm_db_page_info &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-page-info-transact-sql.md)  
 [sys.sysprocesos &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-sysprocesses-transact-sql.md)   
 [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  
  
  
