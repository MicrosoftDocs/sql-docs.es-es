---
description: sys.dm_io_pending_io_requests (Transact-SQL)
title: sys.dm_io_pending_io_requests (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_io_pending_io_requests
- dm_io_pending_io_requests
- dm_io_pending_io_requests_TSQL
- sys.dm_io_pending_io_requests_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_io_pending_io_requests dynamic management view
ms.assetid: d1fb46dd-5c74-4c04-9ecf-8934b1bedb5b
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4a13fe62558b85ab0ba9448793fba797f3e388b0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185034"
---
# <a name="sysdm_io_pending_io_requests-transact-sql"></a>sys.dm_io_pending_io_requests (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve una fila para cada solicitud de E/S pendiente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_io_pending_io_requests**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**io_completion_request_address**|**varbinary(8**|Dirección de memoria de la solicitud de E/S. No admite valores NULL.|  
|**io_type**|**nvarchar(60)**|Tipo de solicitud de E/S pendiente. No admite valores NULL.|  
|**io_pending_ms_ticks**|**bigint**|Solo para uso interno. No admite valores NULL.| 
|**io_pending**|**int**|Indica si la solicitud de E/S sigue pendiente o Windows ya la ha completado. Una solicitud de E/S puede seguir pendiente incluso cuando Windows la ha completado; esto se debe a que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aún no ha efectuado un cambio de contexto en el que se procesaría esta solicitud de E/S ni la ha quitado de esta lista. No admite valores NULL.|  
|**io_completion_routine_address**|**varbinary(8**|Función interna utilizada para llamar cuando se ha completado la solicitud de E/S. Acepta valores NULL.|  
|**io_user_data_address**|**varbinary(8**|Solo para uso interno. Acepta valores NULL.|  
|**scheduler_address**|**varbinary(8**|Programador en el que se ha emitido esta solicitud de E/S. La solicitud de E/S aparecerá en la lista de solicitudes de E/S pendientes del programador. Para obtener más información, vea [sys.dm_os_schedulers &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md). No admite valores NULL.|  
|**io_handle**|**varbinary(8**|Identificador del archivo que se utiliza en la solicitud de E/S. Acepta valores NULL.|  
|**io_offset**|**bigint**|Desplazamiento de la solicitud de E/S. No admite valores NULL.|  
|**io_handle_path**|**nvarchar(256)**| Ruta de acceso del archivo que se usa en la solicitud de e/s. Acepta valores NULL.|
|**pdw_node_id**|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
## <a name="permissions"></a>Permisos  

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, `Server admin` `Azure Active Directory admin` se requiere la cuenta o. En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [I O funciones y vistas de administración dinámica relacionadas &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


