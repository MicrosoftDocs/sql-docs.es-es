---
description: sys.tcp_endpoints (Transact-SQL)
title: sys.tcp_endpoints (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.tcp_endpoints
- sys.tcp_endpoints_TSQL
- tcp_endpoints
- tcp_endpoints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.tcp_endpoints catalog view
ms.assetid: 43cc3afa-cced-4463-8e97-fbfdaf2e4fa8
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: bb3934622b68df4ef46090729afbb05217b89126
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185817"
---
# <a name="systcp_endpoints-transact-sql"></a>sys.tcp_endpoints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila por cada extremo TCP que haya en el sistema. Los extremos que se describen en **Sys.tcp_endpoints** proporcionan un objeto para conceder y revocar el privilegio de conexión. La información que se muestra sobre puertos y direcciones IP no se utiliza para configurar los protocolos y es posible que no coincida con la configuración real del protocolo. Para ver y configurar protocolos, utilice el Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**< columnas heredadas>**||Hereda columnas de [Sys. endpoints](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md).|  
|**port**|int|Número de puerto en el que escucha el extremo. No admite valores NULL.|  
|**is_dynamic_port**|bit|1 = El número de puerto se asignó dinámicamente.<br /><br /> No admite valores NULL.|  
|**ip_address**|**nvarchar (45)**|Dirección IP de escucha según se especifica en la cláusula LISTENER_IP. Acepta valores NULL.|  
  
## <a name="remarks"></a>Observaciones  
 Ejecute la siguiente consulta para reunir información sobre los extremos y las conexiones. Los extremos sin conexiones actuales o sin conexiones TCP aparecerán con valores NULL. Agregue la  cláusula WHERE `WHERE des.session_id = @@SPID` para devolver información sobre la conexión actual.  
  
```  
SELECT des.login_name, des.host_name, program_name,  dec.net_transport, des.login_time,   
e.name AS endpoint_name, e.protocol_desc, e.state_desc, e.is_admin_endpoint,   
t.port, t.is_dynamic_port, dec.local_net_address, dec.local_tcp_port   
FROM sys.endpoints AS e  
LEFT JOIN sys.tcp_endpoints AS t  
   ON e.endpoint_id = t.endpoint_id  
LEFT JOIN sys.dm_exec_sessions AS des  
   ON e.endpoint_id = des.endpoint_id  
LEFT JOIN sys.dm_exec_connections AS dec  
   ON des.session_id = dec.session_id;  
```  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Endpoints Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md) [Vistas de catálogo de extremos &#40;Transact-SQL&#41;]  
  
  
