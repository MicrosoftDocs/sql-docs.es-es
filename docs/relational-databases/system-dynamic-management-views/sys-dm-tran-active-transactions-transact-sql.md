---
description: sys.dm_tran_active_transactions (Transact-SQL)
title: sys.dm_tran_active_transactions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_tran_active_transactions
- sys.dm_tran_active_transactions_TSQL
- dm_tran_active_transactions_TSQL
- dm_tran_active_transactions
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_active_transactions dynamic management view
ms.assetid: 154ad6ae-5455-4ed2-b014-e443abe2c6ee
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 218ecd2aba829859db0617c4a4a8a2c18b4556db
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203365"
---
# <a name="sysdm_tran_active_transactions-transact-sql"></a>sys.dm_tran_active_transactions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve información sobre transacciones de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_tran_active_transactions**.  
  
|Nombre de columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|transaction_id|**bigint**|Identificador de la transacción en el nivel de instancia, no en el de base de datos. Es solo exclusiva en todas las bases de datos dentro de una instancia, pero no es exclusiva en todas las instancias del servidor.|  
|name|**nvarchar(32)**|Nombre de transacción. Se sobrescribe si la transacción está marcada y el nombre marcado reemplaza el nombre de transacción.|  
|transaction_begin_time|**datetime**|Hora en que se ha iniciado la transacción.|  
|transaction_type|**int**|Tipo de transacción.<br /><br /> 1 = Transacciones de lectura/escritura<br /><br /> 2 = Transacción de solo lectura<br /><br /> 3 = Transacción de sistema<br /><br /> 4 = Transacción distribuida|  
|transaction_uow|**uniqueidentifier**|Identificador de la unidad de trabajo (UOW) de la transacción para transacciones distribuidas. MS DTC usa el identificador UOW para trabajar con la transacción distribuida.|  
|transaction_state|**int**|0 = La transacción aún no se ha inicializado completamente.<br /><br /> 1 = La transacción se ha inicializado, pero no ha comenzado.<br /><br /> 2 = La transacción está activa.<br /><br /> 3 = La transacción ha finalizado. Se utiliza para transacciones de solo lectura.<br /><br /> 4 = El proceso de confirmación se ha iniciado en la transacción distribuida. Es solo para transacciones distribuidas. La transacción distribuida aún está activa, pero no se puede realizar ningún otro proceso adicional.<br /><br /> 5 = La transacción está en estado de preparada y esperando una resolución.<br /><br /> 6 = La transacción se ha confirmado.<br /><br /> 7 = La transacción se está revirtiendo.<br /><br /> 8 = La transacción se ha revertido.|  
|transaction_status|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|transaction_status2|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|dtc_state|**int**|**Se aplica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] (versión inicial hasta la [versión actual](/previous-versions/azure/ee336279(v=azure.100))).<br /><br /> 1 = ACTIVE<br /><br /> 2 = PREPARED<br /><br /> 3 = COMMITTED<br /><br /> 4 = ABORTED<br /><br /> 5 = RECOVERED|  
|dtc_status|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|dtc_isolation_level|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|filestream_transaction_id|**varbinary(128)**|**Se aplica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] (versión inicial hasta la [versión actual](/previous-versions/azure/ee336279(v=azure.100))).<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|pdw_node_id|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
## <a name="permissions"></a>Permisos

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, `Server admin` `Azure Active Directory admin` se requiere la cuenta o. En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   
  
## <a name="see-also"></a>Consulte también  
 [sys.dm_tran_session_transactions &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-session-transactions-transact-sql.md)   
 [sys.dm_tran_database_transactions &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-tran-database-transactions-transact-sql.md)   
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funciones y vistas de administración dinámica relacionadas con transacciones &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
