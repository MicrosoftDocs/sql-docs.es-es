---
description: sys.dm_pdw_exec_connections (Transact-SQL)
title: sys.dm_pdw_exec_connections (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 2625466b-d0ef-4c71-bedc-6d13491a8351
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 6819fe66d50d839f1a135b33a329d5d9e57dc8e5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99142981"
---
# <a name="sysdm_pdw_exec_connections-transact-sql"></a>sys.dm_pdw_exec_connections (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Devuelve información acerca de las conexiones establecidas con esta instancia de [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] y los detalles de cada conexión.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|session_id|**int**|Identifica la sesión asociada a esta conexión. Use `SESSION_ID()` para devolver el `session_id` de la conexión actual.|  
|connect_time|**datetime**|Marca de tiempo en que se estableció la conexión. No admite valores NULL.|  
|encrypt_option|**nvarchar(40)**|Indica TRUE (la conexión está cifrada) o FALSE (la conexión no es enctypred).|  
|auth_scheme|**nvarchar(40)**|Especifica el esquema de autenticación [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Windows usado en esta conexión. No admite valores NULL.|  
|client_id|**varchar(48)**|Dirección IP del cliente que se conecta a este servidor. Acepta valores NULL.|  
|sql_spid|**int**|IDENTIFICADOR de proceso de servidor de la conexión. Use `@@SPID` para devolver el `sql_spid` de la conexión actual. En la mayoría de los casos, use `session_id` en su lugar.|  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso **View Server State** en el servidor.  
  
## <a name="relationship-cardinalities"></a>Cardinalidades de relación  
  
| From | En | Relación |
| ---- | -- | ------------ |
|dm_pdw_exec_sessions dm_pdw_exec_sessions.session_id|dm_pdw_exec_connections dm_pdw_exec_connections.session_id|Uno a uno|  
|dm_pdw_exec_requests dm_pdw_exec_requests.connection_id|dm_pdw_exec_connections dm_pdw_exec_connections.connection_id|Varios a uno|  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 Consulta típica para recopilar información sobre una conexión solo para consultas.  
  
```  
SELECT  
    c.session_id, c.encrypt_option,  
    c.auth_scheme, s.client_id, s.login_name,   
    s.status, s.query_count  
FROM sys.dm_pdw_exec_connections AS c  
JOIN sys.dm_pdw_exec_sessions AS s  
    ON c.session_id = s.session_id  
WHERE c.session_id = SESSION_ID();  
```  
  
## <a name="see-also"></a>Consulte también  
 [Azure Synapse Analytics y vistas de administración dinámica de almacenamiento de datos paralelos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  

